# 🐳 Laboratorio de Docker, Contenedores & n8n

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
📌 Monitoreo: Portainer, Grafana + Prometheus  
📌 Red: Bridge personalizada (my_server)  
📌 Workflow: n8n (host local)  
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
* Documentar y monitorear la ejecución del entorno completo.  

---

## 🔹 Configuraciones Clave

### 🔸 Estructura del Proyecto
                           
Docker-Labs/  
├── compose.yaml  
├── app/  
│   ├── Dockerfile  
│   ├── requirements.txt  
│   └── app.py├── prometheus/  
│   ├── prometheus.yml  
│   └── data/  
├── grafana/  
│   └── provisioning/  
│       ├── dashboards/  
│       └── datasources/  
├── portainer/  
│   └── config/  
└── README.md  

---

### 🔸 Docker Compose
Archivo: `compose.yaml`  
Define los servicios, redes y volúmenes persistentes.

```yaml
version: "3.9"

services:
  nginx:
    image: nginx:latest
    container_name: nginx_proxy
    ports:
      - "80:80"
    networks:
      - infra_net
    depends_on:
      - backend_api

  backend_api:
    build: ./app
    container_name: backend_api
    networks:
      - infra_net
    environment:
      - DATABASE_URL=postgresql://postgres:admin123@db:5432/labdb

  frontend_app:
    build: ./frontend
    container_name: frontend_app
    networks:
      - infra_net
    ports:
      - "5173:5173"
    depends_on:
      - backend_api

  db:
    image: postgres:15
    container_name: postgres_db
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: admin123
      POSTGRES_DB: labdb
    volumes:
      - db_data:/var/lib/postgresql/data
    networks:
      - infra_net

  ollama:
    image: ollama/ollama:latest
    container_name: ollama_srv
    volumes:
      - ollama_data:/ollama
    networks:
      - infra_net

  audio_extractor:
    build: ./microservices/audio_extractor
    container_name: audio_extractor
    networks:
      - infra_net

  ffmpeg:
    build: ./microservices/ffmpeg
    container_name: ffmpeg_srv
    networks:
      - infra_net

  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus_srv
    volumes:
      - ./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus_data:/prometheus
    networks:
      - infra_net

  grafana:
    image: grafana/grafana:latest
    container_name: grafana_srv
    ports:
      - "3000:3000"
    volumes:
      - grafana_data:/var/lib/grafana
    networks:
      - infra_net

  portainer:
    image: portainer/portainer-ce:latest
    container_name: portainer_srv
    ports:
      - "9000:9000"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    networks:
      - infra_net

volumes:
  db_data:
  grafana_data:
  prometheus_data:
  ollama_data:
  portainer_data:

networks:
  infra_net:
    driver: bridge


```
---

🔹 Endpoints del Backend
Método -   Endpoint -	    Descripción  
GET	 /api/videos	   Listado de videos cargados  
GET	 /api/videos/{id}	   Detalle de un video  
POST	 /api/transcribe	   Transcribir audio/video  
POST	 /api/embedding	   Generar embeddings de texto/audio  
POST	 /api/rag/query	   Consultar asistente RAG     

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
✅ Frontend accesible en http://localhost.  
✅ Endpoints de transcripción, embeddings y RAG funcionando.  
✅ Prometheus recolectando métricas de contenedores activos.  
✅ Grafana mostrando dashboards de monitoreo funcional.  
✅ Volúmenes persistentes mantienen datos tras reinicios.  

---

## 🔹 Capturas



---

##🔹 Archivos
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



