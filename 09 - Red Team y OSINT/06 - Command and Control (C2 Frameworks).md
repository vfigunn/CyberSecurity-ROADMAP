# 06 - Command and Control (C2 Frameworks)

## 🎯 Objetivos
- Entender el límite del Metasploit clásico para operaciones sigilosas largas (APT).
- Descubrir la Arquitectura C2 (Command & Control) de los Red Teams profesionales y Estados Nación.
- Conocer las herramientas comerciales (Cobalt Strike) y open-source (Mythic).

---

## 🧠 Concepto: El límite del Hacker Ruidoso

En la [[08 - Metasploit/05 - Meterpreter (El Payload Supremo)|Nota 05 de Metasploit]], aprendimos que la Reverse Shell de `Meterpreter` es genial. Pero tiene un problema enorme: **Es extremadamente ruidosa.**

Cuando Meterpreter se conecta desde la víctima hacia la computadora del atacante, mantiene un **túnel abierto y activo el 100% del tiempo**. El Firewall defensivo de la empresa (SOC) va a ver que hay una conexión a Internet sin parar (24/7) saliendo de la PC de la secretaria, lo considerará sospechoso, y cortará el cable. El atacante pierde el acceso para siempre.

En una simulación Red Team real, el atacante no quiere tener una terminal abierta las 24 horas. Quiere que el virus duerma en secreto.
Aquí entra la Fase 6 de la Kill Chain: **El C2 (Command & Control)**.

---

## 📡 ¿Qué es un Servidor C2?

Un servidor C2 (o C&C) es el "Centro de Mando Galáctico" del atacante, alojado en Internet de manera anónima.

La genialidad del C2 es que **no mantiene una conexión activa**. Utiliza el concepto militar de **Beaconing (El Faro)**.
El virus (Payload/Agente) se instala en la máquina de la víctima, y en lugar de abrir una terminal directa al hacker, el virus se pone a dormir, de forma completamente invisible.

**El Flujo del Faro (Beaconing):**
1. El virus duerme 60 minutos en la PC de la víctima. 
2. A los 60 minutos, el virus se despierta 1 microsegundo. Realiza una pequeñísima conexión HTTP, simulando ser el navegador de la víctima buscando una página web común (Ej: Entra a un servidor falso del hacker disfrazado de Foro de Noticias).
3. El virus lee un código oculto en esa página web falsa: *"El hacker dejó una orden diciendo que le descargue las contraseñas"*.
4. El virus descarga las contraseñas, se las envía (las sube como una foto a la página falsa), y se vuelve a dormir otros 60 minutos.

**El Resultado:** Para el analista defensivo, es imposible cazar el virus. Porque lo único que ve en el Firewall es que la secretaria entró 1 segundo a una página de noticias a las 10:00, otro segundo a las 11:00... Parece tráfico web legítimo. Además, introducen "Jitter" (Matemática aleatoria) para que a veces llame a los 40 minutos, a los 82 minutos, rompiendo los patrones robóticos y frustrando a la Inteligencia Artificial del antivirus.

---

## 🔨 Herramientas C2 Famosas

A los payloads C2 no se les llama "Shell", se les llama **Agentes o Beacons**.

1. **Cobalt Strike (El Rey Comercial):** 
   Es la herramienta más temida del planeta. Desarrollada legalmente para Red Teams (Cuesta $5.000 por usuario). Su payload (*Beacon*) es tan avanzado que el 90% del Ransomware criminal moderno utiliza copias piratas de Cobalt Strike para orquestar sus redes de bots mundiales y camuflar el tráfico como DNS o HTTP inofensivo (Táctica conocida como *Malleable C2*).
2. **Sliver C2 / Mythic / Covenant (Los Reyes Open-Source):**
   Frente al monopolio de Cobalt Strike, la comunidad de seguridad desarrolló C2s gratuitos hiper-avanzados. Te permiten controlar redes de 500 computadoras zombis infectadas desde un tablero de control colaborativo (donde varios hackers del Red Team trabajan juntos sobre la misma interfaz compartida).

---

## 📌 Must Know (Imprescindible)
- Qué es la infraestructura **C2 (Command and Control)**: El servidor maestro (y sus canales de comunicación) que utiliza un Red Team o un criminal para comandar un ejército de computadoras comprometidas o para dirigir una infiltración de larga duración (APT).
- Entender la diferencia entre la **Reverse Shell clásica** (Conexión TCP ruidosa, constante y viva 24/7) y el **Beaconing** de C2 (El agente duerme la mayor parte del tiempo y solo "despierta" periódicamente, disimulando su tráfico como peticiones web normales (HTTP) para preguntar si hay nuevas órdenes del amo, eludiendo la detección de tráfico anómalo).
- El término **Jitter**: Introducir ruido o porcentajes aleatorios (aleatoriedad temporal) entre las pausas del Beaconing, impidiendo que el Blue Team note una sincronización robótica exacta de X minutos.

---

## 🔄 Preguntas de repaso
1. Una campaña de Red Team logró infectar a la empresa usando "Cobalt Strike". El Agente/Payload inyectado tiene configurado un mecanismo de *Beaconing* (Comunicación tipo Faro) en base al protocolo HTTP. Explicá con tus palabras por qué esta técnica de comunicación que duerme silenciosamente y despierta en intervalos (frente a una conexión TCP interactiva viva y constante las 24 hs de una Bind/Reverse shell clásica de Metasploit) resulta infinitamente superior para evadir la detección en una red corporativa.
2. Un Blue Team que monitorea los logs del Firewall nota un patrón súper predecible: La IP de un empleado realiza una conexión externa `HTTP GET` diminuta de 2 kilobytes hacia un servidor desconocido cada exactamente 30 minutos (Conexión 10:00am, 10:30am, 11:00am), descubriendo así al agente C2 encubierto y bloqueándolo. Teniendo en cuenta la configuración temporal de los "Beacons", ¿Qué parámetro crítico olvidó agregar el equipo atacante (Red Team) en su C2 (que sirve para aleatorizar matemáticamente el tiempo de espera) resultando así en su descubrimiento?
3. En la arquitectura clásica de un ataque APT (Advanced Persistent Threat), ¿cuál es el software comercial legalmente diseñado para simulaciones tácticas Red Team de nivel militar que se hizo extremadamente popular por sus *Beacons*, pero cuyo abuso masivo por Cibercriminales lo convirtió en el C2 más interceptado del planeta (Su nombre se basa en el elemento Co-27)?

**➡️ Siguiente nota:** [[07 - Armado de Reportes y Triage]]
