# Evidencia de Diseño y Diagramas UML — SSPILLOWY

Este documento contiene los diagramas de modelado **UML (Unified Modeling Language)** de la arquitectura del software, componentes y clases del sistema **SSPILLOWY**, correspondientes a la **Fase 2**.

---

## 1. Flujo de Arquitectura General del Sistema

![Flujo de Arquitectura](./flujo_arquitectura.png)

---

## 2. Diagrama de Componentes de la Aplicación (UML Component Diagram)

Muestra la interacción desacoplada entre el cliente web (React), la aplicación móvil (Flutter), la API Gateway en FastAPI, los motores de seguridad en Docker y el almacenamiento persistente.

```mermaid
graph TD
    subgraph Clientes_FrontEnd ["Capa de Presentación"]
        WEB["Consola Web Hub (React + Vite)"]
        MOB["App Móvil Pulse (Flutter)"]
    end

    subgraph API_Backend ["Capa de Lógica y Servicios (FastAPI)"]
        GW["API Gateway / Routers FastAPI"]
        AUTH["Módulo de Autenticación & RBAC (JWT / Sessions)"]
        GATE["Motor de Evaluación de Security Gates"]
        TRIAGE["Servicio de Triaje & Asistente Pillow (LLM)"]
        ORCH["Orquestador de Escaneos Multi-Motor"]
    end

    subgraph Motores_Seguridad ["Contenedores de Análisis de Seguridad"]
        SEMGREP["Semgrep (SAST)"]
        GITLEAKS["GitLeaks (Secret Detection)"]
        TRIVY["Trivy (SCA / SBOM)"]
        ZAP["OWASP ZAP (DAST)"]
    end

    subgraph Persistencia ["Capa de Datos"]
        DB[("PostgreSQL 16 (22 tablas / Multi-Tenant)")]
        REDIS[("Redis 7 (Colas y Caché)")]
    end

    WEB -->|HTTPS / REST| GW
    MOB -->|HTTPS / WebSockets / Push| GW
    GW --> AUTH
    GW --> GATE
    GW --> TRIAGE
    GW --> ORCH
    ORCH --> SEMGREP
    ORCH --> GITLEAKS
    ORCH --> TRIVY
    ORCH --> ZAP
    AUTH --> DB
    GATE --> DB
    ORCH --> DB
    ORCH --> REDIS
```

---

## 3. Diagrama de Clases UML del Dominio Central (UML Class Diagram)

Modela las entidades y relaciones principales del núcleo de seguridad y gobernanza en el backend (`apps/api/models.py`):

```mermaid
classDiagram
    class Workspace {
        +String id
        +String name
        +String created_by
        +DateTime created_at
        +list repositories
        +list members
        +list policies
    }

    class WorkspaceMember {
        +String id
        +String workspace_id
        +String email
        +String role
        +DateTime created_at
    }

    class Repository {
        +String id
        +String workspace_id
        +String name
        +String url
        +String branch
        +Boolean is_active
        +Int health_score
        +DateTime last_scan_at
    }

    class Scan {
        +String id
        +String repository_id
        +String status
        +String branch
        +Int total_findings
        +Int critical_count
        +Int high_count
        +String gate_status
        +DateTime created_at
    }

    class Finding {
        +String id
        +String scan_id
        +String fingerprint_hash
        +String title
        +String engine
        +String severity
        +String file_path
        +Int line_number
        +String status
        +String ai_verdict
        +DateTime first_seen_at
    }

    class SecurityGatePolicy {
        +String id
        +String workspace_id
        +Boolean enabled
        +Int max_critical_allowed
        +Int max_high_allowed
        +Boolean block_on_secrets
        +Boolean require_ciso_approval
    }

    class GateBypassRequest {
        +String id
        +String repository_id
        +String workspace_id
        +String reason
        +String status
        +String requested_by
        +String decided_by
        +DateTime expires_at
    }

    Workspace "1" *-- "many" WorkspaceMember : tiene
    Workspace "1" *-- "many" Repository : administra
    Workspace "1" *-- "1" SecurityGatePolicy : define
    Repository "1" *-- "many" Scan : ejecuta
    Scan "1" *-- "many" Finding : detecta
    Repository "1" o-- "many" GateBypassRequest : solicita
```

---

## 4. Diagrama de Secuencia UML (Security Gate en CI/CD con Bypass Móvil)

```mermaid
sequenceDiagram
    autonumber
    actor Dev as Desarrollador
    participant CI as GitHub Actions
    participant API as SSPILLOWY API
    participant Engine as Motores (Docker)
    participant CISO as Tech Lead / CISO (App Móvil Pulse)

    Dev->>CI: git push origin feature/nueva-api
    CI->>API: Webhook HMAC (Trigger Scan)
    API->>Engine: Clona y ejecuta Semgrep, Trivy, GitLeaks
    Engine-->>API: Hallazgos normalizados (Findings)
    API->>API: Evalúa Security Gate (umbral: 0 críticas)
    alt Gate BLOQUEADO (Hallazgos Críticos detectados)
        API-->>CI: Decision = BLOCKED (Pipeline detenido)
        CI-->>Dev: Notificación de bloqueo en PR
        Dev->>API: Solicita Bypass temporal (justificación técnica)
        API->>CISO: Notificación Push (Firebase FCM)
        CISO->>CISO: Validación Biométrica (Huella/Rostro en Pulse)
        CISO->>API: Aprobación de Bypass (duración: 24h)
        API-->>CI: Decision = BYPASSED (Permite desplegar)
    else Gate APROBADO
        API-->>CI: Decision = PASSED (Despliegue a producción)
    end
```
