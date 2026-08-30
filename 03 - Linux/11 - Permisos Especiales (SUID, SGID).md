# 11 - Permisos Especiales (SUID, SGID y Sticky Bit)

## 🎯 Objetivos
- Comprender que los permisos básicos (rwx) a veces no son suficientes.
- Entender el concepto de SUID y por qué es el Santo Grial de los atacantes para escalar privilegios.
- Conocer el Sticky Bit para carpetas compartidas.

---

## 🧠 El Problema que Resuelven

Vimos los permisos estándar en la [[10 - Permisos de Archivos (chmod, chown)|nota anterior]]. 
Pero la arquitectura de Linux se encontró con un problema técnico: ¿Qué pasa cuando un usuario sin privilegios necesita hacer algo que requiere permisos de `root` por un segundo, pero no queremos darle acceso a todo el sistema con `sudo`?

*El mejor ejemplo es cambiar tu contraseña.*
El comando para cambiar contraseñas es `passwd`. Este comando toma tu nueva contraseña y la guarda en el archivo ultra-secreto `/etc/shadow`. Como vimos, el único usuario del mundo que tiene permiso de escritura (`w`) en `/etc/shadow` es `root`. 
Entonces, si vos, un usuario mortal, ejecutás `passwd`... tu ejecución debería fallar por "Permiso Denegado". Pero mágicamente funciona. ¿Por qué? Por el **SUID**.

---

## 🔴 1. SUID (Set User ID)

Es un permiso especial que se le aplica a un **programa ejecutable**.
- **Qué hace:** Cuando un usuario normal ejecuta ese programa, **el programa NO corre con los permisos del usuario que lo ejecutó, sino que corre "tomando prestados" los privilegios del Dueño del archivo.**
- En el caso de `passwd`, el dueño del archivo ejecutable es `root`. Tiene el bit SUID activado. Cuando vos ejecutas `passwd`, por esos 5 segundos, el programa corre con los poderes de Dios (`root`), permitiéndole modificar el archivo `/etc/shadow`.

**¿Cómo se ve?**
En un `ls -l`, en la sección del dueño (User), la `x` de ejecución es reemplazada por una letra **`s`** (minúscula).
`-rwsr-xr-x 1 root root passwd`

### ☠️ SUID y la Ciberseguridad (Escalada de Privilegios)
Esto es música para los oídos del Red Team. Si un atacante entra al servidor como un usuario sin privilegios (ej. `www-data`, el usuario del servidor web), su principal objetivo es convertirse en `root` (Privilege Escalation).
- Buscará todos los archivos del sistema que tengan la **`s`** activada. (`find / -perm -4000 2>/dev/null`).
- Si un administrador inexperto le puso el bit SUID a un programa común (como un editor de texto o el propio lenguaje Python) para "solucionar un problema rápido", el atacante puede ejecutar ese programa y ordenar a través de él que se le entregue un Shell con permisos totales (ej. `python -c 'import os; os.execl("/bin/sh", "sh", "-p")'`). ¡Jaque mate!

---

## 🟢 2. SGID (Set Group ID)

Funciona exactamente igual que el SUID, pero para los Grupos.
Si un ejecutable tiene el SGID, corre tomando prestados los permisos del *Grupo Dueño* del archivo, y no los del usuario que lo corrió.

Su uso más interesante está en las **Carpetas Compartidas**.
- Si a una carpeta se le aplica SGID, **cualquier archivo nuevo que alguien cree dentro de ella, heredará automáticamente el Grupo de la carpeta, en lugar del grupo del usuario que lo creó.**
- Es genial para departamentos. Si creás la carpeta `/Ventas` con el grupo "ventas" y le aplicas SGID... cuando Juan cree un Excel adentro, María también podrá modificarlo, porque el archivo pertenecerá al grupo "ventas" por defecto, y no al grupo personal de "juan".

**¿Cómo se ve?**
La `x` de ejecución en el bloque del Grupo (Group) se reemplaza por una **`s`**.
`drwxrwsr-x 2 root ventas 4096 /Ventas`

---

## 🟡 3. Sticky Bit

Imaginá una carpeta pública (como `/tmp`) donde le dimos permisos a "Otros" de lectura, escritura y ejecución (`chmod 777 /tmp`). 
El problema: El permiso de Escritura (`w`) en un directorio significa que cualquiera puede crear archivos... pero también significa que **cualquiera puede borrar cualquier archivo**, incluso si no son suyos.

El **Sticky Bit** soluciona esto. Se aplica a las carpetas públicas.
- **Qué hace:** Obliga a que los archivos dentro de esa carpeta **solo puedan ser borrados por sus respectivos dueños** (o por `root`). Así prevenís que Juan le borre los archivos a María en la carpeta compartida.

**¿Cómo se ve?**
La `x` de ejecución en el bloque de Otros (Others) se reemplaza por una **`t`**.
`drwxrwxrwt 10 root root 4096 /tmp`

---

## 📌 Must Know (Imprescindible)
- Qué es el SUID (Ejecutar tomando los permisos del Dueño) y cómo se ve en un `ls -l` (la letra `s`).
- Por qué buscar ejecutables con SUID mal configurados es el vector principal de Escalada de Privilegios en el pentesting de Linux.
- Para qué sirve el Sticky Bit en carpetas como `/tmp`.

---

## 🔄 Preguntas de repaso
1. Estás auditando la seguridad de un servidor Linux. Ejecutás un escaneo y encontrás un archivo sospechoso con estos permisos: `-rwsr-xr-x`. El dueño del archivo es `root`. ¿Qué ocurriría a nivel de privilegios del sistema si un atacante logra ejecutar ese archivo?
2. ¿Por qué es fundamental que la carpeta de archivos temporales del sistema (`/tmp`) tenga activado el Sticky Bit (`t`) en lugar de ser simplemente `777`?
3. En el comando `ls -l` de la carpeta compartida del área de Finanzas, ves esto: `drwxrwsr-x`. ¿Qué permiso especial está activo (SUID o SGID) y cómo afecta a los nuevos archivos de balances (Excel) que se creen allí adentro?

**➡️ Siguiente nota:** [[12 - Gestión de Procesos (ps, top, kill)]]
