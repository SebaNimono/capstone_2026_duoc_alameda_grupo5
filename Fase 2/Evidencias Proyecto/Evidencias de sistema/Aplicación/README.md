# Evidencias de Sistema — Aplicación y Contenedores Docker

En esta carpeta se presentan las evidencias de despliegue en contenedores **Docker**, especificación de arquitectura y diagramas **UML** de la plataforma **SSPILLOWY**.

---

## 1. Despliegue en Contenedores Docker

La plataforma está diseñada para ejecutarse de forma reproducible y aislada mediante contenedores Docker:

- **`Dockerfile`:** Imagen optimizada en `python:3.12-slim` que instala y ejecuta las herramientas de seguridad nativas en Linux (**Semgrep**, **GitLeaks** y **Trivy**), creando un usuario no privilegiado (`appuser`) y exponiendo health checks activos en `/health`.
- **`docker-compose.yml`:** Orquestación completa de los servicios:
  - `sspillowy_api`: API FastAPI con recarga en vivo y orquestación multi-motor.
  - `sspillowy_postgres`: Motor de base de datos relacional PostgreSQL 16 con volumen persistente.
  - `sspillowy_redis`: Servicio en memoria para colas de trabajo asíncronas y caché.

### Comandos de ejecución local con Docker
```bash
# Iniciar todos los servicios en segundo plano
docker compose up -d

# Verificar el estado de los contenedores
docker compose ps

# Comprobar la salud de la API
curl http://localhost:8000/health
```

---

## 2. Diagramas UML de Arquitectura y Clases

Los diagramas formales de modelado de software se encuentran detallados en:
👉 [Diagrama_UML_Arquitectura.md](./Diagrama_UML_Arquitectura.md)

Incluye:
1. **Diagrama de Componentes UML:** Interacción entre Web Hub (React), Pulse (Flutter), API Gateway (FastAPI), Contenedores de escaneo y PostgreSQL.
2. **Diagrama de Clases UML:** Modelado de entidades del dominio (`Workspace`, `Repository`, `Scan`, `Finding`, `SecurityGatePolicy`, `GateBypassRequest`).
3. **Diagrama de Secuencia UML:** Flujo automatizado de evaluación de Security Gates en CI/CD y aprobación de bypass biométrico desde la app móvil.

---

## 3. Acceso a la Aplicación en Producción

- **Consola Web Hub:** [https://sspillowy.com](https://sspillowy.com)
- **API REST & Health Check:** [https://api.sspillowy.com/health](https://api.sspillowy.com/health)
- **Repositorio Monorepo:** [https://github.com/SebaNimono/sspillowy](https://github.com/SebaNimono/sspillowy)
