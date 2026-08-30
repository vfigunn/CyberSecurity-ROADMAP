# 12 - MITRE ATT&CK (Introducción)

## 🎯 Objetivos
- Entender qué es el framework MITRE ATT&CK y para qué se utiliza.
- Comprender la diferencia entre Tácticas, Técnicas y Procedimientos (TTPs).
- Conocer cómo este framework revolucionó el enfoque de la defensa.

---

## 🧠 Concepto

El framework **MITRE ATT&CK** (Adversarial Tactics, Techniques, and Common Knowledge) es una base de conocimientos curada y de acceso público que documenta el comportamiento de los atacantes (los [[11 - Threat Actors|Threat Actors]]) basada en observaciones del mundo real.

Esencialmente, es la **enciclopedia global de cómo atacan los hackers**.

Antes de MITRE ATT&CK, los defensores se enfocaban en "Indicadores de Compromiso" ([[14 - SOC & SIEM/13 - IOC|IOCs]]), como direcciones IP maliciosas o firmas de virus. El problema es que un atacante puede cambiar su IP en un segundo. 
MITRE ATT&CK propone enfocarse en **el comportamiento (TTPs)**, que es mucho más difícil de cambiar para el atacante.

---

## 🛠️ TTPs: Tácticas, Técnicas y Procedimientos

El framework organiza la información usando TTPs. Entender esta estructura es vital.

### 1. Táctica (Tactic) - *El "Por qué" (El Objetivo)*
Representa la meta táctica que el atacante intenta lograr en un momento específico del ataque. 
Existen 14 tácticas en el modelo Enterprise actual. Ejemplos:
- **Initial Access (Acceso Inicial):** El objetivo de entrar a la red.
- **Privilege Escalation (Escalada de Privilegios):** El objetivo de conseguir permisos de Administrador.
- **Exfiltration (Exfiltración):** El objetivo de sacar los datos robados de la red.

### 2. Técnica (Technique) - *El "Cómo"*
Representa el método específico que el atacante usa para lograr la Táctica. Cada Táctica tiene decenas de Técnicas posibles.
- *Ejemplo (Para la táctica de Acceso Inicial):* La técnica podría ser "Phishing", o podría ser "Explotar aplicación pública", o "Usar credenciales válidas robadas".

### 3. Procedimiento (Procedure) - *La "Ejecución Específica"*
Representa cómo un grupo de ataque específico (por ejemplo, APT29) ejecuta la Técnica paso a paso, incluyendo las herramientas exactas o comandos que utilizan.
- *Ejemplo (Para la técnica de Phishing):* El procedimiento exacto de APT29 fue enviar correos electrónicos haciéndose pasar por el Ministerio de Salud con un archivo adjunto llamado `reporte_covid.zip` que contenía un ejecutable malicioso.

---

## ❓ ¿Por qué importa?

Imaginá que sos un analista defensivo ([[25 - Blue Team|Blue Team]]). Si te enterás de que el grupo cibercriminal APT29 está atacando a empresas de tu sector, podés ir al framework de MITRE ATT&CK, buscar "APT29" y ver exactamente la lista de Técnicas que suelen utilizar. 

Con esa lista, podés revisar tus [[08 - Controles de Seguridad|Controles de Seguridad]] y preguntarte: *"¿Tenemos forma de detectar y bloquear ESTAS 10 técnicas específicas?"*. Esto te permite pasar de una defensa reactiva ("esperar a que suene la alarma") a una defensa proactiva (Threat Hunting).

---

## 🌎 La Pirámide del Dolor (Pyramid of Pain)

El investigador David Bianco creó la "Pirámide del Dolor" para explicar por qué enfocarse en TTPs es tan importante. 
- En la base de la pirámide están los valores Hash de virus, Direcciones IP y nombres de dominio. Cambiar esto le cuesta al atacante "cero esfuerzo".
- En la cima de la pirámide están los **TTPs (Tácticas, Técnicas y Procedimientos)**. Si descubrís la *técnica* favorita de un atacante y construís una defensa contra ella, el atacante tiene que ir a estudiar, reprogramar sus herramientas y cambiar toda su forma de trabajar para poder volver a atacarte. Bloquear un TTP le causa "dolor" al atacante. MITRE ATT&CK se enfoca en esta cima.

---

## ❌ Errores comunes

- **Tratar a MITRE ATT&CK como un checklist (lista de tareas):** Algunas empresas ven la matriz gigante y piensan que deben bloquear *todas* las técnicas que existen para estar seguras. Esto es imposible (hay cientos de técnicas y cambian a diario). El objetivo es enfocarse primero en las técnicas más usadas por los atacantes que representan un [[07 - Riesgo|Riesgo]] real para tu industria.

---

## 📌 Must Know (Imprescindible)
- Qué significan las siglas TTP (Táctica, Técnica, Procedimiento).
- La diferencia fundamental entre una Táctica (el objetivo) y una Técnica (el método).

## 💡 Good to Know (Bueno saberlo)
- Estudiaremos la matriz Enterprise de MITRE ATT&CK en mayor profundidad, incluyendo cómo mapear reglas de detección, en el módulo de [[18 - Threat Intelligence/07 - MITRE ATT&CK en Profundidad|Threat Intelligence (Módulo 18)]].

---

## 📝 Para recordar
> Un atacante puede cambiar de herramienta o de IP en un segundo, pero cambiar su comportamiento (sus TTPs) le requiere reaprender su oficio. Por eso la defensa moderna se basa en comportamientos y no en firmas estáticas.

---

## 🔄 Preguntas de repaso
1. Identificá si la siguiente frase es una Táctica o una Técnica: "El atacante robó la base de datos de contraseñas de los usuarios".
2. Identificá si la siguiente frase es una Táctica o una Técnica: "El atacante usó el software 'Mimikatz' para extraer contraseñas de la memoria de Windows".
3. Según la Pirámide del Dolor, ¿por qué es mejor detectar un TTP que detectar una dirección IP maliciosa?

**➡️ Siguiente nota:** [[13 - Cyber Kill Chain]]
