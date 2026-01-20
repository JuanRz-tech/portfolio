# 🐳 Laboratorio de Docker, Contenedores, n8n & Asistente RAG

## 🔹 Descripción
Este laboratorio forma parte del plan de formación en Infraestructura & Cloud Engineering, enfocado en la automatización, implementación, despliegue y monitoreo de servicios en contenedores.
El objetivo principal es construir un entorno completo de transcripción, workflow y asistente RAG, donde se aprende a:

* Crear imágenes personalizadas para servicios web y bases de datos.
* Configurar redes internas y volúmenes persistentes en Docker.
* Desplegar múltiples servicios con Docker Compose.
* Automatizar workflows de ingestión de audio y video con n8n.
* Implementar un asistente RAG que utiliza modelos locales y embeddings.
* Monitorear el entorno con Portainer, Prometheus y Grafana.

---

## 🔹 Entorno

📌 Plataforma principal: Docker Engine 25+ / Docker Compose V2  
📌 Sistema operativo: Debian 13  
📌 Monitoreo: Portainer, Prometheus, Grafana, cAdvisor, Node Exporter  
📌 Red: Bridge personalizada (my_server)  
📌 Workflow: n8n (ejecutado en el host)  
📌 Repositorio: Docker-Labs       

---

## 🔹 Topología General

**Diagrama lógico del entorno de contenedores**

[ Host Linux / Debian 13 ]  
│  
├── Docker Network (bridge: `my_server`)  
│   │  
│   ├── postgres  
│   │     └── PostgreSQL + pgvector (embeddings RAG)  
│   │  
│   ├── ollama  
│   │     └── Modelos LLM locales + embeddings  
│   │  
│   ├── audio_extractor  
│   │     └── Extracción de audio desde video  
│   │  
│   ├── ffmpeg  
│   │     └── Procesamiento multimedia  
│   │  
│   ├── python-utils  
│   │     └── Scripts de transcripción y parsing  
│   │  
│   ├── backend_api  
│   │     └── API principal (Transcripción + RAG)  
│   │  
│   ├── frontend_app  
│   │     └── Interfaz web de consultas y resultados  
│   │  
│   ├── portainer  
│   │     └── Administración visual de contenedores  
│   │  
│   ├── prometheus  
│   │     └── Recolección de métricas  
│   │  
│   ├── grafana  
│   │     └── Dashboards y visualización  
│   │  
│   ├── cadvisor  
│   │     └── Métricas de contenedores  
│   │  
│   └── node_exporter  
│         └── Métricas del host  
│  
├── n8n (Host)  
│   └── Orquestación de workflows  
│        • Ingesta de video/audio  
│        • Llamadas a backend_api  
│        • Automatización end-to-end  
│  
└── Volúmenes persistentes  
    │  
    ├── db_data  
    │     └── Datos PostgreSQL  
    │  
    ├── ollama_data  
    │     └── Modelos y embeddings  
    │  
    └── portainer_data  
          └── Configuración Portainer  
  

---

## 🔹 Objetivos del Laboratorio  
* Crear imágenes personalizadas para servicios web, microservicios y bases de datos.  
* Desplegar múltiples contenedores con Docker Compose.  
* Configurar redes internas y volúmenes persistentes.  
* Automatizar workflows de ingestión de contenido multimedia con n8n.  
* Implementar un asistente RAG que responde consultas usando embeddings locales.  
* Documentar y monitorear el entorno con Prometheus, Grafana y Portainer.    

---

## 🔹 Configuraciones Clave  
  
### 🔸 Orquestación con Docker Compose  
El laboratorio está organizado en múltiples stacks, separados por dominio funcional:  
  
* Core (my_server): backend, frontend, PostgreSQL + pgvector, Ollama y microservicios.  
* Monitoreo (monitoreo): Prometheus, Grafana, cAdvisor y Node Exporter.  
* Administración: Portainer CE.  
* Esta separación permite modularidad, escalabilidad y mantenimiento independiente.
    
### 🔸 Redes Docker  
* Red bridge personalizada: my_server  
* DNS interno por nombre de servicio  
* Comunicación privada entre contenedores  
* Exposición mínima de puertos  
Ejemplos:  
* backend_api → postgres:5432  
* backend_api → ollama:11434  
* backend_api → audio_extractor:5000  

### 🔸 Volúmenes Persistentes

| Volumen | Uso |
|--------|-----|
| db_data | Datos PostgreSQL + pgvector |
| ollama_data | Modelos y embeddings |
| portainer_data | Configuración Portainer |
| grafana_data | Dashboards Grafana |
| prometheus_data | Métricas históricas |
  

### 🔸 Microservicios

| Servicio | Función |
|---------|---------|
| audio_extractor | Extracción de audio |
| ffmpeg | Conversión multimedia |
| python-utils | Limpieza y parsing de texto |
  
  
Cada microservicio es desacoplado y reutilizable.  

### 🔸 Base de Datos Vectorial  
* PostgreSQL 15 + pgvector  
* Almacenamiento de texto, metadatos y embeddings  
* Búsqueda semántica para RAG  


