# 🐍 Laboratorio de Automatización de Redes con Python

## 🔹 Descripción
Este laboratorio forma parte del módulo de **Infraestructura & Cloud Engineering**, orientado a la **automatización de tareas de red** mediante scripting en Python.  
Su objetivo principal es desarrollar un script que permita realizar **copias de seguridad automáticas de configuraciones Cisco** a través de conexiones SSH seguras, optimizando la administración de dispositivos y reduciendo tareas repetitivas.

---

## 🔹 Entorno
📌 **Lenguaje:** Python 3.10+  
📌 **Librerías:** `paramiko`, `netmiko`, `datetime`, `os`  
📌 **Sistemas probados:** Cisco IOS (Router / Switch)  
📌 **Entorno de pruebas:** Packet Tracer / GNS3 / equipos físicos simulados  
📌 **Sistema operativo base:** Linux o Windows con Python instalado  

---

## 🔹 Objetivos del Laboratorio
- Automatizar el acceso a dispositivos Cisco mediante SSH.  
- Ejecutar comandos remotos (`show run`, `show version`, etc.) y guardar las salidas.  
- Generar respaldos automáticos con nombres dinámicos y fecha.  
- Documentar y organizar los archivos generados.  
- Mejorar la eficiencia operativa de administración de red.

---

## 🔹 Tecnologías y Librerías Usadas
| Librería | Propósito |
|-----------|------------|
| **Netmiko** | Manejo de sesiones SSH con dispositivos Cisco. |
| **Paramiko** | Comunicación SSH segura (opcional). |
| **Datetime** | Generación automática de nombres de archivos con fecha/hora. |
| **OS / Pathlib** | Gestión de rutas locales y carpetas de respaldo. |

---

## 🔹 Script Principal
**Archivo:** `backup_cisco.py`

### ⚙️ Funcionalidad:
1. Conecta automáticamente a una lista de dispositivos Cisco definidos en un archivo `devices.csv`.  
2. Ejecuta comandos de respaldo (`show running-config`).  
3. Guarda las configuraciones en carpetas locales organizadas por fecha.  
4. Genera un log de actividad con estado de cada conexión.  

### 🧱 Estructura del Proyecto
