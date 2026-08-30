# 13 - Ejercicios del Módulo 07

## 📝 Instrucciones
Poné a prueba tu comprensión de la arquitectura web y la interacción Frontend-Backend. Muchos de estos ejercicios reflejan preguntas reales de entrevistas técnicas para roles de AppSec (Seguridad de Aplicaciones).

---

## 🧠 Ejercicios de Lógica y Análisis (Web Hacking)

1. **La Verdad del Precio:**
   - Una página de E-Commerce carga el precio de una bicicleta ($500) enviándolo en un JSON oculto al Frontend para que JavaScript lo dibuje. El atacante intercepta la petición, modifica el precio a $1 en el JSON que entra a su navegador, y el navegador efectivamente pinta la bicicleta a $1. Luego le da a "Comprar".
   - Al finalizar el flujo, la tarjeta del atacante le cobró igualmente $500, sin importar lo que decía la pantalla. Explicá desde el concepto de Arquitectura (Frontend vs Backend) por qué falló su engaño.

2. **Diferenciando el Veneno:**
   - Si inyectás el código `'; DROP TABLE usuarios; --` en una caja de búsqueda, tu objetivo es atacar al **X**.
   - Si inyectás el código `<script> document.cookie </script>` en una caja de búsqueda, tu objetivo es atacar al **Y**.
   - Rellená X e Y detallando si estás atacando al Cliente (Navegador) o al Servidor (Base de Datos), y qué vulnerabilidad estás ejecutando en cada caso.

3. **El Misterio del Estado (Stateless):**
   - El protocolo HTTP, por naturaleza pura, padece de Amnesia (Stateless). 
   - A nivel técnico, si borrás las Cookies (o limpias tu LocalStorage donde está el JWT) de tu navegador en medio de una compra en Amazon. ¿Qué sucede exactamente en tu próxima petición al servidor y por qué ocurre eso?

4. **Bypass del Sistema:**
   - Al interceptar tu tráfico, descubrís que tu perfil en un sitio carga con la URL: `api/v1/user/1005`.
   - Modificás la URL a `api/v1/user/1006` y el servidor arroja un error "403 Forbidden" (No te deja entrar). 
   - A pesar de que lograste cambiar el número exitosamente para pedir otro perfil, tu ataque de IDOR fracasó. Explicá qué mecanismo de programación lógicamente correcto aplicó el desarrollador Backend para bloquearte la lectura de ese objeto.

5. **Engaño de Cookies (CSRF vs XSS):**
   - ¿Por qué en un ataque exitoso de CSRF clásico (falsificando la transferencia desde un banco) el atacante jamás llega a leer ni a poseer físicamente los números y letras de tu Cookie secreta, y cómo esto contrasta con el objetivo principal de un ataque XSS?

6. **Exploración Interna (SSRF):**
   - Sos administrador de red y configuraste el Firewall para que absolutamente nadie de Internet pueda acceder a la dirección IP privada del servidor de Base de Datos.
   - Sin embargo, un atacante logró hacer un SSRF en el Servidor Web (que está expuesto al público). Explicá cómo las matemáticas de confianza (relativas al "origen" de la conexión) hicieron inútil a tu Firewall y le dieron acceso a los datos al atacante.

---

## 🎯 Autoevaluación
Asegurate de dominar el Ejercicio 2 (XSS vs SQLi). Mezclar ambos conceptos (y sus objetivos/capas) es el error número uno de los aprendices de ciberseguridad. Tenés que tener claro que el JS siempre ataca al navegador local, y el SQL siempre ataca a la computadora remota.

**➡️ Siguiente nota:** [[14 - Evaluación]]
