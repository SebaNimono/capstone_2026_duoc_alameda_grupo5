# Evidencias del Proyecto — Fase 2 (SSPILLOWY)

Aquí organizo toda la evidencia técnica, metodológica (Ágil en Jira y Confluence), de base de datos, de sistema en producción y de **Observabilidad** de **SSPILLOWY**.

## Enlaces al producto Open Source

| Recurso | URL |
| --- | --- |
| **Repositorio Open Source (MIT)** | [https://github.com/SebaNimono/sspillowy](https://github.com/SebaNimono/sspillowy) |
| **Consola Web Hub** | [https://sspillowy.com](https://sspillowy.com) |
| **API REST** | [https://api.sspillowy.com](https://api.sspillowy.com) |
| **Health Check (Observabilidad)** | [https://api.sspillowy.com/health](https://api.sspillowy.com/health) |
| **Workflow CI Security Gate** | [https://github.com/SebaNimono/sspillowy/blob/main/.github/workflows/security-gate.yml](https://github.com/SebaNimono/sspillowy/blob/main/.github/workflows/security-gate.yml) |

## Estructura de esta carpeta

```text
Evidencias_Proyecto/
├── Documentacion/          ← Documento técnico y diccionario de datos general
├── Version_Agil/           ← Product Vision, Product Backlog (Jira), Sprints, DoD y Confluence
├── Observabilidad/         ← Monitoreo post-despliegue, métricas Prometheus/Grafana y SLOs
└── Sistema/
    ├── Aplicacion/         ← Capturas del Hub en producción, tablero Jira y espacio Confluence
    └── Base_de_datos/      ← Diccionario de datos y esquema PostgreSQL (22 tablas)
```

| Subcarpeta | Qué contiene |
| --- | --- |
| [Documentacion](./Documentacion/) | `SSPILLOWY_Documento_Tecnico.docx` y `06_Diccionario_Datos_SSPILLOWY.docx`. |
| [Version_Agil](./Version_Agil/) | Artefactos de nuestra metodología Ágil (Scrum) gestionados en **Jira** y **Confluence**: Visión del Producto, Product Backlog, Sprint Backlog, Definition of Done y estructura documental. |
| [Observabilidad](./Observabilidad/) | Evidencias de observabilidad continua una vez desplegado el proyecto: métricas operativas, estado de componentes, latencias de escaneo y alertas. |
| [Sistema/Aplicacion](./Sistema/Aplicacion/) | Capturas reales de la aplicación web, repositorios monitoreados, Security Gates, Jira y Confluence. |
| [Sistema/Base_de_datos](./Sistema/Base_de_datos/) | Especificación y diccionario de datos del modelo relacional en PostgreSQL 16. |
