# 🛡️ Laboratorio de Seguridad Perimetral con FortiGate - GNS3

## 🔹 Descripción

Este laboratorio implementa un entorno de **seguridad perimetral empresarial** utilizando un **firewall FortiGate** desplegado en **GNS3**, con el objetivo de proteger la red interna frente a amenazas externas y controlar el acceso a los servicios críticos de la organización.

El proyecto reproduce un escenario realista en el que una empresa cuenta con distintos segmentos de red internos (LAN, DMZ y Administración) y un enlace WAN hacia Internet.  
Se aplican **políticas de filtrado**, **NAT**, **zonas de seguridad**, **segmentación**, y **reglas de acceso basadas en roles** para garantizar la integridad, disponibilidad y confidencialidad de los recursos internos.

---

## 🔹 Objetivos del Laboratorio

- Implementar un **firewall perimetral FortiGate** en GNS3.  
- Configurar **interfaces, zonas y rutas** para segmentar la red.  
- Aplicar **políticas de seguridad** entre redes internas y externas.  
- Configurar **NAT y políticas de acceso a Internet**.  
- Implementar **una DMZ** con servidores públicos (WEB y DNS).  
- Realizar **pruebas de tráfico** y validación de políticas.  
- Documentar reglas, resultados y buenas prácticas de seguridad.

---

## 🔹 Topología

📌 La red se compone de:

- **Red administrativa** → 192.168.10.0/24  
   Acceso exclusivo para personal TI y gestión de dispositivos.

- **DMZ (zona desmilitarizada)** → 192.168.15.0/24  
  Servidores accesibles desde Internet (por ejemplo, WEB/DNS).

- **LAN interna** → 192.168.20.0/24  
 Segmento de usuarios y equipos internos.

- **WAN (Internet simulado)** → 10.0.0.0/24  
  Segmento externo conectado al router ISP o nube simulada.

### 🔧 Equipos Simulados en GNS3

| Dispositivo | Función | IP Principal |
|--------------|----------|--------------|
| FortiGate VM | Firewall perimetral | WAN: 10.0.0.2 / ADMIN: 192.168.10.1 / DMZ: 192.168.15.1 / LAN: 192.168.20.1 |
| Router ISP | Enlace a Internet | 10.0.0.1 |
| Servidor WEB (DMZ) | Página institucional | 192.168.15.10 |
| Servidor DNS (DMZ) | Resolución interna/externa | 192.168.15.11 |
| PC Usuario | Equipo en LAN | DHCP o IP fija |
| PC Admin | Gestión del firewall | 192.168.10.10 |

---

## 🔹 Tecnologías Implementadas

- **Firewall FortiGate (versión trial)**  
- **Segmentación de red** mediante interfaces y zonas (LAN / DMZ / WAN / ADMIN)  
- **Políticas de seguridad**:
  - Acceso LAN → Internet permitido (HTTP, HTTPS, DNS)
  - Acceso DMZ → limitado solo a servicios públicos
  - Acceso WAN → restringido hacia la red interna
- **NAT (Source NAT y Destination NAT)**  
  - Traducción de direcciones para acceso a Internet y publicación de servicios
- **Rutas estáticas** para conectividad entre segmentos  
- **Filtrado de tráfico y control de puertos**  
- **Pruebas de conectividad y seguridad**

---

## 🔹 Configuraciones Clave

### 1. Interfaces y Zonas
- Creación de interfaces LAN, DMZ, ADMIN y WAN.
- Asignación de direcciones IP a cada segmento.
- Agrupación en zonas de seguridad para simplificar políticas.

### 2. Políticas de Firewall
- **LAN → WAN:** Permitir HTTP/HTTPS/DNS con NAT.  
- **LAN → DMZ:** Solo acceso a servicios específicos (p. ej. DNS).  
- **WAN → DMZ:** Solo HTTP/HTTPS hacia servidor web publicado.  
- **ADMIN → LAN/DMZ/WAN:** Acceso total para gestión.

### 3. NAT
- **Source NAT (SNAT):** LAN accede a Internet mediante IP WAN.  
- **Destination NAT (DNAT):** Redirección de tráfico WAN → servidor web DMZ.

### 4. Ruteo
- Rutas estáticas configuradas para asegurar conectividad entre segmentos internos y el exterior.

### 5. Monitoreo
- Uso de **FortiView** y **Logs en tiempo real** para verificar políticas y tráfico.

---

## 🔹 Resultados de Pruebas

✅ Usuarios de LAN navegan hacia Internet correctamente.  
✅ Servidor web accesible desde Internet (mediante DNAT).  
✅ Acceso restringido desde WAN hacia LAN.  
✅ Administrador con acceso completo para monitoreo y gestión.  
✅ Políticas aplicadas correctamente según roles y zonas.  
✅ Tráfico no autorizado bloqueado y registrado en logs.

---

## 🔹 Archivos del Proyecto

- `fortigate_gns3_topology.gns3` → Archivo de topología para GNS3  
- `config_fortigate.txt` → Configuraciones exportadas del firewall  
- `pruebas_y_resultados.md` → Resultados de las pruebas de tráfico  
- `capturas/` → Carpeta con capturas de pantalla de la topología y políticas

---

## 🔹 Futuras Mejoras

- Integrar **IDS/IPS** para detección de intrusiones.  
- Implementar **VPN site-to-site** para acceso remoto seguro.  
- Añadir **monitoreo centralizado (FortiAnalyzer / Syslog)**.  
- Automatizar backups de configuración.  
- Simular acceso remoto de usuarios mediante **SSL VPN**.

---

## 👨‍💻 Autor

**Juan R.**  
Proyecto académico y profesional de laboratorio sobre **Seguridad Perimetral con FortiGate**, desarrollado en **GNS3**.  
📘 Objetivo: reforzar competencias prácticas en **Network Security** y **gestión de firewalls empresariales**.
