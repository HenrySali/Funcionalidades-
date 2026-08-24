# Monitoreo

Sección principal del sistema SmartTemp orientada a la visualización de datos de sensores de temperatura en tiempo real e histórico.

---

## 1. Monitoreo en Vivo (`grafico/sensor.html`)

Panel principal de operación diaria. Muestra el estado actual de todos los sensores de temperatura conectados al sistema.

### Qué muestra
- **LED de estado WebSocket**: indicador visual verde/rojo que confirma si la conexión en tiempo real con el servidor está activa.
- **Barra de estadísticas**: tarjetas con totales de sensores, sensores online, sensores offline, alertas activas y última actualización.
- **Tabla de sensores en vivo**: cada fila es un sensor con ID, nombre, equipo asociado, sector, temperatura actual, voltaje, umbrales combinados (°C y V), fecha y hora del último dato, y acciones (editar, ver, supervisión).
- **Alertas visuales**: los sensores fuera de umbral (temperatura o voltaje) se resaltan en rojo/naranja con íconos de advertencia.
- **Monitores de estado**:
  - *LEDs de actualización*: parpadean según los sensores visibles en la tabla.
  - *Monitor del servidor*: verde = OK, celeste/naranja = reconectando, rojo = desconectado.
  - *Monitor de compresión*: estado del servicio de compresión de datos.

### Funcionalidades interactivas
- **Filtros de búsqueda**: por sensor, por equipo y por sector.
- **Ajuste de umbrales**: campos editables de temperatura mínima/máxima y voltaje mínimo/máximo por sensor, más un valor de corrección (calibración).
- **Supervisiones**: cuando un sensor está en alerta, el operador puede registrar una supervisión (observación escrita de qué hizo al respecto). Configurable el timeout de supervisión en minutos.
- **Calibración inline**: tabla de mediciones patrón vs sensor, cálculo automático de error y corrección. Genera certificados PDF individuales o masivos (ZIP). Ver [calibracion.md](calibracion.md).
- **Ping MQTT**: botón para verificar conectividad real del sensor vía protocolo MQTT.
- **Configuración remota**: cambiar intervalo de envío del sensor, umbral de voltaje, modo de operación (continuo/deep sleep) directamente desde la interfaz. Ver [configuracion-remota.md](configuracion-remota.md).
- **Accesos rápidos**: botones para ver solo sensores desconectados o fuera de umbrales.
- **Paginación**: selector de 10, 20, 50 o 100 sensores por página.
- **Modo compacto**: vista reducida para pantallas pequeñas o tablets en campo.
- **Dark mode**: tema oscuro para uso nocturno.

### Datos técnicos
- Conexión WebSocket para datos en tiempo real (sin polling).
- Actualización automática cada vez que un sensor envía un dato.
- Filtrado por sector del usuario autenticado (un operador solo ve los sensores de su sector asignado).
- El valor `0.1111` en temperatura es un valor centinela que indica que el sensor está desconectado. Ver [eventos-desconexiones.md](eventos-desconexiones.md).

---

## 2. Gráfica del Sensor (Historial y Gráficos)

Consulta de datos históricos de temperatura y voltaje por sensor, con gráficos interactivos y exportación. Se accede desde el menú o haciendo clic en el ID del sensor en la tabla de Estado.

> ⚠️ **A confirmar:** la documentación previa atribuía esta página a `visualizacion/index.html`, pero en el sitio en vivo los gráficos corresponden a la página **"Gráfica del Sensor"**, mientras que `visualizacion/index.html` es otra página (ver sección 3). Además, el sitio reporta un bug de que `/visualizacion` no carga datos.

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


---

## 3. Visualización (`visualizacion/index.html`)

Página orientada al seguimiento de conectividad y eventos de los sensores (distinta de la Gráfica del Sensor).

### Qué muestra
- **Historial de Conexión**: eventos de conexión/desconexión con filtros por fecha (`datetime-local`), nombre, ID y sector.
- **Consola de Eventos**: terminal estilo consola que muestra eventos de todos los sensores en tiempo real con *polling* cada 5 segundos.
- **Columna Diagnóstico**: formato visual destacado con borde rojo/verde según el tipo de evento.

> ⚠️ **A confirmar:** el sitio en vivo reporta que esta página actualmente no carga datos (bug conocido sin diagnóstico).
