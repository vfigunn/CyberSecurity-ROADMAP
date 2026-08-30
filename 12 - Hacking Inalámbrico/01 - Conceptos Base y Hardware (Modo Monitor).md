# 01 - Conceptos Base y Hardware (Modo Monitor)

## 🎯 Objetivos
- Entender la terminología clave del Wi-Fi (BSSID, ESSID).
- Descubrir la limitación física de las placas Wi-Fi estándar.
- Aprender el concepto supremo de la recolección de RF: El Modo Monitor.

---

## 🧠 Concepto: La radio invisible

A diferencia del cable de red, donde los datos viajan cerrados por un tubo de cobre directo a la PC, el Wi-Fi es como una torre de radio pública.
El Router de tu casa **emite las ondas para todos lados**. Si hay 5 computadoras en tu casa, **las ondas chocan y tocan las antenas de las 5 computadoras al mismo tiempo**.

Para que no sea un caos, la placa de red de tu computadora (La Antena Receptora) tiene un filtro inteligente. La antena lee en el aire *"Ah, este mensaje de radio está dirigido a mi hermano"* y, como no es para vos, **tu placa de red borra y descarta ese mensaje automáticamente**.

Este comportamiento (Donde la placa de red solo te deja ver y leer los paquetes que van dirigidos a TU computadora) se llama **Modo Administrado (Managed Mode)**. Es el modo normal del 100% de los celulares y laptops del mundo.

---

## 📡 El Arma del Hacker: El Modo Monitor

Para hackear una red, el atacante *necesita* escuchar los paquetes que vuelan en el aire dirigidos a *otras personas*. Necesita espiar.

Para lograrlo, el hacker debe cambiar su tarjeta de red al **Modo Monitor** (O Modo Promiscuo Inalámbrico).
Cuando pasás una tarjeta a Modo Monitor, desactivás el filtro inteligente. A partir de ahora, tu tarjeta de red agarra, lee y guarda **absolutamente todos los mensajes, contraseñas y señales que crucen por tu antena**, sin importar a quién iban dirigidos ni en qué red estén.

**Limitación de Hardware:**
- El 95% de las tarjetas Wi-Fi integradas en las Notebooks comunes (Intel/Broadcom) bloquean físicamente el Modo Monitor por cuestiones de drivers.
- Por eso, todo Hacker inalámbrico necesita comprar una **Antena Wi-Fi Externa por USB** especial (Chipsets famosos como *Atheros AR9271* o *Ralink RT5370*, de marcas como Alfa Networks) que soportan inyección y Modo Monitor en Kali Linux de forma nativa.

*(Comando de Kali para activar el Modo Monitor: `airmon-ng start wlan0`)*.

---

## 🏷️ Vocabulario Fundamental (MAC vs SSID)

Cuando pasas a Modo Monitor y abrís la red, ves miles de letras volando. Tenés que saber identificarlas:

- **ESSID (Nombre de la Red):** Es el nombre público en texto (Ej: `"Fibertel-Wi-Fi"`, `"Starbucks_Invitados"`).
- **BSSID (La verdadera identidad):** El ESSID se puede falsificar. El BSSID es la **Dirección MAC** (Huella digital física) del Router (Ej: `00:14:22:01:23:45`). Los hackers atacan a los BSSID, no a los nombres.
- **Canal (Channel):** El espectro de 2.4 GHz está dividido en 14 canales (Como los carriles de una autopista). El Router de tu vecino y el tuyo deben operar en canales distintos para no chocar y hacer interferencia. Un atacante en Modo Monitor debe decirle a su antena qué "Canal" exacto quiere espiar (Sintonizar).

---

## 📌 Must Know (Imprescindible)
- **Modo Administrado (Managed):** Tu antena solo presta atención a los paquetes que van dirigidos específicamente a ti.
- **Modo Monitor:** Tu antena capta TODOS los paquetes que vuelan por el aire en tu radio de alcance, sin importar origen o destino. Obligatorio para realizar Hacking Inalámbrico.
- Diferenciar el nombre comercial de la red (**ESSID**) frente a la dirección física del hardware del Router emisor (**BSSID / MAC**).

---

## 🔄 Preguntas de repaso
1. Un estudiante intenta espiar los paquetes Wi-Fi (Trama 802.11) de sus vecinos utilizando la tarjeta de red interna genérica (Intel) que vino soldada en su laptop de fábrica con Windows 10. Al intentar escuchar la red, no obtiene nada útil más allá de su propio tráfico. Sabiendo la diferencia de estados de las tarjetas de red, explicá brevemente qué "Modo de Captura" requiere activar y por qué su hardware interno probablemente lo esté limitando, obligándolo a comprar una antena USB especial (Como una Alfa Network).
2. Estás en Modo Monitor escuchando el Canal 6 (El más congestionado). A nivel de las tramas de radiofrecuencia (Física), si la red del objetivo de tu auditoría (El router de la empresa a hackear) se encuentra configurado para transmitir su señal en el "Canal 11"; ¿Por qué tu ataque y tu recolección de paquetes será un fracaso absoluto? (Asociá tu respuesta al concepto de "Sintonizar la radio").
3. Si un delincuente crea una red falsa y gratuita en su teléfono celular y le pone el nombre público de texto "Wi-Fi Aeropuerto" para confundir a los viajeros; ¿Qué identificador técnico (que figura como una dirección MAC) debería observar un Analista de Seguridad experimentado en su software analizador para darse cuenta de que ese Access Point (Punto de Acceso) no es el Router original del aeropuerto?

**➡️ Siguiente nota:** [[02 - El Protocolo WPA2 y el 4-Way Handshake]]
