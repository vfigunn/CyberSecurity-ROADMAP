# 💼 Salidas Profesionales en Ciberseguridad

El campo de la ciberseguridad es vasto y no se limita a "ser un hacker". Existen docenas de roles, cada uno con requisitos, responsabilidades y perfiles diferentes.

Esta guía mapea los roles más comunes con los módulos del Vault que necesitas dominar para desempeñarte en ellos.

---

## 🛡️ Blue Team / Defensa (Operaciones)

### 1. Security Operations Center (SOC) Analyst (L1 / L2)
Es el rol de entrada (entry-level) más común en ciberseguridad defensiva. Sos la primera línea de defensa, monitoreando alertas y analizando posibles incidentes.
- **Responsabilidades:** Triage de alertas en el SIEM, análisis inicial de incidentes, contención básica, escalamiento a L2/L3.
- **Módulos Core:** `02` (Networking), `13` (Defensive Security), `14` (SOC & SIEM), `15` (Incident Response).
- **Herramientas Clave:** Splunk, Elastic, EDRs (CrowdStrike, Defender), Wireshark.
- **Certificaciones:** CompTIA CySA+, Cisco CyberOps, BTL1.

### 2. Detection Engineer
Evolución del analista SOC. En lugar de mirar alertas, *creas* las reglas que generan esas alertas.
- **Responsabilidades:** Escribir reglas SIEM/EDR (YARA, Sigma), afinar detecciones para reducir falsos positivos, mapear defensas contra MITRE ATT&CK.
- **Módulos Core:** `14` (SOC & SIEM), `18` (Threat Intel), `25` (Blue Team).
- **Conocimientos Extra:** `10` (Python/Scripting).

### 3. Incident Responder (IR)
Interviene cuando una brecha de seguridad es confirmada. Su objetivo es detener el "sangrado", erradicar al atacante y recuperar el negocio.
- **Responsabilidades:** Contención de amenazas, análisis forense rápido, comunicación de crisis, erradicación de malware.
- **Módulos Core:** `03` (Linux), `04` (Windows), `15` (Incident Response), `16` (DFIR).
- **Herramientas Clave:** Velociraptor, KAPE, herramientas EDR.
- **Certificaciones:** GCIH, CySA+.

### 4. Digital Forensics Analyst (DFIR)
El "detective de la escena del crimen". Investiga sistemas *después* de un incidente para saber exactamente qué pasó, cómo entraron y qué se llevaron.
- **Responsabilidades:** Adquisición forense de discos/memoria, análisis de artefactos de Windows/Linux, reconstrucción de líneas de tiempo.
- **Módulos Core:** `16` (DFIR), `17` (Malware Analysis), `04` (Windows - Event Logs/Registry).
- **Herramientas Clave:** Autopsy, Volatility, FTK Imager.
- **Certificaciones:** GCFA, CHFI.

### 5. Threat Intelligence Analyst
Analiza información global sobre atacantes para predecir y prevenir futuros ataques contra la organización.
- **Responsabilidades:** Recolección de OSINT, análisis de malware a alto nivel, creación de reportes sobre Threat Actors (TTPs).
- **Módulos Core:** `18` (Threat Intelligence).
- **Herramientas Clave:** MISP, MITRE ATT&CK, Maltego.

---

## ⚔️ Red Team / Ataque (Offensive)

### 6. Junior Penetration Tester
Rol de entrada al mundo ofensivo. Realiza evaluaciones de seguridad controladas y autorizadas.
- **Responsabilidades:** Escaneo de redes, identificación de vulnerabilidades, explotación básica, redacción de reportes.
- **Módulos Core:** `02` (Networking), `09` (Web Security), `12` (Offensive Security).
- **Herramientas Clave:** Nmap, Burp Suite, Metasploit, Nessus.
- **Certificaciones:** eJPT, CompTIA PenTest+.

