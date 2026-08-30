# 07 - Bucles (`for` y `while`)

## 🎯 Objetivos
- Entender el concepto de Iteración sobre Estructuras de Datos.
- Aprender la sintaxis del Bucle `for`.
- Aprender la sintaxis del Bucle `while` y cómo detenerlo (`break`).

---

## 🧠 Concepto: La repetición

Al igual que vimos en [[19 - Bash Scripting (Bucles)|Bash]], los bucles (Loops) te permiten tomar un bloque de código y ejecutarlo cientos de miles de veces a la velocidad de la luz. 

En ciberseguridad, esto se usa constantemente: Escanear 65.535 puertos de red (uno por uno), probar 10 millones de contraseñas (una por una), o leer un archivo de registro línea por línea.

---

## 🔄 1. El Bucle `for` (Iterar sobre Colecciones)

El bucle `for` en Python es extremadamente elegante. No cuenta números; está diseñado intrínsecamente para "recorrer" o "iterar" los elementos que hay adentro de una [[04 - Estructuras de Datos (Listas, Tuplas y Sets)|Lista]] o un Diccionario.

**La sintaxis es: `for [elemento_individual] in [coleccion_gigante]:`**

```python
puertos = [21, 22, 80, 443]

# "Para cada PUERTO individual dentro de la lista PUERTOS..."
for puerto in puertos:
    # Este bloque (indentado) se ejecutará 4 veces.
    print(f"Testeando vulnerabilidades en el puerto {puerto}")
    # aca_iria_la_logica_de_ataque()
    
print("Escaneo finalizado.")
```

### La función `range()`
¿Qué pasa si sos un atacante y necesitás probar los puertos del 1 al 1024, pero no tenés una lista prearmada? No vas a escribir mil números a mano. Usás la función mágica `range(inicio, fin)`.
```python
for puerto in range(1, 1025):  # Genera números del 1 al 1024
    atacar(puerto)
```

---

## ⏳ 2. El Bucle `while` (Basado en una Condición)

El `while` ("Mientras que") se usa cuando no tenés una lista de tamaño fijo. Se usa cuando querés repetir código hasta que un evento del mundo real suceda (o deje de suceder). 
Funciona combinando un Bucle con un Condicional (`if`).

```python
intentos_fallidos = 0

# El bucle girará mientras la variable sea menor a 3.
while intentos_fallidos < 3:
    password = probar_password()
    if password == "incorrecta":
        intentos_fallidos = intentos_fallidos + 1
        print("Fallo el login. Intentando de nuevo...")
    else:
        print("¡Acceso conseguido!")
        break # Explicado abajo

print("El sistema bloqueó la cuenta por demasiados intentos.")
```

### Controlando el flujo: `break` y `continue`
- **`break`:** Destruye y finaliza el bucle por completo al instante. 
  *Escenario:* Estás haciendo fuerza bruta (probando contraseñas) dentro de un `while` gigante. Cuando encontrás la correcta, no necesitás seguir probando las demás. Ejecutás `break` para salir del bucle victorioso.
- **`continue`:** Salta el resto del código actual y pasa directamente a la siguiente "vuelta" del bucle.
  *Escenario:* Estás iterando una lista de 100 IPs. En la IP 5, el servidor no responde (timeout). No querés que tu script crashee y aborte, simplemente ejecutás `continue` para ignorar esa IP y saltar a probar la IP 6.

### El Bucle Infinito (Daemons / Listeners)
Si programás un "Listener" (un servidor tuyo que espera recibir una conexión de un malware implantado en la víctima), tu script nunca debe apagarse.
Usás `while True:`. Al ser siempre Verdadero, girará eternamente hasta que mates el proceso en Linux.

---

## 📌 Must Know (Imprescindible)
- Entender que en el `for item in list:`, la variable `item` va tomando el valor de cada elemento de la lista vuelta por vuelta.
- Para generar una secuencia numérica gigantesca se usa la función `range()`.
- La diferencia táctica entre `break` (destruye el bucle) y `continue` (salta a la siguiente vuelta).
- La utilidad del bucle infinito `while True:` para crear scripts de escucha continua (Listeners).

---

## 🔄 Preguntas de repaso
1. Tenés una lista en Python con 5,000 nombres de subdominios (`lista_subdominios`). Explicá por qué elegirías un bucle `for` en lugar de un bucle `while` para enviarles un Ping a todos de forma automatizada.
2. Estás analizando los puertos (del 1 al 1000) usando `for puerto in range(1, 1001)`. A mitad del proceso, la conexión de red se cae. ¿Qué palabra reservada (comando de bucle) insertarías dentro de un bloque `if` para abortar todo el escaneo y salir del bucle si detectás el fallo de red?
3. En el contexto de un Malware diseñado en Python, ¿por qué el creador podría usar un bucle `while True:` acompañado de la instrucción `time.sleep(60)` para que su script se comunique con el servidor de Control (C2) de forma periódica?

**➡️ Siguiente nota:** [[08 - Funciones (def, return, argumentos)]]
