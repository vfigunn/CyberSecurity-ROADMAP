# 32 - Resumen (Cheat Sheet - Networking)

Esta nota condensa los conceptos fundamentales de conectividad, indispensables para comprender cualquier ataque cibernético.

---

## 🏗️ Modelos OSI y TCP/IP

- **Capa 1 (Física):** Pasa Bits. Medios: Cobre, Fibra (segura, sin EMI), Inalámbrico (fácil de interceptar).
- **Capa 2 (Enlace):** Pasa Tramas (Frames). Direccionamiento físico local (MAC Address). Protocolos: Ethernet, Wi-Fi. 
- **Capa 3 (Red):** Pasa Paquetes (Packets). Direccionamiento lógico global (IP) y Enrutamiento (Routers).
- **Capa 4 (Transporte):** Pasa Segmentos. Comunicación Proceso a Proceso mediante **Puertos**. (TCP/UDP).
- **Capa 7 (Aplicación):** Pasa Datos. Protocolos: HTTP, DNS, DHCP, SSH. (En OSI, la 7 incluye a TCP/IP aplicación).

---

## 🌐 Conceptos de Red L2 / L3

- **Dirección MAC:** Única de hardware, 48 bits, hexadecimal. (Válida solo en la LAN).
- **ARP (Address Resolution Protocol):** Traduce una IP conocida a la MAC desconocida del equipo en la LAN. (Vulnerable a *ARP Spoofing/Man-in-the-Middle*).
- **IPv4:** 32 bits (4 octetos decimales). 
  - **IPs Privadas:** `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`. (No enrutables en Internet público).
  - **Localhost:** `127.0.0.1`.
- **IPv6:** 128 bits (hexadecimal). Soluciona el agotamiento de IPs, incluye IPsec nativo (seguridad).
- **Subnetting (Máscara / CIDR):** Divide una red grande en más chicas (Ej. `/24` = 254 IPs usables). Segmentar por seguridad confina el tráfico.
- **NAT / PAT:** Traduce IPs privadas a una IP pública única para navegar por Internet. *NAT no es un firewall ni un control de seguridad*.
- **ICMP:** Protocolo de diagnóstico (Capa 3). Herramientas: `ping`, `traceroute`. A menudo bloqueado por firewalls para evitar el escaneo (Reconocimiento).

---

## 🆚 La Batalla Capa 4 (TCP vs UDP)

- **TCP:** Fiable, orientado a conexión, garantiza entrega (retransmite perdidos), usa ACKs. *Ej: Web, SSH, Correo, FTP*.
  - **3-Way Handshake:** `SYN` --> `SYN-ACK` --> `ACK`.
- **UDP:** Rápido, no fiable, sin conexión previa. Si pierde datos, no importa. *Ej: Streaming video, DNS, Juegos Online*.

---

## 🚪 Puertos Obligatorios (Aprender de memoria)
- **Web:** `80` (HTTP, Inseguro) | `443` (HTTPS/TLS, Seguro).
- **Admin:** `22` (SSH, Seguro) | `23` (Telnet, Inseguro) | `3389` (RDP Windows).
- **Correo:** `25` (SMTP/Envío) | `110` (POP3) | `143` (IMAP/Recibo moderno).
- **Archivos:** `20/21` (FTP, Inseguro).
- **Red:** `53` (DNS) | `67/68` (DHCP).
- **Socket:** Combinación de IP + Puerto (ej. `192.168.1.10:443`).

---

## 🔒 Estructura y Seguridad de Red

- **Switches vs Routers:** Switch (L2) conecta PCs en la misma red (LAN); Router (L3) conecta diferentes redes (WAN).
- **VLANs (Virtual LAN):** Divide lógicamente un switch para separar departamentos (Ej. Wi-Fi Invitados vs Servidores). Mejora rendimiento (rompe dominios broadcast) y seguridad (fuerza a pasar por el firewall).
  - *Puerto Trunk:* Pasa tráfico de múltiples VLANs (Etiquetadas 802.1Q).
- **DHCP:** Proceso DORA (Discover, Offer, Request, ACK). Asigna IPs automáticamente. (Vulnerable a Rogue DHCP / Inanición).
- **DNS:** Traduce dominios a IPs (Ej. `google.com` -> `8.8.8.8`). Funciona en árbol jerárquico (Root -> TLD -> Autoritativo).

### Dispositivos Defensivos
- **Firewall L3/L4 (Stateful):** Bloquea basado en IP y Puertos. Tiene memoria de conexiones permitidas.
- **NGFW (Capa 7):** Firewall de Próxima Generación. Entiende aplicaciones y hace inspección profunda (DPI).
- **IDS (Detectivo):** Analiza tráfico y dispara alertas (alarma pasiva).
- **IPS (Preventivo):** Analiza tráfico "In-Line" y destruye el paquete si detecta ataque (activo).
- **Wireshark / tcpdump:** Herramientas para capturar y analizar tráfico crudo de red (`.pcap`) usando la tarjeta en Modo Promiscuo (Sniffing).

---
🎉 **¡Felicitaciones por completar el hipercrítico Módulo 02!**
Actualizá tu archivo [[Progreso]] y avanzá hacia el [[03 - Linux/00 - Overview|Módulo 03 - Linux]].
