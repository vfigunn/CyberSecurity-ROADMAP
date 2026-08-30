# 07 - Switches y ARP

## 🎯 Objetivos
- Entender el funcionamiento de un Switch de red (Dispositivo de Capa 2).
- Comprender la Tabla MAC (CAM Table).
- Aprender cómo funciona el protocolo ARP (Address Resolution Protocol).
- Conocer los ataques clásicos en redes locales: MAC Flooding y ARP Spoofing.

---

## 🔌 El Switch: El cerebro de la red local

El **Switch** es el dispositivo central en una topología de Estrella moderna. Trabaja en la [[06 - Capa de Enlace de Datos y MAC (L2)|Capa 2]] del Modelo OSI. Su objetivo es recibir cables de todas las computadoras de un edificio y permitir que se comuniquen eficientemente.

A diferencia del viejo Hub (que era "tonto" y enviaba el mensaje que recibía por un puerto hacia *todos* los demás puertos), el Switch es **inteligente**. Cuando recibe una Trama (Frame), lee la **dirección MAC de destino** y la reenvía *solamente* por el puerto (el cable) donde está conectada esa computadora específica.

### La Tabla MAC (CAM Table)
¿Cómo sabe el Switch en qué puerto está cada computadora? Aprende dinámicamente mediante la **MAC Table** (o Content Addressable Memory - CAM Table).

1. Cuando la PC-A envía un mensaje por primera vez, el mensaje entra al Switch por el Puerto 1.
2. El Switch lee la **MAC de origen** en la trama. ¡Ajá! El Switch anota en su memoria: *"La MAC `AA:AA:AA` está conectada en el Puerto 1"*.
3. Si el mensaje de la PC-A va dirigido a la MAC `BB:BB:BB` (PC-B), y el Switch todavía no la conoce, hace un envío a todos los puertos (esto se llama *Unknown Unicast Flooding* o *Flooding* simplemente) para buscarla.
4. Cuando la PC-B responde desde el Puerto 5, el Switch anota: *"La MAC `BB:BB:BB` está en el Puerto 5"*.
5. A partir de ese momento, la comunicación entre A y B es directa y privada. Ninguna otra PC en el Switch recibe esos mensajes. (Esto mejora enormemente la seguridad frente a los Hubs).

---

## 🔍 ARP: Address Resolution Protocol

Este es uno de los protocolos más importantes de entender en ciberseguridad.

Para que tu PC pueda enviarle un paquete a otra PC en la misma red local, necesita conocer su dirección física (MAC) para crear la Trama L2. **Pero vos, como usuario (y tus programas), no usas direcciones MAC, usas direcciones IP (Capa 3).**

¿Cómo traduce tu computadora la IP (que conoce) a la MAC (que no conoce)? Usando **ARP**.

**Cómo funciona ARP:**
1. Tu PC quiere hablar con `192.168.1.50`. Busca en su propia memoria caché (Tabla ARP local) si ya conoce su MAC. No la conoce.
2. Tu PC envía un mensaje **ARP Request** en modo *Broadcast* a toda la red: *"¡Hola a todos! ¿Quién tiene la IP `192.168.1.50`? Por favor, decime tu MAC"*.
3. Todas las PCs reciben el grito, pero solo la PC con esa IP responde.
4. La PC correcta envía un **ARP Reply** directamente a tu PC (Unicast): *"Yo soy la IP `192.168.1.50`, y mi MAC es `BB:BB:BB`"*.
5. Tu PC anota esto en su Tabla ARP para recordarlo, crea la Trama con esa MAC destino y la envía.

---

## ☠️ Ataques de Capa 2

Dado que ARP y las Tablas MAC se diseñaron sin seguridad (asumiendo confianza ciega en la red local), existen ataques devastadores:

### 1. MAC Flooding (Inundación MAC)
La memoria del Switch (CAM Table) tiene un límite. Si un atacante conecta su laptop y envía cientos de miles de Tramas falsas con direcciones MAC de origen aleatorias e inventadas por segundo, el Switch llenará su tabla inmediatamente. 
Cuando la tabla está llena, el Switch ya no puede aprender nuevas MACs y entra en estado de pánico: **comienza a enviar todo el tráfico a todos los puertos** (comportándose como un Hub tonto). El atacante ahora puede capturar (sniffear) el tráfico de todos.

### 2. ARP Spoofing / ARP Poisoning (Envenenamiento ARP)
Como ARP no tiene autenticación, si yo (atacante) te envío un mensaje de ARP Reply no solicitado diciendo *"Hola, yo soy el Router (Puerta de enlace), mi MAC es `C0:FF:EE`"*, tu computadora simplemente te cree y actualiza su Tabla ARP local.
Desde ese momento, todo el tráfico que tu PC quiera enviar a Internet (al router) se lo enviará a la computadora del atacante primero. El atacante lee tu tráfico y luego lo reenvía al router real para que no te des cuenta. Esto es un ataque de **Man-In-The-Middle (MitM)** clásico.

---

## 📌 Must Know (Imprescindible)
- Cómo aprende un Switch direcciones MAC (CAM Table).
- Qué es ARP y para qué sirve (Traducir IP conocida a MAC desconocida).
- El concepto de ARP Spoofing (Man-in-the-Middle en red local).

## 💡 Good to Know (Bueno saberlo)
- Los switches modernos Enterprise (ej. Cisco) tienen mitigaciones contra estos ataques, como **Port Security** (para frenar el MAC Flooding limitando 1 sola MAC por puerto físico) y **Dynamic ARP Inspection (DAI)** para bloquear ARP Spoofing.

---

## 🔄 Preguntas de repaso
1. Si un Switch de red recibe una Trama dirigida a una MAC destino que *no* figura en su tabla (CAM Table), ¿qué acción toma?
2. Explicá cómo un ataque de ARP Spoofing permite a un atacante espiar la navegación por internet de otra persona en la misma red Wi-Fi.
3. (Opcional, para reflexionar): En el comando ARP, ¿el Request viaja como Unicast o Broadcast? ¿Y el Reply?

**➡️ Siguiente nota:** [[08 - Capa de Red e IP (L3)]]
