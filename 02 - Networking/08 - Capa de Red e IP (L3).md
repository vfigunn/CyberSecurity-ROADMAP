# 08 - Capa de Red e IP (Capa 3 / L3)

## 🎯 Objetivos
- Entender el rol de la Capa 3 del Modelo OSI en las redes interconectadas (Internet).
- Diferenciar el Direccionamiento Físico (L2) del Direccionamiento Lógico (L3).
- Conocer la función general del Protocolo de Internet (IP).

---

## 🧠 Concepto

Hasta ahora (Capa 1 y Capa 2) estábamos atrapados en la misma red local (LAN). Si tu computadora quería hablar con otra, el Switch miraba la MAC y la enviaba. Pero, ¿qué pasa si querés hablar con un servidor de Google en California? El Switch no sirve de nada, porque las direcciones MAC no pueden viajar de un edificio a otro (y mucho menos cruzar el mundo).

Para esto nace la **Capa de Red (Layer 3 - Network)**. 
Su función principal es el **Enrutamiento (Routing)** y el **Direccionamiento Lógico**. Su PDU (Unidad de Datos) se denomina **Paquete (Packet)**.

El trabajo de la Capa 3 es encontrar el mejor camino desde la Red A hasta la Red B (o Z), pasando a través de docenas de redes intermedias. Es como el sistema postal internacional.

---

## 📬 Direccionamiento Físico vs. Lógico

Es vital para la ciberseguridad entender la diferencia entre L2 (MAC) y L3 (IP). Usamos la analogía del correo.

Imaginá que querés enviar una carta desde tu casa (Red A) a la oficina central de Google en EE.UU. (Red B).

1. **La Dirección IP (Dirección Lógica / L3):** Es la "Dirección Postal" final (País, Ciudad, Calle, Número). Se la llama "lógica" porque depende de *dónde estés ubicado*. Si te mudás de país, tu IP cambia. Identifica el origen final y el destino final del viaje completo.
2. **La Dirección MAC (Dirección Física / L2):** Es como tu DNI o tu nombre. No cambia nunca. Pero para el viaje, funciona para los saltos de mano en mano.
   - Primero le das la carta en la mano al *Cartero de tu barrio* (Comunicación L2). La dirección L3 sigue siendo "Oficina de Google", pero la dirección L2 destino es la "Mano del cartero".
   - El cartero se la da en mano al *Chofer del camión*. (Nuevo salto L2, origen: Cartero, Destino: Chofer. ¡La dirección IP sigue sin cambiar!).
   - Esto sucede repetidas veces de router en router.

En resumen:
- **La Dirección IP de origen y destino en un paquete NO CAMBIAN durante todo el viaje por Internet.** (Identifican los extremos de la comunicación).
- **Las Direcciones MAC de origen y destino CAMBIAN en cada "salto" (Hop) de router a router.** (Identifican el enlace local).

---

## 🌐 IP: Internet Protocol

El rey absoluto de la Capa 3 (y de Internet) es el **Protocolo de Internet (IP)**. 
Es el protocolo que define la estructura del Paquete y del Direccionamiento Lógico.

Características clave del protocolo IP:
1. **Connectionless (Sin conexión):** IP no establece una llamada telefónica antes de enviar datos. Simplemente agarra el paquete y lo "tira" a la red esperando que llegue. (Es como enviar una carta simple en el correo tradicional, no sabés si llega o no).
2. **Best Effort (Mejor esfuerzo):** IP no garantiza la entrega. Si un router intermedio se apaga o está congestionado, el paquete se pierde (se descarta) y a IP no le importa. (La responsabilidad de reenviarlo si se pierde recae en la Capa 4, como veremos más adelante).
3. **Independiente del medio:** A la Capa 3 no le importa si viaja por fibra óptica, cobre o Wi-Fi. (Se abstrae de L1 y L2).

Existen dos versiones activas hoy en día: **IPv4** (antigua y dominante) e **IPv6** (nueva, diseñada porque nos quedamos sin IPs antiguas). Las estudiaremos a continuación.

---

## ❓ ¿Por qué importa en Seguridad?

La Capa 3 es el frente de batalla principal en la seguridad perimetral de Internet.
- **Firewalls:** La función básica de casi todos los Firewalls es inspeccionar paquetes en Capa 3 y decidir si los dejan pasar o los bloquean según la IP de origen y destino.
- **IP Spoofing:** Un atacante puede crear paquetes IP maliciosos y poner como "IP de origen" la de otra persona (falsificación). Esto se usa mucho en ataques DDoS (Denial of Service) para que las respuestas del servidor abrumen a una víctima inocente (Ataques de Reflexión/Amplificación).
- **Traceroute:** Cuando rastreamos a un atacante o investigamos la arquitectura de red (Reconocimiento en la [[13 - Cyber Kill Chain|Cyber Kill Chain]]), usamos herramientas de Capa 3 para mapear el camino por el que van nuestros paquetes (salto a salto).

---

## 📌 Must Know (Imprescindible)
- La diferencia de roles entre la dirección MAC (L2) y la dirección IP (L3).
- La IP origen y destino no cambian durante el viaje. Las MACs cambian en cada salto.
- La PDU de L3 es el **Paquete**.
- El protocolo IP es de "mejor esfuerzo" y "sin conexión".

## 💡 Good to Know (Bueno saberlo)
- La función de convertir nombres (como `google.com`) a direcciones IP no es trabajo del protocolo IP, es trabajo de DNS (un protocolo de Capa 7). La Capa 3 solo entiende números.

---

## 🔄 Preguntas de repaso
1. Si envío un paquete desde mi computadora en casa a un servidor web en otro país, ¿cuántas veces cambiará la dirección IP de destino en el paquete durante el viaje? ¿Cuántas veces cambiará la dirección MAC destino?
2. ¿Por qué decimos que el protocolo IP funciona "al mejor esfuerzo" (Best effort)?
3. ¿Cuál es el nombre de la Unidad de Datos del Protocolo (PDU) en la Capa 3?

**➡️ Siguiente nota:** [[09 - Direccionamiento IPv4]]
