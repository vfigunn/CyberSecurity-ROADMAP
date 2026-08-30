# 18 - Resumen (Cheat Sheet - Fundamentos)

Esta nota condensa los conceptos críticos del **Módulo 01**. Usala como referencia rápida para repasar antes de pasar al siguiente módulo o antes de un examen de certificación.

---

## 📚 Conceptos Base

- **Ciberseguridad:** Protección de información, sistemas y redes en el ámbito *digital* (es un subconjunto de la Seguridad de la Información).
- **Los 3 Pilares de Defensa:** Personas, Procesos, Tecnología. La tecnología sola no basta.
- **Tríada CIA:**
  - **Confidencialidad (Confidentiality):** Solo usuarios autorizados ven los datos (Cifrado, Permisos).
  - **Integridad (Integrity):** Los datos son exactos y no fueron alterados (Hashing, Auditoría).
  - **Disponibilidad (Availability):** Los sistemas están accesibles cuando se necesitan (Redundancia, Backups).

---

## ⚠️ Amenazas, Vulnerabilidades y Riesgo

- **Vulnerabilidad (Vulnerability):** Debilidad en el código, configuración, proceso o persona (La puerta rota).
  - *Zero-Day:* Vulnerabilidad conocida por atacantes, pero sin parche oficial disponible.
- **Amenaza (Threat):** Entidad o evento potencial capaz de explotar una vulnerabilidad (El ladrón / El huracán).
- **Exploit:** El método, script o software que aprovecha activamente la vulnerabilidad (La herramienta para forzar la puerta).
- **Riesgo (Risk):** Probabilidad de que un ataque ocurra × Impacto que causará.
  - Tratamiento del Riesgo: **Mitigar** (reducir), **Transferir** (seguros), **Aceptar** (asumir el costo), **Evitar** (no hacer el proyecto).

---

## 🛡️ Controles de Seguridad (Security Controls)

**Categorías (El Qué):**
- **Administrativos:** Políticas, procedimientos, entrenamiento.
- **Técnicos (Lógicos):** Firewalls, antivirus, cifrado, contraseñas.
- **Físicos:** Guardias, cámaras, cerraduras.

**Funciones (El Cuándo):**
- **Preventivos:** Bloquean el ataque antes de que ocurra (Firewall).
- **Detectivos:** Avisan que el ataque está ocurriendo (Cámara de seguridad, Alarma IDS).
- **Correctivos:** Restauran a la normalidad post-ataque (Backups).

---

## 🏰 Estrategias de Defensa

- **Superficie de Ataque (Attack Surface):** Todos los posibles puntos de entrada (digitales, físicos y humanos). Regla: *Minimizarla* (apagar lo que no se usa).
- **Defensa en Profundidad (Defense in Depth):** Aplicar múltiples capas de controles superpuestos asumiendo que los controles individuales eventualmente fallarán.
- **Mínimo Privilegio (Least Privilege):** Dar a los usuarios solo los permisos mínimos necesarios para hacer su trabajo.

---

## 🕵️ Threat Actors y Comportamiento

**Tipos de Atacantes:**
- *Script Kiddies:* Inexpertos, usan herramientas de otros (motivación: ego/caos).
- *Hacktivistas:* Motivación política/ideológica.
- *Cibercriminales:* Crimen organizado (motivación: dinero, ej. Ransomware).
- *Estados-Nación (APTs):* Patrocinados por gobiernos, recursos ilimitados, sigilosos (motivación: espionaje, sabotaje).
- *Insider Threats:* Empleados maliciosos o negligentes.

**Análisis de Comportamiento:**
- **Cyber Kill Chain:** 7 Fases tácticas del ataque (Reconnaissance, Weaponization, Delivery, Exploitation, Installation, C2, Actions on Objectives). Romper un eslabón detiene el ataque.
- **MITRE ATT&CK:** La enciclopedia de *Tácticas* (el objetivo del atacante), *Técnicas* (el cómo lo hace) y *Procedimientos* (herramientas exactas). Conocidos como **TTPs**.

---

## 📜 Frameworks de Seguridad

- **Framework:** Conjunto de buenas prácticas y guías estructuradas.
- **NIST CSF:** Estructura estratégica (Govern, Identify, Protect, Detect, Respond, Recover).
- **CIS Controls:** Táctico y técnico (lista priorizada de configuraciones y acciones).
- **ISO 27001:** Norma internacional certificable para Sistemas de Gestión de Seguridad (SGSI).

---

🎉 **¡Felicitaciones por completar el Módulo 01!**
Actualizá tu archivo [[Progreso]] y preparate para ensuciarte las manos con el [[02 - Networking/00 - Overview|Módulo 02 - Networking]].
