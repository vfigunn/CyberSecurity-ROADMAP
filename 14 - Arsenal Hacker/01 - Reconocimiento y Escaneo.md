# 01 - Reconocimiento y Escaneo

Esta categoría agrupa las herramientas encargadas de mapear la superficie de ataque inicial. Sirven para descubrir puertos abiertos, servicios en ejecución, y descubrir IPs o subdominios escondidos.

---

### 1. Nmap (Network Mapper)
- **¿Qué es?:** El escáner de puertos más famoso de la historia. Mapea la red, descubre servicios, sistemas operativos y vulnerabilidades.
- **Comando Básico (Escaneo Rápido):**
  `nmap -sV -sC -p- <IP>` *(Escanea todos los puertos, detecta versiones y usa scripts por defecto).*
- **Comando Sigiloso (Stealth):**
  `nmap -sS -T2 <IP>` *(Escaneo TCP SYN con velocidad lenta para evadir Firewalls/IDS).*

### 2. Masscan
- **¿Qué es?:** Escáner de puertos asíncrono ultra-rápido. Puede escanear Internet entero en 6 minutos. Se usa para mapear redes corporativas masivas gigantes (miles de IPs) donde Nmap tardaría horas.
- **Comando Básico:**
  `masscan -p1-65535,U:1-65535 <IP_o_Rango> --rate=1000` *(Escanea TCP/UDP a 1000 paquetes por segundo).*

### 3. RustScan
- **¿Qué es?:** Una alternativa hiper-veloz a Nmap escrita en el lenguaje Rust. Busca puertos abiertos en 3 segundos y automáticamente se los pasa a Nmap para que haga el análisis profundo.
- **Comando Básico:**
  `rustscan -a <IP> -- -sC -sV` *(El guion doble `--` le pasa los parámetros clásicos a Nmap).*

### 4. Netcat (nc)
- **¿Qué es?:** La "Navaja Suiza" de TCP/IP. Sirve para leer, escribir datos en la red, probar puertos y, lo más importante, crear y recibir **Reverse Shells**.
- **Recibir Reverse Shell (Atacante):**
  `nc -lvnp 4444` *(Pone a la máquina atacante en Modo Escucha en el puerto 4444).*
- **Conexión de prueba a puerto:**
  `nc -vz <IP> 80` *(Avisa si el puerto 80 de esa IP está abierto).*

### 5. Amass (OWASP)
- **¿Qué es?:** Herramienta definitiva para recolección de información sobre la superficie de ataque DNS (Mapeo de dominios de una empresa).
- **Comando Básico:**
  `amass enum -d empresa.com` *(Busca todos los subdominios de la empresa consultando cientos de bases de datos externas).*

### 6. Sublist3r
- **¿Qué es?:** Herramienta rápida en Python diseñada puramente para enumerar subdominios de forma veloz usando motores de búsqueda como Baidu, Yahoo, Google, y Bing.
- **Comando Básico:**
  `sublist3r -d empresa.com -t 100` *(Busca subdominios de empresa.com usando 100 hilos de conexión).*

### 7. Ping / Fping
- **¿Qué es?:** Utilidad del protocolo ICMP para comprobar si una máquina está "viva" y respondiendo en la red.
- **Comando Básico (Ping Sweep):**
  `fping -a -g 192.168.1.0/24` *(Hace ping a las 254 computadoras de la red interna y solo te muestra las que respondan).*

**➡️ Siguiente nota:** [[02 - Descubrimiento y Web]]
