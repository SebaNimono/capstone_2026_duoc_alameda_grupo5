# Pruebas y Aseguramiento de Calidad (QA) — SSPILLOWY

En esta carpeta se consolidan los informes de pruebas funcionales, pruebas de API, pruebas de rendimiento/estrés, análisis estático de código (**SonarQube Cloud**), la matriz de casos de prueba y las pruebas automatizadas de regresión del backend de **SSPILLOWY**.

---

## 1. Documentos y Evidencias en esta Carpeta

| Archivo | Tipo de Prueba | Herramienta | Resumen de Resultados |
| :--- | :--- | :--- | :--- |
| [`Informe_QA_SonarQube_SSPILLOWY.docx`](./Informe_QA_SonarQube_SSPILLOWY.docx) | Análisis estático de calidad y seguridad | SonarQube Cloud | Quality Gate **Passed** (~48k LOC, Mantenibilidad **A**, Duplicación **2,1%**, Security Hotspots **0**, plan de acción sobre deuda acumulada). |
| [`Informe_Pruebas_Estres_JMeter_SSPILLOWY.docx`](./Informe_Pruebas_Estres_JMeter_SSPILLOWY.docx) | Pruebas de rendimiento y estrés | Apache JMeter 5.6.3 | Línea base (1 usuario, 2.000 muestras): **0,00% error**, 838 ms promedio. Carga (50 usuarios simultáneos, 6.071 muestras): 37,5 req/s, identificación de punto de quiebre (13,38% HTTP 500). |
| [`Informe_QA_API_Postman_SSPILLOWY.docx`](./Informe_QA_API_Postman_SSPILLOWY.docx) | Testing funcional de API REST | Postman + DevTools | 5 endpoints evaluados, 10 casos (**90% aprobados**): creación de escaneos y repositorios (`201`), control de sesión (`401`), manejo `404`, notificaciones y detección de bug CORS `OPTIONS`. |
| [`Informe_QA_Mobile_Android_SSPILLOWY.docx`](./Informe_QA_Mobile_Android_SSPILLOWY.docx) | Pruebas funcionales Mobile (Android) | BrowserStack App Live (Galaxy S22 Ultra) | 10 casos (**80% aprobados**) con 2 cuentas distintas; validación de login, incidentes, aprobación y registro; detección de 2 bugs reproducibles (perfil mock y persistencia de sesión). |
| [`Matriz_Casos_Prueba_Fase2.xlsx`](./Matriz_Casos_Prueba_Fase2.xlsx) | Matriz de casos de prueba funcionales | Excel / QA Manual | **59 casos de prueba** (34 Web + 25 Pulse) cubriendo Login, Repositorios, Escaneos, Hallazgos, Security Gates, Excepciones y Móvil. |
| [`tests/test_security_audit.py`](./tests/test_security_audit.py) | Pruebas automatizadas en código | Python `unittest` / `pytest` | Suite de regresión de seguridad para backend FastAPI (auditoría, bypass de Security Gates, Fix Intelligence y aislamiento de workspaces). |

---

## 2. Ejecución de las Pruebas Automatizadas del Backend

Desde el repositorio del código fuente ([`SebaNimono/sspillowy`](https://github.com/SebaNimono/sspillowy)):

```bash
cd apps/api
python -m unittest tests/test_security_audit.py -v
```
