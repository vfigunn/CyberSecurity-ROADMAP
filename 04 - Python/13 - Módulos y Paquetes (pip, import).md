# 13 - Módulos y Paquetes (`pip`, `import`)

## 🎯 Objetivos
- Comprender la filosofía de las "Pilas Incluidas" de Python y su Librería Estándar.
- Aprender a traer código de otros archivos a tu script usando `import`.
- Entender el rol de **PIP** como el instalador de software del ecosistema Python.

---

## 🧠 Concepto: No reinventar la rueda

Un analista de malware Junior quiere escribir un script en Python que analice si una IP es válida. Empieza a escribir 40 líneas de código con matemáticas complejas y divisiones para verificar que cada número de la IP no sea mayor a 255.
Un analista de malware Senior simplemente le dice a Python: *"Ey, traé el módulo oficial de Redes que ya escribió otro ingeniero de software brillante, y usalo"*. En 2 líneas de código resolvió el problema.

En programación, un **Módulo** es simplemente otro archivo `.py` que alguien más escribió, lleno de funciones y clases útiles, y que vos podés insertar dentro de tu propio código.

---

## 📦 1. La Librería Estándar (`import`)

Python viene, de fábrica, con una colección inmensa de módulos pre-instalados en tu computadora (Standard Library). Tienen soporte nativo para Matemáticas, Criptografía Básica, Sistemas Operativos, y Redes.

Para usar estos módulos, tenés que llamarlos explícitamente en **la primera línea de tu archivo** usando la palabra **`import`**.

```python
# Importamos el módulo "os" (Operating System) que viene pre-instalado
import os

# Ahora podemos usar todas las funciones que viven dentro del módulo "os"
# usando la notación de punto. (os.nombre_de_la_funcion)
directorio_actual = os.getcwd()
print(f"Estoy parado en: {directorio_actual}")
```

### Importaciones selectivas (`from ... import ...`)
A veces, un módulo tiene 500 funciones, pero vos solo querés usar una. Para no recargar la memoria, podés importar específicamente esa función.
*(Ejemplo: Importar solo el generador de números aleatorios)*
```python
from random import randint

# Ya no hace falta escribir "random.randint", podemos usarla directamente.
numero_ganador = randint(1, 100)
```

---

## 🌐 2. Paquetes Externos y `pip`

La librería estándar es genial, pero la comunidad de Ciberseguridad ha creado herramientas súper avanzadas que *no vienen instaladas* en Python por defecto (por ejemplo, la librería mágica para hacer peticiones web `requests`, o la librería `pwntools` usada para explotar binarios).

Así como en [[14 - Gestión de Paquetes (apt, dpkg)|Linux usamos el comando APT]] para conectarnos a los repositorios oficiales de Ubuntu e instalar software, el ecosistema Python tiene su propio sistema idéntico.

El repositorio mundial en Internet se llama **PyPI** (Python Package Index). 
La herramienta de la terminal (CLI) que usamos para conectarnos a él, descargar los módulos e instalarlos en nuestra PC se llama **`pip`** (Pip Installs Packages).

**Ejemplo de uso en la terminal de Linux:**
```bash
# Instalando un paquete (librería) desde Internet
$ pip install requests
```
*(A partir de ese momento, podés ir a tu script de Python, poner `import requests`, y funcionará).*

### Archivo `requirements.txt`
En ciberseguridad, a menudo descargarás herramientas de GitHub escritas por otros hackers. Si esa herramienta depende de 15 librerías externas para funcionar, no vas a hacer 15 comandos `pip` a mano.
Los desarrolladores incluyen un archivo llamado `requirements.txt` con la lista de todas las librerías necesarias.
Vos simplemente ejecutás en tu terminal:
```bash
$ pip install -r requirements.txt
```
PIP leerá el archivo de texto y descargará absolutamente todo lo necesario de manera automática.

---

## 📌 Must Know (Imprescindible)
- Cómo y por qué se usa el comando `import modulo` (generalmente en la línea 1 del archivo).
- La sintaxis de importación específica: `from modulo import funcion`.
- Qué es el PyPI, y qué es la herramienta `pip` (el instalador oficial de paquetes de Python).
- Para qué sirve el comando de terminal `pip install -r requirements.txt`.

---

## 🔄 Preguntas de repaso
1. Si un compañero escribe `math.sqrt(9)` en su script y Python arroja un `NameError` diciendo que "math no está definido", ¿qué línea de código vital se olvidó de escribir al principio del archivo?
2. Estás analizando el código fuente de una herramienta de Hacking y ves la línea: `import base64`. Explicá conceptualmente de dónde está sacando Python el código para hacer la decodificación, y si es necesario o no conectarse a Internet para que eso funcione (asumiendo que `base64` es parte de la librería estándar).
3. Descargaste el famoso escáner de vulnerabilidades (escrito en Python) de un repositorio de GitHub, pero al intentar ejecutarlo (`python escaner.py`) falla diciendo `ModuleNotFoundError: No module named 'shodan'`. Viendo que en la carpeta del escáner hay un archivo de texto llamado "requirements", ¿cuál es el comando exacto de terminal que debés ejecutar para solucionar el problema?

**➡️ Siguiente nota:** [[14 - Librerías Core I (os, sys, subprocess)]]
