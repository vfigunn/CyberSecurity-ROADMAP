# 09 - Scope (Ámbito y Vida de las Variables)

## 🎯 Objetivos
- Comprender que no todas las variables son visibles en todas partes del código.
- Entender la diferencia entre Scope Local y Global (y por qué esto genera dolores de cabeza si se ignora).
- Aprender buenas prácticas sobre cuándo evitar variables Globales.

---

## 🧠 Concepto: La Zona de Visibilidad (Scope)

En la nota de [[08 - Funciones (def, return, argumentos)|Funciones]], dijimos que las funciones son como "mini-programas". 
Resulta que estos mini-programas funcionan como cajas fuertes selladas. Todo lo que pasa dentro de Las Vegas, se queda en Las Vegas. Todo lo que creas dentro de una función, vive y muere exclusivamente ahí adentro. A esto se le llama **Scope**.

---

## 1. Variables Locales (Local Scope)

Si declarás una variable (ej. `ip_atacante = "1.1.1.1"`) **adentro** de una función, esa variable se llama **Local**.
- Solo el código que está *dentro* de la función puede verla o modificarla.
- Cuando la función termina su trabajo y ejecuta el `return`, todas sus variables locales son destruidas de la Memoria RAM.
- **El error común:** Intentar usar una variable que nació adentro de una función, desde el programa principal.

*Ejemplo del problema:*
```python
def extraer_ip_del_log():
    # Creamos una variable LOCAL
    ip_descubierta = "10.0.5.50"
    print("IP extraída exitosamente.")

extraer_ip_del_log()

# El programa principal intenta usar esa IP para banearla
print(f"Vamos a banear a: {ip_descubierta}") 
# ¡CRASH! Error: NameError: name 'ip_descubierta' is not defined
```
*Solución:* Como vimos, la única forma de sacar información del "Scope Local" hacia el mundo exterior, es usando la palabra clave **`return`** y guardando el resultado en una variable del mundo exterior.

---

## 2. Variables Globales (Global Scope)

Si declarás una variable en el archivo principal, totalmente a la izquierda (sin estar indentada dentro de nada), se llama **Global**.
- Absolutamente todos (el programa principal y todas sus funciones) pueden **LEER** esa variable.

```python
# Variable GLOBAL (Se suele escribir en MAYÚSCULAS por convención)
SERVIDOR_DNS = "8.8.8.8"

def hacer_ping():
    # La función puede leerla sin problema
    print(f"Haciendo ping al DNS global: {SERVIDOR_DNS}")

hacer_ping()
# Imprimirá: Haciendo ping al DNS global: 8.8.8.8
```

### El peligro de la Modificación Global
Aunque cualquier función puede *leer* una variable global, Python se pone muy estricto si una función intenta **modificarla** (cambiar su valor). 
Si la función intenta cambiar su valor (ej. `SERVIDOR_DNS = "1.1.1.1"`), Python, por seguridad, no modificará la variable global original. En su lugar, Python creará una variable Local nueva (una copia "sombra") que solo vivirá dentro de la función, generando confusión masiva.

Si *realmente* necesitás que la función modifique la variable maestra global, debés declararlo explícitamente en la primera línea de la función usando la palabra clave **`global`**:
```python
MODO_DEBUG = False

def activar_debug():
    # Le decimos a Python: "No crees una variable local, referite a la de afuera"
    global MODO_DEBUG 
    MODO_DEBUG = True  # Ahora sí, afectará a todo el programa.
```

---

## ❓ ¿Por qué importa en Seguridad (y Programación General)?

El uso indiscriminado de variables globales es considerado **Pésima Práctica (Anti-patrón)** en la ingeniería de software (salvo para cosas que nunca cambian, como Constantes, ej. `VERSION_DEL_SCRIPT = "1.5"`).

Si tu herramienta de hacking tiene 5.000 líneas y usás 50 variables globales, cualquier función en cualquier parte del código podría modificarlas accidentalmente. Si una IP se guarda mal a mitad del ataque, tendrás que revisar las 5.000 líneas tratando de rastrear cuál de las cientos de funciones fue la culpable de arruinar el valor de la variable global. 

*Buena práctica:* Mantener todo en Scopes Locales. Mandar la información a las funciones a través de sus `(Argumentos)` y recibir el producto a través de su `return`.

---

## 📌 Must Know (Imprescindible)
- Qué es una variable Local (vive y muere dentro de su función) vs Global (visible para todos).
- El hecho de que el exterior no puede leer las variables creadas dentro de una función, a menos que la función las expulse mediante `return`.
- El uso de la palabra `global` (y por qué evitarlo en la medida de lo posible).

---

## 🔄 Preguntas de repaso
1. Declarás `contador_ataques = 0` al principio de tu script. Luego creás una función `registrar_ataque()` donde le sumás 1 a esa variable. Al ejecutar tu script, notás que el contador nunca sube de 0, a pesar de que la función se llama 100 veces. ¿Qué concepto de Scope, y qué palabra clave ignoraste?
2. Si dentro de la función `desencriptar_password()` creás una variable local llamada `clave_descifrada` que contiene la contraseña vital en texto claro, y luego de que la función termine de trabajar intentás hacer `print(clave_descifrada)` desde el archivo principal (fuera de la función), ¿qué sucederá (conceptual y técnicamente)?
3. Según las buenas prácticas de programación defensiva, ¿cuál es el mecanismo correcto e higiénico para que el archivo principal pueda obtener la información de `clave_descifrada` que mencionamos en la pregunta 2, sin usar variables globales?

**➡️ Siguiente nota:** [[10 - Manejo de Archivos (open, read, write)]]
