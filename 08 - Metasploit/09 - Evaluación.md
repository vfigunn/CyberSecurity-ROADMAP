# 09 - Evaluación del Módulo 08 (Metasploit)

## 📝 Instrucciones
Evaluación final del módulo. Múltiples preguntas basadas en la taxonomía de la explotación, el control de red, y la terminal MSFconsole.
Anotá tus respuestas y luego verificalas con el solucionario.

> **Importante:** Las respuestas correctas y explicaciones están en: `[[15 - Evaluaciones/Respuestas/Módulo 08 - Respuestas]]`.

---

## 🎯 Sección 1: Taxonomía y Payloads

**1. En el lenguaje universal del Red Team y Metasploit, ¿cuál es la definición precisa de un "Exploit"?**
A) Es el código malicioso (virus) que realiza la sustracción de datos o robo de contraseñas.
B) Es una herramienta auxiliar para escanear y enumerar versiones de sistemas operativos.
C) Es un componente de cifrado para ofuscar el código e intentar evitar Antivirus.
D) Es el bloque de código diseñado exclusivamente para aprovechar un fallo en el diseño del objetivo y vulnerar el perímetro/abrir la puerta inicial.

**2. Debido al avance en los cortafuegos (Firewalls) modernos, la industria corporativa bloquea estrictamente los puertos entrantes inusuales, frustrando las técnicas clásicas. Para garantizar la evasión y la obtención del control remoto, ¿qué variante de Shell domina en los ataques actuales?**
A) Local Command Shell
B) Bind Shell (Conexión Directa)
C) Reverse Shell (Conexión Inversa)
D) Telnet Shell

**3. Algunos exploits (ej. Buffer Overflows muy limitados) solo te permiten inyectar 150 bytes de memoria como máximo en la víctima antes de causar que el programa se estrelle, haciendo imposible enviar programas pesados. Para solucionar esto, Metasploit ideó un Payload diminuto cuya única función es engancharse en la red y luego descargar silenciosamente el "Verdadero Troyano Pesado". ¿Cómo se le denomina a esta arquitectura y cómo figura su convención de nombre?**
A) Un Payload de tipo "Stageless", separado mediante el símbolo del Guion Bajo (`_`).
B) Un Payload en fases de tipo "Staged", separado mediante la Barra Inclinada (`/`).
C) Un Payload híbrido "Encoder-only".
D) Un módulo Post-Exploitation Auxiliary.

---

## 🎯 Sección 2: Operación con MSFconsole

**4. Estás operando la línea de comandos de `msfconsole`. Luego de buscar el módulo ideal (ej. `search apache`) y equiparlo (ej. `use exploit/...`), necesitás ver los parámetros de configuración y qué IP objetivo atacar antes de lanzar el fuego. ¿Qué comando utilizarías?**
A) `run options`
B) `exploit scan`
C) `show options`
D) `get_target`

**5. Vas a configurar tu consola para lanzar un ataque y configurar una Reverse Shell. Si tu computadora (Hacker) posee la dirección IP `10.10.10.5` y el servidor que quieres vulnerar tiene la dirección IP `192.168.1.100`. ¿Cuál es la configuración absolutamente correcta que tendrías que aplicar en Metasploit con el comando "set"?**
A) `set RHOSTS 192.168.1.100` y `set LHOST 10.10.10.5`
B) `set RHOSTS 10.10.10.5` y `set LHOST 192.168.1.100`
C) `set LPORT 192.168.1.100` y `set RPORT 10.10.10.5`
D) `set TARGET_IP 10.10.10.5` y `set RETURN_IP 192.168.1.100`

**6. Si bien el comando principal para iniciar la ejecución y lanzar el ataque en Metasploit suele ser "exploit", ¿Qué otro comando de tres letras cumple exactamente la misma función y se utiliza como un equivalente o sinónimo?**
A) `go`
B) `atk`
C) `run`
D) `start`

---

## 🎯 Sección 3: Post-Explotación Avanzada

**7. Un Analista Defensivo (Blue Team) investiga el disco duro (Drive C:) de una máquina Windows comprometida, escaneando absolutamente todos los archivos y ejecutables buscando el virus. No encuentra nada sospechoso. Sabiendo que el atacante utilizó Metasploit e inyectó un payload de "Meterpreter", ¿por qué fracasó la búsqueda clásica del Blue Team?**
A) Porque Meterpreter opera mediante la "In-Memory Execution" (Ejecución reflejada en Memoria RAM), existiendo puramente como hilos inyectados dentro de procesos legítimos (ej. `svchost.exe`) sin haber escrito ningún archivo físico (`.exe`) en el disco duro.
B) Porque Meterpreter encripta por defecto el Disco Duro y formatea los registros MFT de Windows.
C) Porque Meterpreter es un virus escrito en el lenguaje ensamblador de la BIOS (UEFI/Bootkit) y vive en la tarjeta gráfica del sistema.
D) Porque el Blue Team olvidó escanear los dispositivos USB conectados.

**8. Has comprometido la máquina de recepción (Servidor Web) y lograste abrir un Meterpreter. Desde ahí, te das cuenta de que no poseés la visibilidad para atacar a la Intranet Secreta Corporativa (a la que tú, desde tu casa, no llegas; pero el Servidor Web internamente sí). Utilizando Metasploit, configuras tu consola para lanzar ataques A TRAVÉS del Servidor Web hackeado. ¿Qué concepto/táctica militar de Redes Ofensivas acabas de realizar?**
A) Phishing Lateral
B) Routing y Pivoting (Uso del sistema como enrutador malicioso puente)
C) Denegación de Servicio Distribuida (DDoS)
D) Cross-Site Scripting (XSS)

**9. Utilizando la poderosa terminal interactiva remota que nos otorga Metasploit (Meterpreter), quieres volcar todos los hashes criptográficos (Contraseñas NTLM) de las cuentas del sistema operativo de tu víctima, los cuales viven protegidos en el proceso LSASS o la base SAM local. ¿Con qué único y simple comando de Meterpreter extraerás toda esa base hacia tu pantalla?**
A) `sysinfo`
B) `keyscan_start`
C) `download sam.txt`
D) `hashdump`

**10. La inmensa desventaja técnica de operar un Payload puro In-Memory (Memoria RAM) como Meterpreter o un Stager fileless es su temporalidad/volatilidad. Si la recepcionista apaga la computadora al finalizar su turno, ¿qué sucederá y qué mecanismo debió instalar el atacante (Ej. alterar el Registro de Auto-arranque de Windows) para solventarlo?**
A) Se mantendrá viva la sesión; el atacante no requiere hacer nada.
B) La sesión remota perecerá sin retorno. El atacante debió instalar técnicas de "Persistencia" para asegurar que, al día siguiente, el sistema vuelva a establecer forzosamente el contacto y la conexión inversa.
C) Metasploit generará un túnel cuántico.
D) El Firewall del atacante crasheará.

---

> **¿Terminaste?**
> Buscá el archivo en la carpeta de evaluaciones para comparar tus resultados (Módulo 08 - Respuestas).

**➡️ Siguiente nota:** [[10 - Resumen]]
