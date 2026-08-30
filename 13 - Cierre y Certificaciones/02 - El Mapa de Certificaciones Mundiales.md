# 02 - El Mapa de Certificaciones Mundiales

## 🎯 Objetivos
- Entender el filtro de Recursos Humanos (HR).
- Diferenciar certificaciones Teóricas (Multiple Choice) vs Prácticas (Laboratorio).
- Conocer la jerarquía de títulos de la industria.

---

## 🧠 Concepto: Pasando el Filtro

Si mandás tu Currículum diciendo *"Sé hackear Active Directory porque leí unas notas"*, el reclutador de Recursos Humanos lo tirará a la basura. Ellos no saben qué es Active Directory, ellos buscan siglas en tu CV.

Tu objetivo principal es conseguir una **Certificación**. Es un aval internacional de una corporación masiva diciendo que vos aprobaste sus exámenes.

---

## 📜 1. Certificaciones Iniciales / Teóricas (Para CV)

Estas certificaciones demuestran que conocés los conceptos (Ej. Sabés qué es WPA2, SQLi y Phishing). Son exámenes de opción múltiple.
- **CompTIA Security+:** Es la base de pirámide mundial. Extremadamente respetada para puestos de Blue Team, SOC (Centro de Operaciones) o Analista Junior. Es un examen teórico denso sobre criptografía y normativas. (Muy recomendada).
- **CEH (Certified Ethical Hacker) de EC-Council:** Es hiperfamosa, pero la comunidad técnica la menosprecia porque es "solo teoría". Sin embargo, **a Recursos Humanos le fascina**. Muchas ofertas de empleo dicen "Se requiere CEH".

---

## ⚔️ 2. Certificaciones Prácticas (La Verdadera Habilidad)

Estas certificaciones NO son opción múltiple. Te encierran en un laboratorio virtual durante 24 horas y te dicen: *"Hay 5 máquinas. Hackeá al menos 4 de ellas y escribí un Reporte Profesional demostrando cómo lo hiciste. Si no entregás el reporte mañana a la tarde, reprobás"*.

- **eJPT (eLearnSecurity Junior Penetration Tester):**
  - **Nivel:** Principiante / Intermedio.
  - Es el primer gran salto. El examen es 100% práctico. Tenés que usar Metasploit, Nmap, y hacer escaladas locales. Super recomendable para validar que podés usar la consola en el mundo real.

- **PNPT (Practical Network Penetration Tester) de TCM Security:**
  - **Nivel:** Intermedio (Enfocado puramente en Active Directory).
  - La certificación más moderna y querida por la comunidad. Te simulan un entorno corporativo de 5 días. Tenés que abusar de Kerberoasting, hacer Pass-The-Hash, comprometer el DC, escribir el Reporte y... ¡Darle una presentación oral formal en videollamada a los creadores explicándoles por qué los hackeaste! (Igual que en la vida real).

---

## 👑 3. El Santo Grial: OSCP

- **OSCP (Offensive Security Certified Professional):**
  - **Nivel:** Experto.
  - Dictada por *OffSec* (Los mismos creadores de Kali Linux). Es el pasaporte dorado. Si tenés el OSCP en tu currículum, te llueven las ofertas de trabajo a nivel global de forma automática.
  - **El Examen:** El examen de 24 horas más infame y brutal del mundo. Te exigen vulnerar el Active Directory usando exploits manuales y evadiendo defensas, y redactar un informe de 40 páginas. No podés usar el modo automático de Metasploit ni herramientas mágicas; tenés que saber ensamblar todo a mano.
  - **Recomendación:** Es tu meta a 2 años vista. Nadie saca el OSCP en un mes.

---

## 📌 Must Know (Imprescindible)
- **Filtro de RRHH:** Si apuntás al ámbito defensivo, gubernamental o auditorías normativas; el **CompTIA Security+** o **CEH** son las llaves para pasar los filtros de papel.
- **Filtro Técnico (Red Team):** Si apuntás a ser Hacker Corporativo o Pentester; el mundo real exige el **eJPT / PNPT**, con el objetivo final e indiscutido de coronarte algún día con la certificación **OSCP**.

**➡️ Siguiente nota:** [[03 - Creando tu Portafolio y Marca Personal]]
