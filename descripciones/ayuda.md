# Ayuda

Sección de documentación y capacitación integrada en el sistema.

---

## 1. Capacitación (`capacitacion/index.html`)

Página de documentación completa del sistema SmartTemp, pensada como material de capacitación para operadores, coordinadores y administradores.

### Qué muestra
- **Documentación completa** del sistema en formato de manual técnico con secciones navegables.
- **Menú de navegación** con pills/botones para saltar entre secciones.
- **Contenido formateado**: tablas, bloques de código, listas, y diagramas de flujo explicativos.

### Contenido documentado
La Capacitación del sitio en vivo es extensa y cubre tanto el uso como los aspectos técnicos profundos del sistema:

- **Ajustes de Usuarios, Equipos y Sensores**: guías de gestión (ver [gestion.md](gestion.md)).
- **Estado de Sensor y Gráfica del Sensor**: monitoreo en vivo e histórico (ver [monitoreo.md](monitoreo.md)).
- **Visualización**: historial de conexión y consola de eventos.
- **Diagnóstico**: guía de las pestañas técnicas (ver [sistema.md](sistema.md)).
- **Hardware ESP8266**: conexiones, firmware, MQTT y troubleshooting (ver [hardware.md](hardware.md)).
- **Simulador de sensores**: API REST y firmware virtual (ver [simulador.md](simulador.md)).
- **Calibración ISO 17025**: patrón, estadísticas metrológicas y certificados (ver [calibracion.md](calibracion.md)).
- **Sistema de eventos y desconexiones**: detección, falsos positivos y timeouts (ver [eventos-desconexiones.md](eventos-desconexiones.md)).
- **Configuración remota**: flujo MQTT/ACK e historial (ver [configuracion-remota.md](configuracion-remota.md)).
- **Historial de cambios del sistema**: registro de los 62 ajustes exitosos aplicados.
- **Problemas conocidos y pendientes**: bugs, specs parciales y funcionalidades no implementadas.

### Características de la página
- Diseño responsive (funciona en celular, tablet, PC).
- Estilos profesionales con gradientes y tarjetas.
- Tablas con referencia de endpoints (método, ruta, descripción, parámetros).
- Sin dependencia de conexión a API (contenido estático).
- Menú mobile con toggle para pantallas pequeñas.

### Público objetivo
- **Operadores nuevos**: aprenden a usar el dashboard de monitoreo.
- **Coordinadores**: aprenden a interpretar reportes y supervisiones.
- **Administradores**: referencia técnica para configuración avanzada y troubleshooting.
