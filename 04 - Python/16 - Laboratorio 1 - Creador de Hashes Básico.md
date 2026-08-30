# Lab 04.1 - Creador de Hashes Básico

## 🎯 Objetivo
Aplicar los conocimientos de Sintaxis, Strings, Importación de Librerías Core y Manejo de I/O de archivos. Escribirás (de forma teórica) un script que simula una herramienta crítica en el análisis forense: el generador de Hashes.

---

## 🧠 Concepto Rápido: Hashes
Un Hash (algoritmo como MD5 o SHA256) es como una "huella digital" matemática de un archivo o un texto. 
- Si al texto "contraseña" le aplicas la matemática de MD5, te dará: `cb...412`.
- El hash es "unidireccional": no podés descifrar el hash para volver a obtener la palabra original. Por eso las empresas guardan los hashes de tus contraseñas, y no las contraseñas en texto claro (Veremos esto a fondo en el Módulo de Criptografía).

---

## 📋 Requerimiento del Negocio

El equipo de Respuesta a Incidentes necesita que programes una pequeña utilidad en Python llamada `creador_hashes.py`.

El objetivo del programa es:
1. Usar el módulo nativo de Python `hashlib` para hacer matemáticas.
2. Pedirle al usuario por pantalla que tipee una palabra cualquiera.
3. Transformar esa palabra (String) aplicando el algoritmo **MD5** de la librería.
4. Imprimir la huella digital (el hash) final en la pantalla.
5. De forma silenciosa, Guardar/Escribir ese mismo hash al final de un archivo llamado `registro_hashes.txt` (sin borrar los que se hayan calculado ayer).

---

## 🛠️ Procedimiento (Tu Trabajo)

Tomá un papel o block de notas virtual. Intentá estructurar el código basándote en todo lo aprendido en el Módulo 04.

**Tips / Pasos Lógicos:**
1. Importá la librería necesaria en la línea 1: `import hashlib`
2. Usá la función nativa de Python `input()` para pedir algo por pantalla y guardarlo en una variable: 
   `palabra = input("Ingrese texto: ")`
3. Los algoritmos matemáticos como MD5 no procesan Letras, procesan Bytes puros. Tenés que obligar (Castear / Codificar) la palabra (String) a Bytes usando el método de string llamado `.encode()`. (Ej: `palabra.encode()`).
4. Generá el objeto Hash usando la librería (el molde): `hash_obj = hashlib.md5(palabra_codificada_en_bytes)`
5. Extraé la huella digital final (el texto hexadecimal) usando el método `.hexdigest()` sobre el objeto, y guardala en una variable final.
6. Imprimila con un `print(f"...")` amigable (F-String).
7. Abrí el archivo de texto en el **Modo correcto** usando el bloque `with open()`.
8. Escribí la variable final dentro del archivo agregando un salto de línea (`\n`).

---

## 📝 Resultado Esperado (Autoevaluación)

¿Lograste encadenar la lógica? Revisá la solución.

> [!example]- Ver Código Solución del Script
> **Contenido de `creador_hashes.py`:**
> ```python
> import hashlib
> 
> print("--- HERRAMIENTA DE HASHING FORENSE ---")
> 
> # Pedimos el texto al usuario
> texto_claro = input("Ingrese la palabra o contraseña a procesar: ")
> 
> # Convertimos el texto normal a Bytes para que hashlib lo entienda
> texto_en_bytes = texto_claro.encode()
> 
> # Creamos el objeto instanciando la clase MD5 de la librería
> objeto_hash = hashlib.md5(texto_en_bytes)
> 
> # Extraemos el String matemático final ("el resultado")
> hash_final = objeto_hash.hexdigest()
> 
> # Imprimimos en pantalla al usuario usando F-Strings
> print(f"-> La huella digital MD5 es: {hash_final}")
> 
> # Procedemos a la persistencia (Guardado en Disco Duro)
> # Usamos modo "a" (Append) para no borrar los registros anteriores
> with open("registro_hashes.txt", "a") as archivo:
>     archivo.write(f"Texto: {texto_claro} | MD5: {hash_final} \n")
> 
> print("Hash guardado en el archivo de registros con éxito.")
> ```
> 
> *Si guardás este código en un archivo y lo ejecutás en la terminal con `python3 creador_hashes.py`, funcionará perfectamente y creará tu archivo de texto.*

---
¡Excelente! Has integrado I/O de usuario, manipulación de Strings, importación de librerías criptográficas e I/O de archivos en menos de 20 líneas de código.

**➡️ Siguiente nota:** [[17 - Laboratorio 2 - Escáner de Puertos Básico]]
