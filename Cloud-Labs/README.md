# ☁️ Laboratorio de Despliegue Cloud – AWS / Azure

## 🔹 Descripción
Este laboratorio forma parte del plan de formación en **Infraestructura & Cloud Engineering**, enfocado en la práctica y administración de entornos cloud híbridos.  
El objetivo es desplegar instancias en **AWS y Azure**, configurar redes seguras (**VPC / VNet**) y establecer una **VPN site-to-site** con el laboratorio local, garantizando una comunicación segura y gestionada entre ambas infraestructuras.

---

## 🔹 Entorno

📌 **Proveedores Cloud:** Amazon Web Services (AWS) / Microsoft Azure  
📌 **Infraestructura local:** Proxmox / VMware con pfSense como gateway  
📌 **Servicios implementados:** EC2, S3, Azure VM, Azure Storage, VPN IPsec  
📌 **Protocolos:** SSH, HTTPS, IPsec/IKEv2  
📌 **Segmentos:**  
- AWS VPC: `10.0.10.0/24`  
- Azure VNet: `10.1.0.0/24`  
- Local LAN: `192.168.10.0/24`  

---

## 🔹 Topología General

**Estructura general de conexión:**

[ LAN Local ]  
 │  
[ pfSense / Gateway ]  
 │ ⇅ (VPN IPsec)  
 │  
[ Cloud Gateway ]  
 ├── AWS VPC (10.0.10.0/24)  
 │ ├── EC2 (App Server)  
 │ └── S3 Bucket (Backups)  
 └── Azure VNet (10.1.0.0/24)  
  ├── Azure VM (Web)  
  └── Azure Blob Storage  

---

## 🔹 Objetivos del Laboratorio
- Crear y configurar una **VPC en AWS** y una **VNet en Azure**.  
- Establecer subredes públicas y privadas con control de acceso.  
- Conectar ambas nubes con el entorno local mediante **VPN site-to-site**.  
- Configurar **Security Groups / NSG** para restringir tráfico.  
- Implementar **copias de seguridad automatizadas** de servidores y volúmenes.  

---

## 🔹 Configuraciones Clave

### 🔸 AWS
1. Crear **VPC (10.0.10.0/24)** con subred pública (10.0.10.0/26) y privada (10.0.10.64/26).  
2. Configurar **Internet Gateway** y **NAT Gateway**.  
3. Crear instancias **EC2 (Ubuntu 22.04)** con acceso SSH restringido por IP.  
4. Crear **S3 Bucket** para backups con políticas IAM seguras.  
5. Configurar **VPN IPsec** con pfSense local.  

### 🔸 Azure
1. Crear **VNet (10.1.0.0/24)** con subred web y subred interna.  
2. Implementar **Azure Virtual Network Gateway** para VPN.  
3. Crear una **Azure VM** (Ubuntu Server 22.04) y probar conectividad SSH.  
4. Configurar **NSG** (Network Security Group) para acceso limitado.  
5. Configurar **Azure Storage Blob** para backups automáticos.  

### 🔸 Local (On-Prem)
1. Configurar **pfSense** como gateway principal.  
2. Establecer túneles IPsec con AWS y Azure.  
3. Verificar conectividad (ping / traceroute).  
4. Aplicar ACLs para tráfico controlado entre segmentos.  

---

## 🔹 Resultados de Pruebas
- ✅ Comunicación estable entre Proxmox y AWS/Azure vía VPN.  
- ✅ Instancias EC2 y Azure VM accesibles mediante SSH seguro.  
- ✅ Subredes privadas correctamente aisladas.  
- ✅ Backups automáticos exitosos a S3 y Blob Storage.  
- ✅ Reglas de seguridad (SG / NSG) aplicadas y validadas.  
- ✅ Latencia baja y conectividad redundante entre sitios.  

---

## 🔹 Capturas
![Topología Cloud Híbrida](screenshots/cloud_topology.png)
![Instancia EC2 AWS](screenshots/aws_ec2.png)
![Azure VNet Gateway](screenshots/azure_gateway.png)
![VPN Establecida](screenshots/vpn_connected.png)

---

## 🔹 Archivos
- [aws_vpc_config.txt](aws_vpc_config.txt) → Configuración de red en AWS.  
- [azure_vnet_config.txt](azure_vnet_config.txt) → Configuración de red en Azure.  
- [vpn_pfsense.conf](vpn_pfsense.conf) → Configuración de túnel site-to-site.  
- [backup_s3_blob.sh](backup_s3_blob.sh) → Script de automatización de copias de seguridad.  

---

## 🔹 Futuras Mejoras
- Implementar balanceo entre regiones (AWS us-east-1 / Azure East Europe).  
- Integrar monitoreo con **Grafana Cloud**.  
- Desplegar infraestructura automatizada con **Terraform**.  
- Añadir autenticación centralizada con **Azure AD / IAM**.  

---

👨‍💻 **Autor:** Juan R.  
📘 **Repositorio:** [Cloud-Labs](Cloud-Labs/README.md)  
🗓️ **Versión:** 1.0  
