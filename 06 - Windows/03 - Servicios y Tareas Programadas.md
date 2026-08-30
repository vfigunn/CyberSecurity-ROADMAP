# 03 - Servicios y Tareas Programadas

## 🎯 Objetivos
- Entender cómo funcionan los programas silenciosos en segundo plano (Daemons/Servicios).
- Comprender el uso legítimo y malicioso del Programador de Tareas.
- Identificar dos vectores de Persistencia y Escalada de Privilegios muy utilizados en Ciberseguridad.

---

## ⚙️ 1. Servicios de Windows (Windows Services)

Si un usuario cierra su sesión en Windows (Se va a la pantalla de "Log Out"), todos los programas abiertos en User Mode (Chrome, Word, Spotify) se cierran por completo.

Pero, ¿qué pasa si la computadora es un Servidor Web o un Servidor de Base de Datos? La Base de Datos debe seguir funcionando 24/7 de forma invisible, haya o no haya un usuario humano sentado frente al monitor.

Estos programas que operan "detrás de escena" se llaman **Servicios** (El equivalente exacto a los [[04 - Permisos, Servicios y Procesos#⚙️ Servicios y Systemd (La Matrix)|Daemons de Systemd]] en Linux).
El administrador global de estos programas es el proceso **`services.msc`**.

### Relevancia en Seguridad (Escalada de Privilegios):
Los servicios tienen una particularidad extremadamente peligrosa. Muchos de ellos no corren con los permisos del usuario normal, sino que se inician automáticamente usando la cuenta suprema de Windows, llamada **`NT AUTHORITY\SYSTEM`** (El equivalente al usuario `root` de Linux).

**El ataque clásico (Unquoted Service Path):**
Si el atacante encuentra un programa legítimo de terceros instalado como Servicio con permisos `SYSTEM`, y el ingeniero de software programó mal el servicio (dejó espacios en blanco en la ruta del disco sin poner comillas), el atacante puede meter su propio `.exe` falso en medio de la ruta.
Cuando la PC se reinicie, Windows intentará arrancar el servicio legítimo, pero se confundirá y arrancará el virus del atacante. El resultado: **El atacante acaba de ganar permisos SYSTEM (Escalada de privilegios local - LPE)**.

---

## ⏰ 2. Tareas Programadas (Task Scheduler)

A diferencia de los Servicios (que están siempre prendidos, girando infinitamente en segundo plano), las Tareas Programadas existen para hacer acciones "De un solo golpe" y luego apagarse (Igual que el [[19 - Bash Scripting (Bucles)#⏰ El Programador de Tareas cron|Cron]] en Linux).

La herramienta gráfica se llama `Taskschd.msc` (Programador de Tareas).
Son Eventos que se disparan en base a un "Trigger" (Gatillo).
Ejemplos de gatillos:
- *"Todos los martes a las 3:00 AM, ejecutá el actualizador de Adobe."*
- *"Cuando el usuario Juan inicie sesión, lanzá el script de bienvenida."*
- *"Si la PC pasa 15 minutos sin que nadie toque el mouse (Idle), encendé el escáner del Antivirus."*

### Relevancia en Seguridad (Persistencia Avanzada):
En la nota anterior vimos que el Malware inyecta su ruta en la clave `Run` del Registro para arrancar automáticamente (Persistencia básica). 
El problema para el hacker es que esa clave del Registro es lo primero que revisan todos los analistas y los Antivirus para matar al virus.

Para **evadir la detección**, los atacantes modernos utilizan las Tareas Programadas (Persistencia avanzada).
Crean una Tarea invisible en Windows que dice: *"Cada 1 hora, de forma silenciosa, ejecutá un script en la consola de PowerShell que se conecte a Internet y descargue mi troyano, saltándote al usuario"*. 
Es mucho más difícil para un Blue Team encontrar una aguja en el pajar del Programador de Tareas, porque Windows tiene cientos de tareas legítimas de telemetría corriendo de fábrica.

---

## 📌 Must Know (Imprescindible)
- **Servicios:** Programas invisibles que no dependen del inicio de sesión de un usuario. A menudo corren con el privilegio más alto posible (`SYSTEM`), lo que los vuelve blancos perfectos para Escalada de Privilegios.
- **Tareas Programadas:** Programas que se disparan en base a gatillos (Tiempo, Eventos). Son el método por excelencia de las APTs (Amenazas Persistentes) para asegurar el acceso duradero sin ser detectadas en el Registro común.

---

## 🔄 Preguntas de repaso
1. Lograste hackear una PC como un usuario sin privilegios ("Invitado"). Revisando el sistema, notás un Servicio de un software de impresora (que corre con privilegios `SYSTEM`) y descubrís que su código binario (`impresora.exe`) permite ser sobrescrito por cualquier usuario. Si vos reemplazás el archivo `impresora.exe` por tu propio Malware y reiniciás la PC, ¿con qué nivel de privilegios se ejecutará tu Malware, y qué concepto acabas de lograr?
2. Un Antivirus elimina repetidamente un troyano (un archivo .exe malicioso) de la carpeta `C:\Temp\`, pero todos los días a las 14:00 PM, el archivo reaparece mágicamente y vuelve a infectar la red sin que la víctima siquiera toque la computadora. ¿Qué mecanismo del sistema operativo está utilizando probablemente el atacante para revivir el virus diariamente?
3. Contrastá conceptualmente cuál es la diferencia entre que un Malware use el Registro (Clave `Run`) para lograr persistencia, versus utilizar la herramienta `services.msc` (Instalarse como Servicio). ¿En qué momento exacto del ciclo de vida de la PC se ejecutaría cada uno? (Pensá en la pantalla de inicio de sesión).

**➡️ Siguiente nota:** [[04 - Sistema de Archivos y Permisos (NTFS vs Share)]]
