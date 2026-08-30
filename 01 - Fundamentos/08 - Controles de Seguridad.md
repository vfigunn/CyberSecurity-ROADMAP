# 08 - Controles de Seguridad (Security Controls)

## 🎯 Objetivos
- Entender qué es un control de seguridad.
- Clasificar los controles según su *Categoría* (Administrativos, Técnicos, Físicos).
- Clasificar los controles según su *Función* (Preventivos, Detectivos, Correctivos, etc.).

---

## 🧠 Concepto

Para mitigar los [[07 - Riesgo|Riesgos]] (estrategia de Mitigación), implementamos **Controles de Seguridad**.
Un control de seguridad es cualquier medida, mecanismo, herramienta o procedimiento que se pone en marcha para evitar, detectar, contrarrestar o minimizar los riesgos de seguridad.

Los controles de seguridad no son solo software. Se clasifican utilizando un modelo matricial de **Categoría** y **Función**.

---

## 📂 Clasificación por Categoría (El "Qué" es el control)

Se dividen en tres grandes grupos que mapean con los pilares de Procesos, Tecnología y Personas (vistos en la [[01 - Introducción a la Ciberseguridad|Nota 01]]).

1. **Controles Administrativos (o Directivos / Gerenciales):**
   - Son reglas, políticas, procedimientos y normativas que dictan el comportamiento humano y organizacional.
   - *Ejemplos:* Política de contraseñas, acuerdos de confidencialidad (NDAs), entrenamiento de concientización sobre phishing a empleados, plan de respuesta a incidentes.

2. **Controles Técnicos (o Lógicos):**
   - Soluciones de hardware o software implementadas para proteger la información y los sistemas.
   - *Ejemplos:* [[07 - Network Security/02 - Firewalls|Firewalls]], cifrado de discos (Encryption), software antivirus/EDR, sistemas de autenticación (Active Directory), permisos de archivos (ACLs).

3. **Controles Físicos:**
   - Medidas tangibles para proteger las instalaciones, hardware y personas en el mundo real.
   - *Ejemplos:* Guardias de seguridad, cámaras CCTV, cerraduras biométricas en el centro de datos, molinetes, sistemas de extinción de incendios.

---

## ⚙️ Clasificación por Función (El "Para qué" sirve)

Se dividen según el momento de la línea de tiempo de un ataque en el que actúan:

1. **Preventivos (Preventive):**
   - Actúan *antes* del incidente. Su objetivo es detener o bloquear el ataque antes de que tenga éxito.
   - *Ejemplos:* Un guardia en la puerta (Físico), un Firewall bloqueando tráfico (Técnico), una política que prohíbe USBs (Administrativo).

2. **Detectivos (Detective):**
   - Actúan *durante* o inmediatamente después del incidente. No detienen el ataque, pero te avisan que está ocurriendo para que puedas reaccionar.
   - *Ejemplos:* Cámaras de seguridad (Físico), un Sistema de Detección de Intrusos / IDS (Técnico), revisión de logs de auditoría (Administrativo).

3. **Correctivos (Corrective):**
   - Actúan *después* del incidente. Buscan restaurar el sistema a la normalidad o limitar el daño una vez que el ataque ha ocurrido.
   - *Ejemplos:* Restaurar datos desde un Backup (Técnico), un extintor de incendios (Físico), parchar una vulnerabilidad descubierta tras un hackeo (Técnico/Administrativo).

4. **Disuasorios (Deterrent):**
   - Buscan desalentar o asustar al atacante potencial antes de que intente algo.
   - *Ejemplos:* Un cartel de "Propiedad vigilada por CCTV" o un banner legal de "Todo acceso no autorizado será penalizado" al iniciar sesión por SSH.

5. **Compensatorios (Compensating):**
   - Controles alternativos que se usan cuando el control principal es demasiado difícil o costoso de implementar, proporcionando un nivel de protección similar.
   - *Ejemplo:* Si no podés instalar un parche en una máquina de un hospital porque el fabricante no lo permite (riesgo), ponés esa máquina en una red totalmente aislada (control compensatorio).

---

## ❓ ¿Por qué importa?

Entender las categorías y funciones es crucial porque la seguridad efectiva requiere una mezcla de todas ellas. 
Si solo tenés controles técnicos preventivos (ej. un súper firewall), pero no tenés controles detectivos (ej. monitoreo de alertas), un atacante que logre pasar el firewall podrá operar en tu red durante meses sin que te des cuenta.

---

## 🌎 Aplicación en el mundo real (Matriz)

Podés combinar ambas clasificaciones. Ejemplos de combinaciones:

- **Técnico - Preventivo:** Firewall.
- **Técnico - Detectivo:** Alerta de inicio de sesión sospechoso.
- **Técnico - Correctivo:** Sistema automático que restaura un servidor si se cae.
- **Administrativo - Preventivo:** Política de uso aceptable (AUP).
- **Físico - Preventivo:** Molinete con lector de tarjetas magnéticas.
- **Físico - Detectivo:** Sensor de humo.

---

## ❌ Errores comunes

- **Depender excesivamente de controles técnicos:** Comprar la mejor tecnología (Técnico/Preventivo) pero no capacitar a los empleados para usarla o no tener procesos para revisar sus alertas (Administrativo/Detectivo).

---

## 📌 Must Know (Imprescindible)
- Las tres categorías (Administrativo, Técnico, Físico).
- Las tres funciones principales (Preventivo, Detectivo, Correctivo).
- Poder dar ejemplos cruzando categoría y función.

## 💡 Good to Know (Bueno saberlo)
- En certificaciones como Security+ y CISSP, las preguntas a menudo describen un escenario y te piden identificar qué tipo de control se está aplicando (ej. "¿Una cámara de seguridad qué tipo de control es? Respuesta: Físico / Detectivo").

---

## 📝 Para recordar
> Los controles preventivos son ideales, pero los detectivos y correctivos son absolutamente necesarios porque los controles preventivos **eventualmente fallarán**.

---

## 🔄 Preguntas de repaso
1. Clasificá el siguiente control por Categoría y Función: "Un servidor guarda un registro de auditoría (log) de todos los usuarios que intentaron iniciar sesión y fallaron".
2. Clasificá el siguiente control por Categoría y Función: "La empresa obliga por contrato a todos los empleados a tomar un curso de seguridad anual antes de darles acceso a la red".
3. ¿Por qué una organización implementaría un Control Compensatorio en lugar del control de seguridad recomendado originalmente?

**➡️ Siguiente nota:** [[09 - Defense in Depth]]
