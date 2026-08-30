# 19 - Bash Scripting (Bucles / Loops)

## 🎯 Objetivos
- Comprender el inmenso poder de automatización iterativa de los Bucles.
- Aprender la estructura de un Bucle `for`.
- Aprender la estructura de un Bucle `while`.

---

## 🧠 Concepto: No te repitas (DRY - Don't Repeat Yourself)

Un script que verifica si la IP `192.168.1.1` está viva haciendo un `ping` es útil. Pero, ¿y si tenés que verificar las 254 IPs de la subred? Copiar y pegar el comando `ping` 254 veces en tu archivo de script no tiene sentido.

Los **Bucles (Loops)** le dicen a la computadora: *"Tomá esta pequeña porción de código, y ejecutala muchísimas veces seguidas, cambiando solo una cosita en cada vuelta"*.

En Bash, usamos dos tipos principales de bucles: `for` y `while`.

---

## 🔄 1. El Bucle `for` (Repetir sobre una lista)

El bucle `for` es excelente cuando tenés una lista finita de elementos (como una lista de IPs, nombres de usuarios, o números del 1 al 10) y querés ejecutar una acción sobre cada uno de ellos.

**Sintaxis y Ejemplo:**
(Queremos saludar a tres usuarios).
```bash
#!/bin/bash

# La variable "usuario" va tomando cada valor de la lista en cada "vuelta"
for usuario in Juan Maria Carlos; do
    echo "Revisando configuraciones de $usuario..."
    # Aca irian comandos complejos como: chown $usuario /carpeta
done

echo "Bucle finalizado."
```

### Iterando con números (Secuencias)
Si sos un atacante y querés escanear la red desde la IP 1 hasta la 20:
```bash
#!/bin/bash

# El formato {1..20} crea una lista de números del 1 al 20 automáticamente.
for i in {1..20}; do
    echo "Haciendo Ping a 192.168.1.$i"
    # El comando ping real iría aquí, enviando 1 solo paquete (-c 1)
    # ping -c 1 192.168.1.$i
done
```
Este simple script de 4 líneas automatiza lo que a un humano le tomaría 10 minutos tipear a mano.

---

## ⏳ 2. El Bucle `while` (Repetir mientras algo sea cierto)

El bucle `while` se usa cuando *no sabés* cuántas veces tenés que repetir el código, sino que querés que se repita **mientras** una condición (un `if`) se siga cumpliendo. Cuando la condición deja de ser cierta, el bucle frena.

**Sintaxis y Ejemplo:**
(Cuenta regresiva)
```bash
#!/bin/bash

CONTADOR=5

# Repetir "Mientras" el contador sea Mayor que 0 (-gt 0)
while [ $CONTADOR -gt 0 ]; do
    echo "Faltan $CONTADOR segundos para detonar..."
    sleep 1 # Pausa de 1 segundo
    
    # Restamos 1 al contador usando matemáticas (doble parentesis en bash)
    ((CONTADOR--)) 
done

echo "¡BOOM!"
```

### El Bucle Infinito (El Guardián)
El Blue Team a veces crea bucles infinitos para monitorear servicios críticos constantemente.
Para crear un bucle infinito, hacés que la condición siempre sea verdadera (ej. usando el comando `true`).
```bash
#!/bin/bash

while true; do
    echo "Revisando conexiones activas..."
    ss -tulpn >> log_conexiones.txt
    sleep 60  # Pausa 60 segundos antes de dar otra vuelta
done
# (Para detener este script tendrás que usar Ctrl+C en la terminal)
```

---

## ❓ ¿Por qué importa en Seguridad?

El dominio de los bucles separa a un operador manual (junior) de un verdadero automatizador.
- **Red Team:** "Encontré un exploit. Ahora escribiré un bucle `for` que lea un archivo de texto con 5,000 IPs y dispare el exploit contra todas ellas en 30 segundos".
- **Blue Team:** "Escribiré un bucle `while` que lea línea por línea el archivo de Logs de Apache y bloquee temporalmente cualquier IP que sume más de 10 peticiones fallidas por segundo".

---

## 📌 Must Know (Imprescindible)
- Entender el concepto de iteración de código.
- Saber cuándo usar `for` (Tengo una lista fija de cosas o números).
- Saber cuándo usar `while` (Quiero que corra hasta que se cumpla o se rompa una condición).

---

## 🔄 Preguntas de repaso
1. Si un atacante tiene un archivo de texto llamado `passwords.txt` con 10,000 posibles contraseñas y quiere probarlas todas automáticamente contra un formulario web, ¿qué tipo de bucle (for o while) sería la mejor opción conceptual para leer la lista una por una y enviarlas?
2. Explicá cómo un Bucle `while true` y un comando `sleep 300` te podrían ayudar a construir un monitor básico (un proceso en background) que te avise si tu servidor web se cayó, revisándolo cada 5 minutos.
3. Describí cómo la línea `for i in {1..254}; do` podría ayudarte (junto con el comando `ping`) a realizar la fase de "Reconocimiento" de la Cyber Kill Chain en una red `/24`.

**➡️ Siguiente nota:** [[20 - Tareas Programadas (Cronjobs)]]
