# 05 - Meterpreter (El Payload Supremo)

## 🎯 Objetivos
- Entender las inmensas limitaciones de una "Shell de texto clásica".
- Conocer la evolución del control remoto: Meterpreter.
- Aprender las ventajas del In-Memory Execution (Ejecución puramente en RAM).

---

## 🧠 Concepto: La Terminal Inestable (Shell tradicional)

Cuando usamos un Payload tradicional como `windows/shell/reverse_tcp`, logramos que Metasploit nos devuelva a nuestra consola un acceso remoto idéntico a la terminal CMD.exe negra de Windows (Command Prompt).

**Los graves problemas de la Shell tradicional:**
- Es un asco de operar. Si escribís mal un comando, a veces la conexión entera colapsa y perdés tu acceso a la víctima.
- Si querés extraerle archivos a la víctima, no podés. Una shell cruda es solo texto, no tiene comandos mágicos de "Descargar archivo" (Tendrías que codificar todo a mano en Base64).
- ¡Deja rastros! Para que la Shell clásica exista, Windows tiene que levantar el proceso `cmd.exe` y dejarlo corriendo bajo tu usuario. El Blue Team, mirando los procesos activos, instantáneamente nota tu presencia y te aniquila cerrando el proceso.

---

## 🛸 La Evolución: Meterpreter

**Meterpreter** (Meta-Interpreter) no es un sistema operativo. Es un Payload extremadamente complejo diseñado exclusivamente por los creadores de Metasploit para solucionar los problemas post-explotación.

Cuando inyectás Meterpreter, tu consola de MSFconsole cambia al entorno verde de `meterpreter >`.
Acabás de obtener poderes de semidiós sobre la máquina víctima. En lugar de lidiar con comandos toscos de Windows, interactuás con la API de Meterpreter que trae cientos de comandos propios y automatizados para el Hacker.

### Funciones Estrella de Meterpreter
Desde el prompt `meterpreter >`, podés ejecutar comandos con una sola palabra:
- **`sysinfo`**: Te da los datos del Sistema Operativo exacto de la víctima.
- **`getuid`**: Te dice como quién estás logueado en la víctima (¿Admin o Normal?).
- **`download /ruta/secreta.txt`**: Descarga automáticamente un archivo de la víctima a tu máquina atacante.
- **`upload /mi_virus.exe`**: Sube tu propio virus adicional a la víctima en un parpadeo.
- **`hashdump`**: *La gallina de los huevos de oro.* Extrae todos los hashes de contraseñas NTLM almacenados en la base local (SAM) de Windows para que los crackees offline.
- **`keyscan_start`**: Inicia un Keylogger nativo e invisible que graba todas las pulsaciones de teclado que hace el usuario en ese momento.
- **`screenshot`**: Te saca literalmente una "Foto" de la pantalla del monitor del usuario para ver qué está mirando (Ideal para saber si dejó su cuenta bancaria abierta).

---

## 👻 La Invisibilidad Absoluta (In-Memory Execution)

Meterpreter es hermoso y poderoso, pero la razón real de por qué todo hacker del planeta lo usa, radica en **cómo funciona a bajo nivel**.

Meterpreter funciona mediante **Inyección Reflejada de DLL**.
- Cuando lográs el exploit, Meterpreter NUNCA se escribe en el Disco Duro de la víctima. Jamás. No hay ningún archivo `meterpreter.exe` en `C:\`.
- **Vive única y puramente dentro de la Memoria RAM**, enganchándose adentro de las entrañas de otro proceso legítimo que ya estaba encendido en Windows (Ej: Se esconde adentro de un `svchost.exe` del sistema).
- Esta táctica ninja (Fileless) engaña por completo a los Antivirus tradicionales y Forenses Básicos, ya que si escanean el disco buscando tu Payload, no van a encontrar absolutamente nada.

*(Desventaja: Al vivir solo en la RAM temporal, si el usuario apaga o reinicia la computadora, Meterpreter desaparece en el éter y perdiste tu acceso. Deberás volver a explotarlo, o configurar técnicas de Persistencia que veremos en la próxima nota).*

---

## 📌 Must Know (Imprescindible)
- Diferenciar el poder de una **Command Shell cruda** (Terminal de texto básica y ruidosa) vs **Meterpreter** (Payload supercargado con módulos forenses/ofensivos integrados y automáticos).
- Memorizar los 4 comandos salvadores: **`sysinfo`, `hashdump`, `upload`, `download`**.
- El rasgo clave defensivo: Meterpreter corre utilizando **Ejecución en Memoria (In-Memory)** sin tocar el Disco Físico (evadiendo firmas de Antivirus y dejando una huella forense casi nula).

---

## 🔄 Preguntas de repaso
1. Estás auditando una máquina Windows de la cual acabás de obtener un acceso por red utilizando un exploit. Si el Payload inyectado fue una "Bind Shell" tradicional (`windows/shell_bind_tcp`), el analista de Blue Team notará rápidamente la anomalía al ver que un proceso sospechoso se ejecutó (como `cmd.exe`) atado al puerto abierto. ¿Cómo soluciona Meterpreter, a nivel de rastros en el disco duro y Antivirus, la presencia de la intrusión?
2. Un atacante en etapa de post-explotación se encuentra dentro del entorno de la consola interactiva de `meterpreter >`. Quiere obtener los Hashes de contraseña NTLM de los usuarios locales del servidor para realizar Pass-The-Hash. En una terminal Windows normal necesitaría usar herramientas pesadas externas como *Mimikatz*, pero estando en Meterpreter, ¿Qué comando nativo de una sola palabra le extraerá esa base de datos instantáneamente hacia su pantalla?
3. Sabiendo que el diseño nuclear de Meterpreter (que le otorga su extrema ventaja de invisibilidad / evasión de Antivirus) se basa en el principio de inyectar y residir puramente como un hilo dentro del proceso de la Memoria RAM de la máquina (`In-Memory Execution`), ¿Cuál es la mayor desventaja operativa y táctica (el "Talón de Aquiles") que sufre el atacante en caso de que la víctima se vaya a dormir y apague físicamente el servidor Windows?

**➡️ Siguiente nota:** [[06 - Post-Explotación (Pivoting y Persistencia)]]
