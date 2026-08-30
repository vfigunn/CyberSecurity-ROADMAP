# 🗺️ Roadmap de Ciberseguridad

Este es el camino estructurado para ir desde cero hasta un nivel profesional en ciberseguridad.

El roadmap está dividido en niveles de dificultad y contiene hitos (milestones) relacionados con certificaciones de la industria.

---

## 🟢 Nivel: Beginner (Principiante)

En esta fase construirás los cimientos. No se puede proteger (o atacar) lo que no se comprende.

### [[01 - Fundamentos]]
- **Objetivo:** Entender qué es la ciberseguridad, las amenazas, vulnerabilidades y el riesgo.
- **Conocimientos previos:** Ninguno.
- **Horas estimadas:** ~20h
- **Certificaciones relacionadas:** ISC2 CC (Domain 1), Security+ (Domain 1)

### [[02 - Networking]]
- **Objetivo:** Comprender cómo se comunican las computadoras (OSI, TCP/IP, protocolos).
- **Conocimientos previos:** Módulo 01.
- **Horas estimadas:** ~60h
- **Certificaciones relacionadas:** CCNA (Domain 1-4), ISC2 CC (Domain 4)

### [[03 - Linux]]
- **Objetivo:** Administrar y navegar sistemas Linux desde la línea de comandos.
- **Conocimientos previos:** Módulo 01.
- **Horas estimadas:** ~50h

### [[04 - Windows]]
- **Objetivo:** Entender la arquitectura de Windows, PowerShell y Active Directory (Intro).
- **Conocimientos previos:** Módulo 01.
- **Horas estimadas:** ~50h

### [[05 - Security Fundamentals]]
- **Objetivo:** Principios de defensa, Zero Trust, AAA, y controles de seguridad.
- **Conocimientos previos:** Módulos 01, 02.
- **Horas estimadas:** ~30h
- **Certificaciones relacionadas:** ISC2 CC (Domain 1, 3, 5)

> 🏆 **HITO 1: ISC2 Certified in Cybersecurity (CC) Readiness**
> Al completar el Módulo 05, tendrás los conocimientos base para preparar la certificación ISC2 CC. (Revisá la sección de [[30 - Certificaciones/ISC2 CC|Certificaciones]]).

---

## 🟡 Nivel: Intermediate (Intermedio)

Aquí empezamos a profundizar en áreas específicas de seguridad.

### [[06 - Cryptography]]
- **Objetivo:** Entender cifrado, hashing, PKI y certificados.
- **Conocimientos previos:** Módulos 01, 02, 05.
- **Horas estimadas:** ~35h

### [[07 - Network Security]]
- **Objetivo:** Proteger redes (Firewalls, IDS/IPS, VPNs, Segmentación).
- **Conocimientos previos:** Módulos 02, 05, 06.
- **Horas estimadas:** ~35h

### [[08 - Identity & Access]]
- **Objetivo:** Gestión de identidades, MFA, SSO y modelos de control de acceso.
- **Conocimientos previos:** Módulo 05.
- **Horas estimadas:** ~25h

### [[09 - Web Security]]
- **Objetivo:** Comprender vulnerabilidades web (OWASP Top 10) y cómo mitigarlas.
- **Conocimientos previos:** Módulos 02, 05, 06.
- **Horas estimadas:** ~50h

### [[10 - Programming & Scripting]]
- **Objetivo:** Automatizar tareas y crear scripts de seguridad (Python, Bash, PowerShell).
- **Conocimientos previos:** Módulos 03, 04.
- **Horas estimadas:** ~45h

### [[11 - Vulnerability Management]]
- **Objetivo:** Gestionar el ciclo de vida de las vulnerabilidades (escaneo, priorización, parcheo).
- **Conocimientos previos:** Módulos 05, 07.
- **Horas estimadas:** ~30h

> 🏆 **HITO 2: CompTIA Security+ (SY0-701) Readiness**
> Al completar el Módulo 11, tendrás una excelente base para la certificación Security+.
> 🏆 **HITO 3: Cisco CCNA Readiness**
> (Requiere práctica adicional con Packet Tracer).

---

## 🟡/🔴 Nivel: Intermediate to Advanced (Transición)

En esta fase los caminos se empiezan a separar entre ataque (Red) y defensa (Blue).

