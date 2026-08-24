# Reportes

Sección de trazabilidad y auditoría. Registra todo lo que ocurre en el sistema para cumplimiento normativo y seguimiento de incidencias.

---

## 1. Auditoría de Eventos (`administrador/eventos-auditoria.html`)

Registro cronológico de todas las novedades y acciones realizadas sobre los sensores.

### Qué muestra
- **Tabla de eventos**: cada fila es un evento con fecha/hora, sensor involucrado, tipo de novedad, acciones tomadas y usuario que lo registró.
- **Filtros avanzados**: rango de fechas, tipo de evento, sensor específico, usuario.

### Funcionalidades
- **Filtrado combinado**: aplicar múltiples filtros simultáneos para buscar eventos específicos.
- **Búsqueda de texto**: encontrar eventos por palabra clave.
- **Eliminar eventos filtrados**: borrar lotes de eventos obsoletos (con confirmación).
- **Exportar**: descargar los eventos filtrados.

### Tipos de eventos registrados
- Desconexión de sensor
- Reconexión de sensor
- Temperatura fuera de umbral
- Voltaje fuera de umbral
- Cambio de configuración remota
- Supervisión registrada
- Correo de alerta enviado

### Uso típico
Un auditor o responsable de calidad consulta esta pantalla para verificar que todas las desviaciones de temperatura fueron detectadas y atendidas, cumpliendo requisitos de normativas como ISO 17025 o BPL.

---

## 2. Supervisiones (`administrador/supervisiones-auditoria.html`)

Registro de todas las supervisiones realizadas por operadores cuando un sensor presenta una alerta.

### Qué muestra
- **Tarjetas de resumen**: cantidad de supervisiones por tipo (desconectado, fuera de umbral temp, fuera de umbral volt, ambos, total).
- **Filtros**: rango de fechas, tipo de problema, sensor.
- **Tabla de supervisiones**: cada fila muestra sensor, tipo de problema, usuario supervisor, fecha, observaciones escritas, número de supervisión, si está resuelto.

### Funcionalidades
- **Filtrado por tipo de problema**: ver solo desconexiones, solo temperaturas fuera de rango, etc.
- **Filtrado por fecha**: rango personalizado.
- **Eliminar supervisiones filtradas**: limpieza de registros antiguos.
- **Estado resuelto/pendiente**: cada supervisión puede marcarse como resuelta con fecha de resolución.

### Flujo operativo
1. Sensor entra en alerta (desconexión o umbral excedido).
2. Operador ve la alerta en Monitoreo en Vivo.
3. Operador registra una supervisión con observaciones (ej: "se verificó el equipo, puerta estaba abierta").
4. La supervisión queda registrada aquí como evidencia.

---

## 3. Registro de Sesiones (`HistSesiones/index.html`)

Historial de todos los inicios de sesión de usuarios en el sistema.

### Qué muestra
- **Tarjetas de estadísticas**: total de usuarios, inicios hoy, inicios esta semana, último inicio de sesión.
- **Tabla de sesiones**: nombre del usuario, correo, fecha/hora de ingreso, dirección IP, dispositivo usado (navegador/SO).

### Funcionalidades
- **Filtro por usuario**: dropdown con búsqueda para encontrar un usuario específico.
- **Filtro por rol**: ver solo inicios de administradores, operadores, etc.
- **Búsqueda avanzada**: buscar por nombre, correo, dispositivo o IP.
- **Selector de fechas** (Flatpickr): filtrar por rango de fechas.

### Uso típico
Para auditorías de seguridad: verificar quién accedió al sistema, desde dónde, y cuándo. Permite detectar accesos no autorizados o inusuales.
