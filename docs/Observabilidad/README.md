# Documentación de Observabilidad y Monitoreo — SSPILLOWY

En esta sección se documenta la arquitectura integral de **Observabilidad**, monitoreo de infraestructura, salud de persistencia y métricas en tiempo real de la plataforma **SSPILLOWY**, implementada sobre **Grafana Cloud** y **Prometheus** para la base de datos relacional PostgreSQL 16 en **Supabase**.

---

## 1. Arquitectura de Observabilidad y Telemetría

La plataforma implementa los tres pilares de observabilidad moderna para entornos DevSecOps en producción:

1. **Métricas de Infraestructura y Base de Datos (Prometheus & Grafana Cloud):** Telemetría continua con sondeo cada 30 segundos (`refresh=30s`) que audita cómputo (CPU), memoria RAM, almacenamiento persistente (EBS), tamaño de base de datos, tráfico de red, agrupamiento de conexiones (*connection pooling*) y tiempos de respuesta en microsegundos.
2. **Health Checks de la API (FastAPI):** Endpoints activos `/health` y `/ready` con reporte de latencia y estado de dependencias.
3. **Trazabilidad y Auditoría de Seguridad:** Registro inmutable de eventos de evaluación de *Security Gates*, escaneos SAST/DAST y solicitudes de bypass.

---

## 2. Paneles de Monitoreo en Producción (Grafana Cloud)

- **Panel Grafana:** `supabase-project`
- **Datasource Prometheus:** `grafanacloud-curiouscobbler2036-prom`
- **Job de Ingesta:** `integrations/supabase/3736424-metrics-endpoint-partner-qqzzerhiumcmcwubofxn`
- **Identificador de Proyecto Supabase:** `qqzzerhiumcmcwubofxn`
- **Enlace al Panel:** [https://curiouscobbler2036.grafana.net/d/d402d94e-da48-48e4-ac52-53026b96a000/supabase-project](https://curiouscobbler2036.grafana.net/d/d402d94e-da48-48e4-ac52-53026b96a000/supabase-project)

---

### Panel 1: Rendimiento de Cómputo, Memoria y Almacenamiento

Monitorea la carga global del nodo de base de datos, el uso de memoria RAM activa y la utilización del sistema de archivos raíz.

![Panel 1: CPU, Memoria y Disco](./01_grafana_cpu_mem_disk.png)

| Indicador | Valor Observado | Diagnóstico Técnico |
| :--- | :---: | :--- |
| **CPU Busy** | **33.9%** | Rango óptimo durante la ejecución y filtrado de consultas complejas de escaneo. |
| **System Load (1m / 5m)** | **1% / 0%** | Sin contención de hilos en el kernel del sistema operativo. |
| **RAM Used (Activa)** | **61% (625 MiB)** | Consumo normal de procesos de PostgreSQL sobre el total de 1024 MiB asignados. |
| **RAM + Cache (Buffers)** | **91.1%** | PostgreSQL aprovecha eficientemente la memoria para *Shared Buffers*, acelerando lecturas frecuentes sin ir a disco. |
| **Root FS Used** | **77.2%** | Almacenamiento raíz estable sobre 10 GiB de volumen asignado. |
| **Uptime del Clúster** | **11.9 semanas** | Alta disponibilidad continua (> 99.9% de disponibilidad sin reinicios). |

---

### Panel 2: Tráfico de Red, Tamaño de Datos y Conexiones Activas

Monitorea el consumo de ancho de banda, espacio utilizado en volumen de datos (`/data`), tamaño de las 22 tablas y estado de los servidores de conexión (*Supavisor* y *PgBouncer*).

![Panel 2: Red, Tamaño de BD y Conexiones](./02_grafana_network_connections_postgres.png)

| Indicador | Valor Observado | Diagnóstico Técnico |
| :--- | :---: | :--- |
| **Network Traffic (ens5)** | **~5 kb/s In / ~15 kb/s Out** | Tráfico de red ligero y eficiente en interfaz física `ens5`. |
| **Disk Mounted at /data** | **15%** | El volumen dedicado a datos posee un 85% de espacio libre, garantizando escalabilidad a largo plazo. |
| **Database Size** | **~30 MB** | Tamaño compacto de las 22 tablas relacionales gracias a la deduplicación criptográfica (hash SHA-256 en hallazgos). |
| **Postgres Connections** | **~18 activas** | Conexiones concurrentes gestionadas dentro del límite de capacidad. |
| **Supavisor Connections** | **~12 en pool** | Agrupador de conexiones (*Connection Pooler*) activo para evitar sobrecarga de conexiones efímeras. |
| **Postgres & PgBouncer Status** | **Running (Verde)** | Ambos servicios operan al 100% en estado saludable. |

---

### Panel 3: Latencia de Consultas, Transacciones y Control de Bloqueos

Monitorea los tiempos de respuesta de consultas en microsegundos, tasa de confirmación de transacciones (*commits* vs *rollbacks*) y verificación de bloqueos (*deadlocks*).

![Panel 3: Estadísticas de Consultas y Transacciones](./03_grafana_queries_transactions_status.png)

| Indicador | Valor Observado | Diagnóstico Técnico |
| :--- | :---: | :--- |
| **Query Latency (Total time)** | **50 µs - 180 µs** | Tiempos de respuesta extremadamente bajos (< 0.2 milisegundos), demostrando la efectividad de los 36 índices B-Tree. |
| **Tx Committed** | **5 a 10 tx/min** | Flujo continuo de transacciones confirmadas exitosamente por el backend. |
| **Tx Rolled Back** | **0** | Cero transacciones abortadas o con errores de integridad. |
| **Deadlocks Detected** | **0** | Ausencia absoluta de interbloqueos en transacciones concurrentes. |
| **DB Mode** | **ReadWrite** | Base de datos en modo lectura/escritura normal. |
| **In Recovery** | **No** | Nodo principal operando sin procesos de recuperación de emergencia pendientes. |

---

### Panel 4: Rendimiento de Escritura en Segundo Plano (BgWriter) y Pila de Memoria

Monitorea la actividad del proceso escritor en segundo plano (*Background Writer* / *Checkpoints*) y la distribución de memoria a nivel de espacio de usuario y kernel.

![Panel 4: BgWriter y Pila de Memoria](./04_grafana_bgwriter_memory_stack.png)

| Indicador | Valor Observado | Diagnóstico Técnico |
| :--- | :---: | :--- |
| **Checkpoints bgwriter** | **1 ms - 2 ms** | Escrituras periódicas de puntos de control ejecutadas en milisegundos sin pausas notables. |
| **Written by bgwriter** | **En equilibrio** | Escritura controlada de bloques modificados hacia almacenamiento persistente. |
| **Memory Stack (Apps)** | **Constante (~320 MB)** | Memoria de aplicaciones en espacio de usuario estable sin fugas de memoria (*memory leaks*). |
| **Swap Usage** | **0 MB** | No existe intercambio a disco (*swapping*), garantizando máxima velocidad de ejecución en RAM. |

---

## 3. Conclusiones de Observabilidad

La incorporación de telemetría continua en **Grafana Cloud** y **Prometheus** permite certificar con datos reales y medibles que:
1. La persistencia en PostgreSQL opera con **alta disponibilidad comprobada (11.9 semanas continuas)**.
2. Los índices diseñados mantienen la latencia en rangos de **microsegundos (50-180 µs)**.
3. El motor de deduplicación criptográfica mantiene el almacenamiento de datos bajo control (**30 MB** en 22 tablas).
4. El sistema previene bloqueos (**0 deadlocks**) y opera en modo de **alta concurrencia con PgBouncer y Supavisor**.
