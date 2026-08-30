# 14 - Librerías Core I (`os`, `sys`, `subprocess`)

## 🎯 Objetivos
- Conocer los módulos fundamentales (ya integrados) para interactuar con el Sistema Operativo (Linux/Windows).
- Aprender a leer los argumentos que el usuario tipea en la terminal usando `sys`.
- Saber cómo ordenar desde Python que se ejecute un comando real de Linux usando `subprocess`.

---

## 🧠 Concepto: Escapando de la caja

Por seguridad, los lenguajes de programación corren "aislados" del resto de la computadora. Un script de Python que solo sabe sumar 2+2 nunca podría crear una carpeta ni reiniciar tu computadora.
Para que tu script pueda "tocar" el sistema operativo (leer variables de entorno, borrar archivos del disco, lanzar binarios), necesitamos importar las librerías Core que actúan como puente hacia el Kernel de la máquina.

---

## 📂 1. El módulo `os` (Operating System)

Se usa para interactuar masivamente con el sistema de archivos (Filesystem). Te permite hacer con Python todo lo que aprendiste a hacer en la [[03 - Navegación Básica (cd, ls, pwd)|Nota de Navegación de Linux]].

```python
import os

# Obtener y cambiar el directorio de trabajo (pwd / cd)
ruta_actual = os.getcwd() 
print(f"Directorio actual: {ruta_actual}")

# Listar el contenido de una carpeta (ls)
archivos = os.listdir("/var/log/")

# Crear y Borrar carpetas (mkdir / rm)
# os.mkdir("carpeta_evidencia") 
# os.remove("archivo.txt")

# Leer variables de Entorno (Fundamental)
usuario = os.environ.get("USER")
```

---

## ⚙️ 2. El módulo `sys` (System-specific parameters)

Se usa principalmente para interactuar con el Intérprete de Python mismo, y con el momento exacto de la ejecución desde la terminal.

El uso más crítico en Ciberseguridad es **`sys.argv`** (Argument Values).
Si escribís una herramienta de escaneo de red (`escaner.py`), no querés obligar al usuario a abrir el código fuente del `.py` y modificar la variable "IP" cada vez que quiera usar el programa. Querés que el usuario pase la IP directamente desde la terminal de Linux, así: 
`$ python3 escaner.py 192.168.1.5`

`sys.argv` atrapa todas esas palabras escritas en la terminal y las guarda mágicamente en una Lista.
- Posición 0: Siempre es el nombre del script (`escaner.py`).
- Posición 1: Es la primera palabra o parámetro que escribió el usuario (`192.168.1.5`).

```python
import sys

# Validamos que el usuario nos haya pasado al menos 1 parámetro (Longitud de la lista == 2)
if len(sys.argv) != 2:
    print("Uso: python3 escaner.py <IP_OBJETIVO>")
    sys.exit(1) # Cierra el programa de inmediato por uso incorrecto

ip_objetivo = sys.argv[1]
print(f"Iniciando ataque a {ip_objetivo}...")
```

---

## 🚀 3. El módulo `subprocess` (El Lanzador)

Este módulo es una bomba nuclear. Te permite que tu script de Python **lance comandos reales de la terminal de Linux o Windows** (como hacer un `ping`, un `ls` o correr `nmap`) por debajo de la mesa, y capture el resultado en texto para procesarlo en Python.

*Escenario Red Team:* Tu script hace matemáticas para calcular todas las IPs de una subred, pero es más fácil dejar que la herramienta experta `nmap` instalada en Linux haga el escaneo real, así que la llamás desde Python.

```python
import subprocess

print("Lanzando escaneo Nmap por debajo de la mesa...")

# subprocess.run es la función moderna (Python 3.5+). 
# Recibe el comando como una Lista de strings.
resultado = subprocess.run(["nmap", "-sn", "192.168.1.1"], capture_output=True, text=True)

# El output real de la terminal de Linux quedó atrapado en nuestra variable de Python
print("Escaneo finalizado. Resultado:")
print(resultado.stdout)
```

---

## 📌 Must Know (Imprescindible)
- Las 3 librerías vienen incluidas; solo hay que hacerles `import` (no llevan PIP).
- Qué hace `os.getcwd()` y `os.listdir()` (equivalentes a `pwd` y `ls` en Linux).
- Entender profundamente la mecánica de `sys.argv[1]` para leer los parámetros que el analista escribió en la consola.
- La utilidad de `subprocess.run` para "escapar" de Python y ejecutar comandos puros del Sistema Operativo de la víctima.

---

## 🔄 Preguntas de repaso
1. Estás auditando un Malware de tipo Ransomware (Software de secuestro) escrito en Python. En su código, observás que importa masivamente el módulo `os` y usa un Bucle For acompañado de la función `os.listdir("C:\\Users\\")`. ¿Qué parte vital de su ataque está tratando de realizar en este bloque de código?
2. Diseñaste un script forense llamado `analizador.py`. Tu script requiere que el jefe de SOC (usuario de terminal) le envíe por consola la ruta del archivo que debe analizar (ej. `python3 analizador.py /var/log/auth.log`). Usando el módulo `sys`, ¿en qué posición específica de la lista (índice numérico) se encontrará el String `"/var/log/auth.log"`?
3. ¿Por qué un desarrollador de herramientas de Hacking preferiría utilizar el módulo `subprocess` para llamar a la herramienta externa Nmap desde Python, en lugar de intentar programar la compleja lógica del protocolo TCP para escanear puertos desde cero directamente en código Python?

**➡️ Siguiente nota:** [[15 - Librerías Core II (requests, socket, json)]]
