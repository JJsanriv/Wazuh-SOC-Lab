# Laboratorio SOC & Respuesta ante Incidentes con Wazuh

## 🛡️ Descripción del Proyecto
Implementación de un entorno de **Security Operations Center (SOC)** para la detección, monitorización y respuesta automatizada ante ciberataques. El laboratorio simula un entorno de producción donde un servidor (Ubuntu) es monitorizado por un SIEM (Wazuh) para detectar y mitigar amenazas en tiempo real de la máquina atacante Kali.

## 🛠️ Stack Tecnológico
- **SIEM:** Wazuh (Manager, Indexer, Dashboard).
- **SO Víctima:** Ubuntu Server 22.04 LTS.
- **SO Atacante:** Kali Linux.
- **Servicios Monitorizados:** OpenSSH, Apache Web Server.
- **Herramientas de Ataque:** Hydra, Dirb.

## 🏗️ Arquitectura del Laboratorio
![Arquitectura del Laboratorio](https://github.com/user-attachments/assets/aaba229c-9f61-4b0a-ae08-88a4a193cc11)

---

## 🔍 Casos de Uso (Simulación de Ataques)

### 1. Detección y Mitigación de Fuerza Bruta (SSH)
[[Ver Caso de Estudio 1: Fuerza Bruta SSH](docs/evidences/brute_force_ssh.md)]
- **Ataque:** Ejecución de ataque de diccionario contra el puerto 22 usando `hydra`.
- **Detección:** Configuración de reglas de Wazuh (ID 5710) para identificar múltiples fallos de autenticación.
- **Respuesta (SOAR):** Implementación de **Active Response** que bloquea automáticamente la IP del atacante mediante `iptables` durante 60 segundos.

### 2. Detección de Enumeración Web (Apache)
[[Ver Caso de Estudio 2: Escaneo de Directorios](docs/evidences/web_reconnaissance.md)]
- **Ataque:** Escaneo de directorios ocultos y archivos sensibles utilizando `dirb`.
- **Detección:** Ingesta de logs de Apache (`/var/log/apache2/access.log`). Wazuh detecta un comportamiento anómalo (múltiples errores 404) y dispara la regla ID 31151 (Nivel 10).
- **Respuesta (SOAR):** Bloqueo dinámico de la IP origen mediante `firewall-drop`.

---

## ⚙️ Ingeniería de Detección y Respuesta (Blue Team)
Para automatizar la mitigación, se configuró el módulo `Active Response` en el agente:
- **Archivo de configuración:** `/var/ossec/etc/ossec.conf`
- **Acción:** Bloqueo dinámico de IP mediante `firewall-drop` ante alertas críticas.

## 📈 Resultados y Evidencias
- **Dashboard:** Visualización de eventos correlacionados con la matriz **MITRE ATT&CK**.
- **Automatización:** Verificación exitosa mediante los logs de `active-responses.log` y validación de reglas en `iptables`.

## 🚀 Aprendizajes Clave
- **Ingeniería de Detección:** Desarrollo de reglas personalizadas en XML.
- **Visibilidad:** Gestión y parseo de logs en servicios críticos (SSH, Apache).
- **Mitigación:** Implementación de SOAR para la reducción del tiempo de respuesta (MTTR).

## 💡 Próximos pasos
- [ ] Implementar **FIM (File Integrity Monitoring)** en directorios web críticos.
- [ ] Integración de notificaciones (Telegram/Email).
- [ ] Escaneo automatizado de vulnerabilidades con Wazuh.
