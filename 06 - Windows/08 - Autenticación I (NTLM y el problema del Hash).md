# 08 - Autenticación I (NTLM y el problema del Hash)

## 🎯 Objetivos
- Entender cómo las computadoras verifican contraseñas a través de la red (Autenticación).
- Conocer NTLM, el protocolo heredado de Windows y sus vulnerabilidades.
- Comprender el famoso ataque letal "Pass-The-Hash".

---

## 🧠 Concepto: La Contraseña Peligrosa

En el [[05 - Criptografía/00 - Overview|Módulo 05 (Criptografía)]] aprendiste que las contraseñas nunca se guardan en texto claro en la base de datos (se guardan como Hashes).
Windows hace lo mismo. Guarda tu contraseña como un **Hash NTLM (NT LAN Manager)** en el Controlador de Dominio.

El problema gigantesco surge en el momento de la **Autenticación en Red** (Cuando un cliente, PC-Juan, intenta acceder a la Carpeta Secreta alojada en Servidor-Finanzas).

Para que Servidor-Finanzas deje entrar a Juan, necesita asegurarse de que Juan es quien dice ser.
- *Opción Pésima:* Juan manda su contraseña en texto claro por el cable de red. (Cualquier hacker con un Sniffer la roba al vuelo).
- *Opción NTLM:* Microsoft inventó un protocolo de Autenticación de **Desafío/Respuesta** para evitar enviar la contraseña real.

---

## 🔑 El Protocolo NTLMv2 (Desafío/Respuesta)

Veamos cómo Juan (Cliente) se autentica contra el Servidor de Finanzas sin enviar la contraseña en claro:

1. **El Inicio:** Juan tipea su contraseña ("QWERTY") en su PC. Su PC la convierte matemáticamente en un Hash NTLM y la guarda temporalmente en la memoria RAM (el proceso `lsass.exe`, ¿te acordás?).
2. **La Petición:** Juan le dice al Servidor-Finanzas: *"Ey, quiero entrar. Soy Juan"*.
3. **El Desafío (Challenge):** El Servidor no confía. Inventa un número aleatorio larguísimo (ej. `39A4...`) y se lo envía a Juan. Le dice: *"Demostrame que sos Juan. Resolvéme este desafío matemático"*.
4. **La Respuesta (Response):** La PC de Juan toma el Desafío (el número), toma el Hash de Juan que estaba en la RAM, y mezcla ambos utilizando criptografía. Le envía la mezcla resultante (La Respuesta) de vuelta al Servidor.
5. **La Validación:** El Servidor recibe la mezcla. Se da vuelta, habla con el Controlador de Dominio y verifica las matemáticas. Si todo cuadra, el Servidor de Finanzas deja entrar a Juan a la carpeta.

*(El protocolo NTLMv2, a simple vista, parece seguro porque la contraseña real NUNCA viajó por la red, solo viajaron desafíos y mezclas).*

---

## 💥 El Problema del Hash (Pass-The-Hash)

NTLM tiene un defecto arquitectónico catastrófico que Microsoft nunca pudo (o quiso) solucionar del todo por motivos de retro-compatibilidad.

En el paso 1, dijimos que la PC de Juan almacena temporalmente el **Hash NTLM** en la memoria RAM (`lsass.exe`) para poder responder a los desafíos.
Resulta que, en el protocolo NTLM de Windows, **El Hash es funcionalmente idéntico a la contraseña en texto claro**.

**El Ataque (Pass-The-Hash / PtH):**
1. Un Atacante del Red Team logra acceso de Administrador en la computadora local de Juan (PC-Juan) utilizando un malware o explotando una vulnerabilidad.
2. El atacante corre la famosa herramienta *Mimikatz* apuntando al proceso de memoria RAM (`lsass.exe`).
3. El atacante **roba el Hash NTLM** de Juan (`31d6...`). ¡Atención! No sabe cuál es la contraseña ("QWERTY"), solo tiene el Hash.
4. En un sistema normal, el atacante tendría que intentar romper el Hash haciendo fuerza bruta durante semanas. ¡Pero en Windows no hace falta!
5. El atacante usa una herramienta de Hacking que le dice a Windows: *"Engañemos al protocolo NTLM. Cuando el Servidor-Finanzas te envíe un Desafío, no uses mi contraseña, **PASÁ EL HASH** que acabo de robar y usalo para mezclar la respuesta"*.
6. Windows genera la mezcla con el Hash robado. El Servidor-Finanzas la valida y **le otorga acceso total al Hacker**, quien ni siquiera sabe (ni le importa) cuál era la contraseña original de Juan.

---

## ❓ NTLM Hoy en Día

La vulnerabilidad *Pass-The-Hash* causó pánico mundial en las empresas hace una década. Y sigue siendo el ataque de Movimiento Lateral más utilizado hoy.

¿Por qué Microsoft no lo arregló? No es un "Bug", es cómo está diseñado NTLM desde sus cimientos. 
Para solucionar esto, Microsoft inventó en el año 2000 un protocolo de autenticación infinitamente superior y complejo: **Kerberos** (Lo veremos en la siguiente nota).
Sin embargo, **NTLM sigue existiendo y funcionando en todas las redes corporativas hoy en día** por compatibilidad con impresoras y sistemas viejos, siendo un vector constante de abuso.

---

## 📌 Must Know (Imprescindible)
- Qué es el proceso **Desafío/Respuesta** de NTLM (Se inventó para no transmitir la contraseña en claro por la red).
- Por qué, en el mundo Windows, un Hash NTLM es **exactamente igual de valioso** que una contraseña en texto claro (Porque el sistema te permite "Pasar el Hash" directamente para autenticarte).
- Saber que las contraseñas se guardan y procesan en el servicio local `lsass.exe` de la RAM.

---

## 🔄 Preguntas de repaso
1. Si interceptás (Sniffeás) con Wireshark el tráfico de red de un protocolo NTLMv2 mientras un usuario legítimo se está autenticando en un servidor de archivos. ¿Encontrarás la contraseña en texto claro volando por los cables? Justificá utilizando la mecánica de autenticación del protocolo.
2. Un atacante de Blue Team implementa una política que impide la ejecución de herramientas como `Mimikatz` (que leen la memoria del proceso `lsass.exe`). Desde la perspectiva del atacante, ¿qué paso crítico del ataque letal de NTLM (Pass-The-Hash) se está impidiendo al proteger la memoria de ese proceso de Windows?
3. En la arquitectura de autenticación de Windows NTLM, explicá por qué un cibercriminal (Red Team) con acceso de bajo nivel al sistema no tiene ninguna necesidad de perder tiempo realizando "Ataques de Diccionario" o intentando crackear (romper matemáticamente) el Hash NTLM robado de la computadora de un usuario.

**➡️ Siguiente nota:** [[09 - Autenticación II (Kerberos y el Sistema de Tickets)]]
