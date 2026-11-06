# Observabilidad del Sistema — SSPILLOWY

Aquí guardo todas las evidencias, capturas y especificaciones del módulo de **Observabilidad** de **SSPILLOWY**.

## Por qué la observabilidad es clave en nuestro proyecto

Algo fundamental que aprendí y apliqué en el desarrollo de **SSPILLOWY** es que **un proyecto de software no termina cuando se escribe la última línea de código ni cuando se despliega a producción: una vez entregado el sistema, la observabilidad es lo que permite operarlo con confianza, detectar cuellos de botella antes de que afecten a los usuarios y auditar su comportamiento real**.

Tratándose de una plataforma **DevSecOps Open Source** de la que dependen pipelines de integración continua (CI/CD), si un motor de escaneo falla o la API aumenta su latencia, los despliegues de los equipos se ven afectados. Por eso diseñamos la observabilidad como un pilar de primera clase dentro de la arquitectura.

---

## Pilares de Observabilidad implementados en SSPILLOWY

### 1. Monitoreo de Salud y Componentes en Vivo (`/health` y vista de Observabilidad)
- Verificación continua del estado de la **API FastAPI**, la conexión e integridad de **PostgreSQL 16** y la disponibilidad de los 4 motores en el contenedor (**Semgrep**, **GitLeaks**, **Trivy** y **OWASP ZAP**).
- Panel visual de **Observabilidad** integrado directamente en el Web Hub (`https://sspillowy.com`), donde muestro el estado de cada componente, los SLOs de disponibilidad, tiempos de respuesta y estado de integraciones.

> **[Imagen de vista de Observabilidad y estado de componentes en SSPILLOWY Web Hub]**

### 2. Métricas en Formato Prometheus (`/metrics`) y Grafana Cloud
- La API expone métricas estructuradas listas para ser recolectadas por **Prometheus** y visualizadas en **Grafana Cloud**:
  - Total de escaneos ejecutados por estado (`COMPLETED`, `FAILED`, `RUNNING`).
  - Latencia y duración de ejecución desglosada por motor de seguridad (`semgrep`, `gitleaks`, `trivy`, `owasp_zap`).
  - Decisiones emitidas por el *Security Gate* (`PASSED`, `BLOCKED`, `BYPASSED`).
  - Volumen de hallazgos activos por severidad (`CRITICAL`, `HIGH`, `MEDIUM`, `LOW`).

> **[Imagen de dashboard de métricas operativas en Grafana Cloud / Prometheus]**

### 3. Telemetría de Ejecución por Etapas y Diagnóstico de Fallos
- Cada escaneo registra en tiempo real su etapa exacta (`queued` → `cloning` → `semgrep` → `gitleaks` → `trivy` → `owasp_zap` → `gate` → `done`/`failed`) y guarda el detalle técnico (`progress_detail`) para poder diagnosticar de inmediato problemas de red, tokens vencidos o errores al clonar un repositorio.

> **[Imagen de contenedor de escaneo desplegado mostrando telemetría y detalle de ejecución]**

### 4. Trazabilidad y Bitácora de Auditoría (*Audit Trail*)
- Registro cronológico inmutable de cada evento relevante en el ciclo de vida de una vulnerabilidad: primera detección, reapariciones, cambios de estado manuales, sugerencias del asistente **Pillow** y autorizaciones de excepción (*bypass*) desde la consola o desde la app móvil **Pulse**.

> **[Imagen de línea de tiempo de Trazabilidad y bitácora de auditoría en SSPILLOWY]**

---

## Qué archivos van en esta carpeta

- Capturas del módulo de **Observabilidad** en la consola web (`https://sspillowy.com`).
- Capturas de los tableros de métricas en **Grafana Cloud** y salida del endpoint `/metrics` y `/health`.
- Documentos o reportes de rendimiento, disponibilidad (SLOs) y seguimiento operativo post-despliegue.
