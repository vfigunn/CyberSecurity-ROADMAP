# 06 - Capa de Enlace de Datos y MAC (Capa 2 / L2)

## 🎯 Objetivos
- Entender el rol de la Capa 2 en una red de área local (LAN).
- Conocer qué es una dirección MAC y cómo se estructura.
- Diferenciar el concepto de direccionamiento físico (L2) frente al direccionamiento lógico (L3).

---

## 🧠 Concepto

La **Capa de Enlace de Datos (L2)** se encarga de tomar los bits de la Capa 1 y darles sentido y estructura. Su función principal es permitir que los dispositivos se comuniquen **dentro de la misma red local (LAN)**.

Para hacer esto, L2 agrupa los bits en bloques organizados llamados **Tramas (Frames)**. 
Una Trama incluye los datos originales más una cabecera (Header) y una cola (Trailer). En la cabecera L2, lo más importante que se incluye son las **Direcciones MAC** (origen y destino).

> **Aclaración vital:** La Capa 2 (y las direcciones MAC) solo funcionan dentro de la *misma red local*. Las direcciones MAC no viajan a través de Internet (eso es trabajo de la Capa 3 y las IPs).

---

## 🏷️ Direcciones MAC (Media Access Control)

La dirección MAC es la **dirección física** de un dispositivo de red. A diferencia de tu dirección de casa (que cambia si te mudas), la MAC es como tu huella digital o el número de chasis de tu auto.

- **Viene grabada de fábrica:** Cada tarjeta de red (NIC - Network Interface Card), ya sea el chip Wi-Fi de tu celular o la placa Ethernet de un servidor, tiene una dirección MAC única quemada en su hardware durante la fabricación.
- **Formato:** Se compone de 48 bits, escritos generalmente como 6 pares de caracteres hexadecimales, separados por dos puntos o guiones.
  - *Ejemplo:* `00:1A:2B:3C:4D:5E` o `00-1A-2B-3C-4D-5E`.
- **Estructura:**
  - Los primeros 24 bits (3 pares) son el **OUI (Organizationally Unique Identifier)**, que identifica al fabricante del dispositivo (ej. Apple, Cisco, Intel).
  - Los últimos 24 bits son asignados de manera única por ese fabricante (como un número de serie).

### MAC Address Spoofing (Falsificación)
Aunque la dirección MAC viene grabada físicamente en el chip, los sistemas operativos (Windows, Linux, macOS) leen esa dirección al arrancar y la usan por software. Como es por software, **podemos cambiarla temporalmente**. 
Esto se llama *MAC Spoofing* o falsificación de MAC. Los atacantes lo usan constantemente para eludir listas de control de acceso (ej. si el Wi-Fi del hotel solo deja pasar a la MAC de la PC del gerente, el atacante falsifica la suya para ser igual a la del gerente).

---

## 📡 CSMA/CD: Cómo se habla sin interrumpir (Opcional pero útil)

En las redes antiguas con topología de Bus o usando Hubs (donde todos compartían el mismo cable eléctrico), si dos PCs transmitían a la misma vez, las señales eléctricas chocaban y se destruían (Colisión).
Para evitar esto, inventaron **CSMA/CD (Carrier Sense Multiple Access with Collision Detection)**:
1. *Carrier Sense:* La PC "escucha" el cable. ¿Hay silencio?
2. *Multiple Access:* Si hay silencio, transmite sus datos.
3. *Collision Detection:* Mientras transmite, escucha. Si detecta una colisión con otra señal, para de transmitir, espera un tiempo aleatorio, y vuelve a intentar.

En las redes modernas con Switches, esto ya casi no ocurre (porque los Switches segmentan los dominios de colisión), pero es la base de cómo funciona Ethernet históricamente. (En Wi-Fi se usa una variante llamada CSMA/CA).

---

## ❓ ¿Por qué importa en Seguridad?

Los atacantes en la red local operan agresivamente en la Capa 2.

- **Falsificación de identidad local:** Como vimos, el MAC Spoofing permite a un atacante hacerse pasar por otro equipo en la red (ej. la impresora o el servidor) o saltarse filtros restrictivos.
- **Sniffing en redes L2:** Si la red no está bien configurada (ej. si están usando Hubs o si logran engañar a un Switch), el atacante puede capturar tramas de otros usuarios (Wiretapping lógico).
- **Falta de seguridad inherente:** Los protocolos estándar de Capa 2 (como Ethernet y ARP) **no tienen autenticación**. Asumen que todos en la LAN local son amigables. Esta es una falla de diseño que aprovechamos en pentesting.

---

## 📌 Must Know (Imprescindible)
- La PDU de L2 es la **Trama (Frame)**.
- La dirección MAC es la dirección **física (L2)** y se usa **solo para la red local**.
- La MAC se divide en OUI (fabricante) y serial único.
- El concepto de MAC Spoofing.

## 💡 Good to Know (Bueno saberlo)
- La dirección MAC de Broadcast es `FF:FF:FF:FF:FF:FF`. Si una trama va dirigida a esa MAC, todos los dispositivos de la red local la procesan.

---

## 🔄 Preguntas de repaso
1. Si un atacante quiere saber qué marca de computadora está utilizando una víctima en la misma red Wi-Fi, ¿qué parte de la dirección de red debería mirar?
2. ¿Por qué una dirección MAC no te sirve para enviar un mensaje desde tu computadora en Argentina a un servidor en Japón?
3. ¿Cuál es el nombre de la unidad de datos (PDU) que se maneja en la Capa 2?

**➡️ Siguiente nota:** [[07 - Switches y ARP]]
