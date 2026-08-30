# Lab 05.1 - Cracking de Hashes con Python

## 🎯 Objetivo
Combinar lo aprendido en el [[04 - Python/00 - Overview|Módulo 04 de Python]] con la teoría de Hashing de este Módulo. 
Vas a programar (teóricamente) tu propia versión reducida de la famosísima herramienta de Hacking **John The Ripper**, creando un script de ataque de Fuerza Bruta basado en Diccionario.

---

## 📋 Requerimiento del Atacante

Acabas de lograr una inyección SQL en la página web objetivo y robaste de la base de datos la siguiente huella digital (Hash MD5) del usuario Administrador:
**`e10adc3949ba59abbe56e057f20f883e`**

Tu objetivo es encontrar la contraseña en texto plano que generó ese Hash.

El script (`cracker.py`) debe hacer lo siguiente:
1. Tener la variable objetivo: `hash_robado = "e10adc3949ba59abbe56e057f20f883e"`.
2. Leer un archivo físico llamado `diccionario.txt` (que asumes que tiene millones de contraseñas comunes separadas por salto de línea).
3. Iterar cada palabra del archivo usando un bucle.
4. Quitarle el salto de línea (`\n`) a la palabra extraída.
5. Convertir esa palabra a Bytes, calcularle su MD5 localmente, y convertirla a String hexadecimal.
6. Comparar el Hash calculado localmente con el `hash_robado` original.
7. Si son exactamente iguales, el script debe gritar "¡Contraseña encontrada!" e imprimirla en pantalla, para luego abortar el bucle inmediatamente.

---

## 🛠️ Procedimiento (Tu Trabajo)

Este laboratorio requiere que unas tus conocimientos criptográficos con tu lógica de programación defensiva/ofensiva. Tomate tu tiempo para estructurarlo.

**Tips / Pasos Lógicos:**
1. Importá la librería matemática: `import hashlib`.
2. Definí la variable del hash objetivo.
3. Usá el bloque seguro `with open(...)` para abrir el `diccionario.txt` en modo lectura (`r`).
4. Abrí tu bucle: `for palabra in archivo:`
5. Limpiá la palabra actual de sus espacios/saltos de línea usando: `palabra_limpia = palabra.strip()`
6. Convertila a bytes: `palabra_bytes = palabra_limpia.encode()`
7. Calculá el MD5: `hash_calculado = hashlib.md5(palabra_bytes).hexdigest()`
8. Abrí un bloque condicional: `if hash_calculado == hash_robado:`
9. Si coincide, imprimí la `palabra_limpia` y usá la palabra reservada `break` para destruir el bucle y no seguir leyendo el diccionario entero (ahorrando memoria y tiempo).

---

## 📝 Resultado Esperado (Autoevaluación)

¿Lograste encadenar la lógica? Revisá el código de ataque real.

> [!example]- Ver Código Solución del Cracker
> **Contenido de `cracker.py`:**
> ```python
> import hashlib
> 
> # 1. El objetivo extraído de la BD vulnerada
> hash_robado = "e10adc3949ba59abbe56e057f20f883e"
> 
> print("[*] Iniciando ataque de Diccionario MD5...")
> 
> try:
>     # 2. Abrimos la lista de contraseñas (Modo Read)
>     with open("diccionario.txt", "r") as archivo_diccionario:
>         
>         # 3. Leemos línea por línea (ideal para archivos pesados de 5GB)
>         for intento in archivo_diccionario:
>             
>             # 4. Limpiamos el salto de línea (\n)
>             intento_limpio = intento.strip()
>             
>             # 5. Aplicamos la licuadora matemática (MD5)
>             # Recordar: .encode() transforma el String a Bytes puros
>             hash_calculado = hashlib.md5(intento_limpio.encode()).hexdigest()
>             
>             # 6. Comparamos
>             if hash_calculado == hash_robado:
>                 print(f"[+] ¡ÉXITO! Contraseña encontrada: {intento_limpio}")
>                 break # Destruimos el bucle, nuestro trabajo terminó.
>                 
>         # Este 'else' pertenece al 'for', no al 'if'. 
>         # Se ejecuta solo si el bucle terminó TODAS sus vueltas sin ejecutar un 'break'.
>         else:
>             print("[-] Fracaso. La contraseña no estaba en el diccionario.")
>             
> except FileNotFoundError:
>     print("[!] Error: No se encontró el archivo 'diccionario.txt'.")
> ```
> 
> *Nota mental: La contraseña real detrás de ese hash es "123456". Si lo comprobás manualmente en una terminal, verás que la matemática cuadra perfecto.*

---
¡Excelente trabajo! Has construido una herramienta de Cracking funcional. Así es exactamente como operan herramientas legendarias como `Hashcat` y `John The Ripper`, pero ellas utilizan procesadores gráficos (GPUs) para realizar este mismo bucle de Python millones de veces por milisegundo.

**➡️ Siguiente nota:** [[13 - Ejercicios]]
