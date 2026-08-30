# 01 - La Industria (Red, Blue y Purple Team)

## 🎯 Objetivos
- Conocer la taxonomía de colores en Ciberseguridad.
- Entender la diferencia crítica entre un Pentest (Test de Intrusión) y una operación de Red Team.
- Descubrir el objetivo corporativo detrás del "Purple Teaming".

---

## 🧠 Concepto: Los Colores de la Seguridad

Si vas a entrar al mundo corporativo de ciberseguridad, tenés que saber exactamente a qué equipo pertenecés. 
La industria dividió las especialidades usando colores militares.

### 🔴 Red Team (El Equipo Ofensivo)
Su objetivo es **atacar** la infraestructura, los edificios físicos y a los empleados de la organización, emulando a un atacante real del mundo exterior (Hackers de Estados, Cibercriminales, Ransomware).
- El Red Team *NO* es responsable de arreglar nada. Solo rompen, demuestran que es posible, y entregan el reporte.

### 🔵 Blue Team (El Equipo Defensivo)
Su objetivo es **defender**. Son los guardianes de la empresa (Ingenieros de Seguridad, Analistas SOC, Especialistas en Respuesta a Incidentes). 
- Configuran los Firewalls, las Políticas de Grupo (GPO), instalan los EDR/Antivirus, y su tarea principal es mirar el monitor y cazar al Red Team antes de que logren su cometido.

---

## ⚔️ Pentest vs Red Team (La Gran Diferencia)

Un error gravísimo de los analistas junior es usar ambas palabras como sinónimos. 
Son servicios diametralmente opuestos en precio y alcance.

### El Pentest (Test de Intrusión)
- **Alcance:** Ruidoso y rápido (1 a 3 semanas).
- **Objetivo:** "Encuentren **TODAS** las vulnerabilidades posibles de esta página web nueva antes de que la saquemos al público".
- **El Juego:** El Blue Team (Defensores) SABE que los Pentester están atacando. Incluso les abren los puertos del Firewall para facilitarles el trabajo porque el objetivo es revisar el código, no medir las defensas.
- **Resultado:** Una lista de 20 vulnerabilidades (SQLi, XSS, etc).

### La Operación de Red Team
- **Alcance:** Silencioso, sigiloso y muy largo (De 2 a 6 meses).
- **Objetivo:** "Nuestra empresa cree que tiene una defensa impenetrable. Intenten robar la base de datos de los clientes sin que nadie del equipo defensivo se dé cuenta".
- **El Juego:** El Blue Team NO SABE que hay una operación en curso. El Red Team puede usar Hacking de red, Ingeniería Social (Phishing a empleados), clonar las tarjetas de acceso del edificio físico para entrar al Data Center de noche, o disfrazarse de mantenimiento. *Solo buscan UN (1) camino viable hacia el objetivo*. Si encuentran un camino débil por el ascensor, no revisan la página web.
- **Resultado:** Evaluar cómo responde el equipo humano de Defensa de la empresa (Capacidad de detección y respuesta de los empleados).

---

## 🟣 El Futuro: Purple Team

A veces, el ego destruye a la seguridad.
El Red Team ataca en secreto, humilla al Blue Team porque logró entrar, entrega un PDF y se va. El Blue Team se enoja, siente que su trabajo no sirvió de nada, y no aprende cómo detectar el ataque.

Para sanar esto se creó la mentalidad del **Purple Team** (La suma de Rojo y Azul).
El Purple Teaming NO es un equipo de personas distintas. Es un ejercicio de **colaboración**.
- El Red Team se sienta al lado del Blue Team (en la misma mesa).
- El Red Team le dice: *"Mirá la pantalla, voy a inyectar un Payload usando Meterpreter"*. Dispara el ataque.
- En vivo y en directo, ambos miran el monitor del Blue Team para ver si la alerta del Antivirus saltó. Si no saltó, ambos equipos trabajan juntos esa misma tarde para crear la regla de bloqueo. Si saltó, chocan los cinco y prueban otro ataque más difícil.
- Es el escenario corporativo ideal (Seguridad Colaborativa).

---

## 📌 Must Know (Imprescindible)
- **Red Team:** Ofensiva, simulación de atacantes reales y pruebas de la capacidad de respuesta de la empresa.
- **Blue Team:** Defensa, SOC, prevención, detección y respuesta a incidentes.
- **Pentest vs Red Team:** El Pentest busca cantidad de bugs técnicos. El Red Team busca evaluar si las personas y procesos de la empresa detectan una amenaza silenciosa prolongada a cualquier costo.
- **Purple Team:** El Red y el Blue trabajan juntos, abiertamente, para calibrar las defensas en tiempo real.

---

## 🔄 Preguntas de repaso
1. Una corporación de e-commerce acaba de desarrollar su nueva pasarela de pagos. Para cumplir con la normativa internacional de tarjetas de crédito (PCI-DSS), necesitan que una empresa externa confirme que el código de la pasarela no tiene vulnerabilidades de inyección, y les dan 10 días para realizarlo. ¿Deben contratar un servicio de Pentesting Web o una Campaña de Red Teaming? Justificá.
2. Si un equipo es contratado para realizar una simulación ofensiva y logran vulnerar la red de una empresa conectando un "Raspberry Pi" malicioso debajo del escritorio de la secretaria del gerente de finanzas, tras convencer al guardia de seguridad de que eran los reparadores del aire acondicionado, ¿qué tipo de equipo (por color de industria) está llevando a cabo esta operación?
3. Sabiendo el costo altísimo y el tiempo que requiere una campaña de Red Team silenciosa y ciega para los defensores, ¿por qué la tendencia actual de muchas empresas maduras en ciberseguridad se está volcando a realizar ejercicios o consultorías de tipo "Purple Team" para maximizar su presupuesto?

**➡️ Siguiente nota:** [[02 - OSINT I (Inteligencia de Fuentes Abiertas)]]
