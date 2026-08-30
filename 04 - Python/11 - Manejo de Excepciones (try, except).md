# 11 - Manejo de Excepciones (`try`, `except`)

## 🎯 Objetivos
- Entender qué es una Excepción y por qué los scripts de ciberseguridad "crashean" (fallan).
- Aprender la estructura salvavidas `try / except`.
- Evitar que una herramienta ofensiva (o un escaneo largo) se detenga a la mitad por un error imprevisto.

---

## 🧠 Concepto: Cuando la Realidad golpea al Código

Como programador de seguridad, vos podés escribir el script de escaneo de red perfecto (verificando la indentación, cerrando los archivos, sin usar variables globales). 
Pero, ¿qué pasa si en la línea 50, tu script intenta conectarse por Internet a una IP en Rusia... y en ese milisegundo tu conexión Wi-Fi se corta? 
O, ¿qué pasa si intentas abrir el archivo `contraseñas.txt` y resulta que el usuario borró el archivo ayer?

Estos no son errores de sintaxis (no te equivocaste escribiendo), son errores del "mundo exterior". Se llaman **Excepciones**. 
El comportamiento por defecto de Python ante cualquier Excepción es el más seguro pero el más drástico: **Aborter inmediatamente el programa entero** y lanzar un mensaje rojo en la consola (Crash/Traceback).

En ciberseguridad, un escaneo de un millón de IPs puede durar 3 días. Si en el día 2, una sola IP no responde y tu script de 3 días se cancela y se cierra, tirarás tu teclado por la ventana. Necesitamos controlar los errores.

---

## 🛠️ El bloque Salvavidas: `try` / `except`

Esta estructura es idéntica filosóficamente a un bloque `if/else`, pero aplicada a fallos críticos.

- **`try:` (Intentá esto):** Adentro ponés el código peligroso (conectarse a red, abrir archivos, convertir números).
- **`except:` (Si ocurre una explosión):** Adentro ponés el código de emergencia. Si el bloque `try` falla, Python "atrapa" la explosión, NO cierra el programa, y ejecuta el bloque `except`.

### El Ejemplo Clásico:
Vamos a crear una herramienta matemática simple, forzando a que un humano distraído la rompa dividiendo por cero.

```python
# Sin Try/Except (Crashea y muere)
numero = 10 / 0
print("Esta línea nunca llegará a ejecutarse en la vida.")
```

```python
# Con Try/Except (Seguro y profesional)
try:
    print("Intentando la división peligrosa...")
    numero = 10 / 0
    print("Éxito.") # Esta línea se saltea si hay error arriba

except ZeroDivisionError: # Capturamos el error específico
    print("Error crítico: Un humano intentó dividir por Cero.")
    # El programa NO se rompe

print("El script principal sigue vivo y funcionando tranquilamente.")
```

---

## 🎯 Atrapando errores específicos vs. Genéricos

Existen docenas de Excepciones en Python (`FileNotFoundError` si el archivo no existe, `TimeoutError` si la red no responde, `KeyError` si la clave de diccionario no existe).

- **Lo Profesional:** Escribir múltiples `except` para reaccionar diferente según el tipo de error.
- **Lo Genérico (Atrapar todo):** Si usas la palabra `Exception as e`, atraparás cualquier desastre imaginable, y guardarás el mensaje técnico del error original en la variable `e` para imprimirlo. Muy usado en Hacking rápido.

```python
try:
    # Imaginemos que descargamos una página web (Falla si no hay internet)
    descargar_web()
    
    # Abrimos un archivo (Falla si el archivo no existe)
    with open("archivo_raro.txt", "r") as f:
        print(f.read())
        
except FileNotFoundError:
    print("Error: El archivo no existe en tu computadora.")
    
except Exception as e:
    # Atrapa fallos de red, de memoria, TODO.
    print(f"Ocurrió un desastre desconocido: {e}")
```

---

## ❓ ¿Por qué importa en Seguridad?

Los scripts de ciberseguridad interactúan con entornos extremadamente hostiles (servidores de atacantes, redes inestables, payloads corruptos).

Si hacés un bucle `for` para escanear los 65.000 puertos de una computadora, y el puerto 140 no te responde porque el Firewall lo bloqueó y generó un `ConnectionResetError`... si no tenías un bloque `try/except` rodeando tu conexión, el script crasheará en el puerto 140, perdiéndote los 64.000 puertos restantes.
Con `try/except`, el script atrapa el error, lo anota ("Puerto 140 filtrado"), e ignora el problema pasando elegantemente a la siguiente iteración del bucle.

---

## 📌 Must Know (Imprescindible)
- El concepto de Excepción (errores provocados por agentes externos o uso indebido, no por errores de escritura de código).
- La estructura de control `try` y su compañero obligatorio `except`.
- Saber que al atrapar un error, prevenís que el programa completo aborte y muera (Crash).

---

## 🔄 Preguntas de repaso
1. Estás construyendo un script que le pide al usuario de la terminal que ingrese su Año de Nacimiento, y luego lo convierte a entero (`int(variable_año)`). Sabiendo que los humanos cometen errores, ¿qué pasará con tu programa (y con el proceso de conversión) si el usuario escribe la palabra "Noventa" en lugar del número "1990", si NO usaste un bloque `try/except`?
2. Explicá cómo usar un bloque `try/except` en conjunto con un Bucle `for` es fundamental al programar un script de Descubrimiento (Escáner de Red) que recorre 254 IPs y donde es altamente probable que decenas de ellas estén "muertas" (apagadas).
3. ¿Por qué en la industria del software se considera una mala práctica abusar del bloque genérico `except Exception:` (es decir, atrapar todos los errores posibles) en lugar de atrapar los errores por su nombre específico (como `FileNotFoundError`)? *(Pista: Pensá en cómo el programador sabe qué fue lo que realmente falló).*

**➡️ Siguiente nota:** [[12 - Programación Orientada a Objetos (OOP - Intro)]]
