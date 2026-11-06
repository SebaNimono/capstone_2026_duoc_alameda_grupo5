# Documentación de Observabilidad — SSPILLOWY

En esta carpeta documento la arquitectura y operación de **Observabilidad** de **SSPILLOWY**, enfocada en el monitoreo continuo del sistema una vez puesto en producción.

## Qué va en esta carpeta

- Guías de configuración de recolección de métricas con **Prometheus** (`/metrics`) y visualización en **Grafana Cloud**.
- Definición de nuestros **SLOs (Service Level Objectives)** de disponibilidad de la API, tasa de éxito de escaneos y tiempos de respuesta del *Security Gate*.
- Capturas y reportes del monitoreo post-despliegue en producción (`https://sspillowy.com` y `https://api.sspillowy.com/health`).

> **[Imagen de panel de Observabilidad y SLOs en la consola de SSPILLOWY]**

> **[Imagen de métricas de escaneos y latencia en Grafana Cloud]**

*(Ver también las evidencias aplicadas a la Fase 2 en [../Fase 2/Evidencias_Proyecto/Observabilidad/](../Fase%202/Evidencias_Proyecto/Observabilidad/))*
