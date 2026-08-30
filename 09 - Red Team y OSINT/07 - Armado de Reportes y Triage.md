# 07 - Armado de Reportes y Triage

## 🎯 Objetivos
- Comprender que el producto final del Hacking es puramente un documento de Word (El Reporte).
- Entender el concepto de Triage y cómo evaluar los hallazgos mediante CVSS.
- Aprender la estructura corporativa de un Informe Ejecutivo.

---

## 🧠 Concepto: Tu código no importa, importa tu documento

Los mejores Pentesters y Red Teamers del mundo fracasan comercialmente si no saben escribir un reporte.
A la empresa (El cliente) que te contrató por $15,000 dólares, no le importa qué genial fue tu inyección de Python o lo difícil que estuvo saltar el Firewall. 
El cliente espera recibir un **Reporte Ejecutivo y Técnico** que le explique el riesgo real y cómo solucionarlo. Ese documento PDF es el único producto tangible que justifica su inversión.

---

## ⚖️ El Sistema CVSS (Catalogando la Gravedad)

Cuando encontrás una vulnerabilidad (Ej. un XSS Reflejado en una página), no podés simplemente ponerle "Gravedad Alta" basado en tu instinto. Tenés que usar matemática global.

La industria utiliza el **CVSS (Common Vulnerability Scoring System)**. Es una calculadora (del 0.0 al 10.0) que evalúa tu vulnerabilidad sumando diferentes vectores de severidad (Métricas):

1. **Vector de Acceso:** ¿El hacker tiene que estar físicamente en la oficina, o puede hackear la empresa remotamente desde Rusia? (La red remota da más puntos de gravedad).
2. **Complejidad del Ataque:** ¿Es fácil de realizar apretando un botón, o requiere 5 horas de ingeniería?
3. **Privilegios Requeridos:** ¿El hacker necesita iniciar sesión con una cuenta válida del sistema (Baja), o el bug funciona aunque el hacker sea un visitante anónimo de la calle sin cuenta (Alta/Crítica)?
4. **Impacto C-I-A:** ¿Qué ocurre tras el hackeo? ¿Destroza la **C**onfidencialidad (roba datos), afecta la **I**ntegridad (modifica salarios), u obstruye la **D**isponibilidad/Availability (Tira el servidor abajo)?

> **Niveles Oficiales CVSS:** Bajo (0.1 - 3.9), Medio (4.0 - 6.9), Alto (7.0 - 8.9), y Crítico (9.0 - 10.0).
> *(Vulnerabilidades como EternalBlue/Log4j siempre son un 10.0).*

---

## 📑 Estructura del Reporte de Pentesting

Un buen reporte siempre consta de 2 partes fundamentales.

### 1. Resumen Ejecutivo (Para el Dueño/CEO)
El CEO no entiende qué es una "Reverse Shell" y no le interesa leerlo.
El Resumen Ejecutivo tiene un máximo de 2 páginas. Su objetivo es transmitir el **Riesgo del Negocio**.
- *"Logramos acceder a la red financiera. El impacto para el negocio es crítico: los atacantes podrían robar datos de tarjetas de crédito. Identificamos 3 áreas débiles principales (Falta de MFA, Software Antiguo, y Empleados sin entrenamiento). Recomendamos un presupuesto de inversión a 3 meses para parchar estos sectores"*.

### 2. Resumen Técnico (Para los Ingenieros)
Acá volcás toda tu magia (El Hacking técnico).
Cada hallazgo o vulnerabilidad que encontraste (el Triage) debe listarse con este formato estricto:
- **Título de la Vulnerabilidad y Puntaje CVSS** (Ej: XSS Reflejado - CVSS 6.1 Medio).
- **Descripción:** Qué es el error.
- **Evidencia (Proof of Concept - PoC):** Capturas de pantalla con los comandos. Tenés que permitirle a los desarrolladores del cliente *replicar y volver a lanzar* tu ataque exacto en su consola para que ellos comprueben que es real.
- **Remediación:** Tu tarea principal. Si rompés algo, le tenés que explicar exactamente al equipo de desarrolladores (Blue Team) qué línea de código en su sistema tienen que modificar o parchear para que ya no pueda volver a ocurrir. (El Red Team entrena al Blue Team).

---

## 📌 Must Know (Imprescindible)
- **CVSS:** Es la calculadora matemática (del 0 al 10) estándar de la industria utilizada para remover la "opinión humana" y catalogar de forma metódica y fría si una vulnerabilidad es de Riesgo Bajo, Medio, Alto o Crítico.
- Todo reporte profesional tiene **2 públicos**. El público **Ejecutivo (No técnico)**, al cual se le habla de riesgo financiero y operativo; y el público **Técnico (Ingenieros)** al que se le entregan las capturas de pantalla, código malicioso utilizado y formas de reparación o mitigación (Remediation).
- La **Evidencia / PoC (Proof of Concept)** es el pilar del analista de seguridad. Si encontrás una falla pero el cliente no es capaz de replicarla leyendo tus pasos, entonces para la empresa la falla "no existe" y no se puede reparar.

---

## 🔄 Preguntas de repaso
1. Al momento de entregar el documento final del Pentest a la compañía, descubrís que estructuraste todo tu hallazgo crítico utilizando exclusivamente lenguaje binario, explicaciones de ensamblador (ASM), y códigos de exploits, incluyéndolos en la primera página que leerá el Director General (CEO) de la empresa. Basándote en la estructura del "Reporte a 2 Públicos", ¿por qué esta entrega será considerada un rotundo fracaso a nivel consultoría, y cómo deberías re-diseñar el inicio (Resumen Ejecutivo)?
2. Descubrís un error gravísimo que te permite descargar archivos ocultos de la web. Sin embargo, al pasarlo por la calculadora **CVSS** (Vector de Métricas Comunes), el sistema te da un puntaje final de 6.5 (Riesgo Medio), porque el ataque exige un "Privilegio de Acceso Alto" (El hacker debe tener la clave de un Administrador logueado en la empresa para que funcione). Si descubrieras una forma de ejecutar este ataque siendo un usuario "Invitado Anónimo" sin cuenta, ¿qué le sucederá matemáticamente al Puntaje CVSS y a la severidad catalogada?
3. Para la fase del Reporte Técnico dirigido a los programadores del Blue Team, no basta con decirles "Encontré un XSS y les inyecté JavaScript". Tenés que documentar y adjuntar los pasos exactos, capturas de pantalla y el link armado que genera la vulnerabilidad. En el argot de ciberseguridad, ¿cómo se le llama a esa demostración documentada que asegura la repetitividad técnica de tu hazaña (Sus siglas son PoC)?

**➡️ Siguiente nota:** [[08 - Laboratorio Teórico - Campaña de Red Team Completa]]
