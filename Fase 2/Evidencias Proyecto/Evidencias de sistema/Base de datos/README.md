# Evidencias de Sistema — Base de Datos (PostgreSQL 16)

En esta carpeta se documenta el esquema relacional, persistencia, diagrama Entidad-Relación (**DER**) y diccionario de datos de **SSPILLOWY**, operando sobre **PostgreSQL 16** (Supabase / Railway).

---

## 1. Documentos y Archivos en esta Carpeta

| Archivo | Tipo | Descripción |
| :--- | :---: | :--- |
| **`EvidenciaBaseDeDatos_Fase2.docx`** | Documento Word | Informe formal de base de datos para la entrega de Fase 2. |
| **`schema.sql`** | Script DDL SQL | Script completo de creación de tablas, claves foráneas e índices (compatible con Supabase SQL Editor). |
| **`Diagrama_Entidad_Relacion_DER.md`** | Modelo DER (Mermaid) | Diagrama Entidad-Relación completo con cardinalidades, claves foráneas y atributos. |

*(El diccionario detallado de columnas y tipos de datos también se encuentra centralizado en `../Evidencias de documentación/Modelo de datos.docx`).*

---

## 2. Características del Modelo Relacional

- **Multi-Tenancy por Workspace:** Todas las tablas de dominio (`repositories`, `scans`, `policies`, `audit_events`) se encuentran estrictamente particionadas lógicamente por `workspace_id`.
- **Deduplicación Criptográfica:** La tabla `findings` utiliza un hash SHA-256 (`fingerprint_hash`) para evitar la duplicación de vulnerabilidades idénticas entre ejecuciones consecutivas.
- **Auditoría Inmutable:** Registro de cambios de estado, aprobaciones de bypass y triaje en `audit_events`.
