# 11 - Resumen (Cheat Sheet - Red Team y OSINT)

Esta nota agrupa los conceptos estratégicos, arquitecturas de sigilo (C2) y metodologías globales (MITRE / OSINT) del Módulo de Red Team.

---

## 🏗️ La Taxonomía de Equipos y Ejercicios
- **Red Team:** Ofensiva real. Simula ataques de meses (APT), sin ruido. Mide los procesos humanos de detección, asumiendo que el aspecto técnico inicial (Firewalls) fallará ante un atacante con recursos (Ingeniería social).
- **Blue Team:** Defensa integral. Vigila la red, configura el EDR (Antivirus Avanzado), parcha sistemas (Vulnerability Management) y responde ante incidentes.
- **Purple Team:** Ejercicios hiper-efectivos de *colaboración en vivo* en la misma mesa (El Red inyecta malware; el Blue ajusta reglas en caliente).
- **Red Team vs Pentesting:** Pentesting = Identificar la mayor cantidad de agujeros web posibles en poco tiempo. Red Teaming = Lograr una única "Misión/Objetivo crítico de negocio" permaneciendo oculto meses.

---

## 🔍 OSINT (Inteligencia de Fuentes Abiertas)
Recolección y perfusión pasiva. Buscar en Internet sin "tocar/escanear" de frente a la víctima.
- **Google Dorking:** Uso avanzado de parámetros en texto (Ej: `site:banco.com filetype:xlsx "passwords"`) para encontrar en segundos archivos clasificados de la víctima ya cacheados en el motor por descuidos.
- **Shodan:** El buscador oscuro e IoT. Escanea servidores, puertos 3306 (Bases de datos) y dispositivos físicos expuestos a Internet sin clave globalmente, mostrando el banner de respuesta. (Ej: `port:"445" org:"empresa"`).

---

## 🗣️ Ataques de Capa 8 (Vulnerabilidad Humana)
La *Ingeniería Social* saltea cualquier Firewall carísimo, vulnerando psicológicamente al empleado.
- **Phishing:** Envío genérico masivo por E-mail (La red).
- **Spear-Phishing / Whaling:** Envío de UN solo mail a UN solo empleado clave. Basado 100% en la inteligencia pasiva recolectada sobre sus hobbies, nombre del jefe o tecnologías internas, elevando el éxito radicalmente.
- **Vishing:** Voz/Teléfono (Manipular a la víctima simulando autoridad del servicio técnico / Caller-ID Spoofing).
- **Baiting:** Pendrives tirados físicamente en el estacionamiento de la víctima (El cebo para curiosos).

---

## 🪖 Modelos Militares (Kill Chain y ATT&CK)
- **Cyber Kill Chain (Lockheed Martin):** 7 Pasos lineales (1.Reconocimiento -> 2.Armamento -> 3.Entrega -> 4.Explotación -> 5.Instalación -> 6.C2 -> 7.Objetivos). Tesis Defensiva: Cortar la cadena en cualquier paso anterior al #7 anula el ataque enemigo.
- **MITRE ATT&CK:** Matriz, Diccionario universal. Agrupa el Hacking mundial en **Tácticas** (El Objetivo) y **Técnicas / Subtécnicas** (El Cómo). Permite medir matemáticamente la efectividad del EDR o los reportes técnicos en lenguaje unificado.

---

## 📡 Command and Control (El Sigilo: Beaconing)
Supera las limitaciones térmicas/detectables (Ruido de red 24/7 vivo) que causan las Reverse Shells tradicionales y `Meterpreter`.
- **C2 (Cobalt Strike, Mythic):** Servidor coordinador en Internet. Se inyecta un *Agente o Beacon* liviano en la PC víctima.
- **El Modelo Beaconing:** El virus duerme 99% del tiempo (Asincronía). "Despierta" (Ping), se comunica con "casa" (HTTP de apariencia normal) para recibir instrucciones secretas, y se vuelve a dormir.
- **Jitter:** Aleatoriedad temporal de sueño matemático. (Si se despierta rígidamente cada exactos 15 min, el SOC te descubre por patrón. El *Jitter* del 10% hace que despierte entre 13 y 17 min arbitrarios).

---
## 📑 El Documento Final
- El Hacking culmina (Triage y Reporte). Tiene dos partes clave: **Resumen Ejecutivo (No técnico / Impacto y Negocio)** y **Resumen Técnico (Evidencia "PoC" para ingenieros y forma de parchear)**.
- **CVSS:** Es el sistema y calculadora estandarizada (0.0 a 10.0) de vulnerabilidades (Impide asignaciones a sentimiento manuales basadas en la gravedad técnica).

🎉 **¡Felicitaciones gigantes, completaste el Módulo de Red Team (Fase 10)!**
Has masterizado la perspectiva de la guerra ofensiva. La ruta de la Seguridad Ofensiva teórica e Infraestructural queda completamente saldada a este nivel. Actualizá tu archivo de [[Progreso]]. Ahora abriremos paso a módulos de especialización profundos para consolidar tu señorío en Windows con el letal [[10 - Active Directory/00 - Overview|Módulo 10 - Escalada y Active Directory (AD)]].
