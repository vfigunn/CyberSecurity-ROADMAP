# 08 - El Intercambio de Llaves (Diffie-Hellman)

## 🎯 Objetivos
- Entender cómo funciona el Internet real (El Sistema Híbrido).
- Conocer la revolución del algoritmo Diffie-Hellman (DH).
- Comprender el concepto de *Forward Secrecy* (Secreto Hacia Adelante).

---

## 🧠 Concepto: Lo Mejor de Ambos Mundos (El Sistema Híbrido)

Recapitulemos las dos notas anteriores:
1. **La Simétrica (AES)** es genial porque es súper rápida, pero es terrible porque no podemos enviarle la Llave compartida a la otra persona por Internet de forma segura.
2. **La Asimétrica (RSA)** es genial porque podés hablar seguro sin haber compartido un secreto antes (gracias a la Llave Pública), pero es terriblemente lenta para cifrar archivos grandes.

La solución del ser humano fue brillante: **El Sistema Híbrido**. (Así funciona el protocolo TLS/HTTPS o el SSH en este preciso instante mientras lees esto).

### El proceso del Híbrido (La Cápsula)
1. Tu computadora inventa en el momento una contraseña Simétrica (AES) aleatoria para esta sesión de compra en Amazon.
2. Esta contraseña AES es ideal para cifrar toda tu tarjeta de crédito rápido, pero necesitás hacérsela llegar a Amazon.
3. Le pedís la **Llave Pública (Asimétrica)** a Amazon.
4. Metés tu contraseña AES dentro de un "cofre matemático" y lo **cerrás** usando la Llave Pública pesada de Amazon.
5. Enviás el cofre. (El cifrado Asimétrico es lento, pero no importa, porque solo lo estás usando para cifrar un texto minúsculo: tu contraseña AES).
6. Amazon recibe el cofre, lo abre con su Llave Privada, y ahora ambos tienen la contraseña Simétrica (AES).
7. A partir de ese momento, apagan el motor Asimétrico (RSA), y se comunican el resto de la sesión de compras usando la altísima velocidad del cifrado AES.

> **Resumen:** Se usa Criptografía Asimétrica lenta una sola vez, pura y exclusivamente para poder enviarle la Llave Simétrica rápida a la otra persona.

---

## 🎨 El Milagro de Diffie-Hellman (Key Exchange)

A pesar de lo increíble que es el sistema Híbrido (RSA envolviendo a AES), tiene un defecto histórico.
Si durante años un servicio de espionaje interceptó y grabó todo el tráfico incomprensible entre vos y Amazon, y el día de mañana logran asaltar los servidores de Amazon y robar la **Llave Privada de Amazon**... van a poder abrir retrospectivamente TODOS los "cofres" que enviaste en el pasado, encontrar tu contraseña AES, y desencriptar todos tus historiales de tarjetas de crédito. (Carece de Secreto Hacia Adelante).

Para solucionar esto, Whitfield Diffie y Martin Hellman inventaron el **Acuerdo de Llaves Diffie-Hellman (DH)** en 1976.

### La Analogía de la Mezcla de Colores
DH permite que dos personas acuerden una misma contraseña secreta a través de un canal espigado por hackers, **sin enviar jamás la contraseña por la red**. (Parece magia, pero es matemática modular).

1. Ambas partes (Alice y Bob) acuerdan públicamente en el chat usar un Color Común (Ej. **Amarillo**). El atacante lo escucha.
2. Alice elige un Color Secreto en su mente (**Rojo**) y Bob elige uno en su mente (**Cian**). (Estos nunca se envían).
3. Alice mezcla en un balde su Rojo con el Amarillo público, logrando un **Naranja**. Bob mezcla su Cian con el Amarillo, logrando un **Verde**. 
4. Ambos se envían los colores mezclados (Naranja y Verde) a través del chat público. El atacante intercepta ambos. El problema para el atacante es que las matemáticas de mezcla son de *unidireccionalidad*. Es físicamente (y computacionalmente) imposible mirar un balde de pintura Verde y saber exactamente qué proporciones matemáticas de color usó Bob en secreto.
5. El truco final: Alice recibe el Verde de Bob, y le tira una gota de su secreto (**Rojo**). Bob recibe el Naranja de Alice y le tira una gota de su secreto (**Cian**).
6. Sin saberlo, **ambos lograron exactamente la misma mezcla final (Marrón Oscuro)** (Amarillo + Rojo + Cian). 
7. Ambos acaban de generar matemáticamente la misma llave Simétrica idéntica, pero la llave jamás viajó por el chat.

---

## 🔒 Perfect Forward Secrecy (PFS)

Al utilizar Diffie-Hellman Efímero (ECDHE - Con curvas elípticas), cada vez que abrís una página web nueva, el protocolo genera nuevos colores matemáticos temporales (Efímeros) que solo sirven para generar la llave AES de esa única sesión de 5 minutos, y luego las matemáticas se destruyen de la memoria RAM para siempre.

Si un gobierno te espía durante un mes, graba tu tráfico, y luego irrumpe en tu servidor robándose tu Disco Duro (y la Llave Privada del servidor web), no le servirá de nada. Como las llaves de sesión fueron generadas por el algoritmo efímero Diffie-Hellman y destruidas, **tus comunicaciones del pasado están protegidas para siempre**. Esto se llama **Perfect Forward Secrecy (Secreto Perfecto Hacia Adelante)**.

---

## 📌 Must Know (Imprescindible)
- El mundo real (HTTPS/TLS) usa un **Sistema Híbrido**: Usa criptografía Asimétrica (RSA/ECC) únicamente para enviarse de forma segura la llave de sesión, y luego usa criptografía Simétrica (AES) para el tráfico pesado.
- El algoritmo **Diffie-Hellman** no encripta datos, su único trabajo es permitir que dos partes "acuerden" una llave secreta matemática a través de un canal público, sin transmitirla.
- Saber qué es el **Forward Secrecy**: La garantía de que si tu llave privada maestra se compromete hoy, las sesiones del pasado permanecen indescifrables.

---

## 🔄 Preguntas de repaso
1. Un desarrollador de software diseña una aplicación de chat. Para asegurar la máxima velocidad, utiliza el algoritmo AES-256 para todo el contenido de los mensajes. Sin embargo, su problema es cómo lograr que ambos usuarios (que recién instalan la app) compartan la misma contraseña AES sin que los espías de Internet la lean. Describí cómo la aplicación debería utilizar el algoritmo asimétrico RSA (Pública/Privada) en el milisegundo número 1 para solucionar el envío de la clave AES.
2. Si un protocolo criptográfico utiliza el algoritmo Diffie-Hellman para establecer la llave de sesión AES, ¿qué parte de la "contraseña AES final" viaja realmente a través de los routers de Internet para que el atacante pueda interceptarla?
3. Supongamos que WhatsApp (que usa encriptación de extremo a extremo) NO utilizara protocolos con Perfect Forward Secrecy (PFS). Si un atacante interceptara hoy todos tus paquetes encriptados durante el aire y, un año después, lograra robar de tu celular tu Llave Privada Maestra de Identidad. ¿Qué consecuencias retrospectivas tendría esto sobre los mensajes del pasado grabados por el atacante?

**➡️ Siguiente nota:** [[09 - Firmas Digitales (Integridad y No Repudio)]]
