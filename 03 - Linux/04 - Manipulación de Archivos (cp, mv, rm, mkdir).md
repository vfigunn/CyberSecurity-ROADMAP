# 04 - Manipulación de Archivos y Directorios

## 🎯 Objetivos
- Aprender a crear, copiar, mover, renombrar y borrar archivos o carpetas usando solo la terminal.
- Conocer los comandos `mkdir`, `cp`, `mv`, y `rm`.

---

## 🛠️ Comandos de Manipulación

Una vez que sabés dónde estás parado (`pwd`) y a dónde querés ir (`cd`), tenés que saber cómo interactuar con el entorno.

### 1. `mkdir` (Make Directory)
Crea una o varias carpetas nuevas (en el directorio actual, o en la ruta que le indiques).
```bash
$ mkdir Evidencias
$ mkdir /tmp/ataque_oculto
```
- *Truco:* Si querés crear una estructura de carpetas anidadas de golpe (ej. querés crear `2024/Enero/Reportes` pero `2024` ni siquiera existe todavía), usá el flag **`-p` (Parents)**: `mkdir -p 2024/Enero/Reportes`.

### 2. `cp` (Copy)
Copia un archivo (o carpeta) de un lugar a otro, conservando el original.
- **Sintaxis:** `cp [origen] [destino]`
```bash
$ cp reporte.txt reporte_copia.txt
$ cp /etc/passwd /tmp/passwd_robado
```
- **Copiar carpetas (El flag recursivo):** Por defecto, `cp` solo copia archivos sueltos. Si intentas copiar una carpeta llena de archivos, te dará un error (dirá "omitting directory"). Tenés que usar el flag **`-r` o `-R` (Recursivo)** para que copie la carpeta y todo su contenido interno.
  `cp -r /home/usuario/Documentos /tmp/Backup`

### 3. `mv` (Move)
Tiene **dos usos fundamentales** en Linux: Mover y Renombrar.
- **Mover (Cortar y Pegar):** 
  ```bash
  $ mv archivo.txt /tmp/
  ```
- **Renombrar:** En Linux no existe el comando "rename" (no comúnmente). Renombrar se considera "mover el archivo al mismo lugar donde está, pero con un nombre diferente".
  ```bash
  $ mv nombre_viejo.txt nombre_nuevo.txt
  ```

### 4. `rm` (Remove / Borrar)
Borra (elimina) un archivo. **Cuidado:** En la terminal de Linux, salvo que instales utilidades de terceros, **NO hay papelera de reciclaje**. Si hacés `rm`, el archivo se desvanece de inmediato.
```bash
$ rm informe_viejo.txt
```
Al igual que `cp`, por defecto `rm` no borra carpetas. 
- Para borrar una carpeta vacía, podrías usar `rmdir`.
- Para borrar una carpeta **llena** de archivos (y todo su contenido recursivamente), usas el poderoso y peligroso flag **`-r`**:
  `rm -r /tmp/Backup`
- Si la carpeta tiene muchos archivos bloqueados que te piden confirmación "Y/N" para cada uno, los administradores usan el flag **`-f` (Force / Forzar)** para borrarlos sin preguntar.

> [!caution] El comando prohibido: `rm -rf /`
> Si sumamos el flag recursivo (`-r`), el flag forzar (`-f`), y le damos como objetivo el Directorio Raíz (`/`), el sistema borrará absolutamente cada archivo del disco duro de la computadora sin hacer una sola pregunta, destruyendo el sistema operativo. *(Nunca lo ejecutes, ni siquiera en broma).*

---

## ❓ ¿Por qué importa en Seguridad?

Los atacantes (y los equipos de respuesta a incidentes) realizan estas operaciones todo el tiempo.
- **Post-Explotación:** Cuando un atacante vulnera un servidor, lo primero que hace es crear una carpeta oculta (ej. `mkdir .ssh_keys`) o mover un archivo malicioso a la carpeta temporal (`cp exploit.py /tmp/`).
- **Limpieza de Huellas (Covering Tracks):** Al finalizar su ataque, el hacker intentará borrar los registros de su actividad utilizando el comando `rm` (ej. `rm /var/log/auth.log`). Un buen analista defensivo sabe buscar qué se borró.

---

## 📌 Must Know (Imprescindible)
- La doble funcionalidad de `mv` (Mover y Renombrar).
- La necesidad del flag recursivo (`-r`) para copiar o borrar carpetas con contenido.
- Saber que no hay Papelera de Reciclaje al usar `rm`.

---

## 🔄 Preguntas de repaso
1. Si un atacante quiere descargar un archivo log (registro) de la carpeta `/var/log` para estudiarlo en su casa, pero no quiere arruinar el servidor, ¿qué comando debería utilizar para llevarse el archivo: `cp` o `mv`?
2. Intentas borrar una carpeta llamada `proyecto_viejo` haciendo `rm proyecto_viejo`, pero la terminal te devuelve el error: `rm: cannot remove 'proyecto_viejo': Is a directory`. ¿Cómo modificás el comando para lograr borrarla?
3. ¿Cómo usarías el comando `mv` para cambiar el nombre de una imagen de `foto.jpg` a `perfil.jpg` sin cambiarla de carpeta?

**➡️ Siguiente nota:** [[05 - Visualización de Textos (cat, less, head, tail)]]
