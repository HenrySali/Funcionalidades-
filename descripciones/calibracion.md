# Calibración ISO 17025

Sistema de calibración de sensores con generación de certificados basados en la norma **ISO 17025**. Permite calibrar sensores contra un patrón de referencia, calcular estadísticas metrológicas y generar certificados PDF con trazabilidad.

---

## 1. Acceso

*Estado de Sensor* → clic en el ID del sensor → sección colapsable **"Calibración con Patrón"** en la parte inferior de la gráfica.

---

## 2. Formularios

### Patrón de Referencia
- Nombre, marca, modelo, N° de serie.
- Fecha de calibración y fecha de caducidad.

### Responsable
- Nombre, cargo, fecha.
- Firma digital (imagen PNG).

### Tabla de Mediciones
- **3 mediciones**: valor del patrón vs valor del sensor.
- Cálculo en tiempo real al ingresar valores.

---

## 3. Estadísticas Metrológicas Calculadas

Panel de 6 tarjetas estadísticas:

| Estadística | Fórmula |
|-------------|---------|
| Error promedio | Media de 3 errores (patrón − sensor) |
| Corrección promedio | = Error promedio |
| Desviación estándar | Muestral (n − 1 = 2) |
| Incertidumbre tipo A | Desv. estándar / √3 |
| Incertidumbre combinada | √(A² + B²) donde B = resolución del patrón (0.05 °C) |
| Incertidumbre expandida | Combinada × k = 2 (95 % confianza) |

---

## 4. Certificado PDF

Generado con **PDFKit**. Contenido:

- Número único de certificado (`CAL-YYYYMMDD-NNNN`).
- Datos de la organización (nombre, dirección, logotipo).
- Identificación del instrumento (sensor ID, nombre, tipo, ubicación).
- Tabla de 3 mediciones con error y corrección.
- Resumen estadístico (corrección promedio, incertidumbre).
- Trazabilidad metrológica (datos del patrón).
- Declaración de conformidad.
- Firma digital + nombre y cargo del responsable.

---

## 5. API de Calibración

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| POST | `/api/calibracion/mediciones` | Guardar 3 mediciones, calcular corrección e incertidumbre |
| GET | `/api/calibracion/mediciones/:idsensor` | Obtener mediciones existentes |
| GET | `/api/certificados/calibracion/:idsensor` | Generar y descargar certificado PDF |
| GET | `/api/certificados/calibracion/masiva` | Descargar ZIP con todos los certificados |
| POST | `/api/patron-referencia` | Guardar datos del patrón |
| GET | `/api/patron-referencia` | Obtener datos del patrón |
| POST | `/api/calibracion/firma` | Subir imagen de firma digital (PNG) |

---

## 6. Futuro: Calibración Masiva Automatizada

Concepto planificado: cada sensor tendrá un patrón **PT100 + MAX31865** instalado físicamente. Flujo propuesto:

1. El administrador presiona **"Calibración Masiva"**.
2. El sistema enciende los patrones vía MQTT.
3. Período de estabilización (15–30 min).
4. 3 mediciones automáticas cada 5 min por sensor.
5. Cálculo automático de errores, corrección e incertidumbre.
6. Generación de certificados para todos los sensores.

> **Estado:** pendiente de hardware. Las tablas de BD, endpoints y flujo están propuestos pero no creados.

### Mejoras pendientes de Calibración
- Historial de calibraciones (interfaz de visualización).
- Incertidumbre tipo B configurable desde el formulario.
- Error máximo permitido y conformidad automática.
- Click en un certificado del historial que rellene el formulario.
