# Respuestas Evaluación Módulo 04 - Python

A continuación se presentan las respuestas correctas de la evaluación del [[04 - Python/19 - Evaluación|Módulo 04]], junto con la justificación técnica de cada una.

---

### Sección 1: Sintaxis y Tipos de Datos

**1. C) El intérprete lanzará un `IndentationError` (Error de Sintaxis) de inmediato y no ejecutará nada.**
> *Justificación:* En Python, la indentación (los 4 espacios o el tabulador) no es algo meramente estético; es una regla de sintaxis estricta y absoluta. Sin ella, el intérprete no puede saber qué código pertenece adentro del `if` y abortará la ejecución por completo.

**2. C) Tupla `( )`**
> *Justificación:* Las Tuplas, definidas mediante paréntesis, son la única de estas estructuras que es "Inmutable" (de solo lectura) después de ser creada. Un List o un Dictionary pueden ser modificados dinámicamente durante la ejecución del script.

**3. C) `puerto_numerico = int(puerto)`**
> *Justificación:* La función de Casteo (Type Casting) nativa de Python para transformar un texto a un número entero es `int()`.

---

### Sección 2: Control de Flujo y Funciones

**4. B) Riesgo Medio**
> *Justificación:* La variable `intentos` vale exactamente 5. El código evalúa la primera condición (`5 < 3`), la cual es `False`. Pasa a la rama alternativa `elif (5 == 5)`, la cual es `True`, por lo que imprime "Riesgo Medio" y abandona el bloque `if/else`.

**5. B) Imprimirá un error, porque la variable pertenece a un Scope Local y el script principal no puede verla.**
> *Justificación:* Toda variable declarada adentro del bloque indentado de una función `def` vive y muere exclusivamente allí (Scope Local). El mundo exterior (Scope Global) no tiene idea de que esa variable existe, a menos que la función la expulse con un `return`.

**6. B) Asegura que el archivo se cierre automáticamente al terminar el bloque, incluso si ocurre un error en el medio.**
> *Justificación:* El Context Manager `with open()` garantiza el cierre seguro (Liberación de recursos/File Descriptor) en cuanto el código sale de la indentación, previniendo cuelgues del sistema operativo o Memory Leaks, independientemente de si hubo un error de ejecución en su interior o no.

---

### Sección 3: Librerías y Escenarios

**7. A) Módulo `json`, función `json.loads()`**
> *Justificación:* El método `loads()` significa "Load String". Toma una gigantesca cadena de texto que viene de Internet (con formato JSON) y la parsea convirtiéndola en un Diccionario nativo de Python para que puedas interactuar con sus Claves.

**8. B) `sys.argv` en el índice [1]**
> *Justificación:* `sys.argv` es la lista de parámetros introducidos en consola. El índice `[0]` siempre contiene el nombre del script en sí mismo (ej. `escaner.py`). El índice `[1]` contiene la primera palabra escrita después del script (la IP objetivo).

**9. C) Un bloque `try / except`**
> *Justificación:* El bloque `try/except` es el mecanismo de control de daños (Manejo de Excepciones). Permite "atrapar" el TimeoutError del socket, prevenir el Crash (muerte) del script completo, y permitirle al bucle continuar procesando a las víctimas restantes.

**10. C) `pip install requests`**
> *Justificación:* `pip` (Pip Installs Packages) es el gestor y administrador de paquetes oficial del ecosistema Python. Se ejecuta directamente desde la terminal del sistema operativo y descarga paquetes desde el repositorio mundial oficial (PyPI).
