# 04 - Inyección I (SQL Injection - SQLi)

## 🎯 Objetivos
- Entender cómo funciona estructuralmente un ataque de Inyección en la web.
- Comprender el lenguaje de bases de datos `SQL`.
- Conocer la Inyección SQL (SQLi) y cómo un apóstrofe (`'`) puede robarse una empresa entera.

---

## 🧠 Concepto: La Filosofía de la Inyección

La Inyección (Injection), históricamente el rey de los ataques web, ocurre por una falla de diseño mental de los programadores: **Juntar el Código con los Datos en el mismo balde**.

Si el programa espera que en la barra de búsqueda ingreses tu "Nombre" (El Dato), vos en lugar de eso le ingresás "Un Código Matemático o de Computadora".
Si el servidor no filtró (sanitizó) tu respuesta, tomará tu Código como si fuera un simple Dato, pero en el momento de procesarlo, **tu código cobrará vida y el servidor lo ejecutará**. Acabás de hackear el sistema con puro texto.

---

## 💾 Breve repaso de SQL

SQL es el lenguaje universal para hablar con las bases de datos relacionales (donde la información se guarda en Tablas, como en Excel).

Si ingresas a una web y hacés clic en la categoría "Zapatillas", el Backend (Servidor) internamente hace esta consulta (Query) a su base de datos para mostrarte las zapatillas en la pantalla:
`SELECT * FROM productos WHERE categoria = 'Zapatillas';`
*(Traducción: "SELECCIONAR *[TODO]* DESDE la tabla Productos, DONDE la categoría sea igual a Zapatillas").*

---

## 💥 SQL Injection (SQLi) en Acción

El ataque ocurre cuando la página web tiene una barra de búsqueda o una URL donde *el usuario final* puede tipear lo que quiera, y el desarrollador concatena ese texto (lo pega a lo bestia) directamente dentro de la Query SQL, sin limpiarlo (Sanitizarlo) primero.

### El Escenario Clásico: Bypass de Login (Burlar el inicio de sesión)
Imaginá que estás en la pantalla de Log In de una corporación (`usuario` y `contraseña`).
Internamente, el servidor tiene pre-escrita en su código la siguiente Query SQL que ejecuta cuando le das al botón de "Aceptar":

> `SELECT * FROM usuarios WHERE nombre = '`**tu_usuario**`' AND clave = '`**tu_clave**`';`
*(Si el servidor encuentra 1 registro, te deja entrar. Si encuentra 0 registros, te da "Clave incorrecta").*

**El Ataque:**
En la barra de usuario en la pantalla de la web, en lugar de poner "Juan", el Hacker tipea esta maliciosa obra de arte:
**`admin' OR 1=1 --`**

**La Destrucción (Qué pasó en el servidor):**
El servidor bobo pegó literalmente lo que escribió el hacker adentro de su Query. Mirá cómo quedó la Query en la base de datos ahora:
> `SELECT * FROM usuarios WHERE nombre = '`**admin' OR 1=1 --**`' AND clave = '`**cualquiera**`';`

Desglosemos qué acaba de hacer la inyección del hacker:
1. El hacker inyectó el **apóstrofe `'`** después de admin. Ese apóstrofe le avisó a la Base de Datos que ahí terminó el campo del texto. ¡Acaba de escapar del molde de texto!
2. El hacker inyectó **`OR`** (Operador lógico [[04 - Python/06 - Condicionales (if, elif, else)|visto en Python]]). "SI esto pasa O si esto otro pasa, considerá que es Verdadero".
3. El hacker inyectó **`1=1`**. Es una tautología absoluta. El número 1 es y siempre será igual a 1 (Verdadero).
4. El hacker inyectó **`--`** (Doble guion). En el lenguaje SQL, el doble guion significa *"Ignorar (comentar) todo lo que siga de aquí en adelante"*. Esto hace que la Base de Datos borre de su memoria toda la sección de la contraseña que venía a la derecha de la Query original del servidor.

