# 05 - Inyección II (OS Command Injection)

## 🎯 Objetivos
- Entender el daño absoluto (Pwned / Compromiso Total) que significa ejecutar código en el Sistema Operativo subyacente.
- Conocer cómo las vulnerabilidades del diseño permiten escapar de la capa Web hacia la capa OS (Linux/Windows).
- Aprender a detectar vulnerabilidades inyectando operadores de consola.

---

## 🧠 Concepto: Escapando de la Prisión Web

En la nota anterior vimos SQL Injection. Su daño es enorme (te roban la base de datos), pero el hacker sigue estando limitado a "Hablar en lenguaje SQL". No puede usar ese SQL para prender fuego la red de la empresa o iniciar un malware en Windows.

**Command Injection (Inyección de Comandos del Sistema Operativo)** es el santo grial de la explotación remota (Remote Code Execution - RCE). 
Aquí, el atacante explota un error de diseño donde la página web (escrita en PHP o Python) le envía directamente el dato del usuario a la terminal del Sistema Operativo de la computadora subyacente (Ej: A Bash en Linux). 
El atacante logra ejecutar **comandos de terminal nativos (`ls`, `whoami`, `cat`)** logrando control total de la máquina.

---

## 💥 El Escenario: El Error de Diseño

Supongamos que una universidad tiene una herramienta en su portal interno para revisar si un servidor está encendido (Hacer un simple `Ping`).
La web tiene una barra que dice: `"Ingrese la IP a la cual hacerle Ping:"`.
El empleado (de buena fe) ingresa la IP `8.8.8.8` y presiona el botón.

Detrás de escena (en el Backend), el programador de la web, por pereza o ignorancia, en lugar de importar la librería especializada de red de Python, decidió usar el **Módulo `subprocess`** ([[04 - Python/14 - Librerías Core I (os, sys, subprocess)|Visto en la nota de Python]]), para invocar la herramienta de ping del propio Linux donde vive la web:

**Código peligroso del servidor:**
```python
import os
ip_recibida = "8.8.8.8" # El texto que escribió el usuario en la web

# Ejecuta el comando en la terminal Bash de Linux
os.system("ping -c 4 " + ip_recibida) 
```
El servidor ejecuta felizmente `ping -c 4 8.8.8.8`, atrapa el texto del resultado en Linux, y se lo muestra al usuario en la página web.

---

## 🏴‍☠️ El Ataque: Concatenando el Caos

El hacker se da cuenta de que la IP que tipea en la web está siendo copiada sin filtrar y tirada de cabeza dentro del terminal de Linux.
Para explotarlo, el Hacker recuerda la lección del [[03 - Linux/04 - Permisos, Servicios y Procesos|Módulo 03 de Bash]]: En Linux, si pones un punto y coma (`;`), un doble ampersand (`&&`) o barras verticales (`||`), **la consola detiene el primer comando y permite encadenar un segundo comando letal**.

**La Inyección:**
El atacante no escribe la IP sola. En la barra de la web, escribe esta obra de arte:
**`8.8.8.8; cat /etc/passwd`**

**La Destrucción:**
El Backend de Python ciego ejecuta el comando ensamblado enviándolo al sistema operativo Linux (Bash):
> `ping -c 4 `**`8.8.8.8; cat /etc/passwd`**

Bash lee la primera parte. Hace 4 pings inofensivos a la IP 8.8.8.8. 
¡Oh! Ve el punto y coma `;`. Sabe que terminó la primera orden.
Bash procede inmediatamente a ejecutar la segunda orden inyectada: `cat /etc/passwd`. (Lee el archivo madre donde residen los nombres de todos los usuarios vitales del servidor).
La página web, que prometía mostrarte un resultado de un Ping, en su lugar vomita en pantalla la información ultra secreta de las entrañas del servidor Linux.

Con esta técnica el hacker reemplazará el comando por un `wget` para descargar malware y ganar una **Reverse Shell** (Conexión persistente inversa), convirtiéndose en el amo absoluto de la máquina.

---

## ❓ Ceguera y Detección

Al igual que en SQLi (Ciego), a veces la página web NO te muestra el resultado del Ping en pantalla, sino que dice simplemente "El servidor está encendido" (Un Booleano).
Para detectar si el ataque funcionó (a ciegas), los Pentester inyectan Comandos de Tiempo del sistema operativo:
`8.8.8.8; sleep 15`
Si la página web demora exactamente 15 segundos en cargar y darte el cartel de éxito, significa que tu comando encadenado de Linux se procesó. **Bingo. Tienes Command Injection.**

---

## 📌 Must Know (Imprescindible)
- Qué significa Command Injection (Lograr que los inputs de la aplicación web caigan sin filtrar a la capa inferior de Bash/CMD, ejecutando instrucciones OS nativas).
- El pilar del ataque es el uso de **Operadores de Concatenación de Shell** como el punto y coma (`;`), el AND lógico (`&&`), o las tuberías (`|`) para inyectar la carga secundaria destructiva.
- Recordar comandos clave como `whoami`, `cat`, o `sleep` como los "PoC" (Proof of Concept - Prueba de concepto) favoritos que usamos los hackers para demostrar que pudimos inyectar código.

---

## 🔄 Preguntas de repaso
1. Un atacante se encuentra frente a un formulario web que exporta archivos PDF, el cual supuestamente envía los nombres de los archivos por debajo a una terminal de Linux que usa el comando `pdfinfo`. Sabiendo que a veces el punto y coma (`;`) es bloqueado/filtrado por algunos escudos web rudimentarios, ¿qué otros dos caracteres especiales (concatenadores) podrías probar inyectar al final del nombre del archivo en la web para intentar "saltar" y agregar tu comando `whoami`?
2. Si ejecutás exitosamente un OS Command Injection que te escupe en pantalla el resultado del comando inyectado `whoami`, pero el texto de respuesta muestra el usuario limitadísimo `www-data` (el usuario por defecto de servidores web Apache/Nginx, con nulos permisos sobre el sistema), ¿el servidor se encuentra a salvo del atacante? Explicá cuáles serían los próximos pasos naturales (Movimiento) del atacante tras conseguir este acceso restringido a la terminal.
3. El Comando Inyectado es la pesadilla máxima (Remote Code Execution). Si vos fueras el desarrollador/programador Backend a cargo del portal de Ping del ejemplo y tuvieras que mitigar este fallo de raíz, ¿cómo modificarías el manejo de la entrada del usuario (sanitización o uso de listas de control) antes de que esa entrada llegue al módulo `subprocess`?

**➡️ Siguiente nota:** [[06 - Cross-Site Scripting (XSS - Reflejado, Almacenado, DOM)]]
