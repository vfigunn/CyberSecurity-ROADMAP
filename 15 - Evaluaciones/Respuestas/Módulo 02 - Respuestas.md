# Respuestas Evaluación Módulo 02 - Networking

A continuación se presentan las respuestas correctas de la evaluación del [[02 - Networking/31 - Evaluación|Módulo 02]], junto con la justificación técnica de cada una.

---

### Sección 1: Modelo OSI y TCP/IP

**1. B) Capa 3 (Red)**
> *Justificación:* Las direcciones IP (ej. `185.34.20.11`) pertenecen al protocolo de Internet (IP), el cual opera en la Capa 3. Bloquear basándose en una IP es aplicar una regla de L3.

**2. D) Trama (Frame)**
> *Justificación:* En la Capa 1 son Bits, en la Capa 2 son Tramas (Frames), en la Capa 3 son Paquetes, y en la Capa 4 son Segmentos (o Datagramas).

**3. A) 3-Way Handshake (SYN -> SYN-ACK -> ACK)**
> *Justificación:* TCP establece conexiones mediante el Three-Way Handshake, enviando un segmento de sincronización (SYN), recibiendo una confirmación (SYN-ACK) y cerrando el acuerdo (ACK). DORA es para DHCP.

---

### Sección 2: Protocolos, Puertos y Sockets

**4. C) TCP / 22**
> *Justificación:* SSH (Secure Shell) utiliza TCP en el puerto 22 de forma predeterminada para conexiones cifradas. El puerto 23 es el obsoleto e inseguro Telnet.

**5. D) FTP**
> *Justificación:* FTP (Puertos 20 y 21) no posee cifrado nativo. Transfiere todo el contenido, incluidos usuarios y contraseñas de inicio de sesión, en texto claro. SFTP o FTPS son las alternativas seguras.

**6. C) Resolver (descubrir) la dirección MAC asociada a una dirección IP conocida.**
> *Justificación:* Address Resolution Protocol se encarga de preguntar (mediante broadcast) "¿Quién tiene la IP X?" para obtener la MAC destino de esa máquina y poder ensamblar la Trama en la red local.

---

### Sección 3: Arquitectura, Seguridad y Subnetting

**7. C) Privada (Clase C)**
> *Justificación:* El rango `192.168.0.0` a `192.168.255.255` está reservado por el RFC 1918 para redes locales privadas de Clase C.

**8. C) VLANs (Virtual LANs)**
> *Justificación:* Las VLANs dividen un switch físico en múltiples lógicos. Un broadcast en una VLAN no se propaga a las demás, mejorando rendimiento y aislando el tráfico por seguridad (segmentación).

**9. D) Evil Twin (Gemelo Malvado)**
> *Justificación:* El Evil Twin es un ataque activo donde el atacante levanta una red Wi-Fi idéntica pero con mayor potencia para engañar a los dispositivos de las víctimas y forzarlos a conectarse a él.

**10. C) El IDS es un control detectivo que alerta sobre ataques pasivamente; el IPS es un control preventivo que puede destruir/bloquear el tráfico malicioso in-line.**
> *Justificación:* IDS (Intrusion Detection System) detecta y avisa (copia de tráfico). IPS (Intrusion Prevention System) corta el tráfico bloqueando activamente la intrusión en línea.

---

### Sección 4: Análisis de Escenario / Comandos CLI

**11. C) La PC no pudo contactar a un Servidor DHCP y se autoasignó una dirección APIPA (Link-Local).**
> *Justificación:* Cuando una PC configurada para obtener IP dinámica no puede comunicarse con el DHCP (por cable desconectado o DHCP caído), el sistema operativo Windows se autoasigna una IP en el rango `169.254.x.x` (APIPA).

**12. C) `netstat` (o `ss`)**
> *Justificación:* `netstat -tulpn` o `ss -tulpn` en Linux muestra todos los Sockets abiertos (Puertos escuchando o conexiones activas) y el PID del programa que es dueño de ese puerto. Es fundamental en la caza de amenazas.
