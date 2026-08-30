# 05 - Cyber Kill Chain y MITRE ATT&CK

## 🎯 Objetivos
- Entender cómo los militares y las corporaciones modelan los ataques.
- Conocer la Cyber Kill Chain de Lockheed Martin (Las 7 fases del ataque).
- Aprender a leer la Matriz Táctica MITRE ATT&CK, el lenguaje universal defensivo actual.

---

## 🧠 Concepto: Rompiendo el Mito del "Hacker Mágico"

Las empresas creían que el Hackeo era un evento espontáneo. Un segundo estabas bien, al segundo siguiente te caía un rayo y perdías los datos.
La corporación aeroespacial Lockheed Martin cambió la historia al introducir la **Cyber Kill Chain (Cadena de Muerte Cibernética)**.

Comprobaron que un ciberataque APT (Advanced Persistent Threat) no es un rayo. Es una **secuencia de pasos matemáticos obligatorios**. 
Si el atacante no cumple el Paso 1, no puede pasar al Paso 2. Si el Blue Team logra detectar o "romper" la cadena del atacante en el Paso 4, el ataque entero fracasa y la empresa se salva.

---

## 🔗 Las 7 Fases de la Cyber Kill Chain (Lockheed Martin)

1. **Reconocimiento (Reconnaissance):** El atacante usa OSINT, busca empleados en LinkedIn, mapea servidores en Shodan. (El atacante planea).
2. **Armamento (Weaponization):** El atacante decide que va a atacar al empleado de RR.HH. En su casa, fabrica un PDF malicioso (El Exploit + el Payload).
3. **Entrega (Delivery):** El atacante envía el PDF por correo (Phishing), o pone un pendrive en el piso. (El ataque viaja).
4. **Explotación (Exploitation):** El empleado abre el PDF. La vulnerabilidad del Adobe Reader se dispara (Se rompe la puerta).
5. **Instalación (Installation):** El virus (Payload) se instala en el sistema operativo del empleado, estableciendo Persistencia (ej. claves de registro) para sobrevivir a un reinicio.
6. **Command & Control (C2):** El virus instalado llama por teléfono "a casa". Establece un túnel bidireccional cifrado con el servidor del atacante. El atacante ahora tiene control de teclado y ratón remoto.
7. **Acciones sobre el Objetivo (Actions on Objectives):** Recién ahora, 3 meses después de empezar, el atacante roba la base de datos de los clientes, cifra los discos (Ransomware), o destruye los respaldos.

*(Romper cualquiera de los primeros 6 pasos salva los datos de la empresa).*

---

## 🗺️ MITRE ATT&CK (La Matriz del Presente)

La Kill Chain era brillante pero demasiado filosófica. "Armamento" no te dice mucho a nivel técnico.
La corporación MITRE creó el framework **ATT&CK (Adversarial Tactics, Techniques, and Common Knowledge)**.

Hoy, todo el planeta Tierra usa MITRE ATT&CK. Es una Matriz gigante, como una tabla periódica de elementos.
Divide a los ciberataques en 14 Tácticas (El QUÉ quiere lograr el atacante) y cientos de Técnicas (El CÓMO lo logra).

### Cómo se lee la Matriz (Tácticas vs Técnicas):
- **Táctica (La Meta):** Ej. El atacante quiere *"Exfiltrar Datos"* (Robar archivos).
- **Técnica (El Medio):** Para lograr robar archivos, el atacante utiliza la Técnica `"T1048: Exfiltración sobre Protocolo Alternativo"`. Ocultó los archivos de Excel de la empresa adentro de consultas de ping de DNS para burlar el Firewall y sacarlos.

### ¿Para qué sirve MITRE?
Para que el Red Team y el Blue Team hablen el mismo idioma universal.
- El Red Team ya no dice: *"Entré usando un truco loco"*. Dice: *"Utilicé la Táctica de Acceso Inicial mediante la Técnica T1190 (Explotación de Aplicación Pública)"*.
- El Blue Team toma su sistema de Antivirus (EDR) y revisa si están configurados para bloquear la Técnica T1190. 

---

## 📌 Must Know (Imprescindible)
- **Cyber Kill Chain:** Modelo de 7 pasos obligatorios de un ataque. La filosofía defensiva afirma que si el Blue Team interrumpe la cadena en cualquier paso antes del 7, el objetivo del atacante fracasa por completo.
- **MITRE ATT&CK:** La base de datos y matriz moderna (Tácticas y Técnicas) que clasifica exactamente todos los movimientos conocidos de los hackers, dándole un ID único (ej: T1003) a cada técnica ofensiva.
- **Diferencia Filosófica:** Kill Chain es lineal y de alto nivel. MITRE ATT&CK es granular, técnico, y no lineal (El atacante salta de técnica en técnica como en un tablero de ajedrez).

---

## 🔄 Preguntas de repaso
1. Analizando las 7 fases de la Cyber Kill Chain de Lockheed Martin, si un analista ofensivo descarga un Exploit de Internet y lo empaqueta con un Malware en un documento de Microsoft Word dentro del disco duro en su propia casa (Hacker), ¿En qué etapa exacta de la cadena se encuentra trabajando y por qué el Blue Team aún es técnicamente incapaz de detectarlo?
2. Siguiendo con la Kill Chain, la secretaria recibe el documento (Fase 3: Entrega) y lo abre. El documento aprovecha una vulnerabilidad en Word (Fase 4: Explotación), y el virus se ancla en el Registro de Windows (Fase 5: Instalación). ¿Qué hito indispensable de conectividad (Fase 6) debe lograr ahora ese virus instalado para que el hacker obtenga el dominio manual interactivo de la máquina, antes de poder proceder a robar (Fase 7) los archivos?
3. Un analista defensivo detecta que un empleado extrajo 5 GB de datos corporativos a un servidor externo en China. Al hacer el reporte para el jefe, utiliza el lenguaje universal afirmando que el atacante usó la "Táctica de Exfiltración y la Técnica de Exfiltración a Canal Alternativo (T1048)". ¿Qué Framework global y moderno está utilizando el analista (que reemplazó y expandió granularmente a la antigua Kill Chain)?

**➡️ Siguiente nota:** [[06 - Command and Control (C2 Frameworks)]]
