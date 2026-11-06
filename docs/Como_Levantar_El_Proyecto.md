# Cómo Levantar el Proyecto Localmente (SSPILLOWY)

Aquí documento paso a paso cómo levantar **SSPILLOWY** en un entorno local, ya sea usando **Docker Compose** o ejecutando cada servicio directamente en modo desarrollo.

Al ser una herramienta **Open Source bajo licencia MIT**, cualquier desarrollador o evaluador puede clonar el monorepo y ejecutar la plataforma completa sin depender de software privativo.

- **Repositorio oficial del código:** [https://github.com/SebaNimono/sspillowy](https://github.com/SebaNimono/sspillowy)

---

## 1. Requisitos previos

- **Git**
- **Docker Desktop** (recomendado para levantar PostgreSQL y el entorno con los motores de escaneo preinstalados)
- Si prefieres correrlo sin Docker:
  - **Python 3.11 o 3.12**
  - **Node.js 20+** y **npm**
  - **PostgreSQL 16** (o usar SQLite/Postgres local configurando `DATABASE_URL`)

---

## 2. Clonar el repositorio y configurar variables de entorno

1. Clonar el monorepo de SSPILLOWY:
   ```powershell
   git clone https://github.com/SebaNimono/sspillowy.git
   cd sspillowy
   ```

2. Crear el archivo `.env` a partir de la plantilla `.env.example` incluida en la raíz:
   ```powershell
   Copy-Item .env.example .env
   ```

3. Variables principales que puedes ajustar en `.env`:
   | Variable | Descripción | Valor por defecto local |
   | --- | --- | --- |
   | `DATABASE_URL` | Cadena de conexión a PostgreSQL | `postgresql+psycopg://postgres:postgres@localhost:5432/sspillowy` |
   | `FRONTEND_URL` | URL de la consola web para CORS | `http://localhost:5173` |
   | `API_PUBLIC_URL` | URL pública o local de la API | `http://localhost:8000` |
   | `HUB_AUTH_MODE` | Modo de autenticación (`writes`, `all`, `off`) | `writes` |
   | `GITHUB_TOKEN` | Token opcional para clonar repositorios privados | *(opcional en local)* |
   | `GEMINI_API_KEY` | Clave opcional para activar el asistente IA Pillow | *(opcional, incluye fallback)* |

---

## 3. Opción A — Levantar todo con Docker Compose

Desde la raíz del repositorio `sspillowy`:

```powershell
docker compose up -d --build
```

Esto levanta la base de datos PostgreSQL y la API con los motores de seguridad listos para usar.

---

## 4. Opción B — Levantar en modo desarrollo (API + Consola Web)

### Paso 1: Levantar el Backend (FastAPI)

```powershell
cd apps/api
pip install -r requirements.txt
uvicorn main:app --reload --port 8000
```

Al iniciar, la API verifica y crea automáticamente las tablas necesarias en la base de datos.

### Paso 2: Levantar el Frontend (React + Vite)

En una segunda terminal:

```powershell
cd apps/web
npm install
npm run dev
```

---

## 5. Verificación y acceso

Una vez arriba los servicios:

- **Consola Web Hub:** [http://localhost:5173](http://localhost:5173)
- **Health Check de la API:** [http://localhost:8000/health](http://localhost:8000/health)
- **Documentación interactiva OpenAPI (Swagger):** [http://localhost:8000/docs](http://localhost:8000/docs)
- **Métricas de Observabilidad (Prometheus):** [http://localhost:8000/metrics](http://localhost:8000/metrics)

Si deseas cargar datos de demostración iniciales, puedes usar el botón de inicialización desde la consola o invocar:

```powershell
curl -X POST http://localhost:8000/api/v1/admin/seed
```
