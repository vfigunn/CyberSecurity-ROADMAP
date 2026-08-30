# 10 - Permisos de Archivos (`chmod`, `chown`)

## 🎯 Objetivos
- Dominar el sistema de permisos básico de Linux (UGO - User, Group, Others).
- Entender los tres permisos fundamentales: Lectura (r), Escritura (w) y Ejecución (x).
- Aprender a interpretar y modificar permisos usando `chmod` (Modo Simbólico y Modo Octal/Numérico).
- Aprender a cambiar dueños con `chown`.

---

## 🧠 Concepto: La Tripleta de Seguridad (UGO)

Cada archivo y cada directorio en Linux tiene un Dueño (el usuario que lo creó) y un Grupo Dueño.
Linux decide quién puede hacer qué dividiendo al mundo entero en tres categorías:

1. **U (User / Dueño):** El individuo propietario del archivo.
2. **G (Group / Grupo):** Cualquier usuario que pertenezca al grupo asignado al archivo.
3. **O (Others / Otros):** Literalmente, el resto del planeta. Cualquier persona en el sistema que no sea ni el Dueño ni esté en el Grupo.

A cada una de estas tres categorías se le pueden otorgar, de manera independiente, 3 tipos de permisos:
- **r (Read / Lectura):** Permite ver el contenido del archivo (`cat`). En un directorio, permite ver qué hay adentro (`ls`).
- **w (Write / Escritura):** Permite modificar, sobrescribir o vaciar el archivo. En un directorio, permite crear o borrar archivos dentro de él.
- **x (eXecute / Ejecución):** Permite correr el archivo como un programa o un script (ej. un malware). En un directorio, permite entrar en él (`cd`).

---

## 👁️ Leyendo los permisos (`ls -l`)

Cuando hacés un `ls -l`, ves una columna llena de letras locas. Vamos a diseccionarla:

```text
-rw-r--r-- 1 juan devs 1024 Ago 30 secreto.txt
```

La primera cadena de 10 caracteres (`-rw-r--r--`) es el mapa de permisos. Se lee separando el primer carácter, y luego leyendo de a 3 (User, Group, Others).

1. ` - ` : El primer carácter dice qué tipo de archivo es. Un guion (`-`) es un archivo normal. Una `d` indicaría que es un directorio.
2. ` rw- `: Permisos del **Dueño** (Juan). Tiene lectura (`r`) y escritura (`w`). No tiene ejecución (`-`).
3. ` r-- `: Permisos del **Grupo** (devs). Solo pueden leer (`r`).
4. ` r-- `: Permisos de **Otros** (el resto del mundo). Solo pueden leer (`r`).

*Conclusión de seguridad:* Cualquier persona que inicie sesión en este servidor puede leer el secreto de Juan, pero solo Juan puede modificarlo, y nadie puede ejecutarlo.

---

## 🛠️ Modificando Permisos (`chmod`)

Para cambiar los permisos usamos el comando **`chmod`** (Change Mode). Hay dos formas de usarlo: la fácil (Simbólica) y la profesional (Octal).

### 1. Modo Simbólico (Letras)
Le decís a quién (u, g, o, a para "todos"), le sumás (`+`) o restás (`-`) qué permiso (r, w, x).
- Quiero **darle** ejecución al **Dueño**:
  `chmod u+x secreto.txt`
- Quiero **quitarle** lectura a **Otros** (para mejorar la seguridad):
  `chmod o-r secreto.txt`
- Quiero **darle** ejecución a **Todos** (all):
  `chmod a+x script.sh` (o directamente `chmod +x script.sh`)

### 2. Modo Octal / Numérico (El que usan los pros)
Es más rápido y permite configurar los tres bloques de un solo golpe usando matemáticas. Cada letra vale un número:
- **r (Read) = 4**
- **w (Write) = 2**
- **x (eXecute) = 1**
- (Sin permiso) = 0

Para dar permisos, **sumás** los números para cada bloque (Dueño, Grupo, Otros).
Ejemplo: Queremos darle Lectura+Escritura (4+2 = **6**) al dueño. Solo Lectura (4) al Grupo. Ningún permiso (0) al resto.
- El comando será: `chmod 640 secreto.txt`

**Números Mágicos (Aprender de memoria):**
- **`777`:** Permiso absoluto (rwx) para todo el mundo. **Pésima práctica de seguridad.** Si un manual de internet te dice que arregles un error haciendo un `chmod 777` en un servidor web, están abriendo tu servidor a atacantes.
- **`755`:** Clásico para programas/directorios. El dueño hace todo (7). El resto solo lee y ejecuta (5).
- **`644`:** Clásico para archivos de texto/configuración. El dueño lee y escribe (6). El resto solo lee (4).

---

## 👤 Cambiando de Dueño (`chown`)

El creador del archivo es el dueño. Pero a veces el Administrador (`root`) necesita cederle la propiedad a otra persona (ej. a la base de datos). Usamos **`chown`** (Change Owner).
- **Sintaxis:** `chown [usuario]:[grupo] archivo`
- *Ejemplo:* Asignarle el archivo a maria, y al grupo contabilidad.
  `sudo chown maria:contabilidad financiero.xlsx`

---

## 📌 Must Know (Imprescindible)
- Qué significa U, G, O (Dueño, Grupo, Otros).
- Qué significa r, w, x (Lectura 4, Escritura 2, Ejecución 1).
- Entender cómo calcular un permiso Octal (ej. 755 o 640).
- Por qué el permiso `777` es un desastre de seguridad para archivos sensibles (pierde Confidencialidad y pierde Integridad simultáneamente).

---

## 🔄 Preguntas de repaso
1. Analizá los permisos del siguiente archivo: `-rwxrw-r--`. ¿Qué número Octal representan estos permisos?
2. Si intentas ejecutar en la consola un archivo llamado `instalar.sh` usando el comando `./instalar.sh`, pero la terminal te devuelve `Permission denied` (Permiso denegado) a pesar de que vos sos el creador y dueño del archivo, ¿qué permiso exacto te falta y cómo se lo darías usando `chmod` en modo simbólico?
3. Ejecutas un `ls -l` y ves que la carpeta compartida de un proyecto tiene permisos de directorio `drwxr-xr-x`. Explicá qué pueden hacer los usuarios de la categoría "Otros" (Others) con esa carpeta basándote en la teoría.

**➡️ Siguiente nota:** [[11 - Permisos Especiales (SUID, SGID)]]
