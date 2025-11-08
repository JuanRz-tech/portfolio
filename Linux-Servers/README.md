# 🐧 Laboratorio de Administración de Sistemas Linux - Debian

## 🔹 Descripción
Este laboratorio forma parte del entorno de prácticas de **Infraestructura & Cloud Engineering**.  
Simula la implementación y administración de servidores Linux dentro de una infraestructura empresarial.  

Se abordan tareas de instalación, configuración de servicios de red, y prácticas de **hardening** para reforzar la seguridad del sistema.  
El laboratorio combina la gestión manual de servicios con automatización básica mediante scripts y herramientas nativas.

---

## 🔹 Entorno
📌 **Sistema operativo:** Debian 13  
📌 **Entorno de ejecución:** GNS3  
📌 **Red configurada:** 192.168.10.0/24  
📌 **Hostname:** srv-linux01  
📌 **Usuario administrador:** sysadmin  
📌 **Acceso remoto:** SSH habilitado  

---

## 🔹 Servicios Configurados

| Servicio | Función | Detalles |
|-----------|----------|----------|
| **SSH (OpenSSH)** | Acceso remoto seguro | Autenticación por clave, puerto 22, acceso limitado por usuario |
| **Nginx** | Servidor web | Página de prueba y virtual host configurado |
| **DHCP (isc-dhcp-server)** | Asignación automática de IP | Rango 192.168.10.50–100, gateway y DNS definidos |
| **DNS (Bind9)** | Resolución de nombres local | Zona `empresa.local` y resolución inversa |
| **NTP (Chrony)** | Sincronización horaria | Configuración con servidor pool.ntp.org |
| **Firewall (UFW)** | Protección perimetral | Políticas restrictivas con reglas personalizadas |
| **Usuarios y permisos** | Control de acceso | Roles definidos, permisos ajustados y grupos específicos |

---

## 🔹 Configuraciones Clave

1. **Red e interfaz**
   - IP estática configurada en `/etc/systemd/network/`.
   - Gateway: 192.168.10.1, DNS: 8.8.8.8 / 192.168.10.10.  
   - Hostname configurado en `/etc/hostname` y `/etc/hosts`.

2. **SSH**
   - Servicio activo y testeado.
   - Deshabilitado el acceso directo de `root`.  
   - Claves públicas y privadas almacenadas en `/home/sysadmin/.ssh`.

3. **Nginx**
   - Sitio web funcional alojado en `/var/www/html`.  
   - Permisos ajustados con usuario de servicio `www-data`.

4. **DHCP**
   - Configuración en `/etc/dhcp/dhcpd.conf`.  
   - Lease activo y verificado desde cliente en red local.

5. **DNS**
   - Zona directa: `empresa.local`.  
   - Zona inversa: `10.168.192.in-addr.arpa`.  
   - Validación mediante `dig` y `nslookup`.

6. **Firewall UFW**
   - Permitir: SSH (22), HTTP (80), DNS (53).  
   - Bloquear todos los puertos no requeridos.

7. **Usuarios y seguridad**
   - Creación de usuarios `admin`, `webuser`, `guest`.  
   - Permisos personalizados y auditoría de logs en `/var/log/auth.log`.

---

## 🔹 Resultados de Pruebas
- ✅ Acceso remoto SSH con autenticación por clave.  
- ✅ Resolución DNS interna funcional (`server.empresa.local`).  
- ✅ Clientes reciben IP dinámica vía DHCP.  
- ✅ Sitio web accesible desde red interna.  
- ✅ Firewall activo y bloqueando tráfico no autorizado.  
- ✅ Usuarios gestionados con permisos específicos.  
- ✅ Logs del sistema y servicios correctamente registrados.  

---

## 🔹 Capturas
![Configuración de red](screenshots/netplan_config.png)
![Página web local](screenshots/apache_index.png)
![Firewall activo](screenshots/ufw_status.png)

---

## 🔹 Archivos
- [configuraciones.txt](configuraciones.txt) → Detalles de comandos y configuraciones aplicadas.  
- [lab-linux.sh](lab-linux.sh) → Script de automatización para setup inicial.  
- [hardening_checklist.md](hardening_checklist.md) → Guía de endurecimiento y verificación.  

---

## 🔹 Futuras Mejoras
- Integrar monitoreo con **Prometheus + Grafana**.  
- Automatizar despliegue de servicios con **Ansible**.  
- Implementar sistema de backups con **rsnapshot o BorgBackup**.  

---

👨‍💻 **Autor:** Juan R.  
📘 **Repositorio:** [Linux-Servers](Linux-Servers/README.md)  
🗓️ **Versión:** 1.0  