### [[12 - Offensive Security]]
- **Objetivo:** Metodología de pentesting, enumeración, explotación y escalada de privilegios.
- **Conocimientos previos:** Módulos 02, 03, 04, 05, 09, 10, 11.
- **Horas estimadas:** ~60h

> 🏆 **HITO 4: eJPTv2 Readiness**
> Al completar el Módulo 12 y sus laboratorios, estarás listo para el eJPT.

### [[13 - Defensive Security]]
- **Objetivo:** Hardening avanzado, EDR, XDR, y conceptos de detección.
- **Conocimientos previos:** Módulos 05, 07, 11.
- **Horas estimadas:** ~35h

### [[14 - SOC & SIEM]]
- **Objetivo:** Operaciones de un Security Operations Center, análisis de logs y SIEM (Splunk, Elastic).
- **Conocimientos previos:** Módulo 13.
- **Horas estimadas:** ~45h

### [[15 - Incident Response]]
- **Objetivo:** Metodología de respuesta a incidentes (las 7 fases), playbooks y triage.
- **Conocimientos previos:** Módulos 13, 14.
- **Horas estimadas:** ~30h

> 🏆 **HITO 5: CompTIA CySA+ (CS0-003) Readiness**
> Al completar el Módulo 15, tendrás la base teórica y práctica para el CySA+.

### [[18 - Threat Intelligence]]
- **Objetivo:** Inteligencia de amenazas, OSINT, IOCs, y MITRE ATT&CK avanzado.
- **Conocimientos previos:** Módulos 05, 13, 14.
- **Horas estimadas:** ~30h

### [[19 - Active Directory]]
- **Objetivo:** Atacar y defender entornos empresariales Microsoft (Kerberoasting, BloodHound).
- **Conocimientos previos:** Módulos 04, 08, 12.
- **Horas estimadas:** ~45h

> 🏆 **HITO 6: PNPT Readiness**
> Al dominar el Módulo 19 junto con el 12, estarás preparado para el PNPT de TCM Security.

### [[20 - Cloud Security]]
- **Objetivo:** Seguridad en AWS, Azure, GCP y entornos de contenedores (Kubernetes).
- **Conocimientos previos:** Módulos 02, 05, 06, 07, 08.
- **Horas estimadas:** ~40h

### [[22 - GRC]]
- **Objetivo:** Governance, Risk & Compliance (NIST CSF, ISO 27001, auditorías).
- **Conocimientos previos:** Módulos 05, 08, 11.
- **Horas estimadas:** ~35h

---

## 🔴 Nivel: Advanced (Avanzado)

Temas altamente especializados para roles senior.

### [[16 - DFIR]]
- **Objetivo:** Digital Forensics and Incident Response (análisis de disco, memoria y red).
- **Conocimientos previos:** Módulos 03, 04, 15.
- **Horas estimadas:** ~40h

### [[17 - Malware Analysis]]
- **Objetivo:** Análisis estático y dinámico de software malicioso.
- **Conocimientos previos:** Módulos 03, 04, 05, 13.
- **Horas estimadas:** ~35h

### [[21 - DevSecOps]]
- **Objetivo:** Seguridad en el ciclo de desarrollo de software (SAST, DAST, CI/CD).
- **Conocimientos previos:** Módulos 09, 10, 20.
- **Horas estimadas:** ~30h

### [[23 - Security Architecture]]
- **Objetivo:** Diseño seguro de redes, aplicaciones y entornos cloud (Zero Trust).
- **Conocimientos previos:** Módulos 05, 07, 08, 09, 20, 22.
- **Horas estimadas:** ~25h

### [[24 - Red Team]]
- **Objetivo:** Simulación de adversarios, evasión de EDR y operaciones C2.
- **Conocimientos previos:** Módulos 12, 19.
- **Horas estimadas:** ~30h

### [[25 - Blue Team]]
- **Objetivo:** Detección avanzada, Purple Teaming y Threat Hunting.
- **Conocimientos previos:** Módulos 13, 14, 15, 18.
- **Horas estimadas:** ~30h

> 🏆 **HITO 7: OSCP PEN-200 Readiness**
> Requiere dominio absoluto de los módulos 03, 04, 09, 10, 12, 19 y muchísima práctica en laboratorios.

---

## ➡️ ¿Qué sigue?

Dirigite al archivo [[Progreso]] para empezar a trackear tu aprendizaje.
