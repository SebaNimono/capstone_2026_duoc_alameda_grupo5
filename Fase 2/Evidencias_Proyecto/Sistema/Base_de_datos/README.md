# Sistema — Base de Datos (PostgreSQL 16)

Aquí está la documentación y el diccionario de datos de nuestra base de datos relacional en **PostgreSQL 16**, desplegada en Railway.

## Archivos en esta carpeta

| Archivo | Descripción |
| --- | --- |
| `06_Diccionario_Datos_SSPILLOWY.docx` | Diccionario de datos completo con las 22 tablas, 245 columnas, claves primarias/foráneas y 36 índices de rendimiento. |

## Características principales de nuestro modelo de datos

- **Aislamiento Multi-Workspace (*Multi-Tenancy*):** Separación lógica estricta de repositorios, políticas, miembros, credenciales y hallazgos por espacio de trabajo.
- **Deduplicación inteligente de hallazgos:** Uso de huella criptográfica (*fingerprint*) por regla, ruta y línea para evitar duplicar hallazgos entre ejecuciones sucesivas y mantener su trazabilidad histórica (`first_seen_at`, reaperturas y cierres).
- **Seguridad de credenciales:** Tokens de acceso a repositorios privados cifrados en reposo y contraseñas almacenadas con *bcrypt*.
- **Auditoría inmutable:** Registro de eventos de triaje, decisiones de IA (Pillow), cambios de severidad y aprobaciones de *bypass* en el *Security Gate*.

> **[Imagen de diagrama entidad-relación (MER) de PostgreSQL en Confluence]**
