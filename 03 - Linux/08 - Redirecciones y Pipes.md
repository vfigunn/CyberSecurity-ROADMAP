# 08 - Redirecciones y Pipes (`>`, `>>`, `|`)

## 🎯 Objetivos
- Aprender cómo Linux maneja las entradas y salidas de los comandos.
- Entender cómo guardar los resultados de un comando en un archivo de texto (`>`).
- Descubrir el inmenso poder de encadenar comandos usando Pipes (`|`).

---

## 🧠 Concepto: La filosofía Unix

Una de las ideas fundamentales que hizo a Unix (y a Linux) tan poderoso en los años 70 es esta regla: 
**"Escribe programas pequeños que hagan solo una cosa, pero que la hagan a la perfección, y que puedan conectarse con otros programas."**

Por ejemplo, `ls` es perfecto para listar. `grep` es perfecto para buscar. `sort` es perfecto para ordenar alfabéticamente.
¿Qué pasa si querés listar, buscar y ordenar al mismo tiempo? En Windows, crearían un super-programa pesado que haga las tres cosas. En Linux, simplemente "pegamos" los tres programitas juntos usando tuberías (Pipes).

---

## 📥 Redirecciones (`>` y `>>`)

Por defecto, cuando ejecutas un comando (ej. `ping google.com`), el resultado (Output) se imprime en tu pantalla (Standard Output - *stdout*). 
Las redirecciones te permiten tomar ese texto y, en lugar de mostrarlo en pantalla, **guardarlo en un archivo**.

1. **El operador `>` (Sobrescribir):**
   Toma el resultado del comando y crea un archivo nuevo. **Si el archivo ya existe, borra todo su contenido anterior** y lo sobrescribe con lo nuevo.
   ```bash
   $ echo "Hola Mundo" > saludo.txt
   ```
   *(El comando `echo` simplemente repite lo que le decís. El `>` lo guarda en el archivo `saludo.txt` en lugar de imprimirlo).*

2. **El operador `>>` (Añadir / Append):**
   Hace lo mismo, pero es seguro. Si el archivo ya existe, **agrega el nuevo texto al final del archivo** sin borrar lo anterior.
   ```bash
   $ echo "Adiós Mundo" >> saludo.txt
   ```
   *(Ahora saludo.txt tiene dos líneas).*

*Uso en Ciberseguridad:* Un atacante escanea una red con `nmap`. En lugar de dejar que los resultados se pierdan en la pantalla, los redirige: `nmap 192.168.1.0/24 > ips_vivas.txt` para guardarlas como evidencia para más tarde.

---

## 🗑️ El agujero negro (`/dev/null`)

Recordarás de la nota de `find` que a veces los comandos escupen muchos errores molestos a la pantalla (Standard Error - *stderr*), como "Permiso denegado". 
Linux tiene un archivo virtual especial llamado `/dev/null`. Es literalmente un agujero negro. Todo texto que envíes ahí, desaparece para siempre, sin ocupar espacio en disco.

Podemos decirle a la terminal: *"Redirigí el Output de Error (representado por el número `2`) hacia el agujero negro"*.
```bash
$ find / -name "secreto.txt" 2>/dev/null
```
*(Este comando buscará limpio y perfecto, sin mostrar un solo mensaje de "Permiso denegado").*

---

## 🚰 Las Tuberías / Pipes (`|`)

Este es probablemente el operador más utilizado por un profesional de la consola en su día a día. El Pipe (la barra vertical `|`) te permite **tomar el resultado que sale de un comando, y meterlo como entrada del siguiente comando.** 

Es como una línea de ensamblaje en una fábrica.

**Ejemplo Práctico 1:**
Tenés una carpeta con 5,000 archivos. Hacés un `ls` y la pantalla se inunda. Solo te interesan los archivos que dicen "log".
```bash
$ ls | grep "log"
```
*Explicación:* `ls` genera la lista gigante, pero en lugar de imprimirla en pantalla, el Pipe `|` se la inyecta directo a `grep`. `grep` filtra la lista internamente y finalmente escupe solo los que dicen "log" en tu pantalla.

**Ejemplo Práctico 2 (Múltiples Pipes):**
Estás revisando los logs del servidor web. Querés extraer las IPs atacantes, contarlas, y ver cuáles son las que más atacaron.
```bash
$ cat access.log | grep "Failed password" | sort | uniq -c
```
*Explicación (Línea de ensamblaje):*
1. `cat`: Lee el inmenso archivo de log y se lo pasa a `grep`.
2. `grep`: Extrae solo las líneas que dicen "Contraseña fallida" y se las pasa a `sort`.
3. `sort`: Ordena esas líneas alfabéticamente (por IP) y se las pasa a `uniq`.
4. `uniq -c`: Cuenta cuántas veces se repite cada IP, e imprime el resumen final en pantalla.

¡Acabas de hacer un análisis de inteligencia de amenazas (Threat Intel) escribiendo una sola línea de código en 3 segundos! Este es el verdadero poder de Linux.

---

## 📌 Must Know (Imprescindible)
- Diferencia crítica entre `>` (sobrescribe/borra) y `>>` (añade al final).
- Entender el concepto del agujero negro `/dev/null` (específicamente `2>/dev/null` para errores).
- Cómo el Pipe `|` pasa la salida del comando izquierdo hacia el comando derecho.

---

## 🔄 Preguntas de repaso
1. Estás construyendo un script que guarda la hora actual en un archivo llamado `registro.txt` todos los días a las 8 AM. Si usás el comando `date > registro.txt`, ¿qué problema vas a tener el día martes cuando intentes leer el archivo? ¿Cómo lo corregirías?
2. Escribí un comando usando un Pipe (`|`) que lea el archivo `/etc/passwd` (usando `cat`) y luego filtre para mostrar únicamente la línea que contiene al usuario `root`.
3. ¿Para qué sirve enviar el "Output Error (2)" a `/dev/null` durante una búsqueda con el comando `find` en el Directorio Raíz (`/`)?

**➡️ Siguiente nota:** [[09 - Gestión de Usuarios y Grupos]]
