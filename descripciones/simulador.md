# Simulador de Sensores

Simulador **server-side** que replica el comportamiento del firmware ESP8266 en JavaScript. Corre como parte del servidor Node.js y persiste independientemente del navegador. Usa MQTT real (librería `mqtt`) publicando en `sensores/datos`. Sirve para testing y demos sin hardware real.

---

## 1. Acceso

*Diagnóstico → pestaña "Simulador"*. Panel con estilo industrial oscuro (dark slate con acentos cyan).

---

## 2. Funcionalidades

- **Tipos de sensor:** DS18B20 (−55 °C a 125 °C) y PT100 (−200 °C a 850 °C).
- **Campos editables:** IP, MAC, Gateway, Subnet, servidor destino.
- **Modos:** Continuo (MQTT permanente) y Deep Sleep (MQTT solo al enviar batch).
- **Controles:** Iniciar, Detener, Reiniciar, WiFi On/Off, Sensor On/Off.
- **Medición Errónea:** simula timeout de lectura con 3 reintentos y 30 % de recuperación.
- **Persistencia:** configuración guardada en JSON, restaurada al reiniciar el servidor.
- **Almacenamiento offline:** acumula datos cuando el WiFi está apagado y envía el batch al reconectar.
- **Prioridad BD:** al restaurar, consulta `sensor_config` y aplica los intervalos de la BD sobre el JSON.

---

## 3. API REST del Simulador

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| GET | `/api/simulador/lista` | Listar todos los simuladores |
| POST | `/api/simulador/crear` | Crear simulador con `sensorId` configurable |
| POST | `/api/simulador/:id/iniciar` | Iniciar simulador |
| POST | `/api/simulador/:id/detener` | Detener simulador |
| GET | `/api/simulador/:id/estado` | Ver estado y logs |
| PUT | `/api/simulador/:id/config` | Cambiar configuración en caliente |
| POST | `/api/simulador/:id/medida-erronea` | Simular timeout de lectura |
| DELETE | `/api/simulador/:id` | Eliminar simulador |

---

## 4. Firmware Virtual

Traducción modular del firmware ESP8266 a JavaScript en `simulador/firmware-virtual/`:

| Módulo JS | Equivalente Firmware | Función |
|-----------|----------------------|---------|
| `config.js` | `config.h` | Configuración de red y sensor |
| `types.js` | `types.h` | Constantes y tipos |
| `sensor.js` | `sensor_core.cpp` | Lectura de temperatura |
| `storage.js` | `storage.cpp` | Almacenamiento offline (LittleFS) |
| `wifi-manager.js` | `wifi_manager.cpp` | Gestión WiFi |
| `mqtt-manager.js` | `mqtt_manager.cpp` | Comunicación MQTT |
| `mode-manager.js` | `mode_manager.cpp` | Gestión de modo y batería |
| `main.js` | `.ino` | Loop principal |

---

## 5. Notas y pendientes

- Defaults PT100: temperatura base −82 °C, umbral 3.3 V.
- La descarga de batería simulada ocurre solo cada 60 ticks.
- Al apagar WiFi se ejecuta `mqttClient.end(true)` para desconectar MQTT realmente.
- **Pendiente:** `confirmed_at` no se llena en el simulador — la config retenida MQTT no llega en la ventana de 500 ms de escucha (el mensaje retenido puede tener delay adicional al suscribirse).
