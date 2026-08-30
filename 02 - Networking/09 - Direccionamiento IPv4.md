# 09 - Direccionamiento IPv4

## 🎯 Objetivos
- Comprender la estructura y notación de una dirección IPv4.
- Diferenciar entre IPs Públicas y Privadas.
- Conocer direcciones especiales como Loopback y APIPA.

---

## 🧠 Concepto

Una **dirección IPv4** es el identificador lógico de un dispositivo en una red que utiliza el protocolo TCP/IP.
Al igual que un número de teléfono, debe ser único (al menos en el contexto de la red donde está operando).

### Formato de IPv4
- Una dirección IPv4 es en realidad un número binario larguísimo de **32 bits** (ceros y unos).
- Como los humanos somos malos leyendo 32 unos y ceros seguidos, dividimos la dirección en cuatro bloques de 8 bits (llamados "Octetos").
- Convertimos cada octeto de binario a decimal (0-255) y los separamos por un punto. Esto se llama *notación decimal separada por puntos*.
- **Ejemplo:** `192.168.1.50`

La dirección siempre se compone de dos partes lógicas:
1. **La porción de Red (Network portion):** Identifica a qué red o "calle" pertenece el dispositivo.
2. **La porción de Host (Host portion):** Identifica al dispositivo específico "número de casa" dentro de esa calle.
(Cómo sabemos qué parte es cuál lo veremos en la [[10 - Subnetting y CIDR|próxima nota]] con la Máscara de Subred).

---

## 🌍 IPs Públicas vs. IPs Privadas (RFC 1918)

Este es un concepto fundamental en infraestructura y seguridad.

En los inicios de Internet, cada computadora que se conectaba recibía una IP única. Con 32 bits, IPv4 permite un máximo de ~4.3 mil millones de direcciones. En los 90s, los ingenieros se dieron cuenta de que las direcciones se iban a agotar rápido (hoy en día todos tenemos celular, PC, TV inteligente, etc.).

Para ganar tiempo, crearon el concepto de **Direcciones IP Privadas (RFC 1918)**.
Separaron ciertos rangos de IPs que están "prohibidos" en Internet. **Ningún router de Internet (WAN) enrutará jamás un paquete que tenga una IP privada como destino u origen.** 

Las IPs Privadas se usan *solamente* para redes internas (LANs). Vos podés usar la red `192.168.1.0` en tu casa, y tu vecino también, y Google en sus oficinas internas también. Como nunca se cruzan en el Internet público, no hay conflicto (son reutilizables).

### Rangos de IPs Privadas (Memoria obligatoria):
- **Clase A:** `10.0.0.0` a `10.255.255.255` (Suele usarse en redes corporativas inmensas).
- **Clase B:** `172.16.0.0` a `172.31.255.255` (Redes empresariales medianas/grandes, Docker, Cloud).
- **Clase C:** `192.168.0.0` a `192.168.255.255` (Típica red de tu casa u oficina pequeña).

**IPs Públicas:** Cualquier IP que *no* caiga en los rangos privados (y algunos especiales) es pública, y debe ser comprada/alquilada y ser globalmente única. (Ej: La IP de los servidores de Google `8.8.8.8`).

> *Nota mental:* Si tu PC tiene una IP privada y las IPs privadas no pueden viajar por Internet... ¿Cómo es que podés leer esta nota en Internet? La respuesta es [[13 - NAT y PAT|NAT]], que veremos más adelante.

---

## 🛑 Direcciones IPv4 Especiales

Además de las privadas, existen direcciones reservadas para funciones del sistema:

1. **Loopback (`127.0.0.1` a `127.255.255.255`):** 
   - Se utiliza para que la computadora "hable consigo misma" y pruebe su propia tarjeta de red.
   - En ciberseguridad se la llama genéricamente **`localhost`** (`127.0.0.1`). Si estás desarrollando una web en tu PC y querés verla, accedés a tu localhost.
2. **Ruta por defecto (`0.0.0.0`):**
   - Generalmente significa "Cualquier red" o "Cualquier dirección IP". En seguridad, si un servicio en un servidor está escuchando en `0.0.0.0`, significa que está aceptando conexiones desde cualquier tarjeta de red y desde cualquier parte del mundo.
3. **APIPA o Link-Local (`169.254.x.x`):**
   - Si encendés tu computadora y no puede contactar al servidor que le asigna IPs automáticamente ([[20 - DHCP|DHCP]]), Windows (y otros) se autoasignan una IP que empieza con `169.254`. Si ves esta IP, significa que *hay un problema en la red*. No sirve para navegar por Internet.

---

## ❓ ¿Por qué importa en Seguridad?

Los atacantes (y los analistas) usan direcciones IPs constantemente.
- **Enumeración/Escaneo:** En una auditoría interna (Pentest), verás IPs 10.x.x.x. Sabés que estás dentro.
- **Firewall Rules:** Los Blue Teams escriben reglas de firewall bloqueando rangos específicos de IPs públicas sospechosas. (Ej. Bloquear todas las IPs asignadas geográficamente a un país).
- **Riesgo:** Exponer un servicio interno en una IP Pública es uno de los mayores errores. Una base de datos debe tener *siempre* una IP Privada para que nadie desde Internet pueda alcanzarla directamente.

---

## 📌 Must Know (Imprescindible)
- Formato IPv4 (32 bits, 4 octetos).
- Saber de memoria los 3 rangos de IPs Privadas.
- La IP `127.0.0.1` (localhost).

## 💡 Good to Know (Bueno saberlo)
- La "Clase" (A, B, C) es un concepto heredado (Classful networking) que técnicamente ya no se usa gracias a CIDR, pero la industria sigue utilizando los términos para referirse coloquialmente a los tamaños de las redes privadas.

---

## 🔄 Preguntas de repaso
1. Identificá si la dirección `8.8.4.4` es pública o privada, y justificá por qué.
2. Un empleado se queja de que no tiene internet. Al hacer un `ipconfig` en su PC, ves que su IP es `169.254.30.15`. ¿Qué significa esta dirección y cuál es el problema probable?
3. ¿Por qué se crearon las direcciones IP Privadas en RFC 1918?

**➡️ Siguiente nota:** [[10 - Subnetting y CIDR]]
