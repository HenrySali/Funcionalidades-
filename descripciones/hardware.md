# Hardware ESP8266

Documentación del módulo físico que capta y transmite las mediciones de temperatura al sistema SmartTemp. Cada sensor es un microcontrolador ESP8266 con un sensor DS18B20 que publica datos por MQTT.

---

## 1. Requisitos de Hardware

- **ESP8266** (NodeMCU o similar).
- **Sensor DS18B20** (temperatura).
- **Resistencia 4.7 kΩ** (pull-up para la línea de datos).
- Cables de conexión.

---

## 2. Conexiones Físicas

```
ESP8266 NodeMCU:
  D5 (GPIO 14) → Datos del sensor DS18B20
  D1 (GPIO 5)  → Control de alimentación del sensor
  A0           → Lectura de voltaje
  3.3V         → VCC del sensor (a través de D1)
  GND          → GND del sensor

Sensor DS18B20:
  VCC (rojo)      → D1 del ESP8266
  GND (negro)     → GND del ESP8266
  DATA (amarillo) → D5 + Resistencia 4.7 kΩ a VCC
```

---

## 3. Librerías de Arduino

Instalar desde Arduino IDE → *Sketch → Include Library → Manage Libraries*:

- **PubSubClient** (Nick O'Leary) — cliente MQTT.
- **ArduinoJson** (Benoit Blanchon) — serialización JSON.
- **OneWire** (Jim Studt) — bus 1-Wire.
- **DallasTemperature** (Miles Burton) — lectura del DS18B20.

---

## 4. Configuración del Firmware

Modificar `config.h` con los datos de red:

```c
const char* WIFI_SSID = "TU_RED_WIFI";
const char* WIFI_PASSWORD = "TU_PASSWORD_WIFI";
const char* MQTT_SERVER = "192.168.1.XXX";  // IP del servidor
```

> Para obtener la IP del servidor en Windows: `ipconfig` → buscar "Dirección IPv4".

### Carga del firmware
1. Abrir Arduino IDE.
2. Abrir `funcional_remoto_modular_.ino`.
3. Seleccionar placa: *Tools → Board → ESP8266 → NodeMCU 1.0*.
4. Seleccionar el puerto COM correcto.
5. Hacer clic en **Upload (→)**.

### Verificación (Serial Monitor, 115200 baud)
```
=== ESP8266 Sensor MQTT - Versión Optimizada ===
🔌 Conectando a WiFi: TU_RED_WIFI
✅ WiFi conectado! IP: 192.168.1.XXX
🔌 Conectando a MQTT: 192.168.1.105:1883
✅ MQTT conectado!
🔍 Buscando sensores DS18B20...
✅ Sensor encontrado: 28FF1A2B3C4D5E60
📡 Enviando datos cada 60 segundos...
```

---

## 5. Formato de Datos Enviados

El ESP8266 publica en el tópico MQTT `sensores/datos`:

```json
{
  "sensor_id": "28FF1A2B3C4D5E60",
  "temperature": 22.5,
  "voltage": 4.2,
  "module_ip": "192.168.1.100",
  "timestamp": 1692876543000
}
```

---

## 6. Modos de Operación

| Modo | Descripción | MQTT | Intervalo |
|------|-------------|------|-----------|
| **Continuo** | Sensor siempre encendido, WiFi y MQTT permanentes | Conectado siempre | Configurable (ej: 5 min) |
| **Deep Sleep** | Sensor duerme entre mediciones para ahorrar batería | Conecta solo al enviar | Configurable (ej: 30–60 min) |

El modo se determina automáticamente por el voltaje de la batería comparado con el umbral configurado (`voltage_threshold`).

---

## 7. Servidor Web Embebido

**Modo Normal (WiFi conectado):**
- `http://[IP_SENSOR]/` — Página principal.
- `http://[IP_SENSOR]/status` — Estado JSON.
- `http://[IP_SENSOR]/config` — Configuración (password: `admin123`).

**Modo AP (WiFi falló):**
- Red: `SmartTemp_Config` / Password: `smarttemp123`.
- `http://192.168.4.1/` — Configuración WiFi.
- `http://192.168.4.1/status` — Estado JSON.

---

## 8. Troubleshooting

| Problema | Solución |
|----------|----------|
| No conecta a WiFi | Verificar SSID/password, usar red 2.4 GHz (no 5 GHz), acercar al router |
| No conecta a MQTT | Verificar IP del servidor, que Mosquitto esté corriendo, firewall puerto 1883 |
| No encuentra sensor | Verificar conexiones físicas y resistencia pull-up 4.7 kΩ |
| Datos no llegan | Verificar logs del Serial Monitor, servidor corriendo, tópico `sensores/datos` |
| Flash corrupta | Arduino IDE → *Tools → Erase Flash → "All Flash Contents"* y re-cargar firmware |
| Loop MQTT | Verificar que no haya dos módulos con el mismo sensor/client ID |

### ⚠️ Advertencias importantes
- Si el sensor guardó un intervalo incorrecto en LittleFS por un bundle mal armado, necesita recibir la config correcta una vez más o flashear de nuevo.
- El firmware incluye `ESP.eraseConfig()` al inicio para prevenir corrupción de flash con credenciales WiFi antiguas.
- Mosquitto solo permite un cliente por ID — apagar módulos duplicados.
