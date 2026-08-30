# 06 - Condicionales (`if`, `elif`, `else`)

## 🎯 Objetivos
- Entender el flujo de control en la programación.
- Aprender la sintaxis de las decisiones lógicas en Python.
- Dominar los operadores de comparación y los operadores lógicos (`and`, `or`, `not`).

---

## 🧠 Concepto: Bifurcaciones en el camino

Un script corre línea por línea, de arriba hacia abajo. Sin embargo, una herramienta de seguridad no puede hacer siempre lo mismo. Tiene que observar el entorno y tomar decisiones: 
*"Si el puerto 22 está abierto, entonces lanzá el ataque de fuerza bruta. Si no, terminá el script."*

Estas decisiones se toman comparando datos. El resultado de la comparación **siempre es un Booleano** (`True` o `False`).

---

## ⚖️ Operadores de Comparación

En la [[18 - Bash Scripting (Variables y Condicionales)|nota de Bash]] vimos que comparar números requería letras raras como `-gt` o `-eq`.
Python es puramente matemático y lógico, usa los símbolos que conocés de la escuela:

- `==` : **Igual a** (¡Ojo! El doble igual es para preguntar. El igual simple `=` es para asignar/guardar en la variable).
- `!=` : **Distinto de**.
- `>` / `<` : Mayor que / Menor que.
- `>=` / `<=` : Mayor o igual / Menor o igual.
- `in` : Verifica si algo existe dentro de una lista o un texto. (Ej. `"admin" in usuarios_activos`).

---

## 🛠️ La sintaxis: `if`, `elif`, `else`

La estructura es simple. Termina siempre con dos puntos (`:`), y **todo lo que esté por debajo indentado con 4 espacios** es lo que sucederá si la condición fue Verdadera.

```python
puerto = 80

# 1. Condición principal
if puerto == 22:
    print("Iniciando ataque a SSH...")
    
# 2. Condición alternativa (Si falló el IF, probamos esta)
elif puerto == 80:
    print("Iniciando escáner de vulnerabilidades Web...")
    
# 3. Opción por defecto (Si falló todo lo anterior, entra acá)
else:
    print("Puerto desconocido. Abortando.")
    
# El código sin indentar ya está fuera del bloque. Se ejecuta siempre.
print("Script finalizado.")
```

---

## 🔗 Operadores Lógicos (`and`, `or`, `not`)

A menudo, las decisiones en ciberseguridad requieren que se cumplan *múltiples* condiciones al mismo tiempo para evitar falsos positivos.

- **`and` (Y):** Ambas condiciones deben ser ciertas.
  ```python
  if puerto == 443 and certificado_valido == False:
      print("Alerta: Tráfico interceptado (Man in the Middle sospechado)")
  ```
- **`or` (O):** Con que al menos UNA sea cierta, ya alcanza.
  ```python
  if usuario == "admin" or ip_origen == "10.0.0.1":
      print("Otorgando acceso de Superusuario.")
  ```
- **`not` (Negación):** Da vuelta la respuesta. Si era True, la hace False. Se usa mucho para revisar si algo está vacío.
  ```python
  if not base_de_datos_conectada:
      print("Error de conexión.")
  ```

---

## ❓ ¿Por qué importa en Seguridad?

El control de flujo es la columna vertebral de cualquier herramienta automatizada.
- Un script de **Threat Hunting** recorrerá mil líneas de log, pero usará un bloque `if` enorme (lleno de `and` y `or`) para decidir si una línea de texto específica clasifica como una amenaza real o como un falso positivo, antes de enviarte una alerta al correo.
- Un **Exploit** usará `if` para revisar la respuesta del servidor; si el servidor responde con un "HTTP 200 OK", el exploit sabe que el ataque funcionó. Si responde "HTTP 403 Forbidden", el exploit usará el bloque `else` para intentar una técnica de evasión diferente.

---

## 📌 Must Know (Imprescindible)
- La diferencia crítica entre `=` (Asignación) y `==` (Comparación).
- La obligación estricta de usar los dos puntos (`:`) al final de la condición, y la **indentación** en la línea siguiente.
- Cómo funcionan `and` y `or`.
- El uso de `in` para buscar rápidamente en listas (ej. `if "10.0.0.1" in lista_ips_bloqueadas:`).

---

## 🔄 Preguntas de repaso
1. Analizá esta línea de código en Python: `if estado_servidor = "Hackeado":`. ¿Por qué Python detendrá tu script arrojando un Error de Sintaxis?
2. Querés escribir una lógica defensiva: "Si el archivo está encriptado **O** si el usuario que lo abrió no es el administrador, borrar el archivo". ¿Cuál de los dos operadores lógicos (`and` o `or`) deberías usar entre las dos condiciones?
3. En la estructura `if` / `elif` / `else`, ¿es obligatorio escribir un `else` al final de cada validación, o puede existir un `if` solo sin su `else`? (Pensá en la lógica de programación general).

**➡️ Siguiente nota:** [[07 - Bucles (for y while)]]
