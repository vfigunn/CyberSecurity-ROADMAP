# 12 - PowerShell (Administración y Amenazas)

## 🎯 Objetivos
- Entender el salto evolutivo desde el viejo CMD.exe hacia PowerShell.
- Conocer cómo la administración de Objetos hace a PowerShell infinitamente superior a Bash.
- Descubrir la técnica de Malware sin archivos (Fileless Malware).

---

## 🧠 Concepto: La Terminal Orientada a Objetos

Durante 20 años, la terminal de Windows (`cmd.exe` - Command Prompt) fue un chiste comparada con la poderosa consola de Linux (Bash). En 2006, Microsoft inventó **PowerShell**.

PowerShell no es solo una pantalla azul donde tipeas comandos; es un motor completo construido sobre el framework `.NET` de Windows.
Su característica más revolucionaria, que lo hace distinto a Linux, es **cómo maneja la información (Pipes)**.

- En [[08 - Redirecciones y Pipes|Bash (Linux)]], cuando enviás la salida de un comando hacia otro (usando `|`), lo que viaja por el tubo es **Texto Puro**. (Tenés que usar comandos como `grep` o `awk` para recortar las letras que no querés).
- En PowerShell, cuando usás un Pipe (`|`), lo que viaja por el tubo son **Objetos** (¡Acordate de la [[12 - Programación Orientada a Objetos (OOP - Intro)|Nota de OOP de Python]]!).
Si pedís la lista de procesos con el comando `Get-Process` y se la enviás a otro comando, no le estás mandando un texto; le estás mandando *Cientos de Objetos de Procesos vivos*. El siguiente comando puede, matemáticamente, decir: *"De todos estos objetos que me pasaste, agarrá su Propiedad `CPU` y mostrame solo los que gastan más de 50%"*. Es infinitamente más limpio y programable.

---

## 🛠️ Comandos (Cmdlets) básicos

La sintaxis de PowerShell es extremadamente fácil de leer porque siempre usa el formato **Verbo-Sustantivo**.

- **Listar Archivos:** `Get-ChildItem` (o su alias `ls`).
- **Ver Procesos:** `Get-Process`
- **Detener un Proceso:** `Stop-Process -Name "chrome"`
- **Ver Servicios:** `Get-Service`
- **Interactuar con el Registro:** Se maneja como si fuera un disco duro. `cd HKLM:\Software\`
- **Hacer una petición Web:** `Invoke-WebRequest -Uri "http://google.com"`

---

## 💥 La Pesadilla Defensiva (Fileless Malware)

PowerShell le da a los administradores el poder de controlar todo Windows. Naturalmente, los atacantes se dieron cuenta de esto y cambiaron la forma de hacer Malware.

### El problema del Antivirus Clásico
Un Antivirus tradicional revisa tu disco rígido (`C:\`). Si descargás un archivo llamado `virus.exe`, el Antivirus lo analiza y lo borra antes de que lo abras.

### El Ataque "Sin Archivos" (Living off the Land)
El atacante ya no envía virus ejecutables (`.exe`). Envía un simple documento de Word trampa.
1. La secretaria abre el Word.
2. El Word ejecuta una macro invisible que simplemente abre la consola nativa de PowerShell de la propia secretaria.
3. El PowerShell ejecuta el comando `Invoke-WebRequest` para descargar el verdadero código malicioso directo desde Internet... **¡Pero lo carga e inyecta directamente en la Memoria RAM, sin guardarlo nunca en el disco rígido!**
4. Como nunca se escribió ningún `.exe` nuevo en el disco, el Antivirus tradicional no se entera de nada, y el Malware opera impunemente desde la RAM usando las propias herramientas legítimas de Windows ("Vivir de la Tierra" / LoTL).

### La Solución de Microsoft (AMSI + EDR)
Para defenderse de los ataques Fileless de PowerShell, Microsoft tuvo que inventar **AMSI** (Antimalware Scan Interface). Es un escudo que lee el código de PowerShell *mientras está corriendo en la memoria RAM*, justo antes de que se ejecute. Si el código parece sospechoso, lo frena.
Además, obligó a las empresas a reemplazar los "Antivirus" por **EDRs** (Endpoint Detection and Response), que vigilan comportamientos extraños de los procesos (Ej: *"Es sospechoso que Microsoft Word intente abrir una terminal de PowerShell; voy a matarlos a ambos"*).

---

## 📌 Must Know (Imprescindible)
- La diferencia estructural de PowerShell vs Bash: PowerShell pasa **Objetos .NET** a través de los Pipes, no texto plano.
- Sintaxis estándar: **Verbo-Sustantivo** (Ej. `Get-Process`, `Set-ExecutionPolicy`).
- El concepto del **Fileless Malware** y "Living off the Land" (Utilizar herramientas nativas y legítimas del propio sistema operativo, operando solo en memoria RAM, para evadir los antivirus basados en disco).

---

## 🔄 Preguntas de repaso
1. Estás trabajando en una terminal. Ejecutás un comando para ver la lista de usuarios y luego aplicás un "Pipe" (`|`) para intentar ordenar esa salida. Sin usar comandos de manipulación de cadenas de texto (como `cut` o `awk`), el comando de la derecha lee directamente el atributo `UserName` con éxito. ¿Estás trabajando en una terminal de Bash (Linux) o en PowerShell (Windows)? Justificá la respuesta.
2. Un empleado descarga un adjunto PDF de su correo. El PDF contiene un script embutido que se lanza de forma oculta usando el comando `powershell.exe -ExecutionPolicy Bypass -Command "Invoke-WebRequest..."`. El atacante utiliza el parámetro especial de PowerShell (que permite cargar el contenido directamente en memoria en lugar de usar `Out-File`). ¿Cómo se le llama a esta táctica de malware, diseñada para frustrar a los antivirus tradicionales?
3. En tus propias palabras, explicá por qué el fenómeno del "Living off the Land" (Vivir de la tierra) vuelve inútil a las técnicas de bloqueo clásicas de los Blue Teams, donde antes simplemente desinstalaban o bloqueaban los programas peligrosos para que la PC estuviera segura.

**➡️ Siguiente nota:** [[13 - Ejercicios]]
