# 02 - Sintaxis Básica y Variables

## 🎯 Objetivos
- Entender la particularidad más importante de Python: la Indentación.
- Aprender a comentar el código.
- Comprender qué son las Variables, cómo se asignan y la flexibilidad de su tipado.

---

## 📏 La Indentación (Los espacios mágicos)

Si has visto código de otros lenguajes (C, Java, JavaScript), verás que usan llaves `{ }` para agrupar bloques de código (por ejemplo, el código que va adentro de un bucle).

Python es único: **usa el espacio en blanco (Indentación)** para definir qué código pertenece a qué bloque. 
Si ponés un espacio de más o de menos al principio de una línea, tu programa directamente se "romperá" (dará un `IndentationError`).

*Ejemplo (No te preocupes por el `if` todavía, mira los espacios):*
```python
# CORRECTO
if temperatura > 100:
    print("El servidor se está quemando")
    apagar_servidor()

# INCORRECTO (Error de sintaxis asegurado)
if temperatura > 100:
print("El servidor se está quemando")
```
- **La regla de oro:** Por convención mundial (PEP 8), la indentación siempre debe ser de **4 espacios** (o un Tab).

---

## 📝 Comentarios

En ciberseguridad, a menudo trabajamos con scripts creados por terceros (o escribimos exploits que leerá nuestro equipo). Comentar qué hace cada línea es vital para que, 6 meses después, entiendas tu propio código.
- **Comentario de una línea:** Se hace con el símbolo de almohadilla / numeral (`#`). Python ignorará todo lo que haya desde el `#` hasta el final de la línea.
- **Comentario multilínea:** Se utilizan tres comillas simples o dobles consecutivas (`"""`).

```python
# Esto es un comentario de una línea. El intérprete lo ignora.
ip_objetivo = "10.0.0.5" # También puedo ponerlo al final de una orden

"""
Esto es un comentario
de múltiples líneas. Muy útil
para explicar cómo funciona un script.
"""
```

---

## 📦 Variables y Tipado Dinámico

Como vimos en la sección de [[18 - Bash Scripting (Variables y Condicionales)|Bash]], una variable es una "caja" en la memoria RAM donde guardamos un dato, poniéndole una etiqueta con un nombre.

En Python, para declarar (crear) una variable, simplemente inventás un nombre, ponés el signo igual (`=`), y le asignás un valor. **No se usa ningún comando especial** (como el `var` o `let` de Javascript).

```python
puerto = 80
protocolo = "HTTP"
esta_abierto = True
```

### Tipado Dinámico (Dynamic Typing)
Lenguajes estrictos como Java exigen que, si creás la variable `puerto` y decís que va a guardar un Número, no podés poner texto ahí adentro nunca más en la vida.
Python es **Dinámico y Flexible**. La misma caja (variable) puede guardar un número hoy, y mañana guardar una palabra. El intérprete de Python deduce automáticamente qué tipo de dato hay adentro basándose en cómo lo escribiste (si tiene comillas o no).

```python
# Ahora mismo 'estado' es un número (Integer)
estado = 200 

# Dos líneas después, le guardo texto. Python no se queja y lo acepta (String).
estado = "OK" 
```

### Reglas para nombrar variables
- Deben empezar con una letra o un guion bajo (`_`), nunca con un número.
- Distinguen mayúsculas de minúsculas (`Ip` es diferente a `ip`).
- **Convención Pythonica (Snake Case):** A diferencia de otros lenguajes que usan CamelCase (`puertoDelServidor`), en Python es obligatorio moralmente usar todo minúsculas separadas por guiones bajos: `puerto_del_servidor`.

---

## 🖨️ La función `print()`

Para que tu script le muestre información al analista que está mirando la pantalla, usamos la función `print()`. Podés imprimir variables, texto, o una mezcla de ambas.

```python
ip = "192.168.1.1"
# Imprimiendo variables separadas por comas (Python añade un espacio automático)
print("Escaneando la dirección:", ip)
```

---

## 📌 Must Know (Imprescindible)
- La indentación (4 espacios) no es estética, es una **regla estricta obligatoria** en la sintaxis de Python.
- El símbolo `#` para comentar.
- Qué significa el "Tipado Dinámico" (las variables pueden cambiar de tipo libremente).

---

## 🔄 Preguntas de repaso
1. Estás auditando un script de Python que intenta leer un archivo de contraseñas. El script arroja un `IndentationError` en la línea 15. Sabiendo cómo funciona la sintaxis de Python, ¿qué problema estructural tiene esa línea de código?
2. En lenguajes de tipado estático (como C#), si declaras `int intentos_fallidos = 5;`, el sistema reservará memoria estrictamente para un número. Explicá cómo manejaría Python esa misma situación si, en la línea siguiente, escribís: `intentos_fallidos = "Cinco"`.
3. ¿Cuál es el nombre de la convención de escritura estándar de Python para nombrar variables compuestas por varias palabras (como "puerto destino"), y cómo se escribiría correctamente?

**➡️ Siguiente nota:** [[03 - Tipos de Datos (Strings y Números)]]
