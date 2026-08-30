# 02 - FHS (Filesystem Hierarchy Standard)

## 🎯 Objetivos
- Entender el concepto de que "En Linux todo es un archivo".
- Conocer el Directorio Raíz (`/`).
- Memorizar el propósito de las carpetas más importantes del sistema operativo (/etc, /var, /bin, /home).

---

## 🧠 Concepto: El Árbol y la Raíz

En Windows, cada disco duro o partición tiene su propia letra independiente (`C:\`, `D:\`, un pendrive en `E:\`). 

En Linux, existe un único y gigantesco árbol de directorios (carpetas). 
El inicio de todo, la base del árbol, se llama el **Directorio Raíz (Root Directory)** y se representa con el símbolo de la barra diagonal: **`/`**.

Absolutamente todos los discos, pendrives, impresoras y programas están "colgados" en algún lugar dentro de este único directorio `/`.

### "En Linux, todo es un archivo" (Everything is a file)
Esta es una filosofía central del sistema. Un documento de texto es un archivo. Pero tu teclado también está representado como un archivo, tu placa de red es un archivo, y los procesos que se ejecutan en RAM también tienen un archivo virtual asociado. Si podés leer y escribir en esos archivos, podés interactuar con el hardware.

---

## 📂 Directorios vitales para Ciberseguridad

Existe un estándar llamado FHS que dicta para qué sirve cada carpeta. Como atacante (buscando secretos o vulnerabilidades) o como defensor (buscando logs de actividad maliciosa), tenés que saber exactamente a qué carpeta ir.

- **`/` (Root Directory):** El inicio de todo.
- **`/bin` y `/sbin`:**
  - Contienen los "Binarios", es decir, los programas ejecutables (los comandos como `ls`, `ping`, `bash`).
  - `/sbin` contiene programas críticos que solo deberían ser usados por el Administrador (Superusuario).
- **`/home`:**
  - Es el equivalente al "C:\Users" de Windows. Aquí están las carpetas personales de cada usuario humano. Si existe el usuario "maria", tendrá sus archivos privados en `/home/maria/`.
- **`/root`:**
  - El usuario Administrador Supremo (llamado `root`) NO guarda sus cosas en `/home/root`. Tiene su propia carpeta especial y súper protegida directamente en la base: `/root`.
- **`/etc` (¡Súper importante!):**
  - Contiene los archivos de **Configuración** de casi todos los programas del sistema.
  - En esta carpeta no hay ejecutables, solo archivos de texto.
  - *Visión Hacker:* El archivo `/etc/passwd` contiene el listado de todos los usuarios del sistema, y `/etc/shadow` contiene sus contraseñas encriptadas (Hashes). Son el Santo Grial en la escalada de privilegios.
- **`/var` (Variable):**
  - Contiene archivos que crecen o cambian constantemente con el tiempo, como bases de datos, correos en tránsito y, más importantemente, los **Logs**.
  - *Visión Defensor:* La carpeta `/var/log` es donde el Blue Team vive. Allí están los registros de quién inició sesión, errores del servidor web, y alertas del firewall.
- **`/tmp` (Temporal):**
  - Una carpeta para que cualquier programa o usuario pueda escribir archivos de forma temporal (a menudo se borra al reiniciar la computadora).
  - *Visión Hacker:* Como todos tienen permiso para escribir en `/tmp`, los atacantes suelen usar esta carpeta para descargar sus malwares (Payloads) antes de ejecutarlos.
- **`/opt`:**
  - Donde se suele instalar software de terceros que no viene incluido con el sistema operativo por defecto (Software Opcional). Muchas herramientas de pentesting descargadas de GitHub se instalan aquí.

---

## 📌 Must Know (Imprescindible)
- Saber ubicar mentalmente dónde están las configuraciones (`/etc`).
- Saber ubicar los logs del sistema (`/var/log`).
- La diferencia entre el directorio raíz (`/`) y la carpeta del superusuario (`/root`).
- Saber que los usuarios normales viven en `/home`.

---

## 🔄 Preguntas de repaso
1. Si descubrís una vulnerabilidad en un servidor web y querés robar el archivo que contiene la lista de los usuarios registrados del sistema operativo, ¿en qué directorio principal (`/etc`, `/var`, `/tmp` o `/bin`) irías a buscar ese archivo de configuración?
2. Entraste a un servidor atacado. Necesitas revisar si el atacante se conectó por red dejando algún rastro. ¿A qué carpeta principal deberías ir para buscar los archivos de registro (Logs)?
3. Sos un atacante con permisos básicos (usuario de bajos privilegios) y necesitas guardar en el disco duro un script malicioso que trajiste de internet. Sin embargo, no tenés permiso para escribir en `/var`, `/etc`, o `/bin`. ¿Qué carpeta es históricamente famosa por permitir escritura a casi cualquier usuario?

**➡️ Siguiente nota:** [[03 - Navegación Básica (cd, ls, pwd)]]
