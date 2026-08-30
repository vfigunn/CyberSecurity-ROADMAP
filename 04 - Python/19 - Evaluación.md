# 19 - Evaluación del Módulo 04 (Python)

## 📝 Instrucciones
Evaluación final del módulo. Múltiples preguntas basadas en escenarios de código, sintaxis y uso de librerías en un contexto de seguridad.
Anotá tus respuestas y luego verificalas con el solucionario.

> **Importante:** Las respuestas correctas y explicaciones están en: `[[15 - Evaluaciones/Respuestas/Módulo 04 - Respuestas]]`.

---

## 🎯 Sección 1: Sintaxis y Tipos de Datos

**1. ¿Qué pasará si ejecutás un script en Python que contiene una declaración `if` pero olvidaste ponerle los 4 espacios de indentación al bloque de código que va justo debajo?**
A) Python automáticamente arreglará la indentación y ejecutará el script.
B) El bloque de código no se ejecutará, pero el resto del script sí.
C) El intérprete lanzará un `IndentationError` (Error de Sintaxis) de inmediato y no ejecutará nada.
D) El bloque de código se ejecutará siempre, ignorando la condición del `if`.

**2. Querés guardar una lista de Direcciones IP de servidores críticos que NO deben ser modificadas (añadidas ni borradas) accidentalmente por tu programa bajo ninguna circunstancia. ¿Qué estructura de datos es inmutable y deberías elegir?**
A) Lista `[ ]`
B) Diccionario `{ }`
C) Tupla `( )`
D) Set `{ }`

**3. Si el atacante envía datos maliciosos por la URL, el puerto a atacar te llega en tu variable como un String: `puerto = "443"`. ¿Cuál es la forma correcta de convertirlo a un número entero antes de pasárselo a tu socket?**
A) `puerto_numerico = str(puerto)`
B) `puerto_numerico = integer(puerto)`
C) `puerto_numerico = int(puerto)`
D) `puerto_numerico = float(puerto)`

---

## 🎯 Sección 2: Control de Flujo y Funciones

**4. Analizá el siguiente código. ¿Cuál será la salida impresa en consola?**
```python
intentos = 5
if intentos < 3:
    print("Bajo Riesgo")
elif intentos == 5:
    print("Riesgo Medio")
else:
    print("Alto Riesgo")
```
A) Bajo Riesgo
B) Riesgo Medio
C) Alto Riesgo
D) Dará error de sintaxis

**5. Si creás una variable `password_descifrada = "1234"` ADENTRO de la función `crackear_hash()`, y luego intentás hacer `print(password_descifrada)` DESDE AFUERA de la función, en el script principal, ¿qué sucederá?**
A) Imprimirá "1234".
B) Imprimirá un error, porque la variable pertenece a un Scope Local y el script principal no puede verla.
C) El script principal borrará la variable.
D) Imprimirá "None".

**6. Cuando utilizás el bloque moderno `with open("archivo.txt", "r") as f:`, ¿qué ventaja crítica de seguridad/rendimiento te provee automáticamente Python en comparación al método antiguo de `f = open(...)`?**
A) Encripta el archivo al terminar de leerlo.
B) Asegura que el archivo se cierre automáticamente al terminar el bloque, incluso si ocurre un error en el medio.
C) Modifica los permisos del archivo en Linux a 777.
D) Borra el archivo de forma segura tras ser leído.

---

## 🎯 Sección 3: Librerías y Escenarios

**7. Sos un analista forense y recibís un volcado de logs masivo en formato texto `JSON` desde un servidor atacado. ¿Qué módulo preinstalado en Python y qué función específica deberías usar para transformar rápidamente ese texto crudo en un Diccionario manejable?**
A) Módulo `json`, función `json.loads()`
B) Módulo `requests`, función `requests.get()`
C) Módulo `json`, función `json.dumps()`
D) Módulo `sys`, función `sys.argv()`

**8. Tu script de escaneo automatizado (`escaner.py`) debe procesar la IP que el usuario escribe en la terminal de Linux, como en el comando: `python3 escaner.py 10.0.0.5`. ¿Qué lista del módulo `sys` captura los argumentos, y en qué índice específico se encontrará guardada la IP "10.0.0.5"?**
A) `sys.argv` en el índice [0]
B) `sys.argv` en el índice [1]
C) `sys.args` en el índice [0]
D) `os.argv` en el índice [1]

**9. Escribís un script que usa la librería `socket` para conectar a 100 servidores. Uno de los servidores no responde, y la conexión falla por "Timeout". Para evitar que el script crashee y aborte el escaneo de los 99 servidores restantes, ¿qué estructura debés colocar envolviendo el intento de conexión?**
A) Un bloque `if / else`
B) Una función `def`
C) Un bloque `try / except`
D) Un bucle `while True`

**10. Descargaste una herramienta ofensiva de GitHub que depende de la librería `requests` para funcionar. No tenés esa librería en tu computadora. ¿Qué herramienta de consola debés usar (desde tu terminal Linux/Windows) para descargar e instalar esa librería desde el repositorio mundial de Python?**
A) `apt install requests`
B) `python3 import requests`
C) `pip install requests`
D) `wget requests`

---

> **¿Terminaste?**
> Buscá el archivo en la carpeta de evaluaciones para comparar tus resultados (Módulo 04 - Respuestas).

**➡️ Siguiente nota:** [[20 - Resumen]]
