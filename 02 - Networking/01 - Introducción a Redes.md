# 01 - Introducción a Redes

## 🎯 Objetivos
- Entender el concepto básico de qué es una red informática.
- Conocer los componentes principales (Nodos, Medios, Protocolos).
- Diferenciar entre los modos de comunicación (Unicast, Multicast, Broadcast).

---

## 🧠 Concepto

Una **Red Informática (Network)** es simplemente un conjunto de dos o más computadoras u otros dispositivos conectados entre sí con el objetivo de **compartir recursos e información**.

Si tenés una computadora sola sin conexión a internet ni a ningún otro cable, es una isla. Es muy segura (es difícil hackearla), pero no es muy útil. Las redes nacieron por la necesidad de que la Computadora A pudiera enviarle un archivo a la Computadora B sin tener que llevarlo físicamente en un disquete.

### Los tres componentes de cualquier red

Para que exista una red, sin importar si es el Wi-Fi de tu casa o el centro de datos de Google, siempre se necesitan tres cosas:

1. **Nodos (Dispositivos):** Los equipos que se comunican. Pueden ser PCs (End Devices / Hosts), servidores, impresoras, o dispositivos intermedios que ayudan a conectar la red (como Routers y Switches).
2. **Medios (Media):** El camino físico o inalámbrico por donde viajan los datos. Puede ser cable de cobre (Ethernet), fibra óptica (luz) o radiofrecuencia (Wi-Fi, 5G).
3. **Protocolos (Protocols):** Las reglas del juego. Si una PC "habla" chino y la otra "habla" francés, no se van a entender, aunque estén conectadas por el mismo cable. Un protocolo es un conjunto de reglas estándar (como TCP/IP) que asegura que ambos dispositivos se entiendan.

---

## 🗣️ Modos de Comunicación

Cuando un nodo quiere hablar en una red, tiene tres formas de hacerlo. Entender esto es vital para la ciberseguridad, ya que los atacantes suelen abusar de estas formas de comunicación para saturar redes (DDoS) o espiar tráfico (Sniffing).

1. **Unicast (Uno a Uno):** 
   - El mensaje va de un origen único a un destino único.
   - *Ejemplo:* Tu PC enviando un mensaje directo al servidor web de tu banco. Es una conversación privada.

2. **Multicast (Uno a Muchos / Grupo):**
   - El mensaje va de un origen a un *grupo específico* de dispositivos que se suscribieron para escucharlo.
   - *Ejemplo:* Transmisión de TV por IP (IPTV). Solo las TVs que están en ese canal reciben los datos, el resto de la red los ignora.

3. **Broadcast (Uno a Todos):**
   - El mensaje va de un origen a *todos los dispositivos* de la red local, sin excepción. Todos se ven obligados a procesar el mensaje.
   - *Ejemplo:* Cuando tu PC se conecta a una red nueva y grita: "¡Hola! ¿Alguien me puede dar una dirección IP?".
   - *Riesgo de Seguridad:* Los broadcasts generan mucho "ruido" en la red. Los atacantes pueden enviar millones de mensajes broadcast falsos para congelar la red (Broadcast Storm).

---

## ❓ ¿Por qué importa en Seguridad?

Los atacantes rara vez están sentados frente a la computadora que quieren vulnerar. El 99.9% de los ciberataques ocurren *a través de la red*.
- Un [[06 - Exploits|Exploit]] viaja por la red.
- El malware se descarga por la red.
- Los datos robados se extraen (exfiltran) por la red.
- El atacante controla la máquina de forma remota (C2) a través de la red.

Si controlás y entendés la red, podés ver todo lo que hace el atacante. Por eso el [[14 - SOC & SIEM|Blue Team]] ama analizar el tráfico de red: **"Los logs de un sistema se pueden borrar o falsificar; los paquetes de red no mienten."**

---

## 📌 Must Know (Imprescindible)
- Los tres componentes de una red (Nodos, Medios, Protocolos).
- La diferencia fundamental entre Unicast, Multicast y Broadcast.

## 💡 Good to Know (Bueno saberlo)
- A los dispositivos finales (tu laptop, tu celular, un servidor) se los llama **Hosts**. A los dispositivos que ayudan a conectar la red (Routers, Switches, Firewalls) se los llama **Dispositivos Intermedios (Intermediary Devices)**.

---

## 🔄 Preguntas de repaso
1. Si dos computadoras están conectadas físicamente por un cable pero no tienen configurado el mismo protocolo, ¿pueden comunicarse?
2. ¿Por qué el tráfico tipo "Broadcast" representa un riesgo mayor de saturación para una red que el tráfico "Unicast"?
3. Definí con tus propias palabras qué es un Host en el contexto de redes.

**➡️ Siguiente nota:** [[02 - Topologías y Tipos de Redes]]
