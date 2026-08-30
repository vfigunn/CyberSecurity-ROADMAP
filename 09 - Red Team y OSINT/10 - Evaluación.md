# 10 - Evaluación del Módulo 09 (Red Team y OSINT)

## 📝 Instrucciones
Evaluación final de tácticas de inteligencia. Múltiples preguntas basadas en los Frameworks, herramientas de reconocimiento y estructuras de campañas ofensivas.
Anotá tus respuestas y luego verificalas con el solucionario.

> **Importante:** Las respuestas correctas y explicaciones están en: `[[29 - Evaluaciones/Respuestas/Módulo 09 - Respuestas]]`.

---

## 🎯 Sección 1: OSINT e Ingeniería Social

**1. En el contexto cibernético del Reconocimiento Pasivo (OSINT corporativo), si tu objetivo primario como atacante es recopilar decenas de cuentas de correo electrónico de empleados (`@empresa.com`) para luego usarlas en tu campaña de envíos de Phishing, ¿cuál de las siguientes es una herramienta automatizada famosa, legal y universalmente utilizada en esta fase para escrapear/escanear fuentes públicas como Google y LinkedIn sin tocar al cliente?**
A) theHarvester
B) Wireshark
C) Nmap (Port Scanning)
D) Metasploit Framework (Meterpreter)

**2. Utilizando inteligencia técnica basada en "Dorking", necesitás forzar al motor a entregarte únicamente servidores en Internet que muestren accidentalmente su índice o listado en crudo. ¿Cuál de estos Dorks y operadores pertenece a la sintaxis del motor de Google para aislar sitios expuestos mediante sus pestañas de título superior?**
A) `ping: 8.8.8.8 -t`
B) `intitle:"index of"`
C) `port:22 os:"windows"`
D) `select * from users where index`

**3. Un criminal lanza una sofisticada campaña de ataque telefónico. Modifica el identificador de llamadas ("Caller ID spoofing") para que en la pantalla de la recepcionista del banco figure "Soporte Central IT". Llama, se hace pasar por su jefe de redes con tono autoritario, y la engaña psicológicamente para que ella descargue y ejecute un programa troyanizado de reparación. ¿Cómo clasifica técnicamente la industria a esta estricta modalidad de Ataque de Capa 8 (Ingeniería Social)?**
A) Baiting (Uso de memorias Flash)
B) Ransomware As A Service (RaaS)
C) Vishing (Phishing Híbrido de Voz / Voice-Phishing)
D) Phishing clásico de Email (Spam)

---

## 🎯 Sección 2: Campañas (Red/Purple Team y C2)

**4. ¿Por qué razón arquitectónica y conceptual un equipo "Red Team" contratado en una corporación multinacional abandonará el uso clásico de `Metasploit / Meterpreter` a las pocas horas, optando obligatoriamente por utilizar redes y Frameworks avanzados de C2 (Command & Control) como `Cobalt Strike` o `Mythic`?**
A) Porque Metasploit es un software pago (cuesta millones), mientras que Cobalt Strike es gratuito y de código abierto.
B) Porque los Reverse Shells nativos mantienen canales de conectividad (Túneles) caóticos, vivos y constantes que los SOCs del Blue Team detectan por anomalía (ruido constante). Los C2 en cambio, simulan tráfico web inofensivo que "duerme" por largo tiempo y consulta asíncronamente con tácticas de *Beaconing* e inyección de *Jitter* aleatorio.
C) Porque los C2 incluyen una Inteligencia Artificial que desarrolla automáticamente Exploits 0-day diarios.
D) Porque los C2 solo operan en Linux y el objetivo es Windows.

**5. Si tu empresa decide abandonar el paradigma tóxico del "Hacker externo que los ataca a ciegas por sorpresa y les deja el documento PDF al terminar sin ayudar a mejorar", optando en su lugar por un formato donde los equipos "Atacante" y "Defensor" conviven en la misma sala para probar inyecciones maliciosas y calibrar/crear reglas del SOC/Firewall en tiempo real trabajando en conjunto. ¿Qué tipo de simulación colaborativa se está llevando a cabo?**
A) Grey-Box Pentesting
B) Bug Bounty Hunting
C) Black Hat Teaming
D) Purple Teaming

---

## 🎯 Sección 3: Marcos Normativos (Kill Chain, MITRE y CVSS)

**6. De acuerdo a la metodología tradicional militar estructurada por la corporación aeroespacial Lockheed Martin y dividida en 7 pasos interconectados (conocida mundialmente como la Cyber Kill Chain). ¿Cuál es la idea filosófica defensiva (Blue Team) detrás de enmarcar y modelar en este sistema al atacante APT (Avanzado, Persistente y Lento)?**
A) Si los defensores logran interrumpir el paso de ataque en cualquier momento (ej: Bloqueando la Ejecución), el adversario no podrá llegar a las "Acciones de los Objetivos/Exfiltración final", obligándolo a fracasar en su campaña.
B) Asume que el atacante APT no requiere OSINT, sino que empieza mágicamente desde adentro de la empresa el día 1, debiendo frenarlo solo por hardware físico.
C) Establece que los Firewalls deben reestructurar en un 100% el algoritmo AES.
D) Es simplemente una lista para categorizar los costos económicos que deberá pagar la víctima según la región.

**7. Ante la antigüedad de la Cyber Kill Chain, la industria convergió hacia un sistema mucho más moderno, hiper-granular y adoptado como el "Lenguaje Defensivo Universal". Esta Matriz mundial de ciberseguridad clasifica tácticamente cada acción del atacante mediante el sistema Tácticas y Técnicas y es esencial para mapear coberturas defensivas y EDR. ¿A qué base de conocimiento (Framework) nos referimos?**
A) NIST SP-800
B) ISO 27001
C) MITRE ATT&CK Framework
D) Base OVAL de Linux

**8. Realizaste un reporte final de vulnerabilidades (Triage). Explotaste una base de datos mediante un inyección (SQLi) ciega en la página web. Pero, en vez de asignarle "Severidad Extrema" por pálpito instintivo (Opinión manual), introdujiste los vectores de tu descubrimiento técnico en una fórmula que toma parámetros universales tales como "Tipo de Acceso Red vs Local, Requisito de Privilegios Administrativos previos de usuario, e Impacto final sobre la confidencialidad", dándote un puntaje cerrado de `9.1` sobre 10 (Crítico). ¿Qué sistema de calificación estandarizado has empleado correctamente?**
A) NVD
B) Exploit-DB Score
C) CVSS (Common Vulnerability Scoring System)
D) Puntuación OWASP Risk-Level

---

> **¿Terminaste?**
> Buscá el archivo en la carpeta de evaluaciones para comparar tus resultados (Módulo 09 - Respuestas).

**➡️ Siguiente nota:** [[11 - Resumen]]
