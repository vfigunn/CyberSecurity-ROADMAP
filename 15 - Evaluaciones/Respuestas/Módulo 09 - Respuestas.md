# Respuestas Evaluación Módulo 09 - Red Team

A continuación se presentan las respuestas correctas de la evaluación del [[09 - Red Team y OSINT/10 - Evaluación|Módulo 09]], junto con la justificación técnica de cada una.

---

### Sección 1: OSINT e Ingeniería Social

**1. A) theHarvester**
> *Justificación:* OSINT y "Pasividad" significan no interactuar con el objetivo. `Nmap` y `Metasploit` (Opciones C y D) son herramientas ofensivas o escáneres altamente ruidosos y activos (Dejan tu IP en el Firewall). `theHarvester`, por el contrario, es la herramienta madre en la etapa Recon: Simplemente hace miles de peticiones a Google, Bing, PGP Keys o LinkedIn (Fuentes Públicas Abiertas) y escrapea las cuentas corporativas caídas ahí sin tocar jamás la empresa oficial.

**2. B) `intitle:"index of"`**
> *Justificación:* Al combinar el operador oficial de metadato `intitle` (Que aísla la búsqueda sólo al nombre visible de la pestaña en el navegador) con la frase clásica `"index of"` (Que es el título que escupen nativamente por defecto los Servidores Apache y Nginx cuando el desarrollador olvidó poner un archivo `index.html` de barrera), el Hacker consigue en un segundo miles de directorios huérfanos con archivos en crudo indexados en la memoria de Google. 

**3. C) Vishing (Phishing Híbrido de Voz / Voice-Phishing)**
> *Justificación:* El Phishing es el vector macro, pero cuando entra en juego la actuación táctica en tiempo real, utilizando el protocolo de redes de voz (Voice) de telefonía convencional combinado con falsificación de números (Spoofing) y apelando al sentido innato de autoridad del ser humano para by-passear controles ("Hacelo o te despido"), se trata puramente de "Vishing" (Voice Phishing).

---

### Sección 2: Campañas (Red/Purple Team y C2)

**4. B) Porque los Reverse Shells nativos mantienen canales de conectividad (Túneles) caóticos, vivos y constantes que los SOCs del Blue Team detectan por anomalía (ruido constante). Los C2 en cambio, simulan tráfico web inofensivo que "duerme" por largo tiempo y consulta asíncronamente con tácticas de *Beaconing* e inyección de *Jitter* aleatorio.**
> *Justificación:* Metasploit es genial para irrumpir a la fuerza (Pentest clásico 1 semana). Pero los Defensores o Centros de Operaciones modernos cazan rápido las conexiones TCP paradas fijas. Los APTs o Estados-Nación asientan y entierran su malware por meses, optando por usar agentes C2 (Command & Control/Beacons) que interactúan pasivamente 1 segundo de cada 60 minutos, mimetizándose con los empleados.

**5. D) Purple Teaming**
> *Justificación:* El color purpura (Unión física y temporal del rojo y azul). Ha demostrado ser por estadística presupuestaria la mejor manera de "calibrar o afinar" (Tuning) la Defensa de una empresa. En vez del hermetismo hostil donde el Blue Team no ve venir al Red Team, aquí ambos trabajan con guiones, codo a codo, permitiendo potenciar drásticamente el retorno de inversión y las barreras orgánicas EDR a coste mínimo.

---

### Sección 3: Marcos Normativos (Kill Chain, MITRE y CVSS)

**6. A) Si los defensores logran interrumpir el paso de ataque en cualquier momento (ej: Bloqueando la Ejecución), el adversario no podrá llegar a las "Acciones de los Objetivos/Exfiltración final", obligándolo a fracasar en su campaña.**
> *Justificación:* La gran genialidad bélica del corporativo de Defensa `Lockheed Martin` fue transformar un ataque espontáneo en una Cadena de Obligatoriedad Matemática dependiente (7 Fases). Destruir o aislar (Con un Firewall en Fase 6 C2, o con Antivirus en Fase 5 Instalación) rompe la cadena irremediablemente, previniendo la Fase Final (Exfiltración / Destrucción final).

**7. C) MITRE ATT&CK Framework**
> *Justificación:* Ante una Cyber Kill Chain que resultaba "Demasiado Filosófica" en 2018 (Decir "Instalación" no aporta base técnica medible y cruda), el gigantesco MITRE estructuró cientos de técnicas agrupadas con el sufijo T (Ej. T1059: Ejecución de Comandos). Aportó estandarización absoluta y agnóstica de proveedores mundiales, permitiendo medir la efectividad o huecos ciegos (Gaps) de los productos defensores y de los atacantes que intenten ofuscar su origen o herramientas de Red Team.

**8. C) CVSS (Common Vulnerability Scoring System)**
> *Justificación:* En todo documento formal, y para impedir que un Pentester "Suba" a crítico y alarme a la gerencia innecesariamente, existe una métrica agnóstica. Si el CVSS recibe los parámetros ("Ataque sobre LAN remota + Sin Claves Previas Necesitadas + Impacto Completo a la Confidencialidad"), nos lanzará matemáticamente una ponderación. Todo analista califica su *Hallazgo o Triage Técnico* utilizando únicamente vectores CVSS para mantener ecuanimidad y profesionalismo ante el Cliente Ejecutivo.
