# Respuestas Evaluación Módulo 11 - Vulnerability Management

A continuación se presentan las respuestas correctas de la evaluación del [[11 - Vulnerability Management/09 - Evaluación|Módulo 11]], junto con la justificación técnica de cada una.

---

### Sección 1: Selección Múltiple (Multiple Choice)

**1. B) Common Vulnerabilities and Exposures**
> *Justificación:* CVE significa Common Vulnerabilities and Exposures y es el estándar para identificar vulnerabilidades de seguridad conocidas.

**2. B) El año en que se asignó el identificador**
> *Justificación:* En el formato CVE-YYYY-NNNNN, "YYYY" representa el año en que se asignó el identificador CVE a la vulnerabilidad.

**3. B) CVE identifica instancias específicas, CWE identifica categorías generales**
> *Justificación:* Un CVE es una vulnerabilidad específica (ej. CVE-2021-44228), mientras que un CWE es una categoría de debilidad (ej. CWE-502).

**4. D) Crítico**
> *Justificación:* Un CVSS de 9.1 se clasifica como Crítico (rango 9.0-10.0).

**5. B) OpenVAS**
> *Justificación:* OpenVAS es un escáner de vulnerabilidades de código abierto. Nessus es comercial, Burp Suite es para testing web, y Metasploit es un framework de explotación.

**6. C) Detectar la vulnerabilidad**
> *Justificación:* El primer paso es identificar y detectar las vulnerabilidades antes de poder gestionarlas.

**7. B) La probabilidad de que una vulnerabilidad sea explotada**
> *Justificación:* EPSS (Exploit Prediction Scoring System) predice la probabilidad de que una vulnerabilidad sea explotada en los próximos 30 días.

---

### Sección 2: Análisis de Escenario

**8. C) CVE-2020-0796 (Windows SMB)**
> *Justificación:* Esta vulnerabilidad tiene el CVSS más alto (9.8) y afecta a sistemas Windows que podrían estar en la red interna.

**9. B) Corregir inmediatamente**
> *Justificación:* Una vulnerabilidad de ejecución remota de código en un servidor expuesto a internet representa un riesgo crítico que requiere acción inmediata.

**10. B) Corregirla inmediatamente**
> *justificación:* Si una vulnerabilidad está explotada activamente en la naturaleza (confirmado por CISA KEV), debe corregirse independientemente de su CVSS.

---

### Sección 3: Comandos y Herramientas

**11. B) nmap --script vuln**
> *Justificación:* El script "vuln" de Nmap está diseñado específicamente para detectar vulnerabilidades conocidas.

**12. C) FIRST.org EPSS API**
> *justificación:* La API de EPSS está disponible en first.org y proporciona los puntajes de probabilidad de explotación.

---

### Sección 4: Análisis de Resultados

**13. D) CVE-2020-1472**
> *Justificación:* Esta vulnerabilidad tiene el CVSS más alto (10.0) y es conocida como Zerologon, una vulnerabilidad crítica en Active Directory.

**14. B) CVE-2020-1472 (CVSS 10.0)**
> *justificación:* Con un CVSS de 10.0, esta es la vulnerabilidad más crítica y debe priorizarse.

---

### Sección 5: Conceptos Avanzados

**15. B) Una vulnerabilidad reportada erróneamente por la herramienta**
> *Justificación:* Un falso positivo ocurre cuando la herramienta de escaneo reporta una vulnerabilidad que no existe realmente.

**16. B) Para reducir la superficie de ataque**
> *justificación:* La gestión de vulnerabilidades tiene como objetivo principal reducir la superficie de ataque corrigiendo debilidades antes de que sean explotadas.

---

> **Puntuación:**
> - 14-16 correctas: ¡Excelente! Dominas la gestión de vulnerabilidades.
> - 12-13 correctas: Bien, pero revisa los temas que fallaste.
> - Menos de 12: Repasa el módulo antes de continuar.

**➡️ Siguiente nota:** [[11 - Vulnerability Management/10 - Resumen|Resumen del Módulo 11]]
