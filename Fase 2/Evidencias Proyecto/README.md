# Evidencias del Proyecto — Fase 2 (SSPILLOWY)

Aquí se organiza toda la evidencia técnica, metodológica (Scrum, Jira, Confluence), de base de datos, de sistema en producción y de observabilidad de **SSPILLOWY**.

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
Evidencias Proyecto/
├── Evidencias de documentación/   ← Documento 01 a 05, Manual, Modelo de datos, RNF y Retrospectiva
├── Evidencias de sistema/
│   ├── Aplicación/                ← Capturas del Web Hub, repositorios, Security Gates, Jira y Confluence
│   └── Base de datos/             ← Esquema relacional y persistencia PostgreSQL 16
└── Presentación Proyecto.pptx     ← Presentación formal de la Fase 2
```

| Subcarpeta / Archivo | Qué contiene |
| --- | --- |
| [Evidencias de documentación](./Evidencias%20de%20documentaci%C3%B3n/) | Documentos formales: `Documento 05 - Documento de Diseño.docx`, `Modelo de datos.docx`, y especificaciones de Product Vision, Product Backlog, Sprint Backlog y DoD. |
| [Evidencias de sistema/Aplicación](./Evidencias%20de%20sistema/Aplicaci%C3%B3n/) | Capturas del Hub web operativo, Security Gates, tableros Jira y centro Confluence. |
| [Evidencias de sistema/Base de datos](./Evidencias%20de%20sistema/Base%20de%20datos/) | Diccionario y especificación del modelo PostgreSQL de 22 tablas. |
| `Presentación Proyecto.pptx` | Diapositivas de la presentación de la Fase 2. |
