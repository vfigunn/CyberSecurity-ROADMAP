# 14 - Evaluación del Módulo 07 (Web Hacking & OWASP)

## 📝 Instrucciones
Evaluación final del módulo. Múltiples preguntas basadas en arquitecturas web modernas, el protocolo HTTP, las APIs y el Top 10 de OWASP.
Anotá tus respuestas y luego verificalas con el solucionario.

> **Importante:** Las respuestas correctas y explicaciones están en: `[[29 - Evaluaciones/Respuestas/Módulo 07 - Respuestas]]`.

---

## 🎯 Sección 1: Arquitectura y HTTP

**1. En la arquitectura web moderna (REST APIs), cuando el cliente (Navegador/Frontend) solicita los datos del perfil de un usuario, ¿en qué formato de texto estándar el servidor web suele enviarle los datos de regreso para que el JavaScript los parsee y dibuje en pantalla?**
A) HTML (con etiquetas `<body>` y `<head>`)
B) XML
C) JSON
D) Python Data Dictionary

**2. Al analizar el tráfico HTTP con tu Proxy Interceptor (Burp Suite), enviás unas credenciales erróneas a propósito a un portal de Login. El servidor te rebota con el texto "Contraseña incorrecta". ¿En qué familia de Códigos de Estado numéricos (Status Codes) clasifica el servidor web a este evento, al reconocer que la falla proviene del usuario y no del backend?**
A) 2xx (Éxito)
B) 3xx (Redirección)
C) 4xx (Errores del Cliente)
D) 5xx (Errores del Servidor)

**3. En un ataque de SQL Injection, la técnica fundamental para burlar (hacer "Bypass") un formulario de inicio de sesión implica el uso constante del carácter Apóstrofe (`'`) acompañado frecuentemente de secuencias lógicas (`OR 1=1`). ¿Cuál es el propósito funcional primario del Apóstrofe a nivel de Base de Datos?**
A) Terminar abruptamente la petición HTTP y causar un error 500.
B) Transformar el texto ingresado a Base64.
C) Desatar o "Escapar" del límite de texto (String) definido por el programador original, permitiendo que las siguientes palabras ingresadas sean interpretadas como comandos matemáticos válidos de la Base de Datos.
D) Engañar al Frontend (JavaScript) para ocultar la contraseña.

---

## 🎯 Sección 2: El Ataque contra el Navegador (XSS y Sesiones)

**4. ¿Cuál de las siguientes es una vulnerabilidad cuyo daño y código se procesan estricta y puramente en la computadora local del usuario (Frontend/Chrome) y cuyo objetivo principal radica en el secuestro de sesiones (Robo de Cookies) ajenas?**
A) Cross-Site Scripting (XSS)
B) Server-Side Request Forgery (SSRF)
C) Remote File Inclusion (RFI)
D) Command Injection

**5. Identificás un foro público en el cual los usuarios pueden dejar reseñas sobre restaurantes. Subís tu reseña inyectando el código de una etiqueta de script, y descubrís que cualquier persona que abra ese restaurante recibe un mensaje emergente de Hackeo en su pantalla. ¿Qué variante de XSS descubriste?**
A) XSS Reflejado (Reflected)
B) XSS Almacenado (Stored)
C) DOM-Based XSS
D) Blind XSS

**6. El protocolo HTTP carece de memoria temporal (Stateless). Para arreglarlo, las corporaciones aplican métodos de persistencia de sesiones. ¿Cuál es la gran diferencia estructural entre una "Cookie Clásica" y un "Token JWT"?**
A) La Cookie Clásica es un identificador aleatorio de sesión, mientras que toda la información real (como roles y permisos) está almacenada en la Memoria RAM/BBDD del Backend. El Token JWT, por su parte, es auto-contenido (lleva los roles encriptados adentro del propio Token), relevando al Servidor de recordar estados.
B) La Cookie Clásica no se puede robar con XSS, pero el Token JWT sí es altamente robable.
C) La Cookie funciona mediante el protocolo HTTP, mientras que el Token JWT utiliza el protocolo FTP.
D) El Token JWT nunca es firmado por el servidor, siendo vulnerable a manipulación por parte del usuario de forma predeterminada.

---

## 🎯 Sección 3: Fallas Lógicas (IDOR, CSRF, SSRF)

**7. Según el proyecto global OWASP (Top 10), el "Control de Acceso Roto" (Broken Access Control) lidera ampliamente el ranking de severidades del planeta. De los siguientes ejemplos lógicos, ¿Cuál describe perfectamente el fallo crítico conocido como IDOR / BOLA?**
A) Lograr que la base de datos MySQL elimine las tablas de configuración.
B) Cambiar un parámetro secuencial de la URL (Ej: de `factura=10` a `factura=11`) y lograr descargar el documento privado de un gerente sin que el Backend audite los permisos de pertenencia.
C) Falsificar una petición HTTP de transferencia bancaria (engañando al usuario) forzándolo a hacer clic en un link malicioso.
D) Provocar que el Servidor Web (Backend) realice una consulta de DNS externa y revelar las IPs internas de AWS.

**8. En un ataque CSRF exitoso (falsificación de transferencia bancaria), el ciberdelincuente se aprovecha de un mecanismo automatizado, cómodo y predeterminado que tienen todos los navegadores web modernos de las víctimas. ¿Cuál es ese mecanismo crucial abusado en CSRF?**
A) El guardado de contraseñas de Google.
B) El caché dinámico y estático local para agilizar la carga visual.
C) El hecho de que los navegadores adjuntan, envían y disparan silenciosamente las Cookies de sesión hacia cualquier dominio web, independientemente de qué otra página gatilló en segundo plano esa petición.
D) La pre-resolución de nombres de dominio DNS.

**9. Para vulnerar la barrera corporativa externa (El Firewall local) desde adentro, y saltarse la seguridad perimetral para atacar la Intranet corporativa privada o escanear infraestructuras ciegas de AWS (Metadatos). ¿Qué vulnerabilidad crítica del lado del Servidor buscaría explotar el Red Team, haciendo peticiones hacia IPs como `127.0.0.1`?**
A) OS Command Injection
B) Server-Side Request Forgery (SSRF)
C) XSS Reflejado (Reflected)
D) SQLi Blind

**10. Al intentar explotar un error de filtrado en el parámetro "Archivo" de una aplicación web obsoleta (PHP), inyectas un salto de carpetas (`../../../../etc/passwd`). El servidor ejecuta tu ruta pero en lugar de solo entregarte un archivo de texto inerte (LFI), el servidor procede a descargar un código dinámico en un link que le ordenaste y ejecutarlo como si fuera parte íntegra de la app, dándote control remoto total de la shell. ¿A qué vulnerabilidad nos referimos (y que está desactivada por defecto hoy en día en PHP)?**
A) RFI (Remote File Inclusion)
B) CSRF
C) XSS DOM
D) Salto de base de datos relacional (DBMS)

---

> **¿Terminaste?**
> Buscá el archivo en la carpeta de evaluaciones para comparar tus resultados (Módulo 07 - Respuestas).

**➡️ Siguiente nota:** [[15 - Resumen]]
