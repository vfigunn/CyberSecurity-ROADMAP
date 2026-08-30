# 16 - TCP vs UDP

## 🎯 Objetivos
- Conocer a fondo las diferencias entre los dos gigantes de la Capa de Transporte.
- Comprender el Three-Way Handshake de TCP.
- Saber elegir cuándo un sistema debe usar TCP y cuándo UDP.

---

## 🆚 La Batalla de L4

El protocolo de Internet (IP) en Capa 3 se encarga de enviar paquetes, pero como sabemos, "al mejor esfuerzo". No le importa si se pierden.
La Capa 4 tiene que decidir cómo manejar eso. Ofrece dos opciones radicalmente distintas a los programadores de aplicaciones.

### 1. TCP (Transmission Control Protocol)
*El burócrata controlador y perfeccionista.*
- **Connection-oriented (Orientado a conexión):** Antes de enviar ni un solo byte de datos reales, TCP establece una "llamada" o sesión formal con el servidor. Si el servidor no contesta la llamada, TCP cancela todo y no envía nada.
- **Fiable (Reliable):** Garantiza que todos los datos van a llegar, y que van a llegar intactos.
- **Acuse de recibo (Acknowledgments - ACK):** Por cada pedacito de dato (segmento) que envía, TCP espera que el receptor le mande un mensaje diciendo: *"Recibí la pieza 1"*. Si no recibe el ACK en cierto tiempo, asume que se perdió en la red y **lo vuelve a enviar automáticamente**.
- **Control de Flujo (Flow Control):** Si la computadora receptora es muy vieja y lenta, le puede decir a TCP: *"Frená un poco, me estás enviando cosas muy rápido"*. TCP reduce su velocidad para no saturarla (Windowing).
- **Ejemplos de uso:** Páginas Web (HTTP/HTTPS), Transferencia de Archivos (FTP), Correos (SMTP), Secure Shell (SSH). *(¿Aceptarías que al cargar una página web falten letras al azar o que falte una línea de código? No. Se necesita TCP).*

### 2. UDP (User Datagram Protocol)
*El repartidor rápido, descuidado y "Connectionless".*
- **Connectionless (Sin conexión):** UDP no saluda a nadie ni pide permiso. Arma el paquete y lo dispara a la red inmediatamente.
- **No Fiable (Unreliable):** UDP no garantiza que el dato llegue. A UDP no le importa.
- **Sin Acuse de recibo:** UDP nunca espera un ACK. Si el dato se pierde en un router congestionado, se perdió para siempre (a menos que la aplicación de Capa 7 se dé cuenta y pida reenviarlo).
- **Velocidad:** Al no tener que enviar ACKs de confirmación ni saludar antes de empezar, UDP es increíblemente rápido y ligero. Tiene mucha menos latencia (Lag).
- **Ejemplos de uso:** Streaming de video en vivo (Twitch), Videollamadas (Zoom), Juegos Online, Consultas DNS. *(Si estás en un juego de disparos online y perdés el paquete que dice dónde estaba tu personaje hace 0.1 segundos, no te sirve de nada que TCP lo reenvíe 1 segundo más tarde. Querés la información más nueva lo más rápido posible. Es preferible que tu personaje dé un mini "salto" en la pantalla a que el juego se congele).*

---

## 🤝 El Three-Way Handshake (TCP)

Este es uno de los conceptos más importantes en redes y ciberseguridad. Es la forma en que TCP establece esa "llamada" (sesión) inicial antes de enviar datos. Se compone de 3 pasos (por eso *Three-Way*):

Imaginá que tu PC (Cliente) quiere ver la página de Google (Servidor):
1. **SYN (Synchronize):** Tu PC envía un segmento con una bandera o *flag* encendida que se llama SYN. Significa: *"Hola Google, quiero establecer una conexión con vos. Mis números de secuencia empezarán en 100"*.
2. **SYN-ACK (Synchronize-Acknowledge):** Google recibe el mensaje. Si el puerto está abierto y acepta conexiones, envía una respuesta con dos flags encendidos (SYN y ACK). Significa: *"Hola PC, recibí tu mensaje (ACK). Acepto la conexión. Mis números de secuencia empezarán en 500 (SYN)"*.
3. **ACK (Acknowledge):** Tu PC finaliza la ceremonia enviando un último ACK. Significa: *"Entendido Google, recibí tu saludo. La conexión está establecida"*.

Inmediatamente después de este paso 3, la PC comienza a enviar la petición HTTP real ("Dame la página inicial de Google").
Cuando quieren terminar la llamada, hacen un proceso similar pero usando la bandera **FIN** (Finish).

---

## ❓ ¿Por qué importa en Seguridad?

Los analistas de seguridad viven observando estos comportamientos, y los atacantes abusan de ellos.

### Ataques en TCP (SYN Flood)
Un atacante abusa del Three-Way Handshake. Envía millones de peticiones **SYN** (Paso 1) desde IPs falsas. El servidor responde millones de veces con **SYN-ACK** (Paso 2) y guarda en su memoria RAM que hay "conexiones pendientes" esperando el paso 3. El atacante nunca envía el paso 3. La memoria RAM del servidor se llena de llamadas no contestadas hasta que colapsa y deja de funcionar (Ataque DDoS tipo SYN Flood).

### Ataques en UDP (Amplificación)
Como UDP no tiene un saludo inicial ("no pide permiso"), es muy fácil hacer IP Spoofing (Falsificar la IP de origen, [[08 - Capa de Red e IP (L3)|ver Nota 08]]). Un atacante falsifica su IP para que sea la de la víctima, y le envía un pequeño mensaje UDP (ej. DNS) a un servidor enorme. El servidor enorme responde enviando 50 veces más cantidad de datos de los que recibió, pero en vez de enviárselos al atacante, se los envía a la víctima inocente, aplastando su red.

### Escaneo de Puertos (Nmap)
Cuando hacemos un *TCP Connect Scan* (Escaneo estándar), Nmap completa el 3-way handshake para confirmar si un puerto está abierto (deja logs en el servidor). 
Si hacemos un *TCP Stealth Scan* (Escaneo sigiloso o SYN Scan), Nmap envía el SYN, el servidor responde SYN-ACK (¡sabemos que está abierto!), e inmediatamente Nmap le envía un **RST (Reset)** para cancelar la conexión antes de terminarla, buscando evitar ser registrado en los logs (aunque hoy en día todos los firewalls lo detectan).

---

## 📌 Must Know (Imprescindible)
- Las diferencias filosóficas entre TCP y UDP (Fiable vs No Fiable, Orientado a conexión vs Sin conexión).
- Los tres pasos exactos del Three-Way Handshake (SYN -> SYN-ACK -> ACK).
- Entender el concepto de "Flags" (Banderas) en TCP (SYN, ACK, FIN, RST).

---

## 🔄 Preguntas de repaso
1. Si estuvieras desarrollando una aplicación para transferir archivos bancarios ultra-críticos de un banco a otro cada noche, ¿usarías TCP o UDP? ¿Por qué?
2. Si estuvieras desarrollando una aplicación para transmitir el audio de un locutor de radio por Internet, ¿usarías TCP o UDP? ¿Por qué?
3. En un ataque de escaneo de puertos (Stealth Scan), un atacante envía un SYN. El servidor responde con un RST (Reset) en lugar de un SYN-ACK. ¿Qué significa esto para el atacante respecto al estado del puerto?

**➡️ Siguiente nota:** [[17 - Puertos y Sockets]]
