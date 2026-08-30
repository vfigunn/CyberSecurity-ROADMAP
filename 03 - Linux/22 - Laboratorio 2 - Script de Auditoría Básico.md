# Lab 03.2 - Script de Auditoría Básico (Bash)

## 🎯 Objetivo
Vas a escribir, de forma simulada y analítica, un script corto en Bash que automatiza una tarea básica del Blue Team: la auditoría de conexiones de red y usuarios.
Aprenderás a conectar comandos (Redirecciones y Pipes), variables y permisos de ejecución.

---

## 📋 Requerimiento del Negocio

El CISO (Director de Seguridad) de tu empresa te pide una herramienta de automatización. 
Quiere un script llamado `auditoria.sh` que, cuando se ejecute, genere un archivo de reporte automático llamado `reporte_auditoria.txt`.

El reporte de texto resultante debe contener, de forma concatenada, las siguientes 3 piezas de evidencia de seguridad:
1. Una frase inicial con la fecha exacta en la que se generó el reporte.
2. Una lista de todas las cuentas de usuario de la máquina (extraída desde `/etc/passwd`), pero **solamente** las líneas que mencionen la palabra `root`.
3. Un listado de las conexiones de red actuales que están "Escuchando" (LISTEN) buscando detectar si hay un puerto raro abierto (Usando el comando `ss -tulpn`).

*(Finalmente, te pide que ese script corra en automático todos los viernes a las 11:00 PM).*

---

## 🛠️ Procedimiento (Tu Trabajo)

Tomá un papel o un block de notas (físico o virtual) e intentá escribir las líneas de código de este Script en Bash para satisfacer el requerimiento. 

**Tips / Pasos lógicos:**
1. Recordá siempre cuál debe ser la primera línea innegociable de cualquier script.
2. Usá el comando `date` para obtener la fecha, e imprimilo usando `echo`. Guardá eso en un archivo nuevo usando la redirección correcta (sobrescribir, para borrar el del día anterior).
3. Para extraer a los usuarios root, vas a necesitar hacer un `cat` de un archivo famoso, y pasarle su salida (Pipe) a la herramienta que sirve para "buscar texto". Luego, el resultado final debe **añadirse** (`>>`) al reporte.
4. Ejecutá el comando de conexiones, pasaselo (Pipe) a la misma herramienta para buscar la palabra "LISTEN" (en mayúsculas), y el resultado final también debe **añadirse** (`>>`) al reporte.
5. Recordá qué comando especial tenés que correr en la terminal después de guardar el archivo para que Linux te deje ejecutarlo.
6. Escribí (con la sintaxis de 5 asteriscos) la línea que agregarías al `crontab` para ejecutarlo. (Viernes = 5. Hora 11PM = 23).

---

## 📝 Resultado Esperado (Autoevaluación)

¿Terminaste tu código? Revisá cómo se vería una solución clásica e idónea en Bash.

> [!example]- Ver Código Solución del Script
> **Contenido del archivo `auditoria.sh`:**
> ```bash
> #!/bin/bash
> 
> # Paso 1: Generamos el encabezado y sobrescribimos el archivo (>) para limpiarlo.
> # Usamos una variable para almacenar la fecha actual.
> FECHA=$(date)
> echo "--- REPORTE DE AUDITORIA GENERADO EL: $FECHA ---" > reporte_auditoria.txt
> 
> # Añadimos un salto de linea decorativo (opcional)
> echo "" >> reporte_auditoria.txt
> 
> # Paso 2: Extraemos solo las líneas que contienen "root" y lo agregamos (>>) al reporte.
> echo "--- USUARIOS ROOT EN EL SISTEMA ---" >> reporte_auditoria.txt
> cat /etc/passwd | grep "root" >> reporte_auditoria.txt
> 
> echo "" >> reporte_auditoria.txt
> 
> # Paso 3: Listamos puertos escuchando, filtramos, y agregamos al reporte (>>).
> echo "--- PUERTOS EN ESCUCHA (LISTEN) ---" >> reporte_auditoria.txt
> ss -tulpn | grep "LISTEN" >> reporte_auditoria.txt
> 
> # Notificamos al usuario por pantalla que terminamos
> echo "Auditoría finalizada con éxito. Lea el archivo reporte_auditoria.txt"
> ```
> 
> **Paso posterior en la terminal (Darle vida al script):**
> Para que el script pueda correr, tenías que darle permiso de ejecución al dueño:
> `$ chmod u+x auditoria.sh`
> *(Y luego ejecutarlo con `./auditoria.sh`)*.
> 
> **Configuración en Crontab (Automatización final):**
> El requerimiento decía "Todos los viernes (5) a las 11:00 PM (23:00)".
> Sintaxis: *Minuto | Hora | Día_Mes | Mes | Día_Semana | Comando*
> Línea a agregar usando `crontab -e`:
> `0 23 * * 5 /ruta/absoluta/hacia/tu/script/auditoria.sh`

---

Felicidades. Acabas de "programar" tu primera solución de automatización defensiva. Esto es un pilar absoluto del rol de DevSecOps, Administración de Sistemas y Cacería de Amenazas.

**➡️ Siguiente nota:** [[23 - Ejercicios]]
