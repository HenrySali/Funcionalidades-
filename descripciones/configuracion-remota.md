# Configuración Remota de Sensores

Permite cambiar los parámetros de operación de un sensor (intervalos, umbral de voltaje, modo) desde la interfaz, sin acceso físico al hardware. La comunicación se hace por MQTT con confirmación (ACK).

---

## 1. Flujo de Configuración

1. El usuario envía la config desde la UI (*Diagnóstico → Configuración*).
2. El servidor publica en `sensores/config/{sensorId}` con `retain`.
3. El sensor recibe la config, la aplica y envía un **ACK**.
4. El servidor registra `confirmed_at` en `sensor_config`.

---

## 2. Tipos de Configuración

| Config | Descripción | Tabla |
|--------|-------------|-------|
| `sleep_interval` | Intervalo en modo deep sleep (minutos) | `sensor_config` |
| `realtime_interval` | Intervalo en modo continuo (minutos) | `sensor_config` |
| `voltage_threshold` | Umbral de voltaje para cambio de modo | `sensor_config` |
| `sensor_mode` | Modo actual (`continuous` / `deep_sleep`) | `sensor_config` |

---

## 3. Historial de Configuración

La tabla `sensor_config_history` registra automáticamente cada cambio con:

- Tipo de cambio, valor anterior y nuevo.
- Fuente del cambio (UI, MQTT, sistema).
- Confirmación de ACK.

Visible en *Diagnóstico → pestaña "Historial Config"* con filtros por fecha, tipo y límite.

---

## 4. Reenvío Automático (`resendPendingConfigs`)

Cuando un sensor envía datos sin `config_ack`, el servidor:

1. Consulta `sensor_config` buscando configs con `confirmed_at IS NULL`.
2. Reenvía las configs pendientes por MQTT con `retain`.
3. Resetea `retry_count = 0`.

> **Throttle:** no consulta la BD más de 1 vez cada 2 min por sensor.

---

## 5. Envío Paralelo de Configuraciones

Desde la tarjeta "bundle" de la pestaña Configuración se pueden enviar varios parámetros a la vez:

- Envío paralelo con `Promise.all`.
- Selector de sensor con carga automática de la config actual.
- Opción **"No cambiar"** por cada parámetro.
- Resultado individual por config (confirmado / enviado / error).
- Tras aplicar una config, el sensor mide y envía un dato inmediatamente.
