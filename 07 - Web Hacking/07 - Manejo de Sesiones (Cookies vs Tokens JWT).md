# 07 - Manejo de Sesiones (Cookies vs Tokens JWT)

## 🎯 Objetivos
- Entender el concepto universal del "Estado" y por qué la Web sufre de amnesia.
- Comprender las Cookies y las Sesiones clásicas (El candado del servidor).
- Aprender qué son los Tokens JWT y cómo cambiaron la arquitectura de las APIs.

---

## 🧠 Concepto: La Web tiene Amnesia

En la nota del [[02 - El Protocolo HTTP (Peticiones, Respuestas, Headers)|Protocolo HTTP]], dijimos que HTTP funciona a base de *"Pregunta -> Respuesta -> Fin de la comunicación"*.
El diseño arquitectónico de Internet dictamina que HTTP es **Stateless (Sin Estado)**.

Esto significa que el servidor no tiene memoria (sufre de amnesia grave). 
1. Le decís: *"Soy Juan, esta es mi contraseña"*. El servidor dice: *"Perfecto, sos Juan, estás logueado"*. Y corta.
2. Al segundo siguiente, le hacés otra petición (click): *"Mostrame mi perfil"*.
3. El servidor te responde: *"¿Quién sos? ¿Podés volver a poner tu contraseña?"*. (¡Se olvidó de que te habías logueado hace un segundo!).

Como los usuarios no van a tipear su contraseña en cada clic que hacen en la pantalla, la industria inventó las **Sesiones**.

---

## 🍪 Las Sesiones y las Cookies (La forma clásica)

Para curar la amnesia, la aplicación le da al usuario una pulsera VIP (un identificador).

**El Flujo:**
1. Iniciás sesión con tu clave. El Backend verifica tu clave en la base de datos y dice: *"Perfecto"*.
2. El Backend genera una cadena de texto aleatoria kilométrica: `Session_ID: x9z8...`. Y la guarda en su propia Memoria RAM.
3. El Backend te responde la página web y te envía un Ticket. En el Header HTTP de respuesta, le ordena a tu navegador (Chrome): `Set-Cookie: session=x9z8...`.
4. Chrome obedece. Guarda temporalmente esa "Galletita" (Cookie) en tu disco local.
5. De ahora en más, cada vez que hagas clic en cualquier botón de la página, **Chrome enviará automáticamente esa Cookie adjunta a la petición**. 
6. El servidor la recibe, busca el ID en su memoria, te reconoce y te deja pasar sin pedir clave.

### El Riesgo Defensivo:
Como la Cookie viaja automática, si un atacante te roba la Cookie mediante un ataque XSS, o la intercepta en un Wi-Fi sin HTTPS (Session Hijacking), **el atacante la inserta en su propio navegador y clona exactamente tu sesión**. Al servidor no le importa desde qué PC viene; si tenés el `Session_ID` correcto, te considera el dueño.

---

## 🎟️ JSON Web Tokens (JWT) (La forma moderna / APIs)

El método de Cookies clásico tiene un problema enorme para Netflix o Facebook: Si 100 millones de usuarios están conectados, el Servidor tiene que guardar en su Memoria RAM los 100 millones de identificadores `Session_ID`. Eso es carísimo y colapsa los servidores.

Para las APIs modernas (Móviles / React), se inventó el **JWT (Token de Web JSON)**.

**El Flujo del Token JWT:**
1. Iniciás sesión con tu clave.
2. El Backend te valida. Pero **no guarda nada en su memoria RAM**.
3. El Backend fabrica un Token JWT. Es un texto gigantesco dividido en 3 partes por puntos (`Header.Cuerpo.Firma`).
4. Adentro del cuerpo del Token, el Backend escribe literalmente tus permisos: `{"usuario": "juan", "rol": "admin"}`.
5. Para que el usuario no pueda alterar y hacer trampa con el contenido de este Token, **el Servidor lo Firma Digitalmente con su Llave Privada** (¿Te acordás de la [[05 - Criptografía/09 - Firmas Digitales (Integridad y No Repudio)|Firma Digital en Criptografía]]?).
6. El Backend te envía el Token. Tu aplicación móvil (o frontend) lo guarda localmente.

**La Magia del Ahorro (Stateless Real):**
Cada vez que intentás entrar a un área, le enviás el Token al Servidor.
El Servidor NO necesita buscar el Token en ninguna base de datos ni memoria (sigue con amnesia). Simplemente agarra el Token, mira la Firma Digital, la desencripta con su llave, y si la matemática cuadra, el Servidor sabe con absoluta certeza que él mismo expidió ese Token (Autoría / Integridad). 
Lee los datos de adentro (`rol: admin`) y te deja pasar.

---

## 📌 Must Know (Imprescindible)
- El protocolo **HTTP es Stateless** (sin estado/sin memoria). Las sesiones se inventaron para persistir la identidad del usuario a través de los múltiples clics.
- Las **Cookies** clásicas son identificadores aleatorios. Todo el "estado real" (los permisos del usuario) lo guarda el Servidor Web en su memoria.
- Los **Tokens JWT** modernas son Auto-contenidas. El servidor no guarda nada. El Token tiene los datos del usuario adentro, y está protegido matemáticamente contra manipulaciones mediante una Firma Digital generada por el Backend.

---

## 🔄 Preguntas de repaso
1. Una aplicación bancaria desarrollada con tecnología clásica utiliza una Cookie de Sesión (`Session_ID`) para autenticar los clics de sus usuarios. Si un administrador del Banco reinicia el servidor Backend (limpiando así su memoria RAM completamente), ¿qué le sucederá instantáneamente a todos los usuarios que estaban logueados y navegando por la página en ese exacto segundo, independientemente de que los usuarios aún tengan su Cookie en sus computadoras?
2. Un desarrollador migra su aplicación de sesión basada en Cookies a la arquitectura de autenticación JWT para ahorrar recursos (stateless). El JWT que se le entrega al usuario (Frontend) contiene en texto claro (codificado en Base64) su rol: `{"role": "user"}`. Si el usuario intenta hackear su propia sesión editando ese JWT y cambiando la palabra "user" por "admin" antes de enviárselo de vuelta al Servidor, ¿qué parte del JWT (Header, Body o Signature/Firma) fallará rotundamente y le revelará al servidor que el archivo fue manipulado?
3. Compará lógicamente cómo un Servidor verifica quién sos cuando le presentas una Cookie tradicional vs cuando le presentas un JWT moderno, centrándote en qué necesita hacer el Servidor en su propia base de datos/memoria.

**➡️ Siguiente nota:** [[08 - Control de Acceso Roto (IDOR - BOLA)]]
