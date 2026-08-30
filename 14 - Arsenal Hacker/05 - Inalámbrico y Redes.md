# 05 - Inalámbrico y Redes

Estas herramientas operan puramente sobre las tarjetas de red de tu computadora (Modo Promiscuo y Modo Monitor). Su función es interceptar y modificar el tráfico que viaja por los cables o por el aire (Ondas Wi-Fi).

---

### 29. Aircrack-ng (La Suite Wi-Fi)
- **¿Qué es?:** El conjunto de herramientas rey para atacar redes WPA2 (Ver Nota 03 del Módulo 12).
- **Herramientas Claves de la Suite:**
  - `airmon-ng start wlan0` *(Pone tu placa en Modo Monitor espía).*
  - `airodump-ng wlan0mon` *(El radar de redes y capturador de Handshakes).*
  - `aireplay-ng -0 10 -a <MAC_Router> wlan0mon` *(Patea/Desconecta a todos los usuarios de la red disparando Deauths).*
  - `aircrack-ng captura.cap -w rockyou.txt` *(Crackea el Handshake).*

### 30. Wireshark
- **¿Qué es?:** El analizador de protocolos de red gráfico más utilizado del mundo. Captura paquetes en tiempo real y te permite ver hasta el último bit del tráfico HTTP, DNS o TCP que entra a tu PC. Indispensable para Blue Team y Troubleshooting.
- **Uso:** (Interfaz Gráfica - GUI).

### 31. Tcpdump
- **¿Qué es?:** El hermano de consola de Wireshark. Cuando hackeás un servidor Linux remoto (que no tiene interfaz gráfica), no podés abrir Wireshark. Usás Tcpdump por consola para espiar qué hace ese servidor.
- **Comando Básico:**
  `tcpdump -i eth0 -w captura.pcap` *(Captura todo el tráfico de la placa eth0 y lo guarda en un archivo que luego podés abrir en Wireshark).*

### 32. Bettercap
- **¿Qué es?:** La pesadilla de las redes locales. El sucesor moderno de Ettercap. Sirve para hacer ataques "Man-In-The-Middle" masivos. Envenena el protocolo ARP, bloquea páginas web e inyecta Javascript malicioso a todos los celulares conectados a tu misma red Wi-Fi de forma transparente.
- **Comando Básico (Lanzar consola interactiva):**
  `bettercap -iface eth0`

### 33. Kismet
- **¿Qué es?:** Un radar pasivo para Hacking Inalámbrico. A diferencia de airodump, Kismet tiene una interfaz web preciosa, dibuja mapas, y no solo detecta Wi-Fi, sino también Drones, Bluetooth y dispositivos ocultos. (Totalmente indetectable porque no envía paquetes).
- **Comando Básico:**
  `kismet -c wlan0mon`

### 34. Macchanger
- **¿Qué es?:** Herramienta táctica vital. Modifica la dirección MAC (La identidad física) de tu placa de red. Se usa antes de atacar para no dejar rastros de tu fabricante, o para by-passear redes de hoteles que te dan 30 minutos gratis por cada MAC conectada.
- **Comandos Básicos:**
  1. `ifconfig eth0 down` *(Apaga la placa).*
  2. `macchanger -r eth0` *(Le asigna una MAC falsa aleatoria).*
  3. `ifconfig eth0 up` *(La vuelve a prender).*

### 35. Wifiphisher
- **¿Qué es?:** Herramienta ofensiva para montar Puntos de Acceso Falsos (Rogue AP). Automatiza completamente el ataque *Evil Twin* (Gemelo Malvado), cortando la red original y levantando un clon para pescar la contraseña mediante un portal web falso.
- **Comando Básico:**
  `wifiphisher -aI wlan0mon`

**➡️ Siguiente nota:** [[06 - OSINT e Ingeniería Social]]
