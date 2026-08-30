# 03 - La Tríada CIA (CIA Triad)

## 🎯 Objetivos
- Comprender el modelo fundamental de la seguridad de la información: La Tríada CIA.
- Diferenciar claramente entre Confidencialidad, Integridad y Disponibilidad.
- Entender cómo aplicar controles para proteger cada uno de estos pilares.

---

## 🧠 Concepto

La **Tríada CIA** (por sus siglas en inglés: *Confidentiality, Integrity, Availability*) es el modelo base sobre el cual se construyen todas las políticas y controles de seguridad. 

Cualquier ataque informático busca comprometer al menos uno de estos tres pilares. Cualquier defensa busca protegerlos.

### 1. Confidencialidad (Confidentiality)
Consiste en asegurar que la información solo sea accesible para aquellas personas o sistemas que están autorizados a verla. Es el concepto de "secreto" o "privacidad".
- **Amenaza principal:** Robo de datos, espionaje, filtraciones (Data Breach).
- **Cómo se protege:** [[06 - Cryptography/01 - Conceptos Básicos|Cifrado (Encryption)]], controles de acceso ([[08 - Identity & Access/05 - Access Control Models|Access Control]]), contraseñas, autenticación multifactor (MFA).

### 2. Integridad (Integrity)
Consiste en asegurar que la información es exacta, completa y no ha sido modificada de forma no autorizada, ya sea accidental o maliciosamente.
- **Amenaza principal:** Alteración de registros financieros, modificación de código fuente, corrupción de bases de datos.
- **Cómo se protege:** [[06 - Cryptography/07 - Hashing|Hashing]], firmas digitales, control de versiones, auditorías, permisos estrictos de escritura.

### 3. Disponibilidad (Availability)
Consiste en asegurar que la información y los sistemas estén accesibles para los usuarios autorizados en el momento en que los necesiten.
- **Amenaza principal:** Ataques de denegación de servicio (DDoS), fallos de hardware, cortes de energía, Ransomware (que cifra los datos haciéndolos inaccesibles).
- **Cómo se protege:** Redundancia (sistemas de respaldo), balanceadores de carga, planes de recuperación ante desastres (DRP), copias de seguridad (Backups).

---

## ❓ ¿Por qué importa?

La Tríada CIA es la métrica con la que evaluamos el riesgo. Cuando diseñamos un sistema, debemos preguntarnos:
- ¿Qué pasa si estos datos se filtran? (Impacto en la Confidencialidad).
- ¿Qué pasa si alguien altera estos datos sin que nos demos cuenta? (Impacto en la Integridad).
- ¿Qué pasa si el sistema se cae por 24 horas? (Impacto en la Disponibilidad).

Dependiendo del tipo de organización, un pilar puede ser más importante que otro.

---

## 🌎 Aplicación en el mundo real

- **Hospital:** La **Disponibilidad** y la **Integridad** son críticas. Si el sistema de historias clínicas no está disponible (o si alguien altera el tipo de sangre de un paciente), alguien puede morir. La Confidencialidad es importante por leyes de privacidad, pero la vida es prioridad.
- **Banco / App Financiera:** La **Integridad** es rey. Si mi saldo de $100 se convierte mágicamente en $10,000, el banco quiebra. La Confidencialidad también es vital para proteger los números de tarjeta.
- **Gobierno / Militar:** La **Confidencialidad** de los secretos de estado es la prioridad absoluta. A menudo prefieren destruir un sistema (perder Disponibilidad) antes de que el enemigo lea los datos.

---

## ❌ Errores comunes

- **"La seguridad es solo ocultar cosas":** Falso. Muchos principiantes asocian ciberseguridad solo con la Confidencialidad. Proteger la Integridad y la Disponibilidad es igual o más importante dependiendo del negocio.
- **"Podemos tener los tres al 100%":** Falso. En la vida real, hay un equilibrio (trade-off). Si hacés un sistema increíblemente confidencial (cifrado pesado, 5 passwords distintos, aislado de internet), estás perjudicando la Disponibilidad (es difícil y lento acceder a él).

---

## 📌 Must Know (Imprescindible)
- Qué significa C, I y A, y poder dar un ejemplo de cómo proteger cada una.
- Todo incidente de seguridad es una ruptura de uno o más de estos pilares.

## 💡 Good to Know (Bueno saberlo)
- A veces se añaden otros conceptos al modelo, como el **No Repudio** (Non-repudiation), que asegura que una persona no pueda negar haber realizado una acción (se logra usando firmas digitales, garantizando la Integridad y autenticidad).

---

## 📝 Para recordar
> Un ataque de Ransomware clásico (que cifra los archivos y pide rescate) es principalmente un ataque contra la **Disponibilidad** (no podés acceder a tus datos). Si además amenazan con publicarlos en internet, se convierte en un ataque contra la **Confidencialidad**.

---

## 🔄 Preguntas de repaso
1. Si un atacante entra a la red de una universidad y cambia sus notas de un 2 a un 10, ¿qué pilar de la tríada CIA fue violado?
2. Un ataque DDoS satura los servidores de una tienda online en Black Friday. ¿Qué pilar fue afectado?
3. Da un ejemplo de un control técnico que ayude a proteger la Confidencialidad de un documento enviado por correo electrónico.

**➡️ Siguiente nota:** [[04 - Amenazas]]
