# 20 - Resumen (Cheat Sheet - Python)

Esta nota agrupa los conceptos, sintaxis y librerías vitales del lenguaje Python para su rápido uso en la creación de herramientas de hacking y automatización defensiva.

---

## 🏗️ Sintaxis y Datos Fundamentales
- **Indentación:** Obligatoria. 4 espacios para definir qué pertenece a un bloque (`if`, `for`, `def`).
- **Comentarios:** `# Una línea` o `""" Varias líneas """`.
- **F-Strings:** Para inyectar variables en textos de manera limpia.
  `print(f"El puerto {puerto} está abierto.")`
- **Conversión (Casting):** `int("443")` (De texto a número), `str(443)` (De número a texto).

---

## 📦 Estructuras de Datos (Colecciones)
- **Listas `[ ]`:** Mutables y ordenadas. Los índices empiezan en 0.
  `lista.append(item)` (Añade al final) | `lista[0] = nuevo_valor` (Sobrescribe).
- **Tuplas `( )`:** Inmutables (de Solo Lectura). Más rápidas. Excelentes para IPs fijas.
- **Sets `{ }`:** Eliminan duplicados matemáticamente.
- **Diccionarios `{ }`:** Pares `"Clave": Valor`. 
  `dict.get("clave")` es más seguro que `dict["clave"]` para evitar errores si la clave no existe.

---

## 🚦 Control de Flujo (Lógica)
- **Operadores Lógicos:** `and` (Todo cierto), `or` (Alguna cierta), `not` (Inversión).
- **If / Elif / Else:**
  ```python
  if condicion == True:
      hacer_algo()
  elif otra_condicion:
      hacer_otra_cosa()
  else:
      defecto()
  ```
- **Bucle For:** Iterar sobre cada elemento de una lista o un rango de números.
  `for ip in lista_ips:` o `for puerto in range(1, 1025):`
- **Bucle While:** Repite mientras la condición sea cierta. `while True:` hace un bucle infinito (Listeners).
  `break` destruye el bucle por completo. `continue` salta a la siguiente vuelta.

---

## 🧩 Funciones, Scope y Errores
- **Definición:** `def mi_funcion(argumentos):`. Todo lo de adentro es Scope Local.
- **Retorno:** El `return` expulsa un resultado y destruye la función en el acto.
- **Manejo de Errores Seguros:**
  ```python
  try:
      # Código peligroso de red
  except Exception as e:
      print(f"Error evitado: {e}")
  ```

---

## 📂 Archivos (I/O)
- **Modos:** `"r"` (Lectura), `"w"` (Sobrescribe/Borra todo y escribe), `"a"` (Añade al final seguro).
- **Context Manager (Cierra automático):**
  ```python
  with open("archivo.txt", "r") as f:
      for linea in f:
          print(linea.strip()) # strip() limpia los saltos de línea invisibles
  ```

---

## 🌐 Librerías Core (Módulos)
- **`import sys`**: Interactuar con la terminal.
  - `sys.argv[1]` captura la primera palabra/parámetro que el usuario tecleó al ejecutar el script.
- **`import os`**: Sistema Operativo de archivos.
  - `os.getcwd()` (Dónde estoy) | `os.listdir()` (Ver carpeta).
- **`import subprocess`**: Lanza comandos de Linux ocultos (`subprocess.run(["ping", "8.8.8.8"])`).
- **`import json`**:
  - `json.loads(string)`: Convierte Texto de API a Diccionario Python.
  - `json.dumps(dict)`: Convierte Diccionario a Texto (Serializa).
- **`import requests`**: (Requiere `pip install requests`). Peticiones HTTP amigables.
  `res = requests.get(url)` -> Analizás `res.status_code` y `res.text`.
- **`import socket`**: Hacking de red a bajo nivel (TCP/UDP, Puertos).
  ```python
  s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
  s.settimeout(1)
  s.connect((ip, puerto)) # Recibe una TUPLA!
  ```

---
🎉 **¡Felicitaciones por dominar la Programación Básica en Python (Fase 5)!**
Ya tenés las herramientas mentales para crear automatizaciones, leer Exploits y manipular datos masivos. Actualizá tu archivo [[Progreso]] y preparate para cruzar a la ofensiva en el [[05 - Criptografía/00 - Overview|Módulo 05 - Criptografía y Hashes]].
