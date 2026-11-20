# Evidencias de Sistema — Base de Datos (PostgreSQL 16)

En esta carpeta se documenta el esquema relacional, persistencia y diccionario de datos de **SSPILLOWY**, alojado en **PostgreSQL 16** (Railway / Supabase).

## Documento del Modelo de Datos

El documento formal con el diccionario de datos completo de las 22 tablas, 245 columnas, claves foráneas e índices se encuentra centralizado en:
👉 `../Evidencias de documentación/Modelo de datos.docx`

## Características principales del modelo relacional

- **Aislamiento Multi-Workspace (*Multi-Tenancy*):** Separación lógica estricta de repositorios, políticas, miembros, credenciales y hallazgos por espacio de trabajo.
- **Deduplicación inteligente de hallazgos:** Uso de huella criptográfica (*fingerprint*) por regla, ruta y línea para evitar duplicar hallazgos entre ejecuciones sucesivas y mantener su trazabilidad histórica (`first_seen_at`, reaperturas y cierres).
- **Seguridad de credenciales:** Tokens de acceso a repositorios privados cifrados en reposo y contraseñas almacenadas con *bcrypt*.
- **Auditoría inmutable:** Registro de eventos de triaje, decisiones de IA (Pillow), cambios de severidad y aprobaciones de *bypass* en el *Security Gate*.
