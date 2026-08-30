# 24 - Evaluación del Módulo 03 (Linux)

## 📝 Instrucciones
Evaluación final del módulo. Múltiples preguntas basadas en escenarios técnicos y comandos indispensables de consola.
Anotá tus respuestas y luego verificalas con el solucionario.

> **Importante:** Las respuestas correctas y explicaciones están en: `[[15 - Evaluaciones/Respuestas/Módulo 03 - Respuestas]]`.

---

## 🎯 Sección 1: Sistema de Archivos y Navegación

**1. Si estás trabajando en la carpeta `/etc/ssh` y querés volver rápidamente a tu directorio personal (Home), ¿qué comando utilizarías?**
A) `cd ..`
B) `cd /`
C) `cd ~` (o simplemente `cd`)
D) `cd -`

**2. ¿En qué directorio de la jerarquía estándar de Linux (FHS) buscarías típicamente los archivos de registros (logs) del sistema para investigar un incidente de seguridad?**
A) `/etc`
B) `/var/log`
C) `/bin`
D) `/tmp`

**3. Un analista defensivo necesita ver un archivo de texto llamado `errores.log` que tiene 8,000 líneas. Quiere poder desplazarse hacia arriba y hacia abajo y buscar palabras dentro del archivo. ¿Qué comando es el más adecuado?**
A) `cat errores.log`
B) `grep errores.log`
C) `less errores.log`
D) `head errores.log`

---

## 🎯 Sección 2: Búsqueda y Manipulación

**4. ¿Qué comando te permite buscar, en tiempo real y recorriendo el disco duro, todos los archivos que terminen con la extensión `.conf` dentro de la carpeta `/etc`?**
A) `locate -name "*.conf" /etc`
B) `grep "*.conf" /etc`
C) `find /etc -name "*.conf"`
D) `ls -la /etc | grep .conf`

**5. Ejecutas un comando para buscar una IP maliciosa y querés guardar la lista resultante en un archivo de texto llamado `evidencia.txt`. Si ejecutás el comando varias veces en el día, NO querés que se borre lo que habías guardado antes; querés que se vaya sumando al final del archivo. ¿Qué operador utilizás?**
A) `|`
B) `>`
C) `>>`
D) `2>/dev/null`

---

## 🎯 Sección 3: Permisos y Procesos

**6. Tenés un archivo llamado `script.sh` con los permisos `644` (representación octal). ¿Qué significa esto en términos de los permisos de Lectura (r), Escritura (w) y Ejecución (x)?**
A) El Dueño puede leer, escribir y ejecutar (7). Los demás no tienen acceso (0).
B) El Dueño puede leer y escribir (6). El Grupo y los Otros solo pueden leer (4).
C) El Dueño puede leer y ejecutar (5). El Grupo puede escribir (2).
D) Todos tienen permiso de escritura.

**7. Un programa congelado consume el 100% de la CPU. Utilizaste el comando `top` y descubriste que su PID es 5040. Intentaste cerrarlo amablemente con `kill 5040` pero no respondió. ¿Cuál es el comando forzoso para destruirlo a nivel de Kernel?**
A) `kill -9 5040`
B) `sudo stop 5040`
C) `systemctl kill 5040`
D) `rm -rf 5040`

**8. En Linux, si un programa tiene configurado el permiso especial SUID y es propiedad del usuario `root`, ¿qué ocurre cuando un usuario sin privilegios ejecuta ese programa?**
A) El usuario no podrá ejecutarlo (Permiso denegado).
B) El programa se ejecutará con los privilegios limitados del usuario normal que lo inició.
C) El programa se ejecutará temporalmente con los privilegios absolutos de `root`.
D) El programa solicitará obligatoriamente la contraseña de `sudo` antes de abrirse.

---

## 🎯 Sección 4: Servicios, Paquetes y Automatización

**9. Escribiste un script de Bash para limpiar archivos temporales y querés que el servidor lo ejecute automáticamente todos los días a medianoche. ¿Qué herramienta de Linux utilizarías para programar esta tarea?**
A) `systemctl`
B) `apt update`
C) `crontab`
D) `bashrc`

**10. Como SysAdmin, necesitás instalar un nuevo paquete de software llamado `nmap`. Para asegurarte de que descargás la versión más segura, primero debés actualizar la lista local (catálogo) de software disponible conectándote a los repositorios de Internet. ¿Cuál es el comando correcto para actualizar esta lista de catálogo (no los programas instalados)?**
A) `sudo apt upgrade`
B) `sudo apt update`
C) `sudo dpkg -i nmap`
D) `sudo apt install nmap`

---

> **¿Terminaste?**
> Buscá el archivo en la carpeta de evaluaciones para comparar tus resultados (Módulo 03 - Respuestas).

**➡️ Siguiente nota:** [[25 - Resumen]]
