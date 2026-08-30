# 01 - Introducción a Linux y la Terminal

## 🎯 Objetivos
- Entender qué es Linux (y qué es una Distribución o "Distro").
- Comprender la anatomía de un sistema Linux (Kernel vs Shell).
- Conocer por qué la Terminal es superior a la Interfaz Gráfica para tareas de seguridad.

---

## 🧠 ¿Qué es Linux?

Si usás una computadora normal, probablemente uses Windows o macOS. 
**Linux** es otro Sistema Operativo, pero con diferencias filosóficas clave: es de código abierto (Open Source), gratuito, y construido colaborativamente por miles de programadores en todo el mundo (empezando por Linus Torvalds en 1991).

Técnicamente, "Linux" es solo el **Kernel** (el núcleo) del sistema operativo, la parte invisible que habla directamente con el hardware (el procesador, la memoria RAM, los discos duros). 

Para que un ser humano pueda usar ese Kernel, diferentes organizaciones le añaden herramientas, entornos gráficos, navegadores web y administradores de paquetes, creando lo que se conoce como una **Distribución de Linux (Distro)**.

### Distros famosas en Ciberseguridad
- **Debian / Ubuntu:** Son las distribuciones base más populares para servidores web mundiales.
- **Kali Linux / Parrot OS:** Son distribuciones "pre-empaquetadas" específicamente para Seguridad Ofensiva (Red Team). Vienen con cientos de herramientas de hackeo preinstaladas (Nmap, Metasploit, Wireshark). Ambas están basadas en Debian.
- **Red Hat / CentOS / Fedora:** Muy populares en entornos corporativos (bancos, gobiernos).

---

## 🐚 El Shell y la Terminal

En un sistema operativo moderno de escritorio, interactuás usando el mouse y ventanas (Interfaz Gráfica o **GUI** - Graphical User Interface). 

En servidores Linux (que no tienen monitor físico), la única forma de interactuar es mediante la **CLI (Command Line Interface)**.
Para hacer esto usás un programa llamado **Emulador de Terminal**, que a su vez ejecuta otro programa vital llamado **Shell**.

### ¿Qué es el Shell?
El **Shell (Intérprete de Comandos)** es el programa que "escucha" lo que escribís en tu teclado, entiende el comando, y se lo pasa al Kernel para que lo ejecute. 
- En Windows, el shell histórico es `cmd.exe` y el moderno es `PowerShell`.
- En Linux, el shell estándar por excelencia es **Bash** (Bourne-Again Shell). Otro muy popular y moderno (por defecto en Kali y macOS) es **Zsh**.

---

## ❓ ¿Por qué usar la Terminal (CLI) en Ciberseguridad?

Al principio, usar comandos de texto parece un paso atrás en la tecnología (te sentís en los años 80). Pero pronto descubrirás que:

1. **Velocidad y Precisión:** Buscar la palabra "Error" en 500 archivos de texto distintos tomando el mouse y abriendo archivo por archivo tomaría horas. En la terminal toma 0.5 segundos usando `grep`.
2. **Automatización:** Si tenés que crear 100 usuarios nuevos en el sistema, en modo gráfico harías 100 clics y escribirías 100 veces. En la terminal, escribís un archivo (Script) de 3 líneas y la computadora lo hace sola.
3. **Poder sin restricciones:** La interfaz gráfica (GUI) está diseñada para "proteger" a los usuarios comunes, impidiendo que rompan el sistema. La terminal no te protege. Si le ordenás que borre todo el disco duro sin hacer preguntas, lo hará. Esto te da el control absoluto necesario para administrar (o comprometer) un servidor.
4. **Disponibilidad:** El 99% de los servidores del mundo (incluyendo dispositivos de IoT, Routers y Firewalls) ejecutan sistemas operativos basados en Unix/Linux que *no tienen interfaz gráfica instalada* para ahorrar recursos. Si conseguís acceso a un servidor mediante una vulnerabilidad (Reverse Shell), solo tendrás una línea de comandos. Debes saber cómo sobrevivir ahí.

---

## 📌 Must Know (Imprescindible)
- La diferencia entre el Kernel (núcleo invisible) y el Shell (intérprete de comandos como Bash).
- Las ventajas de usar CLI (Velocidad, Automatización) sobre GUI.

## 💡 Good to Know (Bueno saberlo)
- A diferencia de Windows, que usa letras para los discos duros (`C:\`, `D:\`), en Linux todo comienza desde un único punto llamado "Directorio Raíz" (Root Directory), representado por una simple barra diagonal (`/`).

---

## 🔄 Preguntas de repaso
1. Explicá con tus palabras por qué decimos que "Kali" no es un sistema operativo nuevo, sino una Distribución de Linux.
2. Si un atacante logra obtener acceso remoto a un servidor web corporativo, ¿por qué es altamente improbable que tenga un entorno de escritorio con "ventanitas" (mouse y ventanas) disponible para usar?
3. ¿Cuál es el nombre del Shell más utilizado tradicionalmente en entornos Linux?

**➡️ Siguiente nota:** [[02 - FHS (Estructura de Directorios)]]
