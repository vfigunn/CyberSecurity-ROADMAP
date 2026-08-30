# 17 - Puertos y Sockets

## 🎯 Objetivos
- Entender qué es un puerto lógico en la Capa 4 y su numeración.
- Conocer los puertos "Well-Known" más importantes que un profesional de seguridad debe saber de memoria.
- Comprender qué es un Socket.

---

## 🧠 Concepto: Los Puertos

Un **Puerto (Port)** en redes no es un agujero físico en la parte de atrás de tu PC. Es un número de 16 bits (o sea, del 0 al 65535) que el sistema operativo utiliza para dirigir el tráfico de red (que ya llegó a la PC gracias a la IP) a la aplicación de software correcta.

- TCP tiene 65,535 puertos disponibles.
- UDP tiene *otros* 65,535 puertos disponibles. (El Puerto 53 TCP no es el mismo que el Puerto 53 UDP, aunque a menudo se usen para el mismo servicio, como DNS).

La IANA (Internet Assigned Numbers Authority) es la organización mundial que divide estos puertos en tres grandes rangos oficiales:

1. **Puertos Well-Known (Conocidos) o de Sistema:** Del **0 al 1023**. 
   - Están reservados para los servicios más comunes de Internet. 
   - *Seguridad:* En sistemas Unix/Linux (por diseño de seguridad antigua), solo un administrador (usuario `root`) tiene permisos para iniciar un programa que escuche en un puerto por debajo del 1024.
2. **Puertos Registrados (Registered):** Del **1024 al 49151**.
   - Los utilizan empresas para sus aplicaciones específicas (Ej: Microsoft usa el 3389 para RDP, bases de datos como MySQL usan el 3306).
3. **Puertos Efímeros (Dinámicos / Privados):** Del **49152 al 65535**.
   - Estos no están asignados a ningún servidor permanente. Los sistemas operativos de los clientes (como tu PC) los usan aleatoriamente (al vuelo) como **Puertos de Origen** cuando inician una conexión hacia un servidor. Una vez terminada la conexión, el puerto se libera.

---

## 🔌 El concepto de Socket

Un **Socket** es simplemente la combinación matemática y funcional de una **Dirección IP** (L3) + un **Puerto** (L4) + el **Protocolo** (TCP o UDP). Es el "enchufe" completo de la comunicación.

> **Ejemplo de Comunicación (Cliente a Servidor)**
> Tu PC (Cliente) quiere entrar a la web segura de tu banco (Servidor).
> 
> - **IP Origen:** `190.50.20.10` (Tu IP pública).
> - **Puerto Origen:** `51234` (Un puerto efímero elegido al azar por Windows para Google Chrome).
> - **Protocolo:** TCP
> - ***Socket Origen:*** `190.50.20.10:51234 (TCP)`
> --- 
> - **IP Destino:** `8.8.4.4` (La IP del Banco).
> - **Puerto Destino:** `443` (El puerto "Well-Known" para HTTPS).
> - **Protocolo:** TCP
> - ***Socket Destino:*** `8.8.4.4:443 (TCP)`

Cuando el servidor del banco quiera responderte, simplemente invertirá los datos: Enviará la respuesta a la IP `190.50.20.10` al puerto `51234`. Al recibirlo en ese puerto, Windows sabrá instantáneamente que esa respuesta es para la pestaña de Chrome del banco, y no para tu sesión de Spotify (que podría estar usando el puerto de origen `51235`).

---

## 🗂️ Puertos "Well-Known" Obligatorios (Aprender de Memoria)

En ciberseguridad, los puertos son el equivalente a conocer el alfabeto. Cuando ves un escaneo de Nmap o analizas un log de Firewall, tenés que saber inmediatamente qué servicio se esconde detrás de un puerto.

### 🌐 Tráfico Web
- **80 (TCP):** `HTTP` (Tráfico web no cifrado. Inseguro).
- **443 (TCP):** `HTTPS` (Tráfico web cifrado seguro, TLS).

### 🖥️ Administración Remota
- **22 (TCP):** `SSH` (Secure Shell. Línea de comandos remota cifrada. El pan de cada día en Linux).
- **23 (TCP):** `Telnet` (Línea de comandos antigua. Texto plano inseguro. ¡Debe bloquearse!).
- **3389 (TCP):** `RDP` (Remote Desktop Protocol. Escritorio remoto de Windows. Frecuentemente atacado por Ransomware).

### 📧 Correo Electrónico
- **25 (TCP):** `SMTP` (Envío de correo).
- **110 (TCP):** `POP3` (Recepción de correo antiguo).
- **143 (TCP):** `IMAP` (Recepción de correo moderno).

### 📁 Transferencia de Archivos
- **20 y 21 (TCP):** `FTP` (File Transfer Protocol. Inseguro. El puerto 21 es para comandos, el 20 para los datos).
- **69 (UDP):** `TFTP` (Trivial FTP. Muy básico, usado para arrancar equipos por red o cargar configuraciones de routers).
- **445 (TCP):** `SMB` (Server Message Block. Para compartir carpetas y archivos en Windows. Históricamente, la fuente de las peores vulnerabilidades como *EternalBlue/WannaCry*).

### 🔧 Servicios Base de Red
- **53 (TCP/UDP):** `DNS` (Resolución de nombres. TCP para transferencias de zona, UDP para consultas comunes del usuario).
- **67 y 68 (UDP):** `DHCP` (Para dar IPs automáticamente. 67 es el servidor, 68 el cliente).

---

## ❓ ¿Por qué importa en Seguridad?

- **Reducción de Superficie de Ataque:** La regla de oro (Hardening) es que un servidor solo debe tener abiertos los puertos estrictamente necesarios. Si tenés un servidor web público, solo los puertos 80 y 443 deberían estar abiertos. Si el puerto 22 (SSH) o el 3389 (RDP) están abiertos a Internet, un atacante pasará meses intentando adivinar tu contraseña (Fuerza Bruta) hasta que entre.
- **Port Spoofing / Evasión:** A veces los atacantes instalan un malware (Backdoor) en tu servidor, pero en lugar de hacerlo escuchar en el puerto raro `60000` (que el firewall corporativo bloquearía por seguridad), lo hacen escuchar en el puerto `80`. El firewall ve tráfico salir por el puerto 80 y dice "Ah, es tráfico web normal", y lo deja pasar.

---

## 📌 Must Know (Imprescindible)
- Saber de memoria los puertos listados en esta nota.
- El concepto de Socket (IP + Puerto).
- Comprender qué son los Puertos Efímeros.

---

## 🔄 Preguntas de repaso
1. Tapá la lista de puertos de arriba e intentá escribir en un papel (de memoria) el número de puerto y el protocolo (TCP/UDP) para: HTTP, HTTPS, SSH, DNS y RDP.
2. Si querés conectar tu PC remotamente a la consola de comandos de un servidor Linux, pero querés asegurarte de que las contraseñas viajen cifradas, ¿qué puerto "Well-Known" utilizarías? ¿Por qué no el 23?
3. En una conexión establecida, el puerto de origen de la computadora del usuario era el 54321. ¿A qué categoría de puertos (según la IANA) pertenece ese número?

**➡️ Siguiente nota:** [[18 - Capa de Aplicación (L7)]]
