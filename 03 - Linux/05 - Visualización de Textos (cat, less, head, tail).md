# 05 - Visualización de Archivos de Texto

## 🎯 Objetivos
- Aprender a leer el contenido de archivos de texto directamente en la terminal (sin usar editores de texto gráficos ni Bloc de Notas).
- Conocer los comandos `cat`, `less`, `head` y `tail`.

---

## 🧠 Concepto

Como vimos en la [[02 - FHS (Estructura de Directorios)|Nota 02]], en Linux gran parte de la configuración y los logs son simplemente archivos de texto plano. Para investigarlos o administrarlos, pasamos mucho tiempo simplemente *leyendo* qué hay adentro. 

Para leerlos rápidamente usamos herramientas de visualización de consola, también conocidas como "Pagers".

---

## 🛠️ Comandos de Lectura

### 1. `cat` (Concatenate)
Es el comando más famoso para ver el contenido de un archivo. Toma todo el texto del archivo y te lo "escupe" de golpe en la pantalla de la terminal.
```bash
$ cat /etc/hostname
servidor-web-produccion
```
- *Problema con `cat`:* Si el archivo tiene 5.000 líneas (como un Log grande), `cat` imprimirá las 5.000 líneas en un milisegundo. Solo verás las últimas 50 líneas; las primeras habrán pasado volando hacia arriba y será imposible leerlas. `cat` es ideal solo para archivos pequeños (de 1 a 50 líneas).

### 2. `less` (Paginas Interactivas)
Resuelve el problema de `cat`. En lugar de escupir todo, te abre el archivo en un entorno de lectura a pantalla completa, mostrándote solo "la primera página".
```bash
$ less /var/log/syslog
```
**Cómo moverse dentro de `less`:**
- **Flechas Arriba/Abajo:** Bajar línea por línea.
- **Barra Espaciadora:** Bajar una página completa.
- **`/` (Barra):** Sirve para buscar texto. Apretá `/`, escribí la palabra `Error`, dale a Enter, y te saltará directo donde dice error. (Súper útil).
- **`q` (Quit):** Para salir de la lectura y volver a tu terminal normal.

*(Nota nerd: Existe un comando más viejo llamado `more`, pero el chiste en Linux es que "less is more" (menos es más), ya que `less` permite ir hacia arriba y hacia abajo, mientras que `more` solo permitía bajar).*

### 3. `head` (Cabeza / Inicio)
Imprime solamente **las primeras 10 líneas** de un archivo y se detiene.
Es muy útil cuando tenés un archivo desconocido inmenso y solo querés darle un vistazo rápido a la cabecera para entender qué tipo de datos contiene (por ejemplo, ver los títulos de las columnas en un archivo de Excel/CSV).
```bash
$ head usuarios.csv
```
- Si querés ver las primeras 25 líneas, podés modificarlo: `head -n 25 usuarios.csv`.

### 4. `tail` (Cola / Final)
El gemelo de head. Imprime solamente **las últimas 10 líneas** del archivo.
```bash
$ tail /var/log/auth.log
```
- **El superpoder de `tail -f` (Follow):**
  Este es uno de los comandos más usados por los defensores (Blue Team). El flag `-f` significa "Seguir". 
  Si haces `tail -f /var/log/auth.log`, el comando imprimirá las últimas líneas y *no se cerrará*. Se quedará "escuchando". Si en ese preciso instante un atacante intenta loguearse en el servidor, el archivo de Log se actualizará, y `tail -f` te imprimirá el nuevo intento de ataque **en vivo y en tiempo real** en tu pantalla. (Para salir presionás `Ctrl+C`).

---

## 📌 Must Know (Imprescindible)
- Conocer la limitación de `cat` para archivos largos.
- Saber cómo buscar palabras dentro de `less`.
- La utilidad de `head` y `tail` (especialmente `tail -f` para logs en vivo).

---

## 🔄 Preguntas de repaso
1. Tenés que investigar un archivo de Base de Datos de 5 Gigabytes de tamaño de puro texto. ¿Por qué utilizar el comando `cat` en lugar del comando `less` podría colgar temporalmente tu terminal?
2. Un desarrollador te dice: "Fijate en la terminal, voy a intentar iniciar sesión ahora mismo y decime si ves el error que genera". ¿Qué comando y con qué modificador deberías usar sobre el archivo de log para ver el error aparecer en el instante exacto en que ocurre?
3. ¿Qué tecla se debe presionar dentro del visualizador interactivo `less` para salir de él y volver al Shell normal?

**➡️ Siguiente nota:** [[06 - Búsqueda de Archivos (find, locate)]]
