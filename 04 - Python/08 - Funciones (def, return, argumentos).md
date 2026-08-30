# 08 - Funciones (`def`, `return`)

## 🎯 Objetivos
- Comprender el concepto de la modularización del código.
- Aprender a crear y "llamar" Funciones en Python (`def`).
- Entender cómo enviar datos (Argumentos) y recibir resultados (`return`).

---

## 🧠 Concepto: Máquinas Expendedoras (DRY)

La regla número uno del buen código es **DRY (Don't Repeat Yourself - No te repitas)**.
Supongamos que escribís 15 líneas de código muy complejas para calcular matemáticamente si una IP pertenece a una subred específica. Si en tu script necesitás hacer esa validación en 5 momentos distintos, no vas a copiar y pegar las 15 líneas 5 veces. Si el día de mañana descubrís que tenían un error, tendrías que corregirlo en 5 lugares distintos.

Para eso existen las **Funciones**.
Son como máquinas expendedoras (o mini-programas dentro de tu programa). Las "construís" una sola vez, y luego las "llamás" para usarlas cuantas veces quieras. 

---

## 🛠️ Creando y Llamando a una Función

En Python, las funciones se definen (crean) usando la palabra clave **`def`**, seguida del nombre inventado, paréntesis `( )`, y dos puntos `:`. 
Todo el bloque de código de la función debe estar indentado.

### 1. La Función Simple
```python
# Definición (El plano de la máquina). Esto NO ejecuta el código todavía.
def saludar_atacante():
    print("¡Advertencia!")
    print("Se ha detectado un intento de intrusión.")

# Ejecución de nuestro código principal
print("Iniciando monitor...")

# 'Llamamos' (Invocamos) a la función usando su nombre y los paréntesis
saludar_atacante()
```

### 2. Parámetros / Argumentos (La moneda de la máquina)
Las funciones se vuelven útiles cuando pueden recibir datos dinámicos desde el exterior. 
Al definir la función, colocás "variables temporales" (Parámetros) dentro de los paréntesis. Cuando la llamás, le envías los datos reales (Argumentos).

```python
def banear_ip(direccion_ip, motivo):
    # La función usa internamente las variables que le pasaron
    print(f"Ejecutando regla de Firewall...")
    print(f"La IP {direccion_ip} ha sido bloqueada por: {motivo}")

# Llamadas múltiples con distintos datos
banear_ip("192.168.1.5", "Ataque SQL Injection")
banear_ip("10.0.0.99", "Fuerza Bruta a SSH")
```
*¡Increíble! Con 3 líneas de definición, ahorramos tener que escribir las mismas instrucciones decenas de veces en nuestro código.*

### 3. Devolviendo resultados (`return` / El producto de la máquina)
Muchas veces no queremos que la función simplemente imprima algo en la pantalla (como en los ejemplos anteriores). Queremos que la función haga un cálculo difícil de forma invisible y nos **devuelva** el resultado final para que lo guardemos en una variable.
Usamos la palabra clave **`return`**. 
*(Nota vital: Apenas la función ejecuta un `return`, la función se destruye/termina instantáneamente. Ningún código debajo del `return` se ejecutará).*

```python
def analizar_peso(tamanio_bytes):
    # Lógica compleja invisible...
    megabytes = tamanio_bytes / 1024 / 1024
    
    # Devuelve el valor numérico al programa principal
    return megabytes

# Llamamos a la función y GUARDAMOS lo que nos devolvió (el return) en una variable local
peso_archivo = analizar_peso(5000000)

print(f"El malware pesa {peso_archivo} MB")
```

---

## ❓ ¿Por qué importa en Seguridad?

Los scripts de ciberseguridad pueden crecer hasta tener 5.000 líneas (Herramientas completas). 
El código que no usa funciones se llama *Código Espagueti*; es un desastre ilegible e inauditable, imposible de arreglar si se rompe en medio de un test de penetración.
Un buen desarrollador de seguridad separa su código: una función para "Descargar", otra función para "Limpiar Texto", otra función para "Lanzar Exploit". Si algo falla, sabés exactamente en qué "mini-programa" (función) está el error.

---

## 📌 Must Know (Imprescindible)
- La sintaxis `def nombre_funcion():` para declarar (crear) funciones.
- Entender cómo enviar Argumentos a través de los paréntesis.
- La distinción abismal entre imprimir un valor (`print()`) y devolver un valor utilizable al sistema (`return`).

---

## 🔄 Preguntas de repaso
1. Al revisar el código fuente de un script de ofuscación (Malware), ves un bloque enorme de código definido con `def encriptar_archivo(ruta):` en la línea 10, pero al correr el script completo, nunca se encripta nada. Sabiendo cómo funcionan, ¿por qué la simple *definición* de la función no es suficiente para que su código se ejecute?
2. Diseñá mentalmente la primera línea de una función en Python (usando `def`) que se llame `calcular_riesgo` y que requiera que le envíen dos datos para trabajar: una variable llamada `vulnerabilidad` y otra llamada `criticidad`.
3. Una función diseñada para generar contraseñas aleatorias termina con la instrucción `print(contrasena_final)`. Si en tu script principal intentas hacer `clave = generar_password()`, la variable `clave` quedará vacía (`None`). ¿Por qué pasa esto y qué palabra clave deberías poner en la función en lugar de `print` para que la variable se guarde correctamente?

**➡️ Siguiente nota:** [[09 - Scope (Ámbito de las Variables)]]
