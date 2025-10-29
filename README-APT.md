# 🧠 Simulación de Ataques Persistentes Avanzados (APTs)
### Proyecto de Titulación – Universidad Estatal de Milagro (UNEMI)

![Arquitectura del Laboratorio](docs/Arquitectura_Red_Diagramas.png)

---

## 💾 Máquinas Virtuales del Laboratorio APT

Este proyecto incluye tres máquinas virtuales diseñadas para la **simulación de ataques persistentes avanzados (APT)** mediante la integración de **MITRE CALDERA** y **Wazuh**.  
Los archivos `.ova` se encuentran alojados externamente debido a su tamaño.

| **Nombre de la Máquina** | **Sistema Operativo / Versión** | **Dirección IP** | **Rol Principal** | **Archivo (.ova)** | **Descarga / Enlace** |
|---------------------------|----------------------------------|------------------|------------------|--------------------|------------------------|
| 🧠 **CALDERA-APT** | Ubuntu Server 22.04 LTS | 192.168.56.50 | Emulación de adversarios (MITRE CALDERA) | `CALDERA-APT.ova` | [📥 Descargar desde Google Drive](https://drive.google.com/file/d/1iM9P_5avfJ2aMsfDyPHSdbedLfhWR_qH/view?usp=sharing) |
| 🛡️ **WAZUH-ELK1** | Ubuntu Server 22.04 LTS | 192.168.56.51 | SIEM, correlación y monitoreo de eventos | `WAZUH-ELK1.ova` | [📥 Descargar desde Google Drive](https://drive.google.com/file/d/1dWy9XHJLHGy0JAeYDaEZ-hdH8mSzMO0A/view?usp=sharing) |
| 💻 **WINDOWS-CLIENT** | Windows 11 Home | 192.168.56.100 | Cliente víctima y generación de telemetría | `WIN-APT.ova` | [📥 Descargar desde Google Drive](https://drive.google.com/file/d/1Td1OSth7lYd5A-qJjauTiFINZgHfa8m6/view?usp=drive_link) |

---

### 🧩 Requisitos de VirtualBox
- Versión mínima recomendada: **VirtualBox 7.0 o superior**
- Adaptador de red **Host-Only** configurado en `192.168.56.0/24`
- RAM total sugerida: **≥ 10 GB** (distribuida entre las VMs)
- Espacio libre en disco: **≥ 100 GB**

---

### 🧠 Credenciales predeterminadas

| **Máquina** | **Usuario** | **Contraseña** |
|--------------|-------------|----------------|
| CALDERA | `caldera` | `admin` |
| WAZUH | `wazuh` | `admin` |
| WINDOWS | `UsuarioLocal` | `Passw0rd!` |

> ⚠️ *Por motivos de seguridad, se recomienda cambiar las contraseñas tras la primera ejecución.*

---

### ⚙️ Restauración y uso
1. Descarga los archivos `.ova` desde los enlaces anteriores.  
2. Abre **VirtualBox → Archivo → Importar servicio virtualizado (OVA)**.  
3. Selecciona el archivo correspondiente a cada VM y sigue el asistente.  
4. Verifica las IPs asignadas (192.168.56.50 / 51 / 100).  
5. Inicia primero **WAZUH-ELK1**, luego **CALDERA**, y finalmente **WINDOWS-CLIENT**.

---

### 🧾 Notas adicionales
- Las máquinas ya incluyen configuraciones básicas de red y scripts de inicialización.  
- El entorno está diseñado para pruebas controladas **en red local (Host-Only)**.  
- No se recomienda su uso en redes de producción o conectadas a Internet.

---

## 📘 Descripción del Proyecto
El presente proyecto implementa un **laboratorio de simulación de ataques persistentes avanzados (APT)** con el objetivo de fortalecer las pruebas continuas de seguridad en entornos controlados.  
Para ello, se integran las herramientas **MITRE CALDERA** y **Wazuh/ELK**, permitiendo emular adversarios reales, analizar eventos en tiempo real y generar reportes técnicos basados en las tácticas y técnicas del marco **MITRE ATT&CK**.

---

## 🎯 Objetivos
- **Detectar vulnerabilidades críticas** y generar reportes claros que orienten al personal técnico en la mitigación de riesgos.  
- **Diseñar un laboratorio simulado** que permita la ejecución de campañas controladas de ataque y defensa.  
- **Medir indicadores de seguridad**, tales como el MTTD (Mean Time To Detect) y MTTR (Mean Time To Respond), en entornos Windows y Linux virtualizados.

---

## 🧩 Tecnologías Implementadas

| **Componente**    | **Versión** | **Propósito**              |
|--------------------|-------------|-----------------------------|
| MITRE CALDERA      | 5.3.0       | Emulación de adversarios    |
| Wazuh Manager      | 4.7.5       | SIEM y correlación          |
| Sysmon             | 15.x        | Telemetría Windows          |
| VirtualBox         | 7.0         | Virtualización              |
| Ubuntu Server      | 22.04 LTS   | Host SIEM                   |
| Windows 11         | Home        | Cliente objetivo            |

---

## 🌐 Arquitectura y Topología de Red

| **Sistema / Equipo** | **Dirección IP** | **Rol Principal** | **Conexión** |
|----------------------|------------------|--------------------|--------------|
| CALDERA              | 192.168.56.50    | Emulación APT      | Envía órdenes a Windows |
| WAZUH-ELK            | 192.168.56.51    | SIEM y monitoreo   | Recibe logs de Windows |
| WINDOWS CLIENT       | 192.168.56.100   | Agente objetivo    | Reporta telemetría a Wazuh |
| Red Virtual          | 192.168.56.0/24  | Host-Only          | Comunicación interna entre VMs |

📎 **Flujo de datos:**  
1. CALDERA ejecuta escenarios (Adversaries) hacia el cliente Windows.  
2. Sysmon registra los eventos generados.  
3. El agente Wazuh envía los registros al Manager (192.168.56.51).  
4. Wazuh analiza, genera alertas y visualiza los resultados en Kibana.

---

## ⚙️ Instalación Rápida

### 🔸 Requisitos previos
```bash
sudo apt update && sudo apt install git python3-pip docker -y
```

### 🔸 Clonar el repositorio
```bash
git clone https://github.com/TU_USUARIO/APT-Simulation.git
cd APT-Simulation
```

### 🔸 Configuración de red en VirtualBox
- Tipo: **Host-Only Adapter**
- Rango: `192.168.56.0/24`
- Asignaciones:
  - CALDERA → `192.168.56.50`
  - WAZUH → `192.168.56.51`
  - WINDOWS CLIENT → `192.168.56.100`

---

## 🔐 Credenciales de Acceso

| **Sistema** | **Usuario** | **Contraseña** | **Puerto / Acceso** |
|--------------|-------------|----------------|---------------------|
| CALDERA      | admin       | CalderaAdmin2025! | 8888 |
| Wazuh        | admin       | WazuhAdmin2025!   | 5601 / 443 |
| Windows      | UsuarioLocal | Passw0rd!       | 3389 (RDP) |

⚠️ *Por seguridad, se recomienda cambiar estas credenciales en la primera ejecución.*

---

## 📊 Resultados del Laboratorio

| **Métrica** | **Descripción** | **Valor promedio** |
|--------------|------------------|-------------------|
| MTTD (Detección) | Tiempo promedio en detectar amenazas | 3.4 s |
| MTTR (Respuesta) | Tiempo promedio de respuesta | 9.7 s |
| Cobertura ATT&CK | Técnicas simuladas / detectadas | 85% |
| Eventos procesados | Total de logs analizados | 686 |

📄 Los reportes se generan automáticamente en `/results/report_APT_2025.pdf` y en Kibana.

---

## 📂 Documentación Complementaria

| **Archivo** | **Descripción** |
|--------------|----------------|
| [Manual de Usuario](docs/Manual_Usuario_APT_COMPLETO.docx) | Guía paso a paso del entorno |
| [Manual Técnico](docs/Manual_Tecnico_APT_COMPLETO.docx) | Instalación, arquitectura y scripts |
| [Licencia y Contribuciones](docs/Licencia_y_Contribuciones_APT.docx) | Derechos y proceso colaborativo |
| [Arquitectura de Red](docs/Arquitectura_Red_Diagramas.png) | Diagrama visual del laboratorio |

---

## 🤝 Contribuciones

¿Deseas colaborar? Sigue estos pasos:

```bash
# 1. Fork del repositorio
# 2. Crea una rama nueva
git checkout -b feature/nueva-funcionalidad

# 3. Guarda tus cambios
git commit -m "Agregada nueva funcionalidad"

# 4. Sube tu rama y crea un Pull Request
git push origin feature/nueva-funcionalidad
```

💡 Asegúrate de probar el entorno antes de enviar tu PR.  

---

## 📜 Licencia

Este proyecto está bajo la **Licencia MIT**:

```
MIT License
Copyright (c) 2025 Antonio Salamar & Freddy Torres
```

Consulta el archivo [LICENSE](LICENSE) para los detalles completos.

---

## 📬 Contacto

**Autores:**  
- 🧑‍💻 *Antonio Salamar Barre*  
- 👨‍💻 *Freddy Torres*  

**Institución:** Universidad Estatal de Milagro – UNEMI  
**Correo de contacto:** antonio.salaman@unemi.edu.ec  

---

> *Desarrollado como proyecto de titulación en Ingeniería en Tecnologías de la Información – 2025.*
