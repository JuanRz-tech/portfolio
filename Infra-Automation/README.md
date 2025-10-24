# ⚙️ Laboratorio de Automatización de Infraestructura con Ansible

## 🔹 Descripción
Este laboratorio forma parte del plan de especialización en **Infraestructura & Cloud Engineering**, enfocado en la **automatización de entornos híbridos** mediante **Ansible y Terraform**.  
El objetivo principal es desarrollar playbooks y un inventario dinámico para administrar servidores Linux y desplegar infraestructura tanto local como en la nube (AWS / Azure).

---

## 🔹 Entorno

📌 **Herramientas principales:** Ansible 2.16+, Terraform 1.6+  
📌 **Sistemas gestionados:** Ubuntu Server 22.04 / Debian 12  
📌 **Proveedores Cloud:** AWS / Azure  
📌 **Infraestructura local:** Proxmox / VMware  
📌 **Mecanismos de conexión:** SSH, claves públicas y variables de entorno  
📌 **Repositorio:** `Infra-Automation`  

---

## 🔹 Topología General

**Diagrama lógico del entorno de automatización:**

[ Control Node (Ansible Host) ]  
│  
├── Inventario dinámico  
│     ├── servidores_onprem.yml (Proxmox / VMware)  
│     └── servidores_cloud.yml (AWS / Azure)  
│  
├── Playbooks  
│     ├── setup_linux.yml → Configuración base de servidores  
│     ├── deploy_web.yml → Despliegue de aplicación web  
│     └── backup.yml → Automatización de copias de seguridad  
│  
└── Integración con Terraform  
      └── terraform_apply.sh → Crea y configura infraestructura automáticamente

---

## 🔹 Objetivos del Laboratorio
- Desarrollar **playbooks Ansible** reutilizables para tareas comunes (configuración de red, usuarios, paquetes).  
- Gestionar múltiples entornos con **inventarios dinámicos** (on-prem y cloud).  
- Integrar **Terraform** para el aprovisionamiento automatizado de recursos en AWS/Azure.  
- Aplicar buenas prácticas de IaC (**Infrastructure as Code**) y control de versiones con Git.  

---

## 🔹 Configuraciones Clave

### 🔸 Inventario Dinámico
Archivo: `inventario.yml`  
Define los hosts locales y cloud con variables personalizadas.

```yaml
all:
  children:
    onprem:
      hosts:
        srv-linux01:
          ansible_host: 192.168.10.10
    cloud:
      hosts:
        aws-web01:
          ansible_host: 10.0.10.25
        azure-db01:
          ansible_host: 10.1.0.30
```


### 🔸 Playbooks

setup_linux.yml
  * Instala paquetes esenciales (net-tools, htop, git).
  * Crea usuarios y ajusta permisos SSH.
  * Configura timezone, hostname y actualizaciones automáticas.

deploy_web.yml
  * Instala Apache/Nginx.
  * Copia archivos web desde repositorio Git.
  * Habilita firewall UFW con reglas HTTP/HTTPS.

backup.yml
  * Configura backups automáticos diarios a S3 o Blob.
  * Notifica al administrador en caso de error.

### 🔸 Integración con Terraform

* Terraform se encarga del aprovisionamiento de máquinas virtuales en AWS o Azure.
* Ansible toma el control post-provisioning para configurar los sistemas automáticamente.
* Archivo de integración: terraform_apply.sh

## 🔹 Resultados de Pruebas

✅ Ejecución exitosa de playbooks en servidores locales y cloud.

✅ Infraestructura creada automáticamente con Terraform.

✅ Configuración consistente entre entornos híbridos.

✅ Backups funcionales con logs y notificaciones.

✅ Control centralizado de todos los servidores desde un solo nodo.

## 🔹 Capturas




## 🔹 Archivos

* inventario.yml
* setup_linux.yml
* deploy_web.yml
* backup.yml
* terraform_apply.sh

## 🔹 Futuras Mejoras

* Integrar pipeline CI/CD (GitHub Actions / Jenkins).
* Añadir monitoreo con Prometheus + Grafana mediante playbook adicional.
* Usar Vault para cifrado de contraseñas y claves privadas.
* Crear templates de roles reutilizables (roles/ folder).

👨‍💻 Autor: Juan R.  
📘 Repositorio: Infra-Automation  
🗓️ Versión: 1.0  
