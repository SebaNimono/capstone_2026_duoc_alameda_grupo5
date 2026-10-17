# SSPILLOWY — Capstone 2026 DuocUC Alameda (Grupo 5)

**Software Security Platform for Intelligent, Low-latency Oversight & Warning Yield**

Repositorio de **entrega académica** (evidencias por fase), según la estructura de ejemplo de [DuocUC Alameda](https://github.com/FelipeKraussDOUC/capstone_2026_duoc_alameda).

El producto de software vive en: [SebaNimono/sspillowy](https://github.com/SebaNimono/sspillowy)

---

## Nombre del proyecto

**SSPILLOWY** — Plataforma de gobernanza DevSecOps con hub web de auditoría, escaneo multi-motor, Security Gates y capa móvil (Pulse) en desarrollo paralelo.

## Descripción

SSPILLOWY orquesta seguridad de software de punta a punta:

- Escaneo **SAST / secretos / SCA / DAST** (Semgrep, GitLeaks, Trivy, OWASP ZAP)
- **Security Gate** con umbrales reales y check en CI (GitHub Actions)
- Triaje humano (HITL), bypass CISO en el hub y trazabilidad (audit)
- Consola web (React) + API (FastAPI) en producción
- App móvil **Pulse** (Flutter) para alertas y bypass — trabajo del compañero en paralelo

## Tecnologías

| Capa | Stack |
|------|--------|
| API / Hub | Python 3.12, FastAPI, PostgreSQL |
| Web | React, Vite, TypeScript |
| Móvil (Pulse) | Flutter |
| Escaneo | Semgrep, GitLeaks, Trivy, OWASP ZAP (baseline) |
| Cloud | Railway (API), Vercel (web), GitHub Actions (CI gate) |
| Auth | OAuth (GitHub/Google/…) + sesión hub |

## Cómo ejecutar (resumen)

Código y detalle operativo: ver README del monorepo  
→ https://github.com/SebaNimono/sspillowy

URLs de referencia:

- Consola: https://sspillowy.com  
- API: https://api.sspillowy.com  

## Integrantes del equipo (Grupo 5)

| Integrante | Rol |
|------------|-----|
| Sebastián Tralma | Hub web / API DevSecOps, Security Gates, CI, documentación |
| Benjamín Hernández | *(completar rol)* |
| Damián Valencia | Pulse (móvil) / integración Flutter |
| *(completar)* | *(rol)* |

## Metodología de trabajo

- **Scrum / Kanban** ligero por sprints de fase académica  
- **DevSecOps**: gates en pipeline, evidencias en GitHub, seguimiento en **Jira** (enlace en evidencias grupales)  
- Separación Hub (web/API) ↔ Pulse (móvil), con contratos de API compartidos  

## Arquitectura (visión)

```
[ Dev / GitHub ] --push/PR--> [ Webhook SSPILLOWY ] --> [ Motores: Semgrep|GitLeaks|Trivy|ZAP ]
                                      |
                                      v
                              [ Hallazgos + Gate ]
                                      |
              +----------------------+----------------------+
              |                                             |
       [ Consola Web Hub ]                          [ CI Security Gate ]
       React / Vercel                               GitHub Actions
              |
       [ API FastAPI / Railway / Postgres ]
              |
       [ Pulse (móvil) — en construcción ]
```

## Estructura de este repositorio

```
Fase 1/
  Evidencias_Grupales/
  Evidencias_Individuales/

Fase 2/
  Evidencias_Individuales/   ← diario 2.1 por integrante
  Evidencias_Grupales/       ← guía 2.4, PPT de avance/presentación
  Evidencias_Proyecto/       ← documentación, capturas app, BD + links al producto
```

Detalle Fase 2 → [Fase 2/README.md](./Fase%202/README.md)

## Licencia

Entrega académica Grupo 5 — DuocUC Ingeniería Informática, Sede Alameda, 2026.
