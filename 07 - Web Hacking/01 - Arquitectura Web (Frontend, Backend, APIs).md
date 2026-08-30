# 01 - Arquitectura Web (Frontend, Backend, APIs)

## 🎯 Objetivos
- Conocer la separación física y lógica de las páginas web modernas.
- Entender de qué están compuestos el Frontend y el Backend.
- Aprender qué es una API (REST) y por qué los hackers modernos se enfocan en atacarlas a ellas y no a las páginas web visuales.

---

## 🧠 Concepto: La Web Moderna no es un Documento

Hace 20 años, si querías ver noticias, tu navegador le pedía a un servidor en EE. UU. el archivo "noticias.html". El servidor te mandaba el texto gigante con colores, lo leías, y fin de la historia. Eran "Webs estáticas".

Hoy, cuando entrás a Instagram, la página no es estática; la página "piensa", reacciona a tus clics, y se actualiza en tiempo real sin recargar la pestaña. 
Para lograr eso, la industria dividió el desarrollo en dos universos completamente separados, que interactúan constantemente a través de Internet.

---

## 🎨 1. El Frontend (El Cliente / Lo Visual)

Es todo lo que ocurre **delante del telón**. Es el código que corre literalmente en tu propia computadora, adentro de la Memoria RAM de tu navegador (Google Chrome, Firefox).

- **HTML:** Es el esqueleto. Define dónde va el botón, dónde va la imagen, y el texto.
- **CSS:** Es la pintura. Le da color rojo al botón y redondea las imágenes.
- **JavaScript (JS):** Es el cerebro local. Es el lenguaje de programación que corre en tu navegador y detecta cuando le haces clic al botón, lanzando animaciones o enviando mensajes al servidor.

> **Importante para Hacking:** Como el Frontend se ejecuta en *tu* máquina, **¡vos tenés control absoluto sobre él!** Si el programador cometió el error de poner en el código JavaScript: `precio_producto = $100`, un atacante puede abrir la consola de su propio Chrome, cambiar el precio a `$1` e intentar comprarlo. (Por eso se dice: *Nunca confíes en el Frontend*).

---

## ⚙️ 2. El Backend (El Servidor / La Verdad)

Es lo que ocurre **detrás del telón**. Son computadoras masivas (servidores en AWS, Linux) alojadas a miles de kilómetros. El usuario normal nunca ve el código del Backend.

- **El Servidor Web / Aplicación:** Escrito en lenguajes como Python, Java, PHP o NodeJS. Tiene la inteligencia de la empresa. Evalúa si tu contraseña es correcta, y calcula si tenés saldo en el banco.
- **La Base de Datos:** (SQL, MongoDB). El disco duro sagrado donde se guarda todo (Usuarios, saldos, contraseñas, fotos). El Servidor Web es el único que puede hablar con la Base de Datos.

> **Importante para Hacking:** El Backend es la fuente de la verdad absoluta. Si el Frontend intentó cambiar el precio a $1, un buen Backend debe revisar en su propia base de datos, decir *"Falso, esto vale $100"* y rechazar la compra. Los peores hackeos ocurren cuando el Backend es perezoso y confía ciegamente en lo que le manda el Frontend.

---

## 🔌 3. APIs (Application Programming Interface)

Si el Frontend vive en tu PC y el Backend vive en EE. UU., ¿cómo se comunican?
Hablan a través de una **API**.

Una API (comúnmente la arquitectura **REST API**) es el "mesero" del restaurante.
1. Tu Frontend (JavaScript en Chrome) le dice al Mesero (API): *"Che, traeme los últimos 5 tweets del usuario Carlos"*.
2. El Mesero (API) viaja por Internet, entra a la cocina (Backend), saca los tweets de la heladera (Base de Datos).
3. El Mesero (API) te trae los datos de vuelta. 
**Dato Vital:** La API no te devuelve una página web pintada con colores (HTML). La API te devuelve un texto crudo en formato **JSON** (Visto en el [[04 - Python/15 - Librerías Core II (requests, socket, json)|Módulo de Python]]). Tu Frontend recibe ese texto de computadora, y se encarga él mismo de pintar los colores bonitos en la pantalla.

### El Hacking de APIs (El nuevo oro)
Hoy en día, las aplicaciones móviles (Android/iOS) y las páginas web (React) consumen *la misma exacta API* del Backend.
El Red Team y el Bug Bounty ya casi no buscan errores visuales. Ellos conectan sus herramientas de ataque directamente para hablar crudo con la API (el mesero), inyectando formato JSON corrupto para intentar que la base de datos se equivoque.

---

## 📌 Must Know (Imprescindible)
- **Frontend:** Código que corre en tu propia máquina (Chrome). Es manipulable. Nunca se debe confiar en las validaciones que hace el frontend.
- **Backend:** Código que corre en el servidor de la empresa. Es la fuente de la verdad.
- **API (REST):** El canal de comunicación. Intercambia datos puros (usualmente en formato **JSON**) sin enviar código visual (HTML/CSS).

---

## 🔄 Preguntas de repaso
1. Una tienda en línea tiene un formulario de registro. Cuando intentás poner "123" como contraseña, una letra roja aparece instantáneamente diciendo "Muy corta", sin que la página web recargue ni por un segundo. ¿Esta validación la realizó el Frontend (JavaScript en tu PC) o el Backend (Python en el servidor)?
2. En base a tu respuesta de la pregunta anterior, si un atacante utiliza una herramienta de interceptación (Proxy) para evadir las reglas de tu navegador y envía a la fuerza la contraseña "123" por Internet, ¿por qué el sistema colapsará o aceptará la contraseña si el programador de la empresa no configuró validaciones idénticas en el Backend?
3. Las aplicaciones nativas de un iPhone no están escritas en HTML/CSS, sino en lenguajes de Apple (Swift). Sin embargo, cuando abrís la app de Twitter en tu iPhone, podés ver los mismos tweets que ves en tu PC. Explicá cómo la arquitectura de las **APIs (basadas en texto JSON)** permite que plataformas tan diferentes muestren los mismos datos provenientes del mismo Backend.

**➡️ Siguiente nota:** [[02 - El Protocolo HTTP (Peticiones, Respuestas, Headers)]]
