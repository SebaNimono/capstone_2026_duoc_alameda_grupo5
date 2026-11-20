# Diagrama Entidad-Relación (DER) — Base de Datos PostgreSQL 16

Este documento contiene el modelo entidad-relación (**DER**) y la especificación de diseño relacional de la base de datos de **SSPILLOWY**, alojada en **PostgreSQL 16** (Supabase / Railway).

---

## 1. Diagrama Entidad-Relación (DER / MER)

```mermaid
erDiagram
    workspaces ||--o{ workspace_members : "tiene"
    workspaces ||--o{ workspace_invitations : "emite"
    workspaces ||--o{ repositories : "administra"
    workspaces ||--o{ audit_events : "registra"
    workspaces ||--o| security_gate_policies : "configura"
    workspaces ||--o{ compliance_frameworks : "activa"
    workspaces ||--o{ runbooks : "programa"

    users ||--o{ auth_sessions : "inicia"
    users ||--o{ workspace_members : "participa"

    repositories ||--o{ scans : "ejecuta"
    repositories ||--o{ gate_bypass_requests : "solicita"

    scans ||--o{ findings : "detecta"
    findings ||--o{ finding_history : "evoluciona"

    compliance_frameworks ||--o{ compliance_controls : "contiene"
    compliance_controls ||--o{ compliance_mappings : "asocia"

    runbooks ||--o{ runbook_executions : "dispara"
    runbook_executions ||--o{ runbook_execution_results : "genera"

    workspaces {
        string id PK
        string name
        string created_by
        timestamp created_at
    }

    users {
        string id PK
        string email UK
        string name
        string password_hash
        boolean is_active
        timestamp created_at
    }

    workspace_members {
        string id PK
        string workspace_id FK
        string email
        string role
        timestamp created_at
    }

    repositories {
        string id PK
        string workspace_id FK
        string name
        string url UK
        string branch
        boolean is_active
        int health_score
        int open_criticals
        int open_highs
        timestamp last_scan_at
    }

    scans {
        string id PK
        string repository_id FK
        string status
        string branch
        int total_findings
        int critical_count
        int high_count
        string gate_status
        timestamp created_at
        timestamp completed_at
    }

    findings {
        string id PK
        string scan_id FK
        string fingerprint_hash
        string title
        string engine
        string severity
        string file_path
        int line_number
        string status
        string ai_verdict
        timestamp first_seen_at
    }

    security_gate_policies {
        string id PK
        string workspace_id FK
        boolean enabled
        int max_critical_allowed
        int max_high_allowed
        boolean block_on_secrets
        boolean require_ciso_approval
    }

    gate_bypass_requests {
        string id PK
        string repository_id FK
        string workspace_id FK
        string reason
        string status
        string requested_by
        string decided_by
        timestamp expires_at
    }
```

---

## 2. Esquema DDL en SQL

El script completo de inicialización y definición de tablas e índices se encuentra en:
👉 [`schema.sql`](./schema.sql)

Incluye:
- Definición de tablas con tipos nativos (`TEXT`, `INTEGER`, `BOOLEAN`, `TIMESTAMPTZ`).
- Restricciones de unicidad (`UNIQUE`) y eliminación en cascada (`ON DELETE CASCADE`).
- Índices B-Tree especializados para consultas de alto rendimiento (`idx_findings_scan_id`, `idx_scans_repo`, `idx_repos_workspace`).
