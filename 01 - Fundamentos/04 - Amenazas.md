# 04 - Amenazas (Threats)

## 🎯 Objetivos
- Definir qué es una amenaza en el contexto de ciberseguridad.
- Clasificar los diferentes tipos de amenazas (intencionales, accidentales, ambientales).
- Entender el concepto de "Amenaza Persistente Avanzada" (APT).

---

## 🧠 Concepto

Una **Amenaza (Threat)** es cualquier evento, circunstancia o actor potencial que puede explotar una debilidad para causar daño a un activo (información, sistemas, personas).

En palabras simples: **Es "lo malo" que puede pasar.**

Para que una amenaza sea relevante, debe tener la capacidad y la intención (en caso de amenazas humanas) de causar un impacto negativo en la [[03 - CIA Triad|Tríada CIA]].

### Clasificación de Amenazas

No todas las amenazas son hackers encapuchados en una habitación oscura. Se clasifican generalmente en tres categorías:

1. **Intencionales (Maliciosas):** 
   - Hackers externos, cibercriminales buscando dinero.
   - Empleados descontentos (Insider Threat) que roban o destruyen datos a propósito.
   - Espionaje corporativo o ataques de estados-nación.
   
2. **Accidentales (No Maliciosas):**
   - Un empleado que borra accidentalmente una base de datos crítica.
   - Un administrador de red que configura mal un [[07 - Network Security/02 - Firewalls|Firewall]] dejando la red expuesta.
   - Un desarrollador que sube contraseñas a un repositorio público de GitHub por error.

3. **Ambientales / Físicas:**
   - Un incendio en el centro de datos.
   - Un corte de energía prolongado.
   - Inundaciones, terremotos o fallos catastróficos de hardware.

---

## ❓ ¿Por qué importa?

No podemos defender un sistema si no sabemos de qué nos estamos defendiendo. El proceso de identificar qué puede salir mal se conoce como **Modelado de Amenazas (Threat Modeling)**. 

Si tu empresa está en una zona sin terremotos, un terremoto es una amenaza con probabilidad nula (riesgo bajo). Si sos un banco, el robo financiero es una amenaza inminente.

---

## 🔎 El concepto de APT

Una **APT (Advanced Persistent Threat - Amenaza Persistente Avanzada)** es un tipo de amenaza específica, generalmente un grupo altamente organizado y financiado (a menudo por un país o estado).

- **Avanzada:** Usan técnicas muy sofisticadas, desarrollando su propio malware o explotando vulnerabilidades que nadie más conoce (Zero-Days).
- **Persistente:** No buscan un "hackeo rápido". Buscan entrar, ocultarse y permanecer en la red durante meses o años robando información lentamente sin ser detectados.
- **Amenaza:** Tienen un objetivo claro y los recursos para conseguirlo.

---

## ❌ Errores comunes

- **Confundir Amenaza con Vulnerabilidad:** 
  - La *Amenaza* es el ladrón.
  - La *Vulnerabilidad* es la puerta que dejaste sin llave.
  - El *Riesgo* es la probabilidad de que el ladrón pase por esa puerta y robe tus cosas.
- **Ignorar las amenazas internas (Insider Threats):** Las empresas suelen gastar millones en proteger su perímetro exterior (internet), pero ignoran que un empleado con permisos legítimos puede hacer el mismo o más daño (ya sea accidental o intencionalmente).

---

## 📌 Must Know (Imprescindible)
- Definición de Amenaza.
- Diferencia entre amenazas intencionales y accidentales.
- El concepto de Insider Threat.

## 💡 Good to Know (Bueno saberlo)
- El Modelado de Amenazas es el proceso sistemático de identificar amenazas y vulnerabilidades potenciales antes de que el sistema se construya. Frameworks famosos para esto son STRIDE (desarrollado por Microsoft).

---

## 📝 Para recordar
> Un error humano (como borrar un archivo crítico o caer en un correo de phishing) es estadísticamente una de las amenazas más comunes y peligrosas para cualquier organización, mucho más que los "hackers de película".

---

## 🔄 Preguntas de repaso
1. Clasifica las siguientes amenazas como Intencional, Accidental o Ambiental:
   - a) Un huracán destruye el edificio de servidores.
   - b) Un empleado envía un archivo confidencial a un competidor a cambio de dinero.
   - c) Un analista actualiza un software y accidentalmente causa que el sistema colapse.
2. Explica qué significa que una amenaza sea "Persistente" en el término APT.
3. ¿Por qué las amenazas internas suelen ser más difíciles de detectar que las externas?

**➡️ Siguiente nota:** [[05 - Vulnerabilidades]]
