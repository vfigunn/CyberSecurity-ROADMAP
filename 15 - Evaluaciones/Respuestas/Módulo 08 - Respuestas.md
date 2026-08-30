# Respuestas Evaluación Módulo 08 - Metasploit

A continuación se presentan las respuestas correctas de la evaluación del [[08 - Metasploit/09 - Evaluación|Módulo 08]], junto con la justificación técnica de cada una.

---

### Sección 1: Taxonomía y Payloads

**1. D) Es el bloque de código diseñado exclusivamente para aprovechar un fallo en el diseño del objetivo y vulnerar el perímetro/abrir la puerta inicial.**
> *Justificación:* En la separación arquitectónica y lógica de Metasploit, el Exploit no es el troyano, ni roba los datos (Ese es el Payload de inyección posterior). El Exploit es el factor de entrega. Su única finalidad (Como un torpedo militar de apertura) es el de aprovechar la grieta y generar la rotura transitoria de seguridad.

**2. C) Reverse Shell (Conexión Inversa)**
> *Justificación:* Las defensas perimetrales perdonan muy poco los escaneos o intentos de conectar desde Internet (Inbound). Sin embargo, casi todos los servidores internos necesitan enviar correos, descargar parches, navegar (Tráfico Saliente / Outbound). Forzar que el interior de la víctima "Llame por voluntad propia" hacia el exterior (casa del atacante) evade masivamente los bloqueos.

**3. B) Un Payload en fases de tipo "Staged", separado mediante la Barra Inclinada (`/`).**
> *Justificación:* A veces "toda la maleta del virus" es demasiado pesada. Para burlar vulnerabilidades con minúsculo límite de inyección, se diseñó la separación `/`. Esto indica que en Fase 1 viaja un código hiperligero llamado "Stager", y en Fase 2 (cuando ya evadió las barreras), se descarga el grueso ("Stage"). Ej: `windows/meterpreter/reverse_tcp`.

---

### Sección 2: Operación con MSFconsole

**4. C) `show options`**
> *Justificación:* Este comando es el pan y mantequilla de MSF. Nos exhibe la tabla con las opciones tanto del Módulo que va a romper como de la carga Payload seleccionada. Si no ves esta tabla y configuras a ciegas, el misil fallará en 9 de 10 intentos.

**5. A) `set RHOSTS 192.168.1.100` y `set LHOST 10.10.10.5`**
> *Justificación:* Las variables mandatorias: **RHOSTS** ("Remote Hosts") significa a quién le pegamos/objetivo; por eso debe llevar la IP de la víctima (192.168.1.100). **LHOST** ("Local Host") indica quién alojará la respuesta / El atacante en escucha; por ende necesita tu IP (10.10.10.5).

**6. C) `run`**
> *Justificación:* Por practicidad para tipear más rápido o costumbre derivada de módulos que no son meramente Exploits (como los escáneres *Auxiliary* que se "corren"), Metasploit acepta de igual y absoluta forma el alias "run" en lugar de "exploit".

---

### Sección 3: Post-Explotación Avanzada

**7. A) Porque Meterpreter opera mediante la "In-Memory Execution" (Ejecución reflejada en Memoria RAM), existiendo puramente como hilos inyectados dentro de procesos legítimos (ej. `svchost.exe`) sin haber escrito ningún archivo físico (`.exe`) en el disco duro.**
> *Justificación:* Uno de los enormes atractivos del ecosistema Meterpreter es ser de las primeras herramientas estandarizadas "Fileless". Esta capacidad ninja deja ciegos a los antivirus tradicionales que operan por "Firmas" (chequeando discos), requiriendo soluciones hiper-avanzadas (EDR o heurística de comportamiento RAM) para cazarlo.

**8. B) Routing y Pivoting (Uso del sistema como enrutador malicioso puente)**
> *Justificación:* Esta es la base central del Movimiento Lateral (Técnica militar de Red Team). La computadora comprometida, habiendo perdido su honor local, ahora actúa y enruta el tráfico destructivo hacia el ecosistema inexplorado profundo, convirtiéndose en el "Pivote".

**9. D) `hashdump`**
> *Justificación:* A diferencia de las herramientas locales donde requerirías comandos largos manuales, volcados de memoria y extracciones offline; la integración modular de Metasploit ha provisto un solo comando (Hashdump) que automatiza y estandariza el vaciado completo del gestor de identidades sin alertar ni complicar al atacante.

**10. B) La sesión remota perecerá sin retorno. El atacante debió instalar técnicas de "Persistencia" para asegurar que, al día siguiente, el sistema vuelva a establecer forzosamente el contacto y la conexión inversa.**
> *Justificación:* Lo temporal, por naturaleza, muere con la falta de energía. Al estar inyectado y atado 100% en el entorno vivo (RAM) de otro programa, el apagado de Windows limpia todo y destruye el imperio remoto. Las estrategias persistentes en registros/tareas programadas sirven como los salvavidas silenciosos obligatorios de post-explotación.
