# 05 - Vulnerabilidades

## 🎯 Objetivos
- Definir qué es una vulnerabilidad en ciberseguridad.
- Entender los diferentes tipos de vulnerabilidades (software, hardware, humanas, configuraciones).
- Comprender el concepto de "Zero-Day".

---

## 🧠 Concepto

Una **Vulnerabilidad** es una debilidad, fallo o error en un sistema, aplicación, red o proceso que puede ser explotada por una [[04 - Amenazas|Amenaza]] para comprometer la seguridad (afectando la [[03 - CIA Triad|Tríada CIA]]).

Siguiendo la analogía anterior: Si la amenaza es el ladrón, la vulnerabilidad es la ventana rota o la cerradura defectuosa.

Las vulnerabilidades no son solo "errores de código". Existen múltiples tipos:

1. **Vulnerabilidades de Software (Código):** 
   - Errores de programación que permiten hacer cosas para las que el software no fue diseñado (ej. [[09 - Web Security/10 - SQL Injection|SQL Injection]], Buffer Overflows).
2. **Vulnerabilidades de Configuración (Misconfigurations):**
   - El software está bien escrito, pero está mal configurado. (ej. Dejar la contraseña por defecto "admin/admin" en un router, o dejar un bucket de la nube público). Esta es la causa más común de brechas de datos en la nube.
3. **Vulnerabilidades Arquitectónicas / Diseño:**
   - El sistema fue diseñado sin la seguridad en mente (ej. protocolos antiguos como Telnet o FTP que envían contraseñas en texto plano).
4. **Vulnerabilidades Humanas:**
   - La falta de entrenamiento o conciencia de seguridad de los empleados que los hace susceptibles a la Ingeniería Social (ej. Phishing).

---

## 🔎 Zero-Day Vulnerability (Día Cero)

Es un término crítico en ciberseguridad. Una vulnerabilidad **Zero-Day** es un fallo de seguridad en un software que es conocido por atacantes, **pero desconocido para el fabricante del software (vendor)** o para el público en general.

- Se llama "Día Cero" porque el desarrollador ha tenido "cero días" para crear un parche que lo solucione.
- Son extremadamente peligrosas y valiosas. Cuando un atacante usa un Zero-Day, las defensas tradicionales (como antivirus) suelen no detectarlo porque no saben qué están buscando.

---

## ❓ ¿Por qué importa?

Las vulnerabilidades son el objetivo principal de los analistas de seguridad defensiva y ofensiva. 
- El **Blue Team** (Defensores) busca continuamente vulnerabilidades en sus propios sistemas para parchearlas antes de que sean explotadas (este proceso se conoce como Vulnerability Management).
- El **Red Team / Pentesters** (Atacantes) busca vulnerabilidades para demostrar que el sistema puede ser comprometido y ayudar a arreglarlo.

---

## 🌎 Aplicación en el mundo real

El caso de **Equifax (2017)**, una de las mayores agencias de crédito de EE. UU. 
Sufrieron una brecha masiva donde se robaron los datos personales de ~147 millones de personas. 
¿La causa? Una **vulnerabilidad de software** conocida en un framework web llamado Apache Struts. El parche para solucionar esa vulnerabilidad estaba disponible meses antes del ataque, pero Equifax falló en sus procesos internos (Vulnerabilidad de Proceso) y no instaló la actualización a tiempo.

---

## ❌ Errores comunes

- **"Mi software está actualizado, no tengo vulnerabilidades":** Las actualizaciones solucionan las vulnerabilidades *conocidas*. Siempre puede haber vulnerabilidades Zero-Day ocultas en el código que nadie ha descubierto aún.
- **Creer que solo el software tiene vulnerabilidades:** Como vimos, los procesos (cómo se hacen las cosas) y las personas son a menudo más vulnerables que el código en sí.

---

## 📌 Must Know (Imprescindible)
- Definición clara de Vulnerabilidad.
- El concepto de vulnerabilidad por mala configuración (Misconfiguration).
- Qué es una vulnerabilidad Zero-Day.

## 💡 Good to Know (Bueno saberlo)
- Las vulnerabilidades de software conocidas públicamente se catalogan en una base de datos mundial utilizando el sistema **CVE (Common Vulnerabilities and Exposures)**, al cual se le asigna un número (ej. CVE-2021-44228). Lo estudiaremos a fondo en el Módulo 11.

---

## 📝 Para recordar
> Un sistema puede estar lleno de vulnerabilidades y aun así no ser atacado si no hay una amenaza presente, o si las vulnerabilidades no están expuestas (ej. una vulnerabilidad en un servidor que está apagado y desconectado de la red).

---

## 🔄 Preguntas de repaso
1. Describe la diferencia entre una vulnerabilidad de software (código) y una vulnerabilidad de configuración.
2. ¿Por qué las vulnerabilidades "Zero-Day" reciben ese nombre?
3. Si un empleado anota la contraseña de acceso a los servidores en un post-it y lo pega en su monitor, ¿qué tipo de vulnerabilidad es esta?

**➡️ Siguiente nota:** [[06 - Exploits]]
