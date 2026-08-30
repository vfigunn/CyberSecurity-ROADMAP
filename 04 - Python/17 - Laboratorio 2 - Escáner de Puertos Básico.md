# Lab 04.2 - Escáner de Puertos (TCP Connect Scanner)

## 🎯 Objetivo
Vas a simular el corazón de la herramienta de seguridad de red más famosa del mundo (Nmap). Combinarás Bucles (`for`), el uso del Módulo de Bajo Nivel `socket`, control de Flujo (`if/else`) y el manejo crítico de Excepciones (`try/except`).

---

## 📋 Requerimiento del Negocio

El Red Team necesita un Escáner de Puertos "artesanal" y portable, escrito en puro Python, en caso de que logren entrar a un servidor víctima donde `nmap` no esté instalado.

El script (`escaner.py`) debe hacer lo siguiente:
1. Tener una variable principal (Ej: `target_ip = "192.168.1.1"`).
2. Tener una Lista con los 3 puertos más críticos a probar: 22 (SSH), 80 (HTTP), 443 (HTTPS).
3. Utilizar un Bucle para iterar sobre esa lista de puertos uno por uno.
4. Por cada vuelta del bucle, instanciar la librería `socket` e intentar establecer una conexión real TCP de 3 vías con la IP objetivo y ese Puerto específico.
5. El escaneo no puede tardar para siempre. El socket debe tener un "Timeout" de 1 solo segundo.
6. **Requisito Crítico:** Si el puerto está cerrado (o hay firewall), el sistema operativo arrojará una excepción brutal de red (TimeoutError o ConnectionRefused). ¡El script NO debe detenerse! Debe atrapar la explosión, imprimir "Puerto CERRADO", y permitir que el bucle continúe con el siguiente puerto de la lista.

---

## 🛠️ Procedimiento (Tu Trabajo)

Este es el núcleo de la ingeniería ofensiva (Networking + Programación). Tomate el tiempo para intentar construir la estructura mental (o en papel) de los bloques lógicos.

**Tips / Pasos Lógicos:**
1. Importá la librería vital para redes de bajo nivel en Capa 4: `import socket`.
2. Definí la variable de la IP y la lista de puertos.
3. Abrí tu bucle `for puerto in lista:`
4. **Todo lo que sigue ahora va adentro del bucle (indentado)**:
5. Abrí el bloque seguro de "Control de Daños": `try:`
6. Adentro del `try:`, instanciá el objeto Socket y configurá el timeout:
   `s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)`
   `s.settimeout(1)`
7. Usá el método `s.connect( (ip, puerto) )`. ¡Ojo que recibe una Tupla, por eso lleva doble paréntesis!
8. Si la línea de arriba conectó sin explotar, significa que el puerto está abierto. Imprimí el éxito con `print()`.
9. Fuera del `try`, abrí la rama del `except:` genérica, e imprimí que el puerto está cerrado.
10. Como un toque de elegancia final, asegurate de cerrar la conexión de red (el tubo) usando el bloque `finally:` con el comando `s.close()`, para no dejar conexiones colgadas en el servidor atacado.

---

## 📝 Resultado Esperado (Autoevaluación)

¿Listo? Veamos el código real.

> [!example]- Ver Código Solución del Script
> **Contenido de `escaner.py`:**
> ```python
> import socket
> 
> # Configuración inicial
> target_ip = "192.168.1.1" # IP del router de tu casa, por ejemplo
> puertos_a_escanear = [21, 22, 80, 443, 3306, 8080]
> 
> print(f"--- Iniciando Escaneo TCP Connect contra {target_ip} ---")
> 
> # Iteramos sobre la colección
> for puerto_actual in puertos_a_escanear:
>     
>     # El bloque TRY inicia AQUI adentro del bucle para atrapar los fallos individuales
>     try:
>         # 1. Creamos el objeto/tubo de red (IPv4, TCP)
>         s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
>         # 2. Obligamos a que falle rápido (1 seg) si el firewall bloquea o ignora
>         s.settimeout(1)
>         
>         # 3. Lanzamos el paquete real por la red (SYN)
>         # Si falla, saltará como un rayo a la línea de 'except'
>         s.connect((target_ip, puerto_actual))
>         
>         # 4. Si llegó hasta acá, es porque conectó (Recibió SYN-ACK)
>         print(f"[+] Puerto {puerto_actual} : ABIERTO")
>         
>     except Exception as e:
>         # Atrapamos cualquier error de red (Timeout, Connection Refused)
>         # (Para que quede limpio en pantalla, podríamos comentar este print)
>         print(f"[-] Puerto {puerto_actual} : CERRADO o FILTRADO")
>         
>     finally:
>         # Este bloque se ejecuta SIEMPRE al final de esta vuelta, 
>         # haya habido error o no, para asegurar la limpieza del sistema.
>         s.close()
> 
> print("--- Escaneo Finalizado ---")
> ```
> 
> *Este es un escáner total y completamente real y funcional. Lo único que lo diferencia de la función principal de Nmap, es que Nmap usa programación multihilo/asíncrona para escanear cientos de puertos al mismo tiempo, mientras que nosotros vamos uno por uno (Síncrono).*

---
¡Has construido tu primera herramienta ofensiva (Red Team) real! El dominio conjunto de `socket` y `try/except` te convierte en un eslabón peligroso y muy capaz.

**➡️ Siguiente nota:** [[18 - Ejercicios]]
