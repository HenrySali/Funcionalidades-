# Monitoreo

Sección principal del sistema SmartTemp orientada a la visualización de datos de sensores de temperatura en tiempo real e histórico.

---

## 1. Monitoreo en Vivo (`grafico/sensor.html`)

Panel principal de operación diaria. Muestra el estado actual de todos los sensores de temperatura conectados al sistema.

### Qué muestra
- **LED de estado WebSocket**: indicador visual verde/rojo que confirma si la conexión en tiempo real con el servidor está activa.
- **Barra de estadísticas**: tarjetas con totales de sensores, sensores online, sensores offline, alertas activas.
- **Tabla de sensores en vivo**: cada fila es un sensor con temperatura actual, voltaje, estado de conexión, último dato recibido, equipo y sector asignado.
- **Alertas visuales**: los sensores fuera de umbral (temperatura o voltaje) se resaltan en rojo/naranja con íconos de advertencia.

### Funcionalidades interactivas
- **Supervisiones**: cuando un sensor está en alerta, el operador puede registrar una supervisión (observación escrita de qué hizo al respecto). Configurable el timeout de supervisión en minutos.
- **Calibración inline**: tabla de mediciones patrón vs sensor (6 puntos), cálculo automático de error y corrección. Genera certificados PDF individuales o masivos (ZIP).
- **Ping MQTT**: botón para verificar conectividad real del sensor vía protocolo MQTT.
- **Configuración remota**: cambiar intervalo de envío del sensor, umbral de voltaje, modo de operación (continuo/deep sleep) directamente desde la interfaz.
- **Modo compacto**: vista reducida para pantallas pequeñas o tablets en campo.
- **Dark mode**: tema oscuro para uso nocturno.

### Datos técnicos
- Conexión WebSocket para datos en tiempo real (sin polling).
- Actualización automática cada vez que un sensor envía un dato.
- Filtrado por sector del usuario autenticado (un operador solo ve los sensores de su sector asignado).

---

## 2. Historial y Gráficos (`visualizacion/index.html`)

Consulta de datos históricos de temperatura y voltaje por sensor, con gráficos interactivos y exportación.

### Qué muestra
- **Selector de sensor**: dropdown con todos los sensores registrados.
- **Gráfico interactivo Chart.js**: líneas de temperatura y voltaje en el tiempo, con zoom (scroll/pinch), pan, y anotaciones de umbrales.
- **Tarjetas de estadísticas**: temperatura mínima, máxima, promedio, voltaje promedio, cantidad de registros en el rango seleccionado.
- **Tabla de datos**: todas las mediciones del rango con búsqueda, ordenamiento por columna y paginación.

### Funcionalidades interactivas
- **Rangos rápidos**: botones 1h, 6h, 24h, 7d, 30d para selección rápida de período.
- **Rango personalizado**: selector de fecha/hora inicio y fin.
- **Umbrales visuales**: líneas horizontales configurables en el gráfico que marcan los límites permitidos.
- **Exportación PDF**: genera un reporte con gráfico + tabla usando jsPDF.
- **Exportación Excel**: descarga un .xlsx con todos los datos del rango usando SheetJS.
- **Auto-refresh**: toggle que recarga datos cada N segundos (configurable).
- **Dark mode**: tema oscuro completo.

### Datos técnicos
- Usa Chart.js 4 con plugins de zoom (hammerjs), annotation, y adapter date-fns.
- Datos cargados desde endpoint REST con parámetros de rango.
- Soporta miles de registros con paginación en tabla.
