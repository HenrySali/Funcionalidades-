# Funcionalidades — SmartTemp

Documentación funcional y técnica del sistema **SmartTemp**, una plataforma de monitoreo de temperatura de sensores en tiempo real e histórico para equipos de refrigeración (freezers, heladeras, cámaras frías, incubadoras), orientada al cumplimiento normativo (ISO 17025, BPL) con trazabilidad, auditoría y calibración.

> Sitio en vivo: `https://appsmarttemp.diseñosyefectos.com` — el contenido de esta documentación se basa en la sección de **Capacitación** integrada en la app y en las descripciones funcionales del sistema.

## Arquitectura (resumen)

- **Frontend:** HTML/CSS/JS (Chart.js, jsPDF, SheetJS, Flatpickr, SweetAlert2).
- **Backend:** Node.js + Nginx (proxy reverso), base de datos con pool de conexiones.
- **Mensajería:** WebSocket (tiempo real en la UI) + MQTT (sensores/hardware).
- **Hardware:** módulos ESP8266 + sensor DS18B20 (o PT100).
- **Notificaciones:** Email (Nodemailer/Gmail) y WhatsApp (Meta Cloud API o Twilio).

## 🌐 Sitio de documentación

Hay una página web completa y autocontenida que explica todo el sistema con lenguaje técnico pero entendible: **[`docs/index.html`](docs/index.html)**.

Para publicarla con **GitHub Pages**: *Settings → Pages → Source: Deploy from a branch → Branch: `main` / carpeta `/docs`*. Quedará accesible en `https://henrysali.github.io/Funcionalidades-/`.

## Índice de la documentación

### Uso del sistema
- [Monitoreo](descripciones/monitoreo.md) — monitoreo en vivo e historial/gráficos.
- [Gestión](descripciones/gestion.md) — sensores, equipos, usuarios y red de sensores.
- [Reportes](descripciones/reportes.md) — auditoría de eventos, supervisiones y sesiones.
- [Sistema](descripciones/sistema.md) — diagnóstico y notificaciones.
- [Ayuda](descripciones/ayuda.md) — capacitación integrada.

### Documentación técnica
- [Hardware ESP8266](descripciones/hardware.md) — conexiones, firmware, MQTT, modos de operación, troubleshooting.
- [Calibración ISO 17025](descripciones/calibracion.md) — patrón de referencia, estadísticas metrológicas, certificados PDF, API.
- [Simulador de Sensores](descripciones/simulador.md) — simulador server-side, API REST y firmware virtual.
- [Sistema de Eventos y Desconexiones](descripciones/eventos-desconexiones.md) — detección de desconexión, falsos positivos, valor centinela `0.1111`.
- [Configuración Remota](descripciones/configuracion-remota.md) — flujo MQTT/ACK, tipos de config, reenvío automático.

## Notas y discrepancias por revisar

Durante el cruce entre esta documentación y el sitio en vivo se detectaron diferencias a confirmar:

- **Roles de usuario:** `descripciones/gestion.md` describe 4 roles (Administrador / Coordinador / Operador / Visualizador), mientras que la sección de Capacitación del sitio menciona 3 (Administrador / Usuario Regular / Técnico). Conviene unificar cuál es el vigente.
- **Pestañas de Diagnóstico:** `descripciones/sistema.md` describe 9 pestañas; el sitio en vivo describe 7. Revisar el número real de pestañas.
