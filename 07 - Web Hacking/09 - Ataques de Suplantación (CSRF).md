# 09 - Ataques de Suplantación (CSRF)

## 🎯 Objetivos
- Comprender el abuso de la "Confianza Automática" de los navegadores web.
- Entender cómo funciona un ataque CSRF (Cross-Site Request Forgery) forzando al usuario a realizar acciones no deseadas.
- Conocer la mitigación estándar mediante Tokens Anti-CSRF.

---

## 🧠 Concepto: La Trampa de la Cookie Automática

En la [[07 - Manejo de Sesiones (Cookies vs Tokens JWT)|Nota 07]], aprendimos que la Cookie clásica soluciona la amnesia del protocolo HTTP. Cada vez que hacés una petición al banco (`www.banco.com`), tu navegador Chrome dice: *"Ah, esta petición es para el banco, voy a adjuntar automáticamente su Cookie VIP"*.

El **CSRF (Falsificación de Petición entre Sitios)** es el ataque que abusa de esta automatización de Chrome. A diferencia del XSS (que busca robar la cookie), el CSRF **no intenta robar la cookie**. Simplemente la usa sin que te des cuenta.

En CSRF, el atacante te engaña para que visites su página web maliciosa, y desde allí, obliga silenciosamente a tu navegador a ejecutar una acción destructiva (ej. Cambiar tu contraseña o transferir dinero) en el sitio donde vos estabas validado, apoyándose en la Cookie que Chrome inyectará por él.

---

## 💥 Escenario del Ataque (El Clic Destructivo)

**El Contexto Vulnerable:**
Tenés sesión iniciada en tu cuenta de Netflix (`www.netflix.com`). Tu Cookie válida está activa en tu navegador.
Descubrís que la URL en Netflix para borrar definitivamente tu cuenta es una petición sencilla: 
> `GET http://netflix.com/cuenta/borrar`

**El Ataque CSRF:**
1. El atacante construye su propia página web (`www.trampa-hacker.com/gatito.html`).
2. En el código HTML de su página web, el atacante esconde un enlace trampa, o peor aún, una imagen "invisible" (de 1x1 píxeles) que fuerza a tu navegador a visitarla automáticamente.
   > `<img src="http://netflix.com/cuenta/borrar" width="0" height="0">`
3. El atacante te envía el enlace de la página del "gatito" por correo.
4. Vos hacés clic. El navegador (Chrome) abre la página del atacante, ve la etiqueta de la imagen, e intenta "descargar" la supuesta imagen haciendo una petición hacia la URL de Netflix que está ahí programada.
5. **Acá ocurre el desastre:** Como la petición va hacia el dominio `netflix.com`, Chrome dice: *"Oh, Netflix. Espera, yo tengo la Cookie de sesión iniciada del usuario"*. Y Chrome **adjunta automáticamente la Cookie** a la petición maliciosa de fondo.
6. La petición llega al Backend de Netflix (con la orden de Borrar la Cuenta + Tu Cookie válida).
7. El Backend revisa la Cookie. Piensa que vos fuiste el que hizo clic en un botón oficial, aprueba todo, y tu cuenta es eliminada en 1 segundo sin que hayas visto absolutamente nada en pantalla.

---

## 🛡️ Defensas (Mitigación)

Hoy en día es difícil encontrar ataques CSRF tan sencillos (por peticiones GET) porque los navegadores modernos han endurecido las reglas (como el atributo `SameSite` para las cookies, que bloquea el envío automático de cookies si la orden proviene de un dominio externo como el de la trampa).

Pero la verdadera defensa técnica contra CSRF es la implementación obligatoria de **Tokens Anti-CSRF**.

### El Token Sincronizador
Es un concepto donde el Servidor Web (Backend) previene la falsificación.
1. Cuando ingresás a Netflix para ver tu perfil, el servidor te entrega la página web y además, incrusta un "Código Secreto Aleatorio de un solo uso" (El Token Anti-CSRF, ej. `1a2b3c...`) escondido adentro del HTML de la página web.
2. Si querés borrar tu cuenta o transferir dinero, la petición ahora DEBE incluir ese código secreto en el cuerpo del mensaje, además de tu Cookie.
3. El Atacante, desde su página trampa, puede forzar que tu navegador envíe la Cookie. **Pero el Atacante NO puede leer el código secreto incrustado en la página de Netflix (gracias a otra medida de seguridad llamada Same-Origin Policy)**.
4. El Atacante envía la petición trampa sin el Token. El Servidor de Netflix la rechaza, salvando el día.

---

## 📌 Must Know (Imprescindible)
- Qué es el **CSRF**: Un ataque de engaño donde el atacante usa una web controlada por él para forzar a tu navegador (que está logueado en otra web) a realizar una acción no deseada.
- Entender que CSRF es el abuso de la función fundamental de los navegadores (Adjuntar las credenciales ambientales o *Cookies* automáticamente a cualquier petición que coincida con el dominio destino).
- Conocer la mitigación técnica reina para evitarlo: Los **Tokens Anti-CSRF**.

---

## 🔄 Preguntas de repaso
1. Contrastá brevemente los objetivos finales entre un ataque XSS (Almacenado/Reflejado) y un ataque CSRF clásico. (¿En cuál el atacante busca obtener/exfiltrar los datos físicos de tus cookies para clonar tu sesión, y en cuál el atacante simplemente abusa de tu sesión en tu propia máquina sin tocar las cookies?).
2. Un desarrollador argumenta que su página web de transferencias bancarias está protegida contra el envío forzoso de ataques CSRF porque el formulario para transferir fondos no es un simple link de método GET (en una etiqueta `<img>`), sino que requiere un método HTTP POST. Explicá cómo un atacante podría igualmente realizar el CSRF incrustando un formulario POST oculto en su página trampa con un código JavaScript que ejecute el evento `submit()` de forma automática apenas la víctima entra a la web trampa.
3. El atacante diseña su página web fraudulenta (`trampa.com`) e intenta forzar a la víctima para que modifique la configuración de su cuenta en `paypal.com`. Sabiendo que `paypal.com` implementó la defensa estricta mediante *Tokens Anti-CSRF*, y debido a la regla del Same-Origin Policy del navegador (que impide a `trampa.com` leer el contenido HTML renderizado por `paypal.com`), ¿por qué la petición maliciosa formulada por el atacante rebotará instantáneamente al golpear el servidor Backend de PayPal?

**➡️ Siguiente nota:** [[10 - Falsificación del Lado del Servidor (SSRF)]]
