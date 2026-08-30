# 03 - Tipos de Datos Simples (Strings, Ints, Bools)

## 🎯 Objetivos
- Conocer los tipos de datos atómicos fundamentales.
- Dominar el manejo, concatenación y formateo de los Strings (Texto).
- Entender el concepto de Type Casting (Conversión de tipos).

---

## 🧠 Concepto: Cada dato es un Mundo

La computadora necesita saber qué tipo de información guardaste en una variable para saber qué matemáticas aplicar. Si sumás el número `2 + 2`, da `4`. Si sumás el texto `"2" + "2"`, la computadora "pega" las palabras y da `"22"`.

Para Python, todo es un Objeto (lo veremos luego), y existen tres categorías de datos primitivos que usarás en cada script.

---

## 1. Números (Integers y Floats)

- **Integer (`int`):** Números enteros (sin decimales). Útiles para contar intentos de inicio de sesión o especificar puertos.
  `puerto = 443`
- **Float (`float`):** Números de punto flotante (con decimales). Útiles para medir tiempo de respuesta, porcentajes o criptografía compleja.
  `tiempo_respuesta = 0.45`

*Matemáticas básicas:* Suma (`+`), Resta (`-`), Multiplicación (`*`), División (`/`). 
*Exponenciación (Elevar a la potencia):* Se hace con dos asteriscos (`**`).

---

## 2. Booleanos (`bool`)

Solo pueden tener uno de dos valores absolutos: **`True`** (Verdadero) o **`False`** (Falso).
Nota que en Python siempre **se escriben con la primera letra en Mayúscula**.
Son la base de todas las decisiones lógicas y condicionales (If/Else).

```python
servidor_comprometido = True
alerta_activada = False
```

---

## 3. Cadenas de Texto (`str` - Strings)

Cualquier cosa envuelta entre comillas (pueden ser simples `'texto'` o dobles `"texto"`) es un String.
Para Python, un String es en realidad una cadena o arreglo de caracteres. Esto es fundamental para análisis forense, porque te permite "cortar" y analizar letras individuales de un log.

### Operaciones con Strings

**Concatenación (Pegar textos):**
Usamos el signo `+` para unir Strings.
```python
usuario = "admin"
dominio = "@empresa.local"
email = usuario + dominio  # "admin@empresa.local"
```

**Métodos útiles (Herramientas pre-integradas):**
Los Strings vienen con súper-poderes integrados (Métodos) que puedes llamar usando un punto (`.`) al final de la variable.
- `upper()` / `lower()`: Convierte todo a mayúsculas o minúsculas. (Vital si estás comparando contraseñas y no te importa si el usuario usó mayúsculas).
- `replace(viejo, nuevo)`: Reemplaza una palabra por otra.
- `strip()`: Borra los espacios en blanco accidentales que haya al principio o al final del texto. (Súper usado cuando extraemos datos de páginas web o logs sucios).

```python
texto = "  Error Crítico  "
texto_limpio = texto.strip()  # "Error Crítico"
```

### F-Strings (Formateo de Texto Moderno)
Antes, mezclar variables con texto era un dolor de cabeza. Desde Python 3.6, usamos los **F-Strings**. 
Simplemente ponés una letra **`f`** antes de abrir las comillas del texto, y eso te permite inyectar variables en cualquier parte del texto usando llaves `{}`.

```python
ip = "10.0.5.20"
intentos = 5
# La 'f' habilita la inyección de llaves. Es limpio y profesional.
print(f"Alerta: La IP {ip} ha intentado loguearse {intentos} veces.")
```

---

## 🔄 Conversión de Tipos (Type Casting)

¿Qué pasa si intentás sumar un texto con un número? Python explotará (Dará un `TypeError`).
*Ejemplo fallido:*
```python
edad = 25
print("El sospechoso tiene " + edad + " años.") # ERROR: No se puede sumar str con int
```
Para solucionarlo, tenés que obligar (castear) al número a convertirse en un String antes de sumarlo, envolviéndolo en la función **`str()`**.

- `str(variable)`: Convierte a texto.
- `int(variable)`: Convierte a número entero (ej. si el usuario escribió la edad "25" en un formulario y te llegó como texto).
- `float(variable)`: Convierte a decimal.

*Ejemplo exitoso:*
```python
edad = 25
print("El sospechoso tiene " + str(edad) + " años.")
# Aunque hoy en día es mejor usar F-Strings: f"El sospechoso tiene {edad} años"
```

---

## 📌 Must Know (Imprescindible)
- Diferencia entre `int` y `float`.
- Los valores booleanos siempre son `True` o `False` (Mayúscula inicial).
- Dominar el uso de los **f-strings** (`f"Texto {variable}"`).
- Cómo usar funciones de casteo (`int()`, `str()`) para resolver errores de tipo.

---

## 🔄 Preguntas de repaso
1. Estás escribiendo un script para escanear puertos. Definiste el puerto 80 como un número. A la hora de escribir el log de evidencia, hiciste: `log = "Puerto atacado: " + puerto`. ¿Por qué Python detendrá la ejecución del script arrojando un `TypeError`, y cómo lo solucionarías usando Casteo tradicional?
2. Explicá cómo lograrías el mismo resultado del punto anterior (crear el texto del log mezclando palabras y la variable `puerto`) pero utilizando la sintaxis moderna de `F-Strings` (Formateo).
3. Estás leyendo una lista de correos electrónicos desde un archivo de texto viejo creado por humanos descuidados, y notás que muchos correos tienen espacios en blanco invisibles al final (ej. `"ceo@empresa.com   "`). ¿Qué método (función) de los Strings utilizarías sobre la variable para limpiar la cadena antes de procesarla?

**➡️ Siguiente nota:** [[04 - Estructuras de Datos (Listas, Tuplas y Sets)]]