### 7. Network / Infrastructure Penetration Tester
Especialista en comprometer redes corporativas, desde la infraestructura externa hasta el control total del dominio interno.
- **Responsabilidades:** Pivotar por redes, escalar privilegios, explotar fallos de configuración.
- **Módulos Core:** `12` (Offensive Security), `19` (Active Directory).
- **Herramientas Clave:** BloodHound, Impacket, Mimikatz, C2 (Cobalt Strike, Mythic).
- **Certificaciones:** PNPT, OSCP.

### 8. Web Application Penetration Tester (AppSec)
Especialista en encontrar vulnerabilidades en aplicaciones web y APIs antes de que salgan a producción.
- **Responsabilidades:** Pruebas manuales de OWASP Top 10, inyecciones complejas, bypass de WAFs.
- **Módulos Core:** `09` (Web Security), `10` (Programming).
- **Herramientas Clave:** Burp Suite Professional, OWASP ZAP, Postman.
- **Certificaciones:** OSWE, eWPT.

### 9. Red Teamer
A diferencia de un pentester (que busca *todas* las vulnerabilidades), el Red Teamer simula ser un atacante real (APT) para probar cómo responde el Blue Team.
- **Responsabilidades:** Phishing, evasión de antivirus (AV/EDR), persistencia oculta, operaciones a largo plazo, seguridad física (lockpicking).
- **Módulos Core:** `19` (Active Directory), `24` (Red Team).
- **Certificaciones:** CRTO, OSEP.

---

## 🏗️ Security Engineering & Architecture

### 10. Security Engineer
Implementa, mantiene y soluciona problemas en las soluciones de seguridad (Firewalls, Proxies, EDRs).
- **Responsabilidades:** Despliegue de herramientas de seguridad, configuración de políticas, hardening de servidores.
- **Módulos Core:** `03` (Linux), `04` (Windows), `07` (Network Security).
- **Certificaciones:** Sec+, CCNA.

### 11. Cloud Security Engineer
Especialista en asegurar entornos AWS, Azure o GCP. Es uno de los roles con mayor demanda actual.
- **Responsabilidades:** Configuración de IAM en la nube, seguridad de contenedores, monitoreo de buckets/storage.
- **Módulos Core:** `20` (Cloud Security).
- **Certificaciones:** AWS Security Specialty, Azure SC-500.

### 12. DevSecOps Engineer
Trabaja con desarrolladores para integrar la seguridad en el proceso de creación de software, de forma automatizada.
- **Responsabilidades:** Integración de SAST/DAST en pipelines (GitHub Actions, Jenkins), escaneo de contenedores, gestión de secretos.
- **Módulos Core:** `10` (Programming), `21` (DevSecOps).

### 13. Security Architect
Un rol senior que diseña cómo deben encajar todas las piezas de seguridad de una empresa (Zero Trust, segmentación, IAM).
- **Responsabilidades:** Diseño de alto nivel, evaluación de tecnologías, alineación de la seguridad con el negocio.
- **Módulos Core:** `23` (Security Architecture).
- **Certificaciones:** CISSP.

---

## 📋 GRC (Governance, Risk, and Compliance)

### 14. Vulnerability Management Analyst
Gestiona el ciclo de vida de los parches. No explota las vulnerabilidades, sino que coordina su solución.
- **Responsabilidades:** Escaneos periódicos (Nessus, Qualys), priorización de parches basada en riesgo, reporte a equipos de IT.
- **Módulos Core:** `11` (Vulnerability Management), `22` (GRC).

### 15. GRC Analyst / IT Security Auditor
Asegura que la empresa cumpla con las leyes, normativas (ISO 27001, PCI-DSS) y políticas internas. No es un rol altamente técnico, sino más centrado en procesos, leyes y gestión de riesgos.
- **Responsabilidades:** Redacción de políticas de seguridad, gestión de riesgos de terceros, conducción de auditorías internas.
- **Módulos Core:** `22` (GRC).
- **Certificaciones:** CISA, CRISC, CISM.
