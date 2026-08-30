# 31 - Evaluación del Módulo 02 (Networking)

## 📝 Instrucciones
Evaluación final del módulo. Múltiples preguntas basadas en escenarios técnicos y conocimientos "must-know".
Al igual que en el módulo anterior, anota tus respuestas y luego verificalas con el solucionario.

> **Importante:** Las respuestas correctas y explicaciones están en: `[[29 - Evaluaciones/Respuestas/Módulo 02 - Respuestas]]`.

---

## 🎯 Sección 1: Modelo OSI y TCP/IP

**1. Un analista de seguridad bloquea exitosamente un ataque denegando el tráfico entrante desde la dirección `185.34.20.11`. ¿En qué capa del Modelo OSI aplicó principalmente esta mitigación?**
A) Capa 2 (Enlace de Datos)
B) Capa 3 (Red)
C) Capa 4 (Transporte)
D) Capa 7 (Aplicación)

**2. ¿Cuál es el nombre correcto de la Unidad de Datos del Protocolo (PDU) en la Capa 2 del modelo OSI?**
A) Paquete (Packet)
B) Segmento (Segment)
C) Datagrama (Datagram)
D) Trama (Frame)

**3. El protocolo TCP asegura una conexión mediante un proceso inicial. ¿Cómo se conoce este proceso y cuál es su secuencia correcta de banderas (Flags)?**
A) 3-Way Handshake (SYN -> SYN-ACK -> ACK)
B) Proceso DORA (Discover -> Offer -> Request -> Acknowledge)
C) 3-Way Handshake (ACK -> SYN -> SYN-ACK)
D) ARP Request (Request -> Reply -> ACK)

---

## 🎯 Sección 2: Protocolos, Puertos y Sockets

**4. ¿Qué combinación de protocolo y número de puerto es correcta para el servicio de administración segura de terminales por red (Secure Shell)?**
A) TCP / 23
B) UDP / 53
C) TCP / 22
D) TCP / 443

**5. Identificá qué protocolo de capa de aplicación, utilizado para transferir archivos, es inherentemente inseguro porque envía las credenciales en texto plano.**
A) SFTP
B) HTTPS
C) SSH
D) FTP

**6. ¿Qué función cumple el protocolo ARP en una red local (LAN)?**
A) Traducir nombres de dominio legibles a direcciones IP.
B) Proveer direcciones IP de forma dinámica a los hosts que se conectan (DHCP).
C) Resolver (descubrir) la dirección MAC asociada a una dirección IP conocida.
D) Interceptar tráfico malicioso buscando firmas de virus.

---

## 🎯 Sección 3: Arquitectura, Seguridad y Subnetting

**7. La dirección `192.168.1.5` es una dirección:**
A) Pública.
B) Privada (Clase A).
C) Privada (Clase C).
D) Loopback (Localhost).

**8. ¿Cuál de las siguientes tecnologías segmenta físicamente/lógicamente un Switch, rompiendo los dominios de Broadcast y aumentando la seguridad interna de la red?**
A) NAT (Network Address Translation)
B) BGP (Border Gateway Protocol)
C) VLANs (Virtual LANs)
D) IPv6

**9. ¿Qué ataque específico de red inalámbrica (Wi-Fi) consiste en crear un Punto de Acceso (Access Point) falso con el mismo nombre (SSID) que la red legítima para engañar a los usuarios y que se conecten a él?**
A) WEP Cracking
B) MAC Flooding
C) DHCP Starvation
D) Evil Twin (Gemelo Malvado)

**10. En el contexto de los dispositivos de seguridad, ¿cuál es la diferencia principal entre un Sistema de Detección de Intrusiones (IDS) y un Sistema de Prevención de Intrusiones (IPS)?**
A) El IDS solo escanea virus, el IPS escanea ataques de red.
B) El IDS opera in-line (en la línea de tráfico), el IPS opera fuera de línea pasivamente.
C) El IDS es un control detectivo que alerta sobre ataques pasivamente; el IPS es un control preventivo que puede destruir/bloquear el tráfico malicioso in-line.
D) No hay diferencia, son dos nombres para la misma tecnología Firewall de próxima generación (NGFW).

---

## 🎯 Sección 4: Análisis de Escenario / Comandos CLI

**Escenario:** Sos el administrador de red y un empleado se queja de que no tiene acceso a internet. 
Ejecutas el comando `ipconfig` en su PC y ves el siguiente resultado:
- Dirección IP: `169.254.12.80`
- Máscara de subred: `255.255.0.0`
- Puerta de Enlace predeterminada: (Vacio)

**11. Basándote en el resultado de `ipconfig`, ¿cuál es el diagnóstico más probable del problema?**
A) El servidor DNS de Internet (Google) se ha caído.
B) La PC de la víctima sufrió un ataque de ARP Spoofing.
C) La PC no pudo contactar a un Servidor DHCP y se autoasignó una dirección APIPA (Link-Local).
D) La PC está utilizando una dirección IP pública en lugar de una privada.

**12. En una investigación (Threat Hunting), usás un sniffer como Wireshark y ves gran cantidad de tráfico originado en un servidor web de la empresa (que usa Linux) saliendo por el puerto TCP `4444` hacia una IP en un país extranjero. ¿Qué herramienta de línea de comandos en ese servidor Linux usarías primero para ver qué programa/proceso específico abrió ese puerto?**
A) `ping`
B) `traceroute`
C) `netstat` (o `ss`)
D) `nslookup`

---

> **¿Terminaste?**
> Buscá el archivo en la carpeta de evaluaciones para comparar tus resultados (Módulo 02 - Respuestas). Si aprobás holgadamente, preparate para ensuciarte las manos con la terminal en los módulos de Sistemas Operativos.

**➡️ Siguiente nota:** [[32 - Resumen]]
