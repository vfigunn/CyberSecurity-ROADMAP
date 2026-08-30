# 17 - Variables de Entorno y Alias

## 🎯 Objetivos
- Entender el concepto de Variables de Entorno y por qué el sistema operativo las necesita.
- Conocer la variable mágica `$PATH`.
- Aprender a crear atajos útiles con `alias` para acelerar el trabajo en la consola.

---

## 🧠 Concepto: Variables de Entorno (Environment Variables)

Cuando abrís una terminal, el sistema (el Shell, como Bash) necesita saber ciertas "reglas del juego" y configuraciones para ese usuario en particular. ¿Qué idioma habla? ¿De qué color quiere la letra? ¿Dónde está su carpeta personal?

Toda esta configuración invisible se guarda en la memoria en forma de **Variables de Entorno**. 
Una variable es simplemente una caja con una etiqueta que guarda un valor adentro.
- Por convención universal, las variables de entorno se escriben siempre en **MAYÚSCULAS**.
- Para referenciar o acceder al contenido de esa "caja", debés anteponerle el símbolo de dólar (**`$`**).

**Ejemplos clásicos:**
- `$USER` : Contiene el nombre del usuario actual.
- `$HOME` : Contiene la ruta a la carpeta personal del usuario (ej. `/home/juan`).
- `$PWD` : Contiene el directorio de trabajo actual (donde estás parado).

Si hacés `echo $USER` en tu consola, no te devolverá la palabra "USER", sino tu nombre de usuario.

---

## 🛤️ La Variable más importante del Mundo: `$PATH`

Imaginá que estás parado en tu carpeta personal (`/home/juan`) y escribís el comando `ping google.com`. ¿Por qué funciona?
Como aprendimos en la [[02 - FHS (Estructura de Directorios)|Nota 02]], los programas (los binarios ejecutables) viven en la carpeta `/bin` o `/sbin`. El comando real es `/bin/ping`. 

¿Cómo sabe el sistema (Bash) que, al escribir la simple palabra `ping`, debe ir mágicamente a buscar el archivo en la carpeta `/bin/` para ejecutarlo?
**La respuesta es la variable de entorno `$PATH` (El Camino).**

El `$PATH` es una lista de directorios separada por dos puntos (`:`).
Si hacés `echo $PATH`, verás algo como:
`/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin`

**El proceso lógico:**
1. Vos escribís un comando desconocido (ej: `herramienta_hacker`).
2. Bash busca en la primera carpeta de la lista (ej. `/usr/local/sbin`). ¿Está ahí? No.
3. Pasa a la segunda carpeta. ¿Está ahí? No.
4. Así sucesivamente, hasta recorrer todo el `$PATH`.
5. Si lo encuentra, lo ejecuta. Si termina la lista y no lo encontró en ninguna carpeta, te devuelve el famoso error: `bash: herramienta_hacker: command not found`.

**Visión Hacking:** Si descargás un script ofensivo de internet y querés poder ejecutarlo desde cualquier lugar de tu computadora solo escribiendo su nombre (como si fuera un comando nativo), debés agregar la carpeta donde guardaste tu script a la variable `$PATH`.

---

## 🎭 Atajos: Alias

A veces escribís comandos muy largos cientos de veces al día, como `ls -la --color=auto`. Escribir eso agota.
Linux permite crear "apodos" o atajos llamados **`alias`**.

Un Alias le dice al Shell: *"De ahora en adelante, cada vez que yo escriba esta palabra corta, quiero que vos secretamente la reemplaces por este comando larguísimo antes de ejecutarla"*.

**Cómo crear un Alias:**
```bash
$ alias ll="ls -la --color=auto"
$ alias updatesistema="sudo apt update && sudo apt upgrade"
```
A partir de ahora, si escribo `updatesistema` y apreto Enter, la computadora actualizará todos los paquetes de software mágicamente.

**La persistencia del entorno:**
Las variables y los alias que creas en la terminal escribiéndolas **se borran apenas cerrás la ventana**. Viven solo en esa sesión.
Para que sean permanentes, debés escribir los alias (o los cambios al $PATH) dentro de un archivo de texto oculto que se ejecuta cada vez que abrís una terminal nueva. Ese archivo suele llamarse **`.bashrc`** o **`.zshrc`** y se encuentra oculto en tu carpeta `$HOME`.

---

## 📌 Must Know (Imprescindible)
- Qué es una variable de entorno y que se usa con un `$` adelante para leerla.
- La función exacta e indispensable de la variable `$PATH` (Indica en qué carpetas buscar los comandos y programas que el usuario teclea).
- El concepto de un `alias` para crear atajos.

---

## 🔄 Preguntas de repaso
1. Si creás un script ejecutable muy útil y lo guardás en la carpeta `/opt/mis_scripts/`, pero cuando estás en la carpeta de descargas (`/home/juan/Descargas`) e intentas llamarlo por su nombre la consola dice `command not found`, ¿qué variable del sistema tenés que modificar para solucionarlo?
2. Explicá con tus palabras qué sucedería matemáticamente si intentás ejecutar el comando: `cd $HOME`
3. Descubrís que en una terminal, al escribir el inocente comando `ping`, la computadora en realidad apaga el servidor intempestivamente (ejecutando `shutdown`). Suponiendo que el comando `ping` real de `/bin/ping` no fue modificado, ¿qué mecanismo pudo haber usado un compañero bromista o un atacante en tu archivo `.bashrc` para causar esto?

**➡️ Siguiente nota:** [[18 - Bash Scripting (Variables y Condicionales)]]
