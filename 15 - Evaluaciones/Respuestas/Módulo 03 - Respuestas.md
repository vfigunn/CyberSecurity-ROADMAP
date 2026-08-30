# Respuestas Evaluación Módulo 03 - Linux

A continuación se presentan las respuestas correctas de la evaluación del [[03 - Linux/24 - Evaluación|Módulo 03]], junto con la justificación técnica de cada una.

---

### Sección 1: Sistema de Archivos y Navegación

**1. C) `cd ~` (o simplemente `cd`)**
> *Justificación:* La tilde/virgulilla (`~`) representa la ruta de la carpeta personal (Home) del usuario actual en Linux. Ejecutar `cd` a secas hace exactamente lo mismo. `cd ..` subiría un solo nivel, y `cd /` te llevaría a la raíz del disco.

**2. B) `/var/log`**
> *Justificación:* Según el estándar FHS, la carpeta `/var` almacena datos que varían (como bases de datos y registros). Específicamente, `/var/log` es el lugar central para investigar auditorías, intentos de inicio de sesión (`auth.log`) y mensajes del sistema.

**3. C) `less errores.log`**
> *Justificación:* `less` abre el archivo en un formato paginado interactivo que permite subir, bajar y buscar palabras (usando `/`). `cat` imprimiría las 8000 líneas de golpe arruinando la vista, y `head` solo mostraría las primeras 10.

---

### Sección 2: Búsqueda y Manipulación

**4. C) `find /etc -name "*.conf"`**
> *Justificación:* El comando `find` busca en tiempo real y recursivamente dentro de un directorio. La sintaxis correcta es `find [ruta] -name "[nombre/comodín]"`.

**5. C) `>>`**
> *Justificación:* El operador `>>` (Append) añade el Output de un comando al final de un archivo existente sin borrar su contenido. El operador único `>` sobrescribiría el archivo, borrando todo lo guardado en las búsquedas anteriores.

---

### Sección 3: Permisos y Procesos

**6. B) El Dueño puede leer y escribir (6). El Grupo y los Otros solo pueden leer (4).**
> *Justificación:* En notación octal, 4=Lectura, 2=Escritura, 1=Ejecución. El número 6 es la suma de (4+2) para el Dueño. El número 4 es solo Lectura para el Grupo. El último 4 es solo Lectura para Otros.

**7. A) `kill -9 5040`**
> *Justificación:* La señal 9 (SIGKILL) es una instrucción directa al Kernel de Linux para destruir el proceso incondicionalmente y expulsarlo de la memoria RAM de forma violenta, ignorando cualquier intento del programa por bloquearse o congelarse.

**8. C) El programa se ejecutará temporalmente con los privilegios absolutos de `root`.**
> *Justificación:* Ese es el comportamiento exacto del permiso SUID. Permite que un ejecutable corra "tomando prestados" los privilegios del Dueño del archivo (en este caso, `root`), sin importar quién haya lanzado el comando originalmente. Es el principal vector de Escalada de Privilegios en el pentesting de Linux.

---

### Sección 4: Servicios, Paquetes y Automatización

**9. C) `crontab`**
> *Justificación:* `crontab` (apoyado en el demonio `cron`) es la herramienta nativa de Linux para programar tareas (scripts o comandos) para que se ejecuten automáticamente en intervalos de tiempo predefinidos (usando la sintaxis de los 5 asteriscos).

**10. B) `sudo apt update`**
> *Justificación:* `apt update` se conecta a los repositorios configurados y descarga el índice o lista más reciente del software disponible. Debe ejecutarse siempre antes de instalar algo, para que el sistema sepa dónde y qué versión exacta descargar. `apt upgrade` se usa posteriormente para aplicar parches al software ya instalado.
