# 26 - Herramientas de Red en CLI (Línea de Comandos)

## 🎯 Objetivos
- Aprender a utilizar comandos de red básicos e indispensables para diagnóstico y reconocimiento.
- Saber cómo encontrar tu propia IP, probar conectividad, rastrear rutas y consultar DNS.

---

## 🛠️ El kit básico del profesional (y del atacante)

Todo sistema operativo (Windows, Linux, macOS) trae estas herramientas integradas. Son la base para solucionar problemas de IT (Troubleshooting) y para la fase de Reconocimiento (Reconnaissance) en un Pentest.

*(Los ejemplos a continuación están orientados a Linux, pero los comandos son casi idénticos en Windows Command Prompt).*

### 1. `ip addr` / `ifconfig` / `ipconfig` (¿Quién soy?)
Te muestra la configuración de red actual de tus tarjetas de red (Interfaces).
- **Linux moderno:** `ip a` o `ip addr show`
- **Linux antiguo/macOS:** `ifconfig`
- **Windows:** `ipconfig` o `ipconfig /all` (para ver también la MAC y el DNS).
- *Qué buscar:* Tu IP Privada (Capa 3) y tu Dirección MAC (Capa 2).

### 2. `ping` (¿Estás vivo?)
Utiliza el protocolo [[14 - Protocolo ICMP|ICMP]] para enviar mensajes *Echo Request* al objetivo.
- **Uso:** `ping google.com` o `ping 192.168.1.1`
- **¿Qué te dice?** 
  - Si responde, la PC está encendida, conectada, y su Firewall L3 no está bloqueando ICMP.
  - El "Time" (ms): Cuánto tarda el paquete en ir y volver (Latencia). Si es `1ms` está en tu misma LAN. Si es `200ms` está en otro continente.
  - El "TTL" (Time To Live): Ayuda a estimar cuántos routers cruzó el paquete y a adivinar el Sistema Operativo del objetivo.
  - *Dato:* En Windows, `ping` envía 4 paquetes y se detiene. En Linux, no se detiene nunca (hasta que presiones `Ctrl+C`).

### 3. `traceroute` / `tracert` (¿Por dónde estoy yendo?)
Muestra el camino exacto (Router por Router, "salto a salto") que toman los paquetes desde tu PC hasta el destino en Internet.
- **Linux/macOS:** `traceroute google.com`
- **Windows:** `tracert google.com`
- *Uso en Seguridad:* Sirve para mapear la arquitectura de la red de una empresa desde afuera, descubriendo las IPs de sus routers y firewalls perimetrales ocultos. 

### 4. `dig` / `nslookup` (Preguntas de DNS)
Te permiten consultar manualmente al servidor [[19 - DNS en Profundidad|DNS]] para obtener los registros de un dominio.
- **`nslookup google.com`** (Básico, funciona en Windows/Linux).
- **`dig google.com`** (Avanzado, estándar en Linux/macOS).
- *Uso avanzado (dig):* Podés pedirle registros específicos. Ej: `dig mx google.com` te mostrará cuáles son los servidores que manejan los correos de Google. Los atacantes lo usan intensivamente para encontrar subdominios ocultos.

### 5. `netstat` / `ss` (¿Quién está conectado a mí?)
Muestra todas las conexiones de red activas (L4) de tu computadora, es decir, todos los [[17 - Puertos y Sockets|Sockets]] abiertos.
- **Windows:** `netstat -ano` (Muestra conexiones, IPs y el PID del programa que las abrió).
- **Linux moderno:** `ss -tulpn` (Muestra puertos TCP/UDP a la escucha y el programa).
- *Uso en Seguridad:* El Blue Team, o un analista de malware, ejecuta este comando en un servidor para ver si hay algún puerto "raro" abierto, o si la PC está conectada a la IP de un servidor de cibercrimen (C2) en Rusia.

### 6. `arp` (¿Quién es quién en mi LAN?)
Muestra la Tabla ARP de tu computadora (la traducción entre IPs y MACs de la red local, ver [[07 - Switches y ARP|Nota 07]]).
- **Comando:** `arp -a`
- *Uso en Seguridad:* Si ves que la IP del Router de tu casa y la IP de la computadora de tu hermano misteriosamente tienen anotada *la misma dirección MAC*, estás sufriendo un ataque de ARP Spoofing (tu hermano te está interceptando el tráfico).

---

## 📌 Must Know (Imprescindible)
Saber para qué sirve y qué información devuelve cada uno de los 6 comandos listados arriba. 

## 💡 Good to Know (Bueno saberlo)
Existe una herramienta hiper-poderosa llamada **`nmap`** (Network Mapper). Es la navaja suiza del escaneo de puertos. No es nativa del SO (hay que instalarla), pero es la herramienta ofensiva más usada para mapear redes y descubrir puertos abiertos. La estudiaremos a fondo en el [[12 - Offensive Security/00 - Overview|Módulo 12]].

---

## 🔄 Preguntas de repaso
1. Si un usuario se queja de que una página web no le carga, y al hacerle un `ping` obtenes "Tiempo de espera agotado", pero al hacer un `nslookup` de esa página obtenés una IP correctamente. ¿Qué protocolo funciona y qué protocolo está bloqueado/fallando?
2. Entraste por consola a un servidor Linux que sospechás que está infectado con malware. ¿Qué comando utilizarías para ver qué puertos TCP tiene abiertos y escuchando en ese momento?
3. Para Windows, ¿cuál es la diferencia entre los comandos `ipconfig` y `tracert`?

**➡️ Siguiente nota:** [[27 - Análisis de Tráfico (Wireshark y tcpdump)]]
