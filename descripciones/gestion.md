# Gestión

Sección administrativa para configurar la infraestructura del sistema: sensores, equipos, usuarios y conectividad de red.

---

## 1. Configurar Sensores (`sensores/index.html`)

ABM (Alta/Baja/Modificación) de sensores y su asociación a equipos.

### Qué muestra
- **Panel de sensores**: lista de todos los sensores registrados con nombre, tipo, ubicación, ID.
- **Panel de equipos**: lista de equipos disponibles para asociar.
- **Panel de asociaciones**: relación sensor ↔ equipo actual.

### Funcionalidades
- **Registrar sensor**: el sistema auto-detecta sensores nuevos que envían datos. Desde esta pantalla se les asigna nombre, descripción, tipo y ubicación para formalizarlos.
- **Filtrar por ID**: búsqueda rápida para encontrar un sensor específico entre los detectados.
- **Crear equipo**: registrar un nuevo equipo (freezer, heladera, cámara fría, incubadora, etc.).
- **Asociar sensor a equipo**: vincular un sensor registrado a un equipo específico.
- **Editar/Eliminar**: modificar datos de un sensor o equipo existente.

### Relación de datos
```
Sensor → sensor_equipos → Equipo → sector_equipos → Sector → user_sectors → Usuario
```
Al asociar un sensor a un equipo que pertenece a un sector, los usuarios de ese sector pueden ver sus datos automáticamente.

---

## 2. Equipos (`equipos/index.html`)

Gestión avanzada de equipos, direcciones MAC y sectores organizacionales.

### Qué muestra
- **Formulario de equipo**: nombre, descripción, tipo, ubicación, MAC address opcional.
- **Asociar MAC**: vincular una dirección MAC de red a un equipo existente (para identificación por hardware).
- **Gestión de sectores**: crear sectores con nombre y color identificativo.
- **Actualizar color de sector**: modificar el color asignado a un sector existente.

### Funcionalidades
- **CRUD de equipos completo** con todos los campos.
- **Asociación MAC → Equipo**: permite identificar físicamente qué módulo ESP8266 corresponde a qué equipo.
- **CRUD de sectores** con color picker.
- **Asociar equipo a sector**: vincular un equipo a un área organizacional.

---

## 3. Usuarios (`usuarios/index.html`)

Gestión de usuarios del sistema, roles y asignación a sectores.

### Qué muestra
- **Formulario de creación**: nombre, correo, contraseña, rol (administrador/coordinador/operador/visualizador).
- **Lista de usuarios**: búsqueda por nombre.
- **Formulario de sector**: crear nuevos sectores.
- **Asociación usuario-sector**: vincular un usuario a un sector para controlar qué sensores puede ver.
- **Consulta por sector**: ver todos los usuarios asignados a un sector específico.

### Roles disponibles
| Rol | Acceso |
|-----|--------|
| Administrador | Todo el sistema |
| Coordinador | Monitoreo + Reportes |
| Operador | Monitoreo en vivo + Historial |
| Visualizador | Solo monitoreo en vivo |

### Funcionalidades
- Crear/editar/eliminar usuarios.
- Asignar usuarios a uno o más sectores.
- El sector determina qué sensores y equipos puede ver cada usuario.

---

## 4. Red de Sensores (`red-sensores/index.html`)

Diagnóstico de conectividad de red de todos los sensores del sistema.

### Qué muestra
- **Tarjetas resumen**: sensores online, offline, ping promedio, total.
- **Tabla de red por sensor**: ID, dirección IP, módulo (MAC), estado del sensor, modo de operación (continuo/deep sleep), ping MQTT (ms), última conexión, acciones.

### Funcionalidades
- **Actualizar red**: botón que refresca el estado de todos los sensores.
- **Ping individual MQTT**: enviar señal de verificación a un sensor específico y medir latencia.
- **Estado visual**: cada fila muestra si el sensor está online (verde) u offline (rojo) con el tiempo transcurrido desde su última conexión.
- **Modo de operación**: muestra si el sensor está en modo continuo (envía datos constantemente) o deep sleep (envía datos cada X minutos y duerme entre envíos para ahorrar energía).
