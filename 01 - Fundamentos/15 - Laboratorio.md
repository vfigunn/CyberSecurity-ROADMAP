# Lab 01 - Modelado de Amenazas Básico (Threat Modeling)

## 🎯 Objetivo
El objetivo de este laboratorio introductorio no es usar terminales ni comandos complejos, sino aprender a **pensar como un profesional de seguridad**. Vas a analizar un escenario realista y aplicar los conceptos teóricos aprendidos en el Módulo 01 (Vulnerabilidades, Amenazas, Riesgo, Controles, Defensa en Profundidad).

---

## 📋 Prerrequisitos
- Haber leído y comprendido todas las notas del [[01 - Fundamentos/00 - Overview|Módulo 01]].
- Papel y lápiz, o un documento de texto para anotar tus respuestas.

---

## 🏢 Escenario: "MedPlus Clinic"

Trabajas como analista de seguridad junior para "MedPlus Clinic", una clínica médica privada pequeña (50 empleados).

La infraestructura tecnológica de la clínica es la siguiente:
1. **Recepción:** Hay dos computadoras. Las recepcionistas las usan para agendar turnos y responder emails. Las computadoras están encendidas todo el día a la vista del público y tienen puertos USB accesibles. Tienen el mismo usuario y contraseña genérico (`recepcion/1234`).
2. **Servidor Local:** Hay un servidor físico en un armario (que a menudo se deja sin llave) en el pasillo. Este servidor almacena las historias clínicas electrónicas de 10,000 pacientes.
3. **Red (Network):** Todos (empleados y los pacientes en la sala de espera) se conectan a la misma red Wi-Fi utilizando la contraseña `medplus2024`.
4. **Respaldo (Backups):** El administrador de IT hace una copia de seguridad de las historias clínicas una vez al mes en un disco duro externo que guarda en un cajón de su escritorio.

---

## 🛠️ Procedimiento (Tu Trabajo)

Lee el escenario detenidamente y responde a las siguientes consignas documentando tus hallazgos.

### Paso 1: Identificación de la Superficie de Ataque y Vulnerabilidades
Identificá al menos **3 vulnerabilidades específicas** (debilidades) en el entorno de MedPlus Clinic. Mencioná si son vulnerabilidades digitales, físicas o humanas (según la nota [[10 - Attack Surface]]).

### Paso 2: Identificación de Amenazas (Threats)
Para cada vulnerabilidad que identificaste en el Paso 1, describí un **Actor de Amenaza** (Threat Actor) probable que podría intentar explotarla, y cuál sería su motivación (dinero, venganza, accidente, etc.). (Ver nota [[11 - Threat Actors]]).

### Paso 3: Análisis de Impacto (Tríada CIA)
Elegí el escenario de ataque más peligroso de los que pensaste en el Paso 2 e imaginá que tiene éxito (el ataque ocurre). Describí cuál sería el impacto específico en la organización utilizando los conceptos de la **Tríada CIA** (Confidencialidad, Integridad, Disponibilidad). (Ver nota [[03 - CIA Triad]]).

### Paso 4: Propuesta de Controles (Defensa en Profundidad)
Sos el encargado de proponer soluciones. Para el ataque que describiste en el Paso 3, proponé **3 Controles de Seguridad diferentes**, asegurándote de usar el enfoque de **Defensa en Profundidad** (Ver notas [[08 - Controles de Seguridad]] y [[09 - Defense in Depth]]). 
- Proponé 1 control de categoría *Física*.
- Proponé 1 control de categoría *Administrativa* (Política/Procedimiento).
- Proponé 1 control de categoría *Técnica*.
- Indicá para cada uno si su función es *Preventiva*, *Detectiva* o *Correctiva*.

---

## 📝 Resultado Esperado (Autoevaluación)

Una vez que hayas terminado, compará tus respuestas con este análisis sugerido (¡No lo leas antes de intentar hacerlo vos mismo!).

> [!example]- Ver posibles respuestas
> **Paso 1: Vulnerabilidades**
> 1. Computadoras de recepción con usuario/contraseña débil y genérica (Vulnerabilidad Digital/Configuración).
> 2. Servidor crítico en un armario sin llave (Vulnerabilidad Física).
> 3. Wi-Fi compartido entre pacientes y red interna (Vulnerabilidad Digital/Arquitectura).
> 4. Backups poco frecuentes guardados en el mismo lugar físico (Vulnerabilidad de Proceso).
> 
> **Paso 2: Amenazas**
> - Para la vulnerabilidad 2 (Servidor físico): Un *Actor de amenaza accidental o intencional interno (Insider Threat)*, como personal de limpieza desconectando el servidor por error, o un empleado enojado robándolo.
> - Para la vulnerabilidad 3 (Wi-Fi): Un *Cibercriminal o Script Kiddy* (externo) en la sala de espera que usa el Wi-Fi abierto para escanear y atacar el servidor local desde su teléfono. Motivación: Dinero (Ransomware) o curiosidad.
> 
> **Paso 3: Impacto (Tríada CIA)**
> Asumiendo que el atacante (Script Kiddy en sala de espera) logra entrar al servidor vía Wi-Fi e instala un Ransomware y roba los datos:
> - **Confidencialidad:** Violada. Se filtran historias clínicas de 10,000 pacientes. (Impacto legal masivo, HIPAA/GDPR).
> - **Integridad:** Violada. El atacante podría alterar diagnósticos médicos.
> - **Disponibilidad:** Violada. Las historias clínicas están encriptadas. Los médicos no pueden ver los datos de sus pacientes (impacto operativo crítico). El disco duro de respaldo (backup) de hace 1 mes significa perder todos los datos del último mes.
> 
> **Paso 4: Controles de Seguridad (Ejemplos)**
> 1. **Control Físico (Preventivo):** Poner una cerradura de alta seguridad en el armario del servidor y solo darle llave al administrador.
> 2. **Control Administrativo (Preventivo):** Crear una política que prohíba a los recepcionistas compartir contraseñas, y requerir contraseñas individuales y fuertes.
> 3. **Control Técnico (Preventivo):** Segmentar la red (VLANs) para que el Wi-Fi de los invitados/pacientes esté totalmente separado de la red interna de la clínica, impidiendo que se comuniquen con el servidor local.
> 4. **Control Técnico (Correctivo):** Automatizar copias de seguridad diarias encriptadas en la nube, para poder restaurar los datos rápidamente (Disponibilidad) en caso de un ataque.

---

## 🧹 Cleanup
En este laboratorio teórico no hay recursos que limpiar. Guardá tus notas, te servirán de referencia para el Módulo de GRC y Risk Management más adelante.

**➡️ Siguiente nota:** [[16 - Ejercicios]]
