# 20 - DHCP (Dynamic Host Configuration Protocol)

## 🎯 Objetivos
- Entender el propósito del protocolo DHCP.
- Conocer el proceso "DORA" de asignación de direcciones.
- Comprender los riesgos de seguridad: DHCP Starvation y Rogue DHCP.

---

## 🧠 Concepto

Para que un dispositivo se comunique en una red IP, necesita configuración: una **Dirección IP**, una **Máscara de subred** y una **Puerta de Enlace (Default Gateway)**. (A menudo también necesita saber cuál es el servidor [[19 - DNS en Profundidad|DNS]]).

- **IP Estática:** Un administrador humano escribe estos datos manualmente en cada computadora. Es seguro y predecible (se usa siempre para Servidores y Routers).
- **IP Dinámica (DHCP):** Imagina una red Wi-Fi de un aeropuerto con 5.000 viajeros diarios. No podés tener a un humano escribiendo IPs. 

**DHCP (Capa 7, Puertos UDP 67 y 68)** soluciona esto. Es un servidor (que suele vivir dentro del router) que administra un "pool" (una pileta) de IPs disponibles y las "alquila" (Lease) automáticamente a los dispositivos que se conectan, por un tiempo determinado (ej. 24 horas).

---

## 🤝 El Proceso D.O.R.A.

Cuando conectas tu PC a la red (o te unís al Wi-Fi), tu PC todavía no tiene IP, por lo tanto no sabe a quién hablarle. ¿Cómo le pide la IP al servidor DHCP si no sabe dónde está? Usando Broadcast (gritos).

El proceso se memoriza con el acrónimo **D.O.R.A.** (Discover, Offer, Request, Acknowledge).

1. **Discover (Descubrimiento):** La PC (Cliente) envía un mensaje de Broadcast a la red (Capa 2): *"¡Hola a todos! No tengo IP. ¿Hay algún servidor DHCP escuchando?"*.
2. **Offer (Oferta):** El Servidor DHCP escucha el grito y responde (usando la dirección MAC de la PC que guardó): *"¡Acá estoy! Te ofrezco la IP `192.168.1.55`"*.
3. **Request (Petición):** La PC dice (nuevamente en Broadcast, por si hubo varios servidores ofreciendo al mismo tiempo): *"¡Acepto la oferta del Servidor X de la IP `.55`!"*.
4. **Acknowledge (Reconocimiento):** El servidor finaliza el proceso (ACK) y oficialmente alquila la IP a la PC, enviándole también la máscara, el router y el DNS.

---

## ☠️ Ataques de Capa Local (Ataques a DHCP)

DHCP tiene un problema de diseño grave: **no tiene autenticación incorporada por defecto**. Confía en todo el mundo.
Esto permite dos de los ataques de red local más comunes y peligrosos para el Pentesting (Red Team) y que el Blue Team debe detener:

### 1. DHCP Starvation (Inanición DHCP)
- *Ataque:* El atacante escribe un pequeño script (como la herramienta `Yersinia`) que se hace pasar por cientos de miles de PCs nuevas (falsificando las direcciones MAC usando MAC Spoofing, ver [[06 - Capa de Enlace de Datos y MAC (L2)|Nota 06]]). Envía miles de mensajes **Discover** por segundo.
- *Resultado:* El Servidor DHCP legítimo cree que se están conectando miles de usuarios nuevos y "alquila" todas las IPs de su Pool. Cuando se queda sin IPs, los usuarios legítimos reales no pueden conectarse a la red (Ataque de Denegación de Servicio - DoS).

### 2. Rogue DHCP Server (Servidor DHCP Falso / Man-in-the-Middle)
A menudo es la segunda parte del ataque anterior.
- *Ataque:* Una vez que el atacante "mató" al servidor DHCP real (o simplemente es más rápido que él), el atacante activa su propio software de Servidor DHCP malicioso (Rogue) en su laptop.
- *Resultado:* Cuando un usuario legítimo se conecta a la red y hace un grito **Discover**, el servidor falso del atacante responde primero (**Offer**). El atacante le da una IP válida, pero le dice a la víctima: *"Ah, por cierto, tu Puerta de Enlace (Router) soy YO, y tu servidor DNS también soy YO"*.
- *Impacto Final:* Todo el tráfico de Internet de la víctima pasa por la laptop del atacante antes de ir al router de verdad. El atacante puede leer contraseñas no cifradas, interceptar sesiones y hacer phishing perfecto falsificando el DNS. (Este es el peor escenario en una red LAN sin seguridad).

---

## 🛡️ Defensas (Mitigaciones)

Como profesionales, debemos mitigar esto en los Switches corporativos:
- **DHCP Snooping:** Es una tecnología en los Switches (Enterprise) donde un administrador configura que el Servidor DHCP real está, por ejemplo, conectado al Puerto 24. Si el Switch detecta que alguien en el Puerto 5 (donde hay un empleado o un atacante) intenta enviar un mensaje de Servidor (Offer o Acknowledge), el Switch bloquea el puerto inmediatamente. Previene los Rogue DHCP.

---

## 📌 Must Know (Imprescindible)
- El propósito de DHCP (asignar IP, máscara, gateway, DNS dinámicamente).
- Los 4 pasos del proceso DORA.
- Conceptos de los ataques Rogue DHCP y DHCP Starvation.

## 💡 Good to Know (Bueno saberlo)
- La IP asignada se llama "Lease" (arrendamiento) porque tiene un tiempo de expiración. Cuando llega a la mitad del tiempo de expiración, tu PC intenta contactar al servidor para renovarlo.
- Si una PC no puede conseguir una IP por DHCP (ej. por un ataque de Starvation o fallo del servidor), se asignará automáticamente a sí misma una IP "APIPA" (Rango `169.254.X.X`), con la cual no podrá navegar por internet.

---

## 🔄 Preguntas de repaso
1. En el proceso DORA, ¿cuál es el objetivo del primer paso (Discover) y cómo viaja ese mensaje en la red local (Unicast o Broadcast)?
2. Explicá cómo un atacante utiliza un servidor Rogue DHCP para lograr realizar un ataque de Man-in-the-Middle (Hombre en el medio).
3. (Integración) Si una red sufre un ataque de Rogue DHCP exitoso, ¿qué otro servicio que vimos en la nota anterior (Nota 19) probablemente sea falsificado también por el atacante para redirigir tráfico?

**➡️ Siguiente nota:** [[21 - HTTP y HTTPS Intro]]
