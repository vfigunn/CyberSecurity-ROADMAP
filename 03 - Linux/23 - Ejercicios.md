# 23 - Ejercicios del Módulo 03

## 📝 Instrucciones
Pon a prueba tu agilidad mental de consola. En el mundo real, los atacantes y defensores no tienen tiempo de googlear cómo se hace un Pipe en medio de una respuesta a incidentes. Intentá resolver estos ejercicios mentales sin ver tus notas previas.

---

## 🧠 Ejercicios de Reflexión

1. **Rutas y Navegación Oculta:**
   - Estás parado en la carpeta `/var/log/apache2`. Dentro de esa carpeta sabés que existe una subcarpeta oculta llamada `.backups`. 
   - Escribí el comando exacto (usando una **ruta relativa**) para entrar a esa carpeta oculta.
   - Una vez adentro, ¿qué modificador o *flag* tendrías que pasarle al comando `ls` para ver qué archivos hay adentro si todos los archivos también están ocultos?

2. **La Cadena de Comandos (Pipes):**
   - Un analista junior te pide ayuda. Quiere ver únicamente las primeras 15 líneas de un archivo inmenso llamado `registros.csv`, y además, quiere que ese resultado se busque para ver si aparece la palabra `URGENTE`.
   - Escribí la línea de comandos usando un *Pipe* (`|`) conectando las herramientas `head` y `grep` para lograr esto.

3. **Escalada de Privilegios y Permisos:**
   - Encontrás un script en la carpeta de un compañero con los permisos `-rwx------`. El dueño de ese archivo es el usuario `maria`. Vos sos el usuario `juan` y tu jefe te pidió que ejecutes ese script urgentemente porque María está de vacaciones.
   - Si intentas ejecutarlo (`./script.sh`), ¿qué te dirá la consola? 
   - Suponiendo que tenés permiso para usar `sudo`, ¿cuál sería el comando completo para ejecutar el script exitosamente tomando los poderes necesarios?

4. **El Peligro del SUID:**
   - Explicá conceptualmente y con tus propias palabras por qué otorgarle permisos de SUID (`s`) a un editor de texto de línea de comandos (como `nano` o `vim`) es considerado una vulnerabilidad crítica de seguridad, si ese editor pertenece al usuario `root`. *(Pista: Pensá qué archivos podría modificar un usuario común usando ese editor).*

5. **Exfiltración Segura:**
   - Sos un pentester. Encontraste un archivo valioso llamado `datos.db` en el servidor comprometido. Querés descargarlo a tu máquina local usando el protocolo SSH porque sabés que el Firewall de la empresa bloquea FTP. 
   - ¿Qué comando (derivado de SSH) utilizarías para realizar esta transferencia segura por red? 

6. **Automatización Zombie (Cron):**
   - Desinfectaste un servidor, matando el proceso de un malware mediante `kill -9`. Sin embargo, cada mañana a las 5:00 AM, el malware vuelve a aparecer en la RAM.
   - ¿Qué comando usarías para ver la lista de tareas programadas del usuario actual y buscar dónde se esconde el reinicio del malware?

---

## 🎯 Autoevaluación

El ejercicio 2 (Unir comandos con Pipes) y el ejercicio 4 (Entender el peligro del SUID en herramientas comunes) son la verdadera medida de que estás pensando como un profesional de Linux (y no como un simple usuario). Si dudaste, repasá la [[08 - Redirecciones y Pipes|Nota 08]] y la [[11 - Permisos Especiales (SUID, SGID)|Nota 11]].

**➡️ Siguiente nota:** [[24 - Evaluación]]
