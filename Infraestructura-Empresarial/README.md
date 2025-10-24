# 🧩 Laboratorio de Infraestructura Empresarial - VMware / Proxmox + AWS

## 🔹 Descripción
Este laboratorio integra conceptos avanzados de **infraestructura de red, virtualización y cloud computing**, simulando el entorno de una empresa moderna con operaciones híbridas (local + nube).  
El objetivo es diseñar, implementar y documentar una **topología empresarial completa**, con **VLANs, routing dinámico, ACLs, servicios internos y conectividad hacia AWS**.

---

## 🔹 Entorno

📌 **Plataforma local:** Proxmox VE / VMware Workstation  
📌 **Entorno cloud:** AWS Free Tier  
📌 **Sistemas operativos:** Ubuntu Server 22.04, Debian 12, Windows Server 2022  
📌 **Rango de red principal:** 192.168.10.0/24  
📌 **Conectividad cloud:** VPN site-to-site con VPC AWS  
📌 **Roles principales:** Servidor DNS/DHCP, Router local, Servidor web y Firewall perimetral (pfSense)

---

## 🔹 Topología

📌 **Segmentos definidos:**

| VLAN | Descripción | Subred | Servicios |
|------|--------------|--------|------------|
| 10 | Administración | 192.168.10.0/24 | DNS, DHCP, NTP |
| 20 | Servidores | 192.168.20.0/24 | Web, Base de Datos |
| 30 | Finanzas | 192.168.30.0/24 | Acceso restringido |
| 40 | Soporte | 192.168.40.0/24 | Escritorio remoto |
| 50 | Backup | 192.168.50.0/24 | Rsync, NFS |
| Cloud | AWS VPC | 10.0.10.0/24 | EC2 + S3 backups |

---

## 🔹 Tecnologías Implementadas
- VLANs (segmentación por departamentos).  
- Enrutamiento dinámico con **OSPF** y rutas estáticas de respaldo.  
- **VPN site-to-site** entre Proxmox y AWS VPC.  
- Firewall **pfSense** con ACLs extendidas.  
- DNS y DHCP distribuidos entre servidores internos.  
- Servidor web interno con Nginx y acceso controlado.  
- Balanceo básico de carga en servidores de aplicación.  
- Backup automatizado hacia S3 en AWS.  

---

## 🔹 Configuraciones Clave

1. **Red y VLANs**
   - VLANs creadas en switches virtuales y asignadas a VMs.
   - Interconexión entre VLANs autorizadas mediante OSPF.

2. **pfSense Firewall**
   - Interfaces: LAN, DMZ, VPN, WAN.
   - Reglas ACL para permitir solo tráfico necesario.
   - Bloqueo de acceso interdepartamental no autorizado.

3. **Servidor DHCP/DNS**
   - Configuración en servidor Linux (Bind9 + isc-dhcp-server).
   - DNS interno con resolución de dominios `empresa.local`.

4. **VPN con AWS**
   - Conexión IPsec con VPC 10.0.10.0/24.
   - Permite tráfico hacia instancias EC2 y almacenamiento S3.

5. **Alta Disponibilidad**
   - Dos routers virtuales con failover configurado (VRRP).  
   - Monitorización de interfaces con scripts de salud (`ping check`).  

6. **Servicios Cloud**
   - EC2 con web demo (`nginx + flask`).  
   - S3 configurado para backups automáticos diarios.  

---

## 🔹 Resultados de Pruebas
- ✅ Conectividad estable entre VLANs autorizadas.  
- ✅ Resolución DNS y asignación DHCP correctas.  
- ✅ Comunicación segura entre Proxmox y AWS mediante VPN.  
- ✅ Firewall filtra correctamente tráfico interno y externo.  
- ✅ Backups automáticos a S3 verificados.  
- ✅ Alta disponibilidad en gateway local mediante VRRP.  

---

## 🔹 Capturas
![Topología general](screenshots/topologia_infra.png)
![pfSense dashboard](screenshots/pfsense_dashboard.png)
![VPN AWS establecida](screenshots/vpn_status.png)

---

## 🔹 Archivos
- [topologia_infraestructural.vsdx](topologia_infraestructural.vsdx) → Diagrama de red.  
- [configuraciones.txt](configuraciones.txt) → Detalle de comandos y configuraciones.  
- [vpn_config.conf](vpn_config.conf) → Configuración de túnel IPsec.  
- [backup_s3.sh](backup_s3.sh) → Script de respaldo automatizado.  

---

## 🔹 Futuras Mejoras
- Migrar servicios internos a **contenedores Docker**.  
- Añadir **monitorización con Grafana + Prometheus**.  
- Implementar **Ansible/Terraform** para automatización de despliegues.  
- Extender topología a múltiples regiones de AWS.  

---

👨‍💻 **Autor:** Juan R.  
📘 **Repositorio:** [Infraestructura Empresarial](Infraestructura-Empresarial/README.md)  
🗓️ **Versión:** 1.0  