### 🔸 Motor LLM & Embeddings  
* Servicio: Ollama  
* Modelos locales (sin dependencia cloud)  
* Persistencia mediante volumen dedicado

### 🔸 Frontend  
* SPA (Vite / Vue)  
* Visualización de transcripciones  
* Interfaz de consulta RAG  
* Comunicación exclusiva con backend_api

### 🔸 Automatización con n8n  
* Ejecutado en el host  
* Ingesta automática de audio/video  
* Orquestación completa del pipeline  
* Uso de Webhooks y REST API

### 🔸 Observabilidad  
* Prometheus: métricas de host y contenedores
* Grafana: dashboards personalizados
* cAdvisor: métricas por contenedor
* Node Exporter: métricas del sistema

Puertos:  
* Grafana: 3000  
* Prometheus: 9090  
* cAdvisor: 8081  
* Node Exporter: 9100

### 🔸 Administración  
* Portainer CE  
* Gestión visual de stacks, contenedores, redes y logs
* Acceso: http://localhost:9000/  
---

### 🔸 Docker Compose  
NOTA:  
El siguiente docker-compose es una versión simplificada y representativa  
de la arquitectura general del laboratorio.  

No incluye:  
- configuraciones avanzadas de seguridad  
- optimizaciones de performance  
- definición completa de microservicios  
- pipelines internos de procesamiento  

El objetivo es mostrar la topología y relación entre servicios,  
no exponer la implementación completa.  
  
Archivo: `compose.yaml`  
Define los servicios, redes y volúmenes persistentes.

```yaml
version: "3.9"

services:
  portainer:
    image: portainer/portainer-ce
    ports:
      - "9000:9000"

  postgres:
    image: pgvector/pg16
    ports:
      - "5432:5432"
    environment:
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: ********
      POSTGRES_DB: n8n

  frontend_app:
    build: ./frontend
    ports:
      - "8080:80"

  backend_api:
    build: ./backend
    ports:
      - "8000:8000"
    environment:
      DATABASE_URL: postgresql://admin:********@postgres:5432/n8n

  prometheus:
    image: prom/prometheus
    ports:
      - "9090:9090"

  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"

  cadvisor:
    image: gcr.io/cadvisor/cadvisor
    ports:
      - "8081:8080"

  node_exporter:
    image: prom/node-exporter
    ports:
      - "9100:9100"

  audio_extractor:
    build: ./audio_extractor
    ports:
      - "5000:5000"

networks:
  my_server:
    external: true


```
---

## 🔹 Endpoints del Backend

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| GET    | /api/videos | Listado de videos cargados |
| GET    | /api/videos/{id} | Detalle de un video |
| POST   | /api/transcribe | Transcribir audio/video |
| POST   | /api/embedding | Generar embeddings de texto/audio |
| POST   | /api/rag/query | Consultar asistente RAG |
     

---

## 🔹 Integración con n8n
* Workflows automatizados de ingestión de contenido (Drive, S3, Carpetas locales).
* Llamadas a backend_api para transcripción, procesamiento y generación de embeddings.
* Trigger por carpeta o por webhook.
* Logging y notificaciones al finalizar procesos.

---

## 🔹 Resultados de Pruebas

✅ Contenedores desplegados correctamente con docker compose up -d.  
✅ Acceso web a Portainer: http://localhost:9000.  
✅ Frontend accesible en http://localhost:8080.  
✅ Endpoints de transcripción, embeddings y RAG funcionando.  
✅ Prometheus recolectando métricas de contenedores activos.  
✅ Grafana mostrando dashboards de monitoreo en http://localhost:3000.  
✅ Volúmenes persistentes mantienen datos tras reinicios.  

---

## 🔹 Capturas

### 🧰 Portainer
Captura de la interfaz de administración de contenedores.  

![Portainer - Dashboard](ruta/a/tu/imagen-portainer-dashboard.png)

---

### 🔄 n8n - Workflows  

![n8n - Workflow](ruta/a/tu/imagen-n8n-workflow.png)

---

### 📊 Grafana - Dashboards
Capturas de los dashboards de métricas.

![Grafana - Dashboard 1](ruta/a/tu/imagen-grafana-dashboard1.png)

![Grafana - Dashboard 2](ruta/a/tu/imagen-grafana-dashboard2.png)

---

### 🌐 Web App
Capturas de la aplicación web en funcionamiento.

![Web App - Home](ruta/a/tu/imagen-web-home.png)




---

## 🔹 Archivos
* compose.yaml → Archivo principal de orquestación.  
* Dockerfile  → Imagen personalizada para la aplicación web.  
* prometheus.yml → Configuración de monitoreo.  
* grafana_dashboards → Dashboards personalizados.  
* scripts/start_lab.sh → Script de despliegue automatizado.  

---

## 🔹 Futuras Mejoras
* Implementar autenticación con Traefik + Let's Encrypt.  
* Integrar alertas con Prometheus Alertmanager.  
* Añadir logs centralizados con Loki y Promtail.  
* Desplegar el entorno mediante Terraform + Ansible para CI/CD.  

---

👨‍💻 Autor: Juan R.  
📘 Repositorio: Docker-Labs  
🗓️ Versión: 1.0  



