# 🧩 Laboratorio de Infraestructura Híbrida – GNS3 + AWS

## 🔹 Descripción
Laboratorio de infraestructura híbrida orientado a la integración funcional de servicios empresariales básicos, seguridad perimetral y despliegue complementario en la nube.   

El objetivo principal es unificar distintos mini-proyectos en un entorno cohesivo donde:

* Un Firewall FortiGate controla y segmenta la red.
* Un Servidor Linux Debian 13 provee múltiples servicios internos.
* Clientes Windows y Linux consumen dichos servicios.
* Se integra una implementación básica en AWS como extensión cloud del laboratorio.
* El enfoque está en la integración de infraestructura, no en configuraciones avanzadas de switching o routing.

---

## 🔹 Entorno

📌 **Plataforma local:** GNS3  
📌 **Firewall: FortiGate (configuración básica funcional)  
📌 **Servidor: Debian 13    
📌 **Clientes: Linux Mint / Windows    
📌 **Entorno Cloud: AWS Free Tier    
📌 **Roles principales:** Servidor DNS/DHCP, Servidor web y Firewall perimetral (Fortigate)

---

## 🔹 Topología

📌 **Segmentos definidos:**

| Segmento        | Descripción                          | Subred              | Servicios |
|-----------------|--------------------------------------|---------------------|-----------|
| LAN-Sistema     | Red de administración                | 192.168.10.0/24     | SSH, Gestión |
| LAN-Departamentos| Clientes Windows / Linux             | 192.168.20.0/24     | Navegación, DNS |
| DMZ             | Red de servidores                    | 192.168.15.0/24     | DNS, DHCP, NTP, Web |
| WAN             | Salida a Internet / Cloud            | IP pública / DHCP ISP | NAT, Reglas de navegación |
| Cloud (AWS)     | Entorno complementario en la nube    | Red VPC AWS         | EC2 (servicio web / pruebas) |

---

## 🔹 Tecnologías Implementadas

- Plataforma de simulación de red en GNS3.  
- Firewall FortiGate con segmentación por interfaces.  
- Reglas de firewall para control de navegación y acceso entre redes.  
- NAT para salida a Internet.  
- DHCP habilitado en interfaz específica del firewall.  
- Implementación de red DMZ para aislamiento de servidor.  
- Servidor Debian 13 con servicios integrados:
  * DNS (Bind9)
  * DHCP
  * NTP
  * Servidor Web (Nginx)
  * Administración remota por SSH
- Integración con AWS (EC2 en Free Tier) como extensión cloud del laboratorio.  

---

## 🔹 Configuraciones Clave

1. **Segmentación de Red**
   - Separación lógica mediante interfaces independientes en FortiGate.
   - Creación de red DMZ para alojar el servidor.
   - Aislamiento entre segmentos mediante políticas firewall.

2. **Firewall FortiGate**
   - Configuración manual de IP en cada interfaz.
   - Activación de DHCP solo en interfaz definida.
   - Reglas de navegación LAN → WAN.
   - Políticas específicas para acceso hacia DMZ.
   - Configuración de NAT para salida a Internet.

3. **Servidor Debian 13**
   - Configuración de Bind9 como DNS interno.
   - Servicio DHCP para red específica.
   - NTP para sincronización horaria.
   - Servidor web Nginx accesible según reglas del firewall.
   - Acceso administrativo mediante SSH desde red autorizada.

4. **Integración con AWS**
   - Creación de instancia EC2 en AWS Free Tier.
   - Pruebas de conectividad desde red local.
   - Uso de entorno cloud como extensión funcional del laboratorio.

---

## 🔹 Resultados de Pruebas

- ✅ Segmentación funcional entre LAN-Sistema, LAN-Usuarios y DMZ.  
- ✅ Aislamiento correcto del servidor en red DMZ.  
- ✅ Asignación DHCP operativa en la interfaz configurada del FortiGate.  
- ✅ Resolución DNS interna funcionando correctamente desde clientes.  
- ✅ Acceso web al servidor Debian validado según políticas de firewall.  
- ✅ Navegación a Internet operativa mediante NAT en FortiGate.  
- ✅ Conectividad exitosa con instancia EC2 en AWS desde la red local.  
- ✅ Administración remota por SSH funcional desde red autorizada.  

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
