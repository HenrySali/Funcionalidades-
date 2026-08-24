# Sistema

Sección técnica para administradores. Diagnóstico del servidor, configuración avanzada y gestión de notificaciones.

---

## 1. Estado del Sistema (`diagnostico/index.html`)

Panel de diagnóstico completo con 9 pestañas que cubren todos los aspectos técnicos del sistema.

### Pestañas

#### 1.1 Estado
Panel resumen del sistema:
- **Uptime del servidor**: tiempo que lleva corriendo sin reiniciarse.
- **MQTT Broker**: estado de conexión del broker de mensajería, cantidad de clientes conectados.
- **Base de datos**: conexiones activas, límite del pool, consultas en cola.
- **Últimos correos enviados**: tabla con fecha, sensor, destinatario de cada alerta enviada por email.

#### 1.2 Servidor y Red
- Estado del proceso Node.js (memoria, CPU).
- Estado de Nginx (proxy reverso).
- Conectividad con servicios externos.

#### 1.3 Sensores
- Lista de sensores con estado detallado (último dato, tiempo sin datos, modo de operación).
- Detección de sensores desconectados por timeout.
- Tabla de interfaces con IP, MAC, modo, intervalo configurado, umbral de voltaje.

#### 1.4 Configuración
- Tabla editable de configuración por sensor: intervalo, umbral de voltaje, modo.
- Estado de confirmación (si el sensor ya aplicó el cambio o está pendiente).
- Historial de cambios de configuración.

#### 1.5 Herramientas
- Ping masivo MQTT (verificar todos los sensores a la vez).
- Reset remoto de sensores.
- Validación de integridad de datos.

#### 1.6 Logs
- Consola de logs en tiempo real del servidor.
- Filtro por nivel (info, warning, error).
- Últimas 500 entradas.

#### 1.7 Simulador
- Crear sensores virtuales que envían datos simulados.
- Útil para testing y demos sin hardware real.
- Guardar/cargar configuraciones de simulación.

#### 1.8 Historial Config
- Registro de todos los cambios de configuración remota realizados a sensores.
- Quién lo cambió, cuándo, valor anterior, valor nuevo, si fue confirmado.

#### 1.9 Contingencia
- Pestaña oculta para situaciones de emergencia.
- Permite bloquear/desbloquear el sistema.
- Programar bloqueos futuros.

---

## 2. Notificaciones WhatsApp (`administrador/notificaciones.html`)

Panel de configuración y monitoreo del sistema de alertas por email y WhatsApp.

### Qué muestra
- **Estado del servicio**: tarjetas con estados de conexión (WhatsApp conectado/desconectado, Email activo, sensores alertando).
- **Configuración global**: toggles para activar/desactivar canales de notificación (email, WhatsApp).
- **Preferencias por usuario**: tabla donde se configura qué tipo de alertas recibe cada usuario y por qué canal.
- **Historial de notificaciones**: tabla paginada con todas las notificaciones enviadas (fecha, sensor, tipo, canal, destinatario, estado).

### Funcionalidades
- **Activar/desactivar canales**: switches globales para email y WhatsApp.
- **Seleccionar eventos para WhatsApp**: checkboxes para elegir qué eventos disparan mensaje (desconexión, temperatura, voltaje, reconexión).
- **Preferencias individuales**: por cada usuario, configurar si recibe alertas y por qué medio.
- **Ver historial**: consultar qué notificaciones se enviaron, a quién, y si fueron entregadas.
- **Test de envío**: probar que el sistema envía correctamente antes de confiar en él.

### Proveedores soportados
- **Email**: Nodemailer con Gmail (App Password).
- **WhatsApp**: Meta Cloud API (oficial) o Twilio.
