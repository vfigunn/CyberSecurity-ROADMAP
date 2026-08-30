# 07 - Búsqueda en Textos (`grep`)

## 🎯 Objetivos
- Aprender a utilizar la herramienta más importante de análisis de texto en Linux: `grep`.
- Conocer los modificadores básicos (`-i`, `-v`, `-r`).
- Comprender su valor incalculable en la auditoría y respuesta a incidentes.

---

## 🧠 Concepto: La aguja en el pajar

Si tenés un archivo llamado `usuarios.txt` con 3 líneas, podés simplemente usar `cat` y leerlo con tus ojos. 
Pero en ciberseguridad, a menudo trabajamos con archivos de logs (ej. `/var/log/auth.log`) que pueden tener **2 millones de líneas** registrando cada acceso al servidor durante un año.

Si un analista quiere saber a qué hora exacta intentó loguearse el usuario "admin", no puede leer 2 millones de líneas con `less`. Necesita una herramienta que extraiga mágicamente solo las líneas que contienen la palabra "admin". Esa herramienta es **`grep` (Global Regular Expression Print)**.

---

## 🛠️ Uso básico de `grep`

La sintaxis básica es: `grep [Palabra_a_buscar] [Archivo]`

```bash
$ grep "admin" /var/log/auth.log
```
Ese comando ignorará los millones de líneas del log y solo te imprimirá en pantalla las líneas exactas donde aparezca la palabra "admin".

### Modificadores (Flags) vitales

1. **`-i` (Ignore Case):** 
   Linux es "Case Sensitive" (Sensible a mayúsculas/minúsculas). Si buscás `Error`, no encontrará `error` ni `ERROR`. El flag `-i` le dice a grep que no sea estricto con las mayúsculas.
   ```bash
   $ grep -i "error" /var/log/syslog
   ```

2. **`-v` (InVert Match):** 
   Este es el "filtro negativo". En lugar de mostrarte las líneas que *contienen* la palabra, te muestra todas las líneas del archivo **EXCEPTO** las que contienen la palabra.
   Es hiper útil para limpiar "ruido". Si tu log está lleno de conexiones correctas de la IP `192.168.1.50`, podés decirle a grep que te muestre todo *menos* esa IP, para ver qué más está pasando:
   ```bash
   $ grep -v "192.168.1.50" /var/log/nginx/access.log
   ```

3. **`-r` (Recursivo - Buscar en múltiples archivos):**
   A veces no sabés en *qué* archivo está la palabra. Con `-r` (o `-R`), podés decirle a grep que lea *todos los archivos* dentro de una carpeta buscando la palabra.
   *(Por ejemplo, un atacante guardó un script malicioso en la carpeta web con una contraseña oculta que dice "hacker123", pero no sabemos en qué archivo está).*
   ```bash
   $ grep -r "hacker123" /var/www/html/
   ```

---

## 🔎 Grep y Expresiones Regulares (RegEx) - Nivel Dios

`grep` es tan poderoso porque soporta "Expresiones Regulares" (RegEx). Las RegEx son patrones de búsqueda.

- En vez de buscar la palabra exacta "admin", podés decirle a grep usando símbolos matemáticos y comodines: *"Buscame cualquier línea que empiece con la letra A, seguida de tres números, y termine con un arroba"*.
- *Uso en Seguridad:* Podés escribir un RegEx en grep para que, en un archivo de texto de un millón de líneas, te extraiga automáticamente todos los patrones que parezcan **Direcciones IP** o **Tarjetas de Crédito** (Útil tanto para ofensiva robando datos, como para defensiva buscando qué IPs atacaron).

*(No entraremos en la complejidad de RegEx aquí, pero es una habilidad avanzada que todo senior debe aprender eventualmente).*

---

## 📌 Must Know (Imprescindible)
- Qué hace `grep` (Extrae líneas de texto que coinciden con una palabra).
- Los flags `-i` (ignorar mayúsculas) y `-v` (invertir).
- Entender que `find` (Nota 06) busca *nombres de archivos*, mientras que `grep` busca *contenido de texto DENTRO de los archivos*. Esta confusión es el error #1 de los principiantes.

---

## 🔄 Preguntas de repaso
1. Un compañero intenta buscar la palabra "Contraseña" dentro de un directorio lleno de documentos, y ejecuta: `find /Documentos -name "Contraseña"`. El comando no le devuelve nada, a pesar de que vos sabés que hay un archivo `notas.txt` que contiene esa palabra adentro. ¿Qué error conceptual cometió?
2. Escribí el comando de `grep` para buscar la palabra "Failed" (Fallido) en el archivo `/var/log/auth.log`, asegurándote de que encuentre "failed", "FAILED" o "Failed".
3. Tenés un archivo de texto llamado `empleados.csv` que tiene a todos los empleados de la empresa. Querés listar a todos los empleados, **excepto** a los del departamento de "Ventas". Escribí el comando `grep` que usarías (asumiendo que la palabra Ventas aparece en la línea del empleado).

**➡️ Siguiente nota:** [[08 - Redirecciones y Pipes]]
