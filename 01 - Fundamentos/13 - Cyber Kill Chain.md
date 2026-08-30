# 13 - Cyber Kill Chain

## 🎯 Objetivos
- Entender el concepto de la Cyber Kill Chain (Cadena de Muerte Cibernética).
- Conocer las 7 fases de un ciberataque tradicional.
- Comprender el valor de este modelo para la defensa proactiva.

---

## 🧠 Concepto

La **Cyber Kill Chain** es un modelo desarrollado por Lockheed Martin (una empresa aeroespacial y militar estadounidense) para describir las etapas de un ciberataque, especialmente los llevados a cabo por Amenazas Persistentes Avanzadas (APTs).

La idea fundamental (tomada de tácticas militares) es que **un ataque no es un evento único, sino un proceso en cadena**. Para que el ataque tenga éxito, el atacante debe completar una serie de fases secuenciales. 

La gran ventaja para los defensores es que **solo necesitan romper un eslabón de la cadena para detener todo el ataque**.

---

## 🔗 Las 7 Fases de la Cyber Kill Chain

1. **Reconnaissance (Reconocimiento):**
   - El atacante investiga a su objetivo. Recopila direcciones de email, escanea la red en busca de servidores expuestos (aumentando la comprensión de su [[10 - Attack Surface|Superficie de Ataque]]), e investiga tecnologías que usa la empresa.
   - *Ejemplo:* Buscar perfiles de empleados de Recursos Humanos en LinkedIn.

2. **Weaponization (Armamentización):**
   - El atacante crea o selecciona la herramienta que usará para el ataque. Junta un archivo "señuelo" con un [[06 - Exploits|Payload malicioso]]. Esta fase ocurre en las máquinas del atacante, por lo que es casi imposible de detectar para la víctima.
   - *Ejemplo:* Crear un archivo PDF falso llamado "Curriculum.pdf" que contiene un virus.

3. **Delivery (Entrega):**
   - El atacante transmite el arma al objetivo.
   - *Ejemplo:* Enviar el PDF malicioso por correo electrónico al departamento de Recursos Humanos de la empresa (Phishing).

4. **Exploitation (Explotación):**
   - El código malicioso aprovecha una [[05 - Vulnerabilidades|Vulnerabilidad]] para ejecutarse en el sistema de la víctima.
   - *Ejemplo:* El empleado abre el PDF, el archivo explota una vulnerabilidad antigua en Adobe Reader y ejecuta el código en la computadora del empleado.

5. **Installation (Instalación):**
   - El malware se instala de forma persistente en la computadora de la víctima. El objetivo es que el atacante no pierda el acceso si la computadora se reinicia.
   - *Ejemplo:* El malware crea una entrada en el Registro de Windows para ejecutarse automáticamente cada vez que se prende la PC.

6. **Command and Control (C2):**
   - El malware instalado se comunica de vuelta "a casa" (a un servidor controlado por el atacante) para pedir instrucciones. A partir de este momento, el atacante tiene control manual sobre la computadora infectada (hands-on-keyboard).
   - *Ejemplo:* La PC del empleado infectado empieza a enviar y recibir datos encriptados hacia una dirección IP extraña en Rusia.

7. **Actions on Objectives (Acciones sobre los Objetivos):**
   - El atacante, ya con control total, realiza su misión original.
   - *Ejemplo:* Robar bases de datos de clientes (Violación a la [[03 - CIA Triad|Confidencialidad]]) y luego cifrar todos los servidores pidiendo un rescate de Ransomware (Violación a la Disponibilidad).

---

## ❓ ¿Por qué importa?

La Kill Chain cambia la mentalidad defensiva. Permite a las organizaciones alinear sus [[08 - Controles de Seguridad|Controles de Seguridad]] en diferentes puntos de la línea de tiempo del ataque (usando [[09 - Defense in Depth|Defensa en Profundidad]]).

- ¿Podemos bloquear el ataque en la fase de **Delivery**? (Usando un filtro antispam avanzado).
- Si falla, ¿podemos detenerlo en **Exploitation**? (Usando un Antivirus o EDR).
- Si falla, ¿podemos detectarlo en **Command and Control**? (Monitoreando el tráfico de red de salida buscando IPs sospechosas).

---

## 🌎 Cyber Kill Chain vs. MITRE ATT&CK

Son modelos complementarios, no opuestos.
- La **Cyber Kill Chain** te da la "foto grande", la línea de tiempo de alto nivel de cómo progresa un ataque desde afuera hacia adentro.
- **MITRE ATT&CK** es el "microscopio". Entra en detalle granular sobre las tácticas y técnicas exactas que el atacante usa una vez que ya está dentro (desde la fase de Explotación en adelante).

En la práctica profesional, las empresas suelen usar la Kill Chain para reportar incidentes a la gerencia ("*Detuvimos el ataque en la fase de Comando y Control*") y usan MITRE ATT&CK para el trabajo técnico diario ("*El atacante usó la técnica T1059.001 - PowerShell*").

---

## ❌ Errores comunes

- **Pensar que la cadena siempre es rígida:** En ataques modernos, especialmente los de amenazas internas (Insider Threats), las fases de Reconocimiento, Armamentización y Entrega a menudo se saltan, ya que el atacante ya tiene acceso al sistema.
- **Falta de visibilidad en las fases tempranas:** Muchas empresas solo se dan cuenta del ataque en el paso 7 (cuando los servidores ya están cifrados). El objetivo de un buen equipo defensivo es detectar el ataque en la fase 3, 4 o 5.

---

## 📌 Must Know (Imprescindible)
- Qué es la Cyber Kill Chain y cuál es su premisa principal (romper un eslabón detiene el ataque).
- Las 7 fases en orden.

## 💡 Good to Know (Bueno saberlo)
- Unified Kill Chain (UKC) es un modelo más moderno desarrollado por Paul Pols que combina la simplicidad de la Cyber Kill Chain tradicional con el detalle táctico de MITRE ATT&CK.

---

## 📝 Para recordar
> Cuanto más a la izquierda (antes) en la cadena detectes un ataque, más barato será mitigarlo y menor será el daño para la organización.

---

## 🔄 Preguntas de repaso
1. Si un atacante registra un nombre de dominio falso que se parece al de tu empresa (ej. `paypa1.com`) para usarlo en un ataque futuro, ¿en qué fase de la Cyber Kill Chain se encuentra?
2. Un empleado descarga un archivo malicioso, el antivirus lo detecta y lo borra inmediatamente antes de que el archivo pueda hacer algo. ¿En qué fase se rompió la cadena?
3. ¿Por qué es tan crítico detectar la fase de Command and Control (C2)?

**➡️ Siguiente nota:** [[14 - Frameworks de Seguridad]]
