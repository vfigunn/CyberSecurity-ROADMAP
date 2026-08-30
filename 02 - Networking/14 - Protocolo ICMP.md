# 14 - Protocolo ICMP (Ping y Traceroute)

## 🎯 Objetivos
- Conocer el propósito del protocolo ICMP.
- Comprender cómo funcionan las herramientas `ping` y `traceroute`.
- Entender por qué los atacantes abusan de ICMP y por qué los defensores a menudo lo bloquean.

---

## 🧠 Concepto

Habíamos dicho que el protocolo IP (Capa 3) funciona "al mejor esfuerzo". Si un paquete se pierde porque un router en medio de Internet se apagó, IP simplemente lo descarta sin avisarle a nadie.

Para solucionar esta falta de información, se inventó **ICMP (Internet Control Message Protocol)**.
ICMP es un protocolo de Capa 3 que se utiliza para **diagnósticos y reporte de errores** de la red. Si el router tira tu paquete, usa ICMP para mandarle un mensaje a tu computadora diciéndole *"Che, el paquete que me diste no pudo llegar al destino porque la red es inalcanzable"*.

A diferencia de protocolos como HTTP que transportan datos del usuario (imágenes, texto), ICMP solo transporta mensajes de control sobre la salud de la red.

---

## 🛠️ Herramientas basadas en ICMP

Los administradores de red (y los hackers) utilizan dos comandos fundamentales que aprovechan los mensajes ICMP. (Aprenderemos a usarlos en detalle en la [[26 - Herramientas de Red en CLI|Nota 26]]).

### 1. Ping
Se usa para verificar si una computadora remota está "viva" y si podemos alcanzarla a través de la red.
- Tu computadora envía un mensaje **ICMP Echo Request**.
- Si el destino está encendido y permite este tráfico, responde con un **ICMP Echo Reply**.
- Si el destino no existe, el último router en el camino responde con un **Destination Unreachable** (Destino inalcanzable).

### 2. Traceroute / Tracert
Se usa para ver el camino exacto (salto a salto, router por router) que toma un paquete desde tu computadora hasta el destino final en Internet, y cuánto tiempo tarda en cada paso. Es vital para detectar dónde está roto un enlace de red.

---

## ☠️ Abuso de ICMP y Ciberseguridad

Aunque ICMP se creó para ayudar a los administradores de red a diagnosticar problemas (como el estetoscopio de un médico), los atacantes lo utilizan constantemente para actividades maliciosas.

1. **Reconocimiento (Fase 1 de la [[13 - Cyber Kill Chain|Kill Chain]]):**
   - Un atacante puede hacer un *Ping Sweep* (Enviar pings a miles de IPs consecutivas) para hacer un mapa de qué servidores están encendidos en una red corporativa. Es la forma más rápida de descubrir objetivos viables.
2. **Denegación de Servicio (Ping of Death / Smurf Attack):**
   - Si miles de computadoras (Botnets) le envían millones de Pings simultáneamente a un servidor, este utilizará toda su CPU y ancho de banda intentando procesarlos y responder con *Echo Replies*, causando que deje de funcionar para los usuarios legítimos.
3. **Exfiltración de Datos y C2 (ICMP Tunneling):**
   - Este es un ataque avanzado. Dado que muchos Firewalls corporativos bloquean la salida a Internet a protocolos inseguros, pero permiten que ICMP (Ping) salga libremente para que los administradores diagnostiquen la red... los atacantes pueden esconder información robada *dentro* del espacio de datos (payload) de un paquete Ping inofensivo. Al Firewall le parecerá que la PC está simplemente haciendo pings a Internet, cuando en realidad está filtrando secretos de la empresa (o recibiendo comandos del servidor atacante).

### La Defensa: Bloquear ICMP

Por todas estas razones, **es una mejor práctica de seguridad (y casi un estándar) que las redes corporativas y los Firewalls configuren reglas para BLOQUEAR (Drop) todo el tráfico ICMP entrante desde Internet**.

Si intentas hacerle un `ping` a la página web del Pentágono, no recibirás respuesta. Esto no significa que el servidor esté caído, significa que el Firewall del Pentágono está configurado para "ignorar y tirar a la basura" tus ICMP Echo Requests para evitar que hagas reconocimiento de su red (Stealth Mode).

*Nota para Blue Teams:* A menudo se bloquea ICMP entrante (desde Internet hacia adentro), pero se suele permitir (o controlar estrictamente) ICMP saliente (de la red local hacia Internet) o ICMP interno para permitir a los equipos de IT trabajar. Bloquear ICMP totalmente en el interior de una LAN rompe muchas funcionalidades legítimas.

---

## 📌 Must Know (Imprescindible)
- ICMP es el protocolo de reporte de errores de Capa 3.
- Ping y Traceroute utilizan paquetes ICMP (Echo Request y Echo Reply).
- Por qué los Firewalls suelen bloquear pings desde Internet (evitar el escaneo/reconocimiento).

## 💡 Good to Know (Bueno saberlo)
- A nivel técnico, el Traceroute moderno en Linux (y a veces el Ping) no solo usa ICMP, sino que envía paquetes UDP a puertos cerrados para forzar a los routers a responder con un mensaje ICMP del tipo "Time Exceeded" (Tiempo Excedido).

---

## 🔄 Preguntas de repaso
1. Si un atacante quiere saber qué computadoras están encendidas dentro de una red, ¿qué mensaje ICMP específico debería enviar y qué respuesta debería esperar?
2. ¿Por qué una página web puede estar funcionando perfectamente (podés navegarla), pero al hacerle `ping` en la terminal, te dice "Request timed out" (Tiempo de espera agotado)?
3. ¿Cómo funciona conceptualmente el ataque de ICMP Tunneling?

**➡️ Siguiente nota:** [[15 - Capa de Transporte (L4)]]
