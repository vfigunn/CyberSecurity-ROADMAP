# 11 - Actores de Amenaza (Threat Actors)

## 🎯 Objetivos
- Conocer quiénes son los atacantes detrás de las [[04 - Amenazas|Amenazas]].
- Clasificar a los atacantes según su motivación y nivel de habilidad.
- Comprender el panorama actual del cibercrimen.

---

## 🧠 Concepto

Un **Actor de Amenaza (Threat Actor)**, a menudo llamado simplemente "atacante", es cualquier individuo o grupo que tiene la intención y la capacidad de llevar a cabo una actividad maliciosa contra sistemas de información, redes o dispositivos.

Para defenderse eficazmente, no basta con saber *cómo* atacan (los [[06 - Exploits|Exploits]]); es crucial saber *quién* ataca y *por qué*. Conocer la motivación del atacante ayuda a predecir qué buscarán en tu red.

---

## 🕵️‍♂️ Tipos de Actores de Amenaza

Se clasifican tradicionalmente en varias categorías, ordenadas generalmente desde menor a mayor sofisticación:

### 1. Script Kiddies
- **Descripción:** Atacantes novatos con muy poca o nula habilidad técnica propia.
- **Técnicas:** Usan herramientas, scripts y exploits creados por otros (a menudo descargados de internet) sin entender realmente cómo funcionan por debajo.
- **Motivación:** Emoción, curiosidad, presumir ante sus amigos o causar caos menor.
- **Peligrosidad:** Aunque son inexpertos, las herramientas que usan son poderosas, por lo que pueden causar un daño real (especialmente ataques de denegación de servicio o *defacement* de páginas web).

### 2. Hacktivistas
- **Descripción:** Individuos o grupos unidos por una ideología. (Ejemplo clásico: Anonymous).
- **Técnicas:** DDoS (Distributed Denial of Service), doxing (publicar información privada de personas), robo y filtración masiva de datos (Data Leaks).
- **Motivación:** Política, social o religiosa. Su objetivo es enviar un mensaje, avergonzar a una organización (daño reputacional) o forzar un cambio, no el dinero.

### 3. Amenazas Internas (Insider Threats)
- **Descripción:** Personas con acceso legítimo al sistema (empleados, ex-empleados, contratistas). Son extremadamente peligrosos porque ya están "dentro del castillo" y conocen las defensas.
- **Motivación:** Venganza (empleado despedido), ganancia financiera (vender datos a un competidor), o simplemente negligencia (Insider accidental que hace clic donde no debe).

### 4. Cibercriminales (Crimen Organizado)
- **Descripción:** Mafias digitales. Grupos altamente estructurados que operan como empresas multinacionales. Tienen equipos de desarrollo, servicio al cliente y equipos de lavado de dinero.
- **Técnicas:** Ransomware, robo de credenciales masivo, fraude financiero, extorsión. Tienen el modelo de *Cybercrime as a Service* (venden herramientas de ataque a otros).
- **Motivación:** Principalmente **Dinero**.

### 5. Estados-Nación (Nation-State Actors / APTs)
- **Descripción:** Grupos de hackers militares o de inteligencia patrocinados por el gobierno (Ej: Fancy Bear de Rusia, Lazarus de Corea del Norte, Equation Group de EE. UU.).
- **Técnicas:** Altamente sofisticadas. Tienen acceso a vulnerabilidades *Zero-Day*, capacidad de crear malware indetectable y recursos casi ilimitados. Operan lentamente y en silencio (Amenaza Persistente Avanzada - APT).
- **Motivación:** Espionaje geopolítico, sabotaje de infraestructura crítica, robo de propiedad intelectual (secretos industriales) y financiamiento del estado.

---

## ❓ ¿Por qué importa?

La motivación del atacante define tu perfil de [[07 - Riesgo|Riesgo]].
- Si sos una ONG de derechos humanos, tu principal amenaza pueden ser los Estados-Nación o los Hacktivistas (buscando censurarte o exponerte).
- Si sos un Banco, tu principal amenaza es el Crimen Organizado buscando dinero, o el Insider Threat buscando vender datos.
- Si sos un estudio pequeño de abogados, tu amenaza probablemente sea el Ransomware no dirigido lanzado por Cibercriminales de bajo nivel.

Tus [[08 - Controles de Seguridad|Controles de Seguridad]] deben alinearse contra tu Actor de Amenaza más probable.

---

## 🌎 El concepto de "As a Service" en el Cibercrimen

El mayor cambio en la última década es la mercantilización de los ciberataques. Hoy en día, un cibercriminal no necesita saber programar para lanzar un ataque de Ransomware.

Existe el **Ransomware-as-a-Service (RaaS)**: Grupos expertos programan el ransomware y se lo "alquilan" a afiliados (que a veces son Script Kiddies). El afiliado se encarga de infectar a la víctima, y si la víctima paga, los desarrolladores originales se quedan con un % del rescate. Esto multiplicó exponencialmente la cantidad de ataques.

---

## ❌ Errores comunes

- **Atribuir cada ataque a un Estado-Nación:** Es común en las noticias decir que "hackers rusos o chinos" atacaron una empresa. La atribución (saber quién fue exactamente) en ciberseguridad es extremadamente difícil. A menudo, el cibercrimen usa las herramientas que filtran los estados para confundir a los investigadores (Falsas Banderas).
- **Subestimar a los Script Kiddies:** Pensar que porque no saben programar no son un riesgo. Las herramientas automáticas hoy en día pueden destruir una base de datos en segundos.

---

## 📌 Must Know (Imprescindible)
- Las cinco categorías principales de Threat Actors.
- Diferenciar la motivación de un Hacktivista frente a un Cibercriminal.
- Entender por qué un Insider Threat es tan difícil de defender.

## 💡 Good to Know (Bueno saberlo)
- La comunidad de seguridad (como CrowdStrike o Mandiant) suele poner nombres en código a los actores de amenazas de Estados-Nación para rastrearlos. Por ejemplo, CrowdStrike usa animales: Rusia = Bear, China = Panda, Irán = Kitten, Corea del Norte = Chollima, Crimen organizado = Spider.

---

## 📝 Para recordar
> No podés protegerte contra "los hackers" en general. Tenés que modelar tus defensas pensando: "¿Quién tiene la motivación y los recursos para atacarme a mí específicamente?".

---

## 🔄 Preguntas de repaso
1. Si un grupo ataca los servidores de una empresa petrolera para robar secretos de ingeniería y dárselos al gobierno de otro país, ¿en qué categoría clasificarías al atacante?
2. ¿Por qué el modelo "Ransomware as a Service" (RaaS) hizo que los ataques aumentaran tan drásticamente?
3. Da un ejemplo de un ataque realizado por un "Insider Threat" que esté motivado por el dinero.

**➡️ Siguiente nota:** [[12 - MITRE ATT&CK Introducción]]
