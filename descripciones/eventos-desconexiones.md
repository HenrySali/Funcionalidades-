# Sistema de Eventos y Desconexiones

Lógica de backend que detecta cuándo un sensor deja de enviar datos, distingue desconexiones reales de falsos positivos y registra cada cambio de estado para trazabilidad y alertas.

---

## 1. Valor centinela `0.1111`

El valor `0.1111` en el campo de temperatura **indica que el sensor está desconectado**. Es un valor centinela usado internamente para marcar desconexiones en la base de datos (constante `VALOR_DESCONEXION`).

---

## 2. Flujo de Detección de Desconexión

1. El monitor de timeout detecta que un sensor no envía datos en el tiempo esperado.
2. Registra un evento `disconnected` en la tabla `sensor_events`.
3. Envía un **reset remoto MQTT** al sensor.
4. Espera 1 minuto:
   - Si el sensor responde → **"Falso positivo"**: no se inserta `0.1111` y no se envía correo.
   - Si no responde → **"Desconexión confirmada"**: se inserta `0.1111` y se envía correo.

---

## 3. Protección contra Falsos Positivos

- **Reset de verificación:** antes de confirmar una desconexión se envía un reset MQTT y se espera respuesta.
- **Flag `sensorVerificando`:** bloquea el envío de correo durante la verificación.
- **No re-detección:** si el sensor ya está marcado como `disconnected`, no se genera otro evento.
- **Escritura unificada:** un único punto de escritura en `sensor_events` (función `registerSensorEvent()`).

---

## 4. Cálculo de Timeout

| Modo | Timeout | Notas |
|------|---------|-------|
| Continuo | `realtime_interval` + margen | Usa `max(previous_value, value)` si hay cambio pendiente |
| Deep Sleep | `sleep_interval` + 5 min | Si `confirmed_at` es NULL → timeout amplio (65 min) |

El modo se determina por `sensor_mode` de la BD, **no** por el voltaje reportado.

---

## 5. Tabla `sensor_events`

Registra todos los cambios de estado con:

- `event_type`: `connected` / `disconnected`.
- `description`: diagnóstico en español (ej: *"dejó de enviar hace 8 min"*, *"Falso positivo"*, *"Desconexión confirmada"*).
- **Análisis batch:** recorre los datos acumulados en orden cronológico usando el timestamp real.

---

## 6. Verificaciones pendientes (tests de correos)

- Desconexión física de sensor en modo continuo → debe mandar correo.
- Reconexión de sensor → debe mandar correo "volvió a la normalidad".
- Sensor en deep sleep real → **NO** debe mandar correo de desconexión.
- Sensor que estuvo en deep sleep y volvió a continuo → debe mandar correo si se desconecta.

### Bug conocido
`previous_value` se sobreescribe cada vez que llega un dato con el mismo modo. Si llegan dos datos con `mode = continuous`, se pierde la referencia al modo anterior. Solución propuesta:

```sql
ON DUPLICATE KEY UPDATE
  previous_value = IF(value != VALUES(value), value, previous_value),
  value = ?
```
