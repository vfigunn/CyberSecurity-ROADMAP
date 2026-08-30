# 03 - Tipos de Payloads (Staged vs Stageless)

## 🎯 Objetivos
- Entender el problema del "Tamaño y Espacio" a la hora de inyectar código en la Memoria RAM de una víctima.
- Diferenciar entre los Payloads en Fases (Staged) y de una sola pieza (Stageless).
- Conocer la nomenclatura oficial en los nombres de Metasploit (El símbolo `/` vs `_`).

---

## 🧠 Concepto: El límite de la valija

Imaginá que encontraste una vulnerabilidad matemática en un servidor para inyectarle código (Buffer Overflow).
Para explotarla, le enviás tu Payload (que es el virus, la cabeza nuclear).
Pero resulta que el programa de la víctima solo te deja inyectar 100 caracteres de texto en su Memoria RAM. ¡Y el Payload completo que querés enviarle (como el poderoso Meterpreter) pesa miles de caracteres! No entra en la valija.

Para solucionar el problema del "Espacio limitado", los desarrolladores de Metasploit dividieron los Payloads en dos categorías estratégicas.

---

## 📦 1. Payload de una pieza (Stageless o Inline)

Es el concepto tradicional. Todo el virus, la lógica de red y las funciones vienen empaquetadas en **un solo y pesado bloque de código**.
- **Ventaja:** Es ultra estable. Si el Payload llega a la máquina, se ejecuta íntegro sin depender de nada más.
- **Desventaja:** Es "Gordo". Si la vulnerabilidad solo te da un pedacito chico de Memoria RAM para inyectar (ej. 300 bytes), este Payload fallará por no entrar físicamente en el espacio asignado. Además, al ser tan grande, es fácilmente detectable por Antivirus en la red.

---

## 🪜 2. Payload en Fases (Staged)

Es la genialidad táctica de Metasploit para vulnerabilidades de espacio reducido. Se envía el ataque dividido en "Fases" o "Etapas" (Stages).

**Fase 1: El Stager (El paracaidista):**
Se crea un Payload **diminuto**, de poquísimos bytes. 
Este diminuto Stager no tiene la lógica para abrir una Shell completa ni robar contraseñas. 
Su único y minúsculo objetivo en la vida es: *Ejecutarse en la víctima, agarrar el teléfono, conectarse silenciosamente por la red de regreso hacia el servidor del atacante y decir: "Listo, entré. Mandame el verdadero virus"*. 
Como es tan chico, entra en cualquier vulnerabilidad de desbordamiento sin alertar defensas.

**Fase 2: El Stage (La carga pesada):**
Una vez que el Stager conectó a la víctima con el atacante, el servidor de Metasploit (a través de ese túnel invisible) descarga dinámicamente **el resto gigante del Payload a la Memoria RAM de la víctima**, evadiendo el Firewall web porque la conexión de descarga la inició la propia víctima desde adentro.

---

## 🔍 ¿Cómo identificarlos en Metasploit? (El secreto del símbolo)

Cuando busques Payloads en la consola de Metasploit, verás cientos de opciones. Saber cuál es Staged y cuál es Stageless es un requisito indispensable para configurar bien el servidor atacante.
El secreto está en la convención de nombres: **La barra (`/`) versus el guion bajo (`_`)**.

### Nomenclatura Stageless (Guion bajo `_`):
Si las palabras están unidas por un guion bajo, el Payload viaja COMPLETO en un solo bloque.
> `windows/meterpreter_reverse_tcp`

### Nomenclatura Staged (Barra `/`):
Si las palabras están separadas por una barra, el Payload es en fases. La parte de la izquierda dice qué herramienta querés, y la parte derecha dice qué protocolo usará el paracaidista (Stager) para conectarse y descargarla.
> `windows/meterpreter/reverse_tcp`

*(Memorizá esto: La barra `/` separa las dos fases del Staged Payload).*

---

## 📌 Must Know (Imprescindible)
- **Stageless (Inline):** Payload completo enviado todo junto. Muy estable pero de gran tamaño. Se identifica con guion bajo (`shell_reverse_tcp`).
- **Staged:** Payload dividido en dos. Primero va el "Stager" (pequeño cuentagotas) y éste descarga el resto del programa. Ideal para exploits de espacio reducido y evasión. Se identifica con barra inclinada (`shell/reverse_tcp`).

---

## 🔄 Preguntas de repaso
1. Un atacante está explotando un desbordamiento de memoria (Buffer Overflow) súper estricto en un servidor FTP antiguo, el cual solo le permite inyectar una cadena máxima de 250 bytes de memoria antes de crashear el servidor. Si el atacante desea ejecutar la pesada consola `meterpreter` en la víctima, ¿qué arquitectura de Payload elegirá forzosamente para evadir la restricción de espacio inicial: Staged o Stageless?
2. Al revisar tu consola de opciones en Metasploit (`msfconsole`), el analista elige el Payload: `linux/x64/shell_bind_tcp`. Según la nomenclatura estándar del Framework, ¿este Payload enviará su lógica de red y ejecución en una sola pieza inyectada, o enviará un pequeño descargador en Fase 1 para traer el resto de la shell por la red?
3. En un Payload Staged (Ej: `windows/shell/reverse_tcp`), explicá brevemente cuál es el propósito táctico y la tarea exclusiva que debe cumplir la Fase 1 (El "Stager") apenas logra tocar la Memoria RAM de la máquina víctima.

**➡️ Siguiente nota:** [[04 - El Ecosistema MSFconsole (Búsqueda y Configuración)]]
