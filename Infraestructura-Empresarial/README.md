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
📌 **Firewall: FortiGate (configuración básica)  
📌 **Servidor: Debian 13    
📌 **Clientes: Linux Mint / Windows    
📌 **Entorno Cloud: AWS Free Tier    
📌 **Roles principales:** Servidor DNS/DHCP, Servidor web y Firewall perimetral (Fortigate)

---

## 🔹 Topología

📌 **Segmentos definidos:**

| Segmento        | Descripción                          | Subred              | Servicios |
|-----------------|--------------------------------------|---------------------|-----------|
| LAN-Sistemas    | Red de administración                | 192.168.10.0/24     | SSH, Gestión |
| LAN-Departamentos| Clientes Windows / Linux            | 192.168.20.0/24     | Navegación, DNS |
| DMZ             | Red de servidores                    | 192.168.15.0/24     | DNS, DHCP, NTP, Web |
| WAN             | Salida a Internet / Cloud            | IP pública / DHCP ISP | NAT, Reglas de navegación |
| Cloud (AWS)     | Entorno complementario en la nube    | Red VPC AWS         | EC2 (servicio web / pruebas) |

---

## 🔹 Tecnologías Implementadas

- Plataforma de simulación de red en GNS3.  
- Firewall FortiGate con segmentación por interfaces.  
- Reglas de firewall para control de navegación y acceso entre redes.  
- NAT para salida a Internet.  
- DHCP habilitado en interfaz específica(Sistemas) del firewall.  
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
   - Activación de DHCP solo en interfaz definida(Sistemas).
   - Reglas de navegación LAN → WAN.
   - Políticas específicas para acceso hacia DMZ.
   - Configuración de NAT para salida a Internet.

3. **Servidor Debian 13**
   - Configuración de Bind9 como DNS interno.
   - Servicio DHCP para red específica(Departamentos).
   - NTP para sincronización horaria.
   - Servidor web Nginx accesible según reglas del firewall.
   - Acceso administrativo mediante SSH desde red autorizada.

4. **Integración con AWS**
   - Creación de instancia EC2 en AWS Free Tier.
   - Pruebas de conectividad desde red local.
   - Uso de entorno cloud como extensión funcional del laboratorio.

---

## 🔹 Resultados de Pruebas

- ✅ Segmentación funcional entre LAN-Sistemas, LAN-Departamentos y DMZ.  
- ✅ Aislamiento correcto del servidor en red DMZ.  
- ✅ Asignación DHCP operativa en la interfaz configurada(Sistemas) del FortiGate.  
- ✅ Resolución DNS interna funcionando correctamente desde clientes.  
- ✅ Acceso web al servidor Debian validado según políticas de firewall.  
- ✅ Navegación a Internet operativa mediante NAT en FortiGate.  
- ✅ Conectividad exitosa con instancia EC2 en AWS desde la red local.  
- ✅ Administración remota por SSH funcional desde red autorizada.  

---

## 🔹 Capturas

![Topología en GNS3](screenshots/topologia_gns3.png)
![FortiGate - Interfaces y Políticas](screenshots/fortigate_dashboard.png)
![Servidor Debian - Servicios activos](screenshots/debian_services.png)
![Instancia EC2 en AWS](screenshots/aws_ec2.png)

---

## 🔹 Archivos

- [topologia_gns3.gns3](topologia_gns3.gns3) → Proyecto base de la topología en GNS3.  
- [config_fortigate.txt](config_fortigate.txt) → Resumen de configuración de interfaces y políticas.  
- [config_debian_servicios.txt](config_debian_servicios.txt) → Configuración DNS, DHCP, NTP y Web.  
- [pruebas_conectividad.txt](pruebas_conectividad.txt) → Resultados de pruebas (ping, nslookup, curl, ssh).  

---

## 🔹 Futuras Mejoras

- Implementar VPN site-to-site entre red local y AWS.  
- Separar servicios en múltiples servidores (DNS/DHCP independiente del Web).  
- Migrar servicios a contenedores Docker.  
- Implementar monitoreo con Prometheus + Grafana.  
- Automatizar despliegues con Ansible.  
- Implementar control de logs centralizado.  

---

👨‍💻 **Autor:** Juan R.  
📘 **Repositorio:** [Infraestructura Empresarial](Infraestructura-Empresarial/README.md)  
🗓️ **Versión:** 1.0  
