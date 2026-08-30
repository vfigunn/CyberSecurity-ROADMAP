# 03 - Navegación Básica (pwd, ls, cd)

## 🎯 Objetivos
- Aprender a moverte por el sistema de archivos sin usar ventanas ni mouse.
- Diferenciar entre Rutas Absolutas y Rutas Relativas (Concepto crítico).
- Dominar los comandos `pwd`, `ls` y `cd`.

---

## 🧠 Concepto: Estás en un solo lugar a la vez

En la interfaz gráfica, podés tener abiertas tres carpetas distintas en tres ventanas. 
En la Terminal de comandos, estás "físicamente parado" en una sola carpeta a la vez. Esa carpeta se llama tu **Directorio de Trabajo Actual (Current Working Directory - CWD)**.
Cualquier comando que ejecutes (como "crear un archivo" o "borrar un archivo") ocurrirá en ese lugar donde estás parado, a menos que especifiques lo contrario.

---

## 🛠️ Comandos de Navegación

### 1. `pwd` (Print Working Directory)
Te dice exactamente dónde estás parado ahora mismo. Es tu brújula.
```bash
$ pwd
/home/maria/Documentos
```

### 2. `ls` (List)
Lista (te muestra) el contenido de la carpeta en la que estás. Sería el equivalente a hacer doble clic en una carpeta para ver qué hay adentro.
```bash
$ ls
archivo.txt   fotos   informe.pdf
```
**Modificadores (Flags) comunes de `ls`:**
*(En Linux, podés cambiar el comportamiento de un comando pasándole banderas u opciones, usualmente con un guion).*
- **`ls -l` (Formato largo):** Te muestra detalles de los archivos: permisos, dueño, tamaño y fecha de modificación. (Fundamental en seguridad).
- **`ls -a` (All/Todos):** En Linux, **cualquier archivo o carpeta que empiece con un punto (ej. `.secreto.txt`) está Oculto**. Un `ls` normal no lo muestra. Para ver los archivos ocultos, tenés que usar `ls -a`.
- *Combinación:* Podés sumar flags: **`ls -la`** te mostrará todo en formato largo, incluyendo los ocultos.

### 3. `cd` (Change Directory)
Te permite viajar y cambiar tu directorio actual. Es el comando de movimiento.
```bash
$ cd /var/log
```

**Atajos mágicos con `cd`:**
- **`cd ..` (Dos puntos):** Te hace subir o retroceder un nivel (hacia la carpeta padre). Si estás en `/home/maria/fotos` y escribís `cd ..`, pasarás a estar en `/home/maria`.
- **`cd .` (Un punto):** El punto representa "la carpeta actual". (Lo usaremos más adelante para ejecutar scripts).
- **`cd ~` (O simplemente `cd` vacío):** La tilde (Virgulilla) representa tu carpeta "Home". No importa en qué parte del disco duro estés, si escribís `cd ~`, te teletransportás instantáneamente a tu casa (ej. `/home/maria`).
- **`cd -` (Guion):** Te devuelve exactamente a la última carpeta donde estuviste antes de saltar. (Botón "Atrás" del navegador).

---

## 🗺️ Rutas Absolutas vs Rutas Relativas (¡Importante!)

Este concepto es fuente del 90% de los errores cuando la gente empieza a usar Linux.

**Ruta Absoluta:** Es la dirección *completa* de un archivo, empezando siempre desde el Directorio Raíz (`/`). No importa en qué lugar de la PC estés parado, la ruta absoluta nunca cambia y siempre funcionará.
*Ejemplo:* `cd /var/log/apache2`

**Ruta Relativa:** Es la dirección de un archivo *partiendo desde donde estás parado en este momento*. Por lo tanto, NO empieza con la barra (`/`).
*Ejemplo:* Si actualmente estás parado en `/var`, y querés ir a `/var/log`, podés usar una ruta relativa y simplemente decir:
`cd log` (El sistema asume que te referís a "la carpeta log que está acá adentro conmigo").

> [!warning] El error clásico
> Si estás parado en `/home` y escribís `cd /log`, el sistema te dará un error ("No existe el archivo o directorio"). ¿Por qué? Porque al poner la barra inicial `/`, le estás dando una ruta absoluta. Le estás diciendo "Andá al inicio del disco duro (Root) y buscá la carpeta log". Y ahí no existe. Lo correcto (relativo) era `cd log`.

---

## 📌 Must Know (Imprescindible)
- Los comandos `pwd`, `ls` y `cd`.
- Cómo ver archivos ocultos (`ls -a`).
- La diferencia entre un punto (`.`) y dos puntos (`..`).
- Diferencia técnica entre Ruta Absoluta y Relativa.

---

## 🔄 Preguntas de repaso
1. Estás en la carpeta `/etc/ssh` y querés volver rápidamente a tu carpeta personal (`/home/tu_usuario`). ¿Cuál es el atajo más rápido (de uno o dos caracteres) usando el comando `cd`?
2. ¿Por qué el comando `ls` a secas no te mostraría un archivo que un atacante nombró intencionalmente como `.historial_comandos`?
3. Si actualmente tu CWD (Directorio de Trabajo) es `/var/log`, e intentas hacer `cd /auth`, te da error. Si hacés `cd auth` (sin la barra), funciona. ¿Por qué el primero falló y el segundo tuvo éxito? Explicá la teoría de rutas.

**➡️ Siguiente nota:** [[04 - Manipulación de Archivos (cp, mv, rm, mkdir)]]
