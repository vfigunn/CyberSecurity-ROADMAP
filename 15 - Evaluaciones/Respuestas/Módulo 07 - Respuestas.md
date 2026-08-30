# Respuestas Evaluación Módulo 07 - Web Hacking

A continuación se presentan las respuestas correctas de la evaluación del [[07 - Web Hacking/14 - Evaluación|Módulo 07]], junto con la justificación técnica de cada una.

---

### Sección 1: Arquitectura y HTTP

**1. C) JSON**
> *Justificación:* En la arquitectura API REST moderna, el servidor web casi nunca entrega el código visual HTML empaquetado. El servidor expulsa exclusivamente datos estructurados crudos en notación ligera (JavaScript Object Notation - JSON). El Frontend de React, Vue o iOS, consume este JSON, lo procesa localmente en RAM y luego "dibuja" el contenido de forma nativa.

**2. C) 4xx (Errores del Cliente)**
> *Justificación:* En la convención global del protocolo HTTP, toda la familia `400` denota un problema originado en el usuario final. Ya sea un error de escritura (400 Bad Request), que estás intentando entrar a una página restringida a la cual no tienes permisos (403 Forbidden), o una Autenticación fallida por contraseña errónea (401 Unauthorized).

**3. C) Desatar o "Escapar" del límite de texto (String) definido por el programador original, permitiendo que las siguientes palabras ingresadas sean interpretadas como comandos matemáticos válidos de la Base de Datos.**
> *Justificación:* En la programación subyacente de la consulta `SELECT`, el programador rodea a la variable (la que el usuario rellena) con apóstrofes (`WHERE nombre = 'variable_texto'`). Al inyectar un Apóstrofe extra (`'`), le indicamos al intérprete de SQL que la variable textual terminó prematuramente, "desatando" o "escapando" al texto siguiente hacia el territorio de los comandos lógicos activos y provocando así la vulnerabilidad.

---

### Sección 2: El Ataque contra el Navegador (XSS y Sesiones)

**4. A) Cross-Site Scripting (XSS)**
> *Justificación:* A diferencia del resto de ataques listados (que intentan dañar o abusar del servidor corporativo en el Backend remoto), el Cross-Site Scripting (XSS) inyecta código de *JavaScript* letal, el cual es ignorado por el Backend, pero descargado, renderizado y procesado 100% en la máquina local (Navegador Chrome/Firefox) de un usuario víctima, con el objetivo de secuestrar su sesión o manipular su percepción visual de la página oficial.

**5. B) XSS Almacenado (Stored)**
> *Justificación:* Si la inyección JavaScript reside de forma permanente en la infraestructura del negocio (como en un comentario de la Base de Datos), se trata de una vulnerabilidad Almacenada. Es devastadora porque ya no se requiere hacer "Phishing" ni enviar enlaces engañosos individualizados; afecta silenciosamente y en masa a cada uno de los individuos que visite la URL legítima contaminada.

**6. A) La Cookie Clásica es un identificador aleatorio de sesión, mientras que toda la información real (como roles y permisos) está almacenada en la Memoria RAM/BBDD del Backend. El Token JWT, por su parte, es auto-contenido (lleva los roles encriptados adentro del propio Token), relevando al Servidor de recordar estados.**
> *Justificación:* JWT (JSON Web Tokens) solucionó los problemas de almacenamiento masivo y saturación de RAM de los servidores. Al guardar todos los parámetros críticos dentro de un "Paquete", sellado por una Firma Digital impenetrable en su pie, el servidor web recupera su capacidad de ser verdaderamente "Stateless" (sin memoria) verificando pura criptografía instantánea.

---

### Sección 3: Fallas Lógicas (IDOR, CSRF, SSRF)

**7. B) Cambiar un parámetro secuencial de la URL (Ej: de `factura=10` a `factura=11`) y lograr descargar el documento privado de un gerente sin que el Backend audite los permisos de pertenencia.**
> *Justificación:* IDOR (Referencia Insegura a Objetos Directos) carece por completo de la inserción de malware. Pertenece puramente al "Broken Access Control", donde se explota un fallo humano lógico porque la base de código nunca se cercioró si el ID que solicita el usuario le pertenece legal y horizontalmente a su propia jerarquía o a la del prójimo.

**8. C) El hecho de que los navegadores adjuntan, envían y disparan silenciosamente las Cookies de sesión hacia cualquier dominio web, independientemente de qué otra página gatilló en segundo plano esa petición.**
> *Justificación:* La amnesia crónica de la web (HTTP) obligó a que navegadores desarrollen el envío automático de Cookies. El ciberdelincuente se aprovecha (abusando de una etiqueta invisible u oculta `<img>`) obligando al Chrome de su víctima a efectuar una petición HTTP a espaldas de esta hacia su cuenta bancaria. Como Chrome "conoce" que debe anexar la Cookie a todo flujo destinado al banco, se la incrusta de oficio, autenticando instantánea e involuntariamente la transacción maliciosa del delincuente. 

**9. B) Server-Side Request Forgery (SSRF)**
> *Justificación:* La "Falsificación del Lado del Servidor" atenta contra la base perimetral: engañando al servidor para que el propio Backend emita (efectúe) las peticiones por sí mismo (a nombre del atacante). Como las conexiones internas del Backend están "Listadas en Blanco" y autorizadas dentro del propio entorno privado (Intranet o capa IAM MetaData en AWS), logran burlar de forma pasmosa la arquitectura Firewall completa, dándole vista privilegiada interna total al hacker externo.

**10. A) RFI (Remote File Inclusion)**
> *Justificación:* Al permitirte no solamente la inclusión y lectura de un texto en el propio disco base (LFI/Path Traversal local), sino que explícitamente se conectó a una URL de Internet controlada por ti (hacker.com/malware.php) y extrajo el texto ajeno dinámico para ejecutarlo nativamente, caíste ante un RFI. Debido al daño instantáneo de *Control Total Remoto (RCE)*, este fallo se mitigó severamente hace más de una década prohibiendo el uso del atributo `allow_url_include` en los configuradores de PHP (php.ini) modernos.
