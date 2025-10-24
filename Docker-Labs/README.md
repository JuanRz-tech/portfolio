# 🐳 Laboratorio de Docker & Contenedores

## 🔹 Descripción
Este laboratorio forma parte del plan de formación en **Infraestructura & Cloud Engineering**, enfocado en la **implementación, despliegue y monitoreo de servicios en contenedores**.  
El objetivo principal es aprender a crear imágenes personalizadas, configurar redes internas entre contenedores, definir volúmenes persistentes y desplegar múltiples servicios usando **Docker Compose**, con monitoreo integrado mediante **Portainer, Grafana y Prometheus**.

---

## 🔹 Entorno

📌 **Plataforma principal:** Docker Engine 25+ / Docker Compose V2  
📌 **Sistema operativo:** Ubuntu Server 22.04 LTS / Debian 12  
📌 **Monitoreo:** Portainer, Prometheus y Grafana  
📌 **Red:** Bridge personalizada con DNS interno  
📌 **Repositorio:** `Docker-Labs`  

---

## 🔹 Topología General

**Diagrama lógico del entorno de contenedores:**

[ Host Linux ]  
│  
├── Red Docker (bridge: `infra_net`)  
│     ├── nginx_container → Proxy / Frontend  
│     ├── app_container → Aplicación principal  
│     ├── db_container → Base de datos (MySQL / PostgreSQL)  
│     ├── prometheus_container → Recolección de métricas  
│     ├── grafana_container → Visualización de métricas  
│     └── portainer_container → Administración visual  
│  
└── Volúmenes persistentes  
      ├── db_data  
      ├── grafana_data  
      └── prometheus_data  

---

## 🔹 Objetivos del Laboratorio
- Crear **imágenes personalizadas** para servicios web y bases de datos.  
- Desplegar múltiples contenedores con **Docker Compose**.  
- Configurar **redes internas y volúmenes persistentes**.  
- Implementar **monitoreo básico con Portainer y Prometheus + Grafana**.  
- Documentar y automatizar la ejecución del entorno completo.  

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
      - app

  app:
    build: ./app
    container_name: web_app
    networks:
      - infra_net

  db:
    image: mysql:8.0
    container_name: mysql_db
    environment:
      MYSQL_ROOT_PASSWORD: admin123
      MYSQL_DATABASE: labdb
    volumes:
      - db_data:/var/lib/mysql
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

networks:
  infra_net:
    driver: bridge

```

---

## 🔹 Resultados de Pruebas

✅ Contenedores desplegados correctamente con docker compose up -d.

✅ Acceso web a Portainer (http://localhost:9000).

✅ Aplicación web accesible en http://localhost.

✅ Prometheus recolectando métricas de contenedores activos.

✅ Panel de Grafana con dashboard de monitoreo funcional.

✅ Volúmenes persistentes mantienen los datos tras reinicios.


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



