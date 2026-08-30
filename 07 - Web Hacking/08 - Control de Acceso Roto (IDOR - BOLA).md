# 08 - Control de Acceso Roto (IDOR / BOLA)

## 🎯 Objetivos
- Descubrir la vulnerabilidad Número 1 del mundo según el ranking OWASP.
- Entender cómo fallos puramente Lógicos superan a los ataques técnicos (como el SQLi).
- Aprender el concepto letal de IDOR (Insecure Direct Object Reference).

---

## 🧠 Concepto: La Puerta sin Candado

En las notas pasadas (XSS / Command Injection), el hacker usaba caracteres extraños y comandos letales para engañar al sistema.
**En IDOR, no hay código malicioso.** Todo el tráfico que envía el atacante es 100% legal, limpio y bien formateado.

IDOR ocurre cuando una página web asume ciegamente que el usuario tiene derecho a acceder a la información que está pidiendo, solo por el hecho de estar logueado.

---

## 💥 Escenario: El Ataque IDOR (BOLA en las APIs)

*(Nota: En la web clásica se le llama IDOR. En las APIs modernas, se le llama BOLA - Broken Object Level Authorization. Son el mismo fallo exacto).*

**El Contexto:**
Estás logueado en la página de tu obra social (con tu usuario y clave correctos). 
Hacés clic en "Ver mi factura". Tu navegador envía la siguiente petición al Backend:
> `GET /facturas/descargar?id=1004`

El Backend ve tu pedido. El Backend extrae la factura **1004** de la base de datos y te la muestra en la pantalla. Funciona perfecto.

**El Hacker Ataca (Modificando Referencias):**
El atacante intercepta su propia petición (usando el Proxy Burp Suite) o simplemente edita la barra de la URL en Chrome.
El atacante cambia el número 1004, lo aumenta por uno, e intenta pedirle al servidor una factura que NO es suya:
> `GET /facturas/descargar?id=1005`

**La Destrucción (Falla de Autorización Lógica):**
El Backend de la obra social (mal programado) recibe el pedido 1005. 
El Backend comprueba tu Cookie y dice: *"Ah, estás logueado en la página (Autenticación), dejalo pasar"*.
El Backend agarra la factura 1005 (que le pertenece al Presidente del País) y se la entrega completa al atacante.
El hacker acaba de leer historiales médicos hiper-secretos, simplemente cambiando el número de la URL con el teclado numérico.

> **Regla Crítica de AppSec:** El programador Backend falló porque verificó la *Autenticación* (Sí, tenías iniciada sesión) pero olvidó programar en código la *Autorización Horizontal* ("¿Esta factura 1005 le pertenece a la persona que tiene la sesión activa?").

---

## ❓ Variantes del Control de Acceso Roto

1. **Escalada Horizontal (El caso anterior):** Un usuario estándar accede a información de otro usuario estándar (de su mismo nivel). Generalmente logrado manipulando Parámetros (IDOR).
2. **Escalada Vertical (Bypass de Rol):** Un usuario estándar (Nivel bajo) manipula la URL para acceder a las funciones del Administrador (Nivel alto).
   - El usuario normal entra a su perfil: `web.com/usuario/perfil`
   - El atacante "adivina" una ruta oculta probando a ciegas: `web.com/admin/panel`
   - Si el Backend no revisa si tu Cookie es nivel "Admin" antes de cargar la página (solo verifica que estés logueado como alguien genérico), el atacante entra al panel y borra usuarios del sistema.

---

## ⚙️ Prevención: UUIDs vs Enteros

¿Por qué es tan fácil hacer un ataque IDOR en páginas mal hechas?
Porque los programadores usan números consecutivos (Enteros autoincrementales) en su base de datos. (Factura 1, Factura 2, Factura 3). Un hacker que descubre su propia factura puede simplemente crear un script de Python que descargue las 5.000 facturas anteriores en 30 segundos contando hacia abajo.

Para mitigar fuertemente esto, la industria moderna utiliza **UUIDs / GUIDs** (Universally Unique Identifiers). En lugar de usar `id=1004`, los objetos se nombran con hashes aleatorios de 36 caracteres:
> `GET /facturas/descargar?id=550e8400-e29b-41d4-a716-446655440000`

Aunque la aplicación siga siendo vulnerable a IDOR de fondo (porque el programador sigue sin validar quién pide el archivo), el atacante **jamás podrá adivinar matemáticamente el ID de la factura de otra persona**, frustrando el ataque por completísimo.

---

## 📌 Must Know (Imprescindible)
- Qué significa **Broken Access Control** (Puesto N°1 en OWASP) y qué es el **IDOR**: Vulnerabilidades donde la web no verifica correctamente si el usuario que está pidiendo un objeto específico, tiene permiso legal de acceso a *ese* objeto en concreto.
- Entender que no requiere inyectar caracteres especiales ni código malicioso; es un ataque "lógico" utilizando herramientas legítimas (manipulando IDs o números directos).
- Saber por qué el uso de secuencias predecibles (Ej: 1, 2, 3) facilita enormemente este ataque.

---

## 🔄 Preguntas de repaso
1. Mientras realizás pruebas de seguridad en una plataforma de foros, identificás que al hacer clic en "Borrar mi post", tu navegador envía la petición `DELETE /api/posts/80`. Cambias ese `80` por `79` (el post de tu amigo) y descubrís que la página de tu amigo se borró con éxito. Explicá cómo este ejemplo clásico de IDOR demuestra la diferencia abismal entre la validación de Autenticación (Estar logueado) y la validación ausente de Autorización Horizontal (Los permisos).
2. Un desarrollador argumenta que su página web es inmune a la manipulación de URLs y al ataque IDOR/BOLA porque "El enlace hacia la factura oculta no existe visualmente en ningún botón visible del Frontend". Haciendo alusión al rol de un Analista Web con un Proxy (como Burp Suite), ¿por qué esa afirmación (Seguridad por Oscuridad) de esconder las cosas en el frontend no tiene validez alguna en ciberseguridad?
3. En la mitigación contra la enumeración en ataques IDOR, la industria recomienda usar UUIDv4 (Identificadores universales aleatorios) en lugar de números secuenciales como `id=5`. ¿Por qué reemplazar los identificadores por cadenas aleatorias frena el ataque de extracción masiva automatizada, a pesar de que el código del Backend siga teniendo el fallo de autorización interno?

**➡️ Siguiente nota:** [[09 - Ataques de Suplantación (CSRF)]]
