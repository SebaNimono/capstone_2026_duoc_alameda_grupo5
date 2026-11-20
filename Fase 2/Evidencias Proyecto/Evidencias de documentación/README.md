# Evidencias de Documentación del Proyecto (Fase 2)

En esta carpeta se consolida toda la documentación formal de ingeniería, arquitectura, gestión ágil y diseño del proyecto **SSPILLOWY**, organizada de acuerdo al estándar de entrega de Capstone.

---

## Estructura de Documentos

Siguiendo la pauta formal del proyecto, la documentación se compone de los siguientes artefactos:

| Documento | Estado | Descripción |
| --- | --- | --- |
| **Documento 01 - Product Vision** | *Redactado (abajo)* | Visión del producto, público objetivo, propuesta de valor y diferenciadores. |
| **Documento 02 - Product Backlog** | *En Jira (`SSPIL-1`..`61`)* | Backlog priorizado en Jira Software con las 4 épicas principales del monorepo. |
| **Documento 03 - Sprint Backlog** | *En Jira (Sprint 1)* | Desglose de historias y tareas del Sprint 1 (MVP & Vertical Slice). |
| **Documento 04 - Definition of Done** | *Definido (abajo)* | Criterios de terminado (DoD) de ingeniería, calidad, seguridad y despliegue. |
| **Documento 05 - Documento de Diseño.docx** | **Disponible (.docx)** | Arquitectura de software, stack tecnológico, motores DevSecOps y flujos de integración. |
| **Manual de operación del panel.docx** | *Pendiente exportación* | Guía de uso y operación del Web Hub y monitoreo para operadores y CISOs. |
| **Modelo de datos.docx** | **Disponible (.docx)** | Diccionario y modelo entidad-relación de las 22 tablas en PostgreSQL / Supabase. |
| **Requisitos no funcionales.docx** | *Documentado en Confluence* | Requisitos de rendimiento, latencia (<3s), disponibilidad (SLOs), seguridad y estándares. |
| **Retrospectiva de la Fase 2.docx** | *Pendiente redacción* | Ceremonia de retrospectiva Scrum con lecciones aprendidas, mejoras y compromisos. |

---

## 01. Product Vision (Visión del Producto)

**Para** equipos de desarrollo de software, líderes técnicos (DevSecOps) y responsables de ciberseguridad (CISO),  
**que necesitan** asegurar la calidad y seguridad de su código sin frenar la velocidad de entrega ni depender de licencias empresariales cerradas y costosas,  
**SSPILLOWY** es una plataforma **DevSecOps Open Source (Licencia MIT)**  
**que** unifica análisis SAST (Semgrep), detección de secretos (GitLeaks), análisis de dependencias/SBOM (Trivy) y pruebas dinámicas DAST (OWASP ZAP) con *Security Gates* automatizados en CI/CD, triaje asistido por IA con validación humana (*Human-in-the-Loop*), gobernanza móvil en tiempo real (**Pulse**) y observabilidad post-despliegue.  
**A diferencia de** ejecutar escáneres sueltos por consola o pagar suites privativas cerradas,  
**nuestro producto** ofrece trazabilidad completa de extremo a extremo, deduplicación automática, bloqueo preventivo en *Pull Requests* y código 100% abierto y auditable.

---

## 02. Product Backlog y Épicas en Jira

El *Product Backlog* en Jira está organizado en **4 Épicas principales**:
1. **Consola Web Hub (React):** Dashboards de *Risk Posture*, matriz de hallazgos, *Fix Intelligence*, *Security Gates*, trazabilidad y reportes ejecutivos.
2. **Infraestructura & Core Backend:** API en FastAPI, modelo PostgreSQL de 22 tablas, orquestación en Docker de Semgrep, Trivy, GitLeaks y ZAP, webhooks HMAC, asistente IA Pillow y endpoints de métricas.
3. **App Móvil Pulse (Flutter):** Aplicación móvil conectada a la API en producción, notificaciones push con Firebase, autenticación biométrica (huella/rostro) y aprobación/rechazo de excepciones (*bypass*) del *Security Gate*.
4. **Calidad, Testing & Documentación:** Matriz de pruebas funcionales, análisis continuo con SonarQube, pruebas de regresión, documentación en Confluence e informes de fase.

*(Ver capturas en `../Evidencias de sistema/Aplicación/07_jira_sprint1_epicas.png` y `08_jira_product_backlog.png`)*.

---

## 03. Sprint Backlog (Sprint 1 — MVP & Vertical Slice)

- **SSPIL-1:** `[Infra] Configuración de entorno, contenedores y despliegue base` (Done)
- **SSPIL-2:** `[Web] Maquetación de vistas principales del Web Hub en React` (Done)
- **SSPIL-3:** `[Web] Capa de resiliencia y cliente HTTP con soporte offline/fallback` (Done)
- **SSPIL-4:** `[Mobile] Scaffolding SSPILLOWY Pulse en Flutter` (Done)
- **SSPIL-5:** `[Mobile] Consumo de API REST desde la aplicación móvil` (Done)
- **SSPIL-6:** `[Backend] Implementar persistencia de repositorios, escaneos y hallazgos` (Done)
- **SSPIL-7:** `[Mobile] Fusión de rama e integración de pantallas base` (Done)
- **SSPIL-8:** `[QA] Matriz de pruebas de validación funcional` (Done)
- **SSPIL-9:** `[QA] Preparación de repositorios vulnerables de prueba` (Done)
- **SSPIL-10:** `[Scrum] Gestión del Backlog, épicas y tablero del equipo` (Done)
- **SSPIL-11:** `[Docs] Informe de entrega y documentación de arquitectura Fase 1` (Done)
- **SSPIL-12:** `[Demo] Ensayo general y validación del flujo extremo a extremo` (Done)

---

## 04. Definition of Done (DoD)

1. **Código integrado y compilando sin errores:** Fusionado en la rama `main` del monorepo (`SebaNimono/sspillowy`), pasando el chequeo de tipos (`tsc`) y construcción (`vite build` / Docker build).
2. **Despliegue automático verificado en producción:** Funcionalidad activa y accesible en `https://sspillowy.com` (Vercel) y `https://api.sspillowy.com` (Railway), con esquema PostgreSQL sincronizado.
3. **Security Gate propio aprobado (Dogfooding):** El repositorio pasa por el workflow `.github/workflows/security-gate.yml` sin vulnerabilidades críticas ni secretos expuestos.
4. **Validación QA:** Casos de prueba ejecutados y verificados por QA sin defectos bloqueantes abiertos.
5. **Documentación y Observabilidad al día:** Cambios de API reflejados en Confluence, manteniendo operativos los endpoints `/health` y `/metrics`.
