# Lab 02.1 - Análisis de Paquetes (Packet Analysis)

## 🎯 Objetivo
El objetivo de este laboratorio es que apliques mentalmente tus conocimientos sobre protocolos, capas OSI y puertos. Actuarás como un analista de SOC revisando transcripciones de capturas de red (`.pcap`) ficticias para identificar qué está pasando y si hay algún comportamiento malicioso.

---

## 📋 Prerrequisitos
- Haber leído todas las notas del Módulo 02, especialmente desde la 15 a la 27.
- No se requieren herramientas instaladas; analizarás los extractos de logs aquí provistos.

---

## 🛠️ Procedimiento (Tu Trabajo)

A continuación, se presentan tres escenarios de capturas de tráfico. Basándote en los datos de IPs y Puertos, deberás responder a las preguntas de cada escenario.

### Escenario 1: Tráfico en la Red Local
Estás revisando los logs del tráfico interno entre dispositivos de la misma oficina.
```text
Paquete 1:
Origen: 192.168.10.15
Destino: 192.168.10.255
Protocolo: ARP
Info: Who has 192.168.10.50? Tell 192.168.10.15

Paquete 2:
Origen: 192.168.10.50
Destino: 192.168.10.15
Protocolo: ARP
Info: 192.168.10.50 is at 00:1A:2B:3C:4D:5E
```
**Preguntas E1:**
1. En el Paquete 1, ¿el mensaje se envió en modo Unicast, Multicast o Broadcast?
2. ¿Por qué la máquina `192.168.10.15` generó el Paquete 1 en primer lugar? (¿Qué intentaba averiguar?).
3. Si inmediatamente después observas un "Paquete 3" idéntico al Paquete 2, pero proveniente de la IP `192.168.10.99` diciendo que la IP `.50` está en la MAC `FF:AA:BB:CC:DD:EE`... ¿qué ataque está ocurriendo muy probablemente?

### Escenario 2: Análisis Perimetral
Revisas los logs de conexión (Netflow) del Firewall perimetral de tu empresa.
```text
Conexión 1:
IP Origen: 10.0.5.10 | Puerto Origen: 55210
IP Destino: 8.8.8.8 | Puerto Destino: 53
Protocolo: UDP

Conexión 2:
IP Origen: 10.0.5.10 | Puerto Origen: 55211
IP Destino: 142.250.78.46 | Puerto Destino: 443
Protocolo: TCP
```
**Preguntas E2:**
4. En la Conexión 1, ¿qué servicio o protocolo de Capa 7 está intentando usar el usuario interno (`10.0.5.10`)? ¿Cómo lo sabes?
5. En la Conexión 2, el usuario está navegando por una página web (Puerto 443). ¿Podrás, como analista (usando Wireshark sobre esta conexión), leer el contenido de la página web o las contraseñas que envíe? ¿Por qué sí o por qué no?
6. ¿Los puertos de origen `55210` y `55211` a qué categoría de la IANA pertenecen (Well-Known, Registrados o Efímeros)?

### Escenario 3: Alarma de Seguridad (Sospecha de Brecha)
Tu Sistema de Detección de Intrusiones (IDS) emitió una alerta de "Posible Exfiltración de Datos o C2" sobre el tráfico de un servidor web interno (`172.16.0.50`). 
El Firewall permite a este servidor salir a internet *solo* para descargar actualizaciones (permitiendo HTTP/HTTPS, DNS y ping para diagnóstico).
```text
Tráfico anómalo detectado:
IP Origen: 172.16.0.50
IP Destino: 203.0.113.88 (Ubicada en Rusia)
Protocolo de Red: ICMP (Echo Request)
Tamaño del Payload (Datos adjuntos): 1400 Bytes
Frecuencia: 1 paquete cada 5 segundos durante 3 horas.
```
**Preguntas E3:**
7. El protocolo ICMP normalmente se usa para el comando `ping` y sus payloads (paquetes de datos) suelen ser muy pequeños (32 a 64 bytes). ¿Por qué el tamaño del Payload de 1400 bytes resulta extremadamente sospechoso en este escenario?
8. Basándote en lo aprendido en la nota [[14 - Protocolo ICMP]], ¿cómo se llama el ataque/técnica que el atacante está utilizando muy probablemente para saltarse el Firewall y extraer datos de la empresa hacia Rusia?

---

## 📝 Resultado Esperado (Autoevaluación)

Revisá tus respuestas con este análisis sugerido.

> [!example]- Ver posibles respuestas
> **Escenario 1 (E1):**
> 1. Broadcast (La IP de destino termina en `.255`, es la dirección de broadcast de la subred).
> 2. La máquina `.15` quería comunicarse con la `.50`, pero solo conocía su dirección IP (Capa 3). Necesitaba descubrir su dirección física MAC (Capa 2) para poder enviarle la Trama.
> 3. Ataque de ARP Spoofing (Envenenamiento ARP). La máquina `.99` está mintiendo e intentando redirigir el tráfico hacia su propia dirección MAC para realizar un Man-in-the-Middle.
> 
> **Escenario 2 (E2):**
> 4. DNS. Se sabe porque está usando el Puerto Destino 53 en UDP.
> 5. No. El puerto 443 es HTTPS (TLS), lo que significa que la conexión de Capa 7 está cifrada de punto a punto. En Wireshark solo verías tráfico ilegible.
> 6. Efímeros (Dinámicos). Son creados temporalmente por el sistema operativo del cliente para la conexión.
> 
> **Escenario 3 (E3):**
> 7. Un ping normal (ICMP Echo Request) no necesita llevar 1400 bytes de datos adjuntos. 1400 bytes de datos por paquete de manera constante indica que están "transfiriendo" archivos o información de forma oculta (Exfiltración).
> 8. ICMP Tunneling. El atacante esconde datos robados dentro del espacio "vacío" de los paquetes de Ping, sabiendo que el Firewall de la empresa permite que los Pings salgan hacia Internet sin restricciones.

**➡️ Siguiente nota:** [[29 - Laboratorio 2 - Diseño de Red]]
