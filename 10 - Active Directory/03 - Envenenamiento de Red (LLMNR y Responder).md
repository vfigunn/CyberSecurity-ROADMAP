# 03 - Envenenamiento de Red (LLMNR y Responder)

## 🎯 Objetivos
- Conocer los protocolos predeterminados (y anticuados) que rompen la seguridad de Windows.
- Entender el concepto de Envenenamiento de Red local.
- Descubrir la herramienta `Responder`, la navaja suiza para el robo de Hashes.

---

## 🧠 Concepto: Los Protocolos Gritos de Windows

En la [[02 - Networking/02 - Capa de Aplicación (DNS, HTTP y Protocolos)|Nota de DNS]], aprendimos que cuando una computadora quiere ir a `www.servidor.com`, le pregunta al Servidor DNS: *"¿Quién tiene esta dirección?"*.

Pero Windows tiene una característica súper antigua y peligrosa (heredada para redes pequeñas sin servidores centralizados). 
Si un empleado (Juan) se equivoca al escribir en su barra de carpetas y pone: `\\servidor_impresoras_falso` (Escribe un nombre que no existe), el Servidor DNS oficial le dirá: *"Lo siento Juan, no conozco esa máquina"*.

¿Qué hace Windows? En lugar de rendirse, Windows activa protocolos de emergencia llamados **LLMNR** (Link-Local Multicast Name Resolution) y **NBT-NS**.
Windows literalmente **GRITA en Broadcast** a toda la red entera: 
> *"¡EL SERVIDOR DNS NO LO ENCUENTRA! ¿ALGUIEN EN LA RED SABE QUIÉN ES `servidor_impresoras_falso`?"*

---

## ☠️ El Ataque del Hombre en el Medio (Responder)

Aquí es donde entra el atacante o el Red Team en la red local.

El Hacker, enchufado a la red corporativa (o desde una máquina vulnerada), enciende una poderosísima herramienta en Python llamada **`Responder`**.

`Responder` es un programa diabólico (Un envenenador). Se queda callado y escucha todos los gritos de la red.

1. **La Víctima se equivoca:** Juan intenta entrar a `\\servidorr_archivos` (Lo escribió con dos R). El DNS falla. Windows lanza el grito desesperado en Broadcast: *"¿Alguien sabe la IP de `servidorr_archivos`?"*.
2. **El Engaño del Envenenador:** La herramienta `Responder` (del Hacker) escucha el grito. Rápidamente se levanta, alza la mano, engaña a la red y le miente a Juan: *"¡Sí, yo soy el `servidorr_archivos`, conectate a mi IP!"*.
3. **El Robo (Extracción de NTLMv2):** Juan, creyendo que habló con una máquina legítima, se conecta al Hacker. El Hacker (simulando ser un servidor de Windows) le dice: *"Okay, si querés entrar a mi carpeta, validá quién sos y dame tu Hash de Contraseña"*. Windows (que intenta facilitar las conexiones automáticamente), le **entrega obedientemente el Hash de Contraseña NetNTLMv2 de Juan al Hacker**.
4. **La Huida:** `Responder` agarra el Hash, lo guarda en un archivo de texto en la PC del atacante (`hashes.txt`), y luego le corta la conexión a Juan mostrándole un falso "Error de Red".

---

## 🔨 Cracking Offline de los Hashes (Hashcat)

`Responder` no nos entrega la contraseña en texto claro (No dice *"Clave123"*). Nos entrega un **Hash NTLMv2** criptográfico (una mezcla enorme de letras y números ilegibles).

Como aprendimos en el [[05 - Criptografía/03 - Hashes (Integridad)|Módulo de Criptografía]], los hashes NO se pueden revertir. Solo se pueden Crackear adivinando.
El Hacker se va a su casa. Mete ese Hash robado en una herramienta de Fuerza Bruta extrema usando Placas de Video (GPUs) llamada **`Hashcat`** o **`John The Ripper`**. 
- Le indica a `Hashcat` que use un diccionario gigante de millones de contraseñas mundiales populares (`rockyou.txt`).
- `Hashcat` prueba millones de combinaciones matemáticas por segundo.
- Si Juan tenía una clave débil (ej. *"Otoño2023!"*), la tarjeta gráfica del hacker lo adivinará en 5 minutos y el hacker habrá conseguido la clave en texto plano del usuario. 
*(Si la clave es larguísima y ultra compleja, el crackeo nunca terminará y el hacker no obtendrá la clave en texto).*

*(Nota de Mitigación Defensiva: El Blue Team soluciona este gigantesco agujero de seguridad desactivando por completo los protocolos LLMNR y NBT-NS mediante GPO en toda la empresa).*

---

## 📌 Must Know (Imprescindible)
- **El comportamiento por defecto de Windows:** Ante la falla del Servidor DNS (Ej. nombre mal escrito), los protocolos **LLMNR** y **NBT-NS** intentan solucionar la vida lanzando peticiones públicas en broadcast a todas las máquinas de la subred local.
- **La Herramienta `Responder`:** El estándar universal para capturar estos gritos y envenenarlos, haciéndose pasar por el servidor solicitado obligando a la víctima a entregarle las credenciales encriptadas (Hash NetNTLMv2).
- Entender que lo que robás es un Hash. No sirve de nada hasta que no apliques **Cracking Offline (Hashcat / Diccionarios / Fuerza Bruta)** en hardware potente para descubrir su forma en texto plano.

---

## 🔄 Preguntas de repaso
1. Un empleado del departamento legal está intentando acceder a una carpeta compartida escribiendo en la barra de direcciones `\\Finanzass` en vez de `\\Finanzas`. Explique detalladamente por qué este simple error tipográfico humano, dentro de una red Windows estándar, desencadena automáticamente la oportunidad perfecta de robo masivo para un atacante posicionado en la red interna.
2. Tras dejar la herramienta `Responder` ejecutándose durante 2 horas en una VLAN corporativa, el Analista de Red Team recolecta exitosamente el archivo `.txt` que contiene el Hash `NetNTLMv2` de un Supervisor del área IT. ¿Por qué el atacante no puede simplemente agarrar ese Hash y pegarlo directamente en la pantalla de inicio de sesión web del portal Microsoft Office365 del Supervisor? (¿Qué necesita hacer antes con él offline?).
3. Si la política de la empresa (GPO) exige que todos los administradores usen contraseñas "Hiper-Complejas de 40 caracteres, con mayúsculas, minúsculas, símbolos, números, sin ningún tipo de patrón humano, generadas en bóvedas criptográficas". Y el atacante mediante `Responder` logra envenenar la red y robarse el Hash gigantesco NetNTLMv2 de un administrador; ¿Por qué este ataque fracasará estrepitosamente en la etapa de Post-Recolección (Cracking offline en GPU / Hashcat)?

**➡️ Siguiente nota:** [[04 - Ataques NTLM (Pass-The-Hash y Relay)]]