**El resultado final evaluado por el sistema:**
La base de datos lee: *"Tráeme la cuenta de admin... ¡OH, o de paso fijate si 1 es igual a 1!"*. 
Como 1=1 siempre es verdad, la Base de Datos le responde al servidor web: *"¡Afirmativo! Query exitosa. Dejalo pasar"*.
El Hacker acaba de iniciar sesión como el Administrador Supremo de la empresa... sin saber la contraseña en lo más mínimo.

---

## ❓ Otros tipos de Inyecciones SQL

- **Error-Based:** El hacker inyecta una comilla `'` en la URL de una tienda (ej: `tienda.com/item?id=5'`). El servidor choca porque la Query quedó coja (con una comilla suelta) y le escupe una pantalla blanca (Error 500) revelándole al hacker el nombre real de las tablas internas de la base de datos en pantalla.
- **UNION-Based:** El hacker inyecta la palabra `UNION` en la barra de búsqueda para forzar al servidor a que junte la tabla "Zapatillas" con la tabla secreta "Tarjetas_Credito" y devuelva ambas a la pantalla del atacante.
- **Blind (Ciega):** El servidor jamás escupe errores en pantalla y es silencioso. El atacante inyecta comandos de *Tiempo* (Sleep/Wait). Le dice: *"Base de datos, si la primer letra de la contraseña del jefe es una 'A', demorá 5 segundos en cargar la página web"*. El atacante cuenta con un cronómetro cuánto tarda la web en responderle, logrando extraer toda la base de datos a ciegas mediante demoras inducidas (Extracción inferencial). 

---

## 📌 Must Know (Imprescindible)
- **Concepto de Inyección:** Alterar la lógica original programada por el desarrollador insertando comandos del lenguaje interpretado (SQL) desde el cliente.
- El rol del apóstrofe o comilla simple (`'`) como el iniciador ("escapador") del 99% de los ataques SQL, diseñado para cerrar el parámetro de texto de la query.
- Conocer la Inyección más famosa del mundo para engañar el flujo y evadir el inicio de sesión: `' OR 1=1 --`.
- *Defensa (Blue Team):* Para evitar esto, los desarrolladores JAMÁS deben concatenar texto (`+ variable +`), deben usar **Consultas Preparadas (Prepared Statements / Parametrización)**, que bloquean matemáticamente que cualquier texto del cliente sea interpretado como código lógico.

---

## 🔄 Preguntas de repaso
1. Estudiá esta inyección SQL básica: `admin' --`. Al ingresar esto en un campo de usuario de login, un atacante logra entrar a la plataforma sin ingresar contraseña. Detallá lógicamente qué hace el apóstrofe (`'`) a nivel de base de datos, y qué hace el doble guion medio (`--`) para inutilizar el chequeo de la contraseña.
2. Analizando el concepto de SQLi UNION-Based. Supongamos que una página web utiliza una Query SQL que, legítimamente, está programada para devolver 3 columnas a la pantalla de tu computadora (Nombre del producto, Precio, y Stock). Si un atacante inyecta el comando `UNION SELECT usuario, clave FROM usuarios`, el ataque fallará rotundamente arrojando un error de compatibilidad. ¿Qué regla de estructura estricta del comando UNION está violando el atacante con esa inyección al pedir solo 2 cosas?
3. En una auditoría Pentesting Ciego (Blind SQLi), el analista no logra que el servidor web le escupa ningún error SQL visible en el HTML, pero nota que al inyectar el código `... AND IF(1=1, SLEEP(10), 0) --` la página demora exactamente 10 segundos adicionales en terminar de cargar. ¿Qué deduce el analista instantáneamente sobre la validación (sanitización) en el código del servidor (Backend)?

**➡️ Siguiente nota:** [[05 - Inyección II (Command Injection)]]
