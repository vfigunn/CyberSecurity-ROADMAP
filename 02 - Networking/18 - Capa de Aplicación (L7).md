# 18 - Capa de Aplicación (Capa 7 / L7)

## 🎯 Objetivos
- Entender el rol de la Capa de Aplicación en el Modelo OSI (y TCP/IP).
- Diferenciar entre el software de aplicación y los *protocolos* de aplicación.
- Comprender que L7 es donde residen la mayoría de los datos valiosos (y los ataques más complejos).

---

## 🧠 Concepto

Hemos llegado a la cima de la montaña.
- **L1 (Física):** Pasa los bits por el cable.
- **L2 (Enlace):** Entrega a la PC correcta en la LAN (MAC).
- **L3 (Red):** Encuentra la ruta por Internet (IP).
- **L4 (Transporte):** Entrega al proceso/programa correcto, asegura los datos y corta el tráfico en pedazos (TCP/Puertos).

La **Capa de Aplicación (Layer 7)** (que en el modelo TCP/IP engloba las capas 5 y 6 del OSI) es la capa más cercana al usuario final.
Es aquí donde los datos por fin tienen "sentido" humano. Es donde reside el texto de la página web, el archivo adjunto de tu email, o la imagen del gatito que estás descargando. Su PDU son los **Datos (Data)**.

> **⚠️ Aclaración Crucial:**
> La Capa 7 **NO** es la aplicación de software en sí (como Google Chrome, Outlook, o un videojuego).
> La Capa 7 contiene los **Protocolos y Servicios** que esas aplicaciones utilizan para comunicarse con la red.
> *Ejemplo:* Google Chrome (el software de usuario) utiliza el protocolo `HTTP` (el protocolo de Capa 7) para traer una página web.

---

## 🌐 Protocolos Comunes de Capa 7

Existen cientos de protocolos en esta capa, diseñados para propósitos muy específicos. Algunos de los más conocidos son:
- **DNS (Domain Name System):** Para traducir nombres a IPs (Nota 19).
- **DHCP (Dynamic Host Config Protocol):** Para asignar IPs automáticamente (Nota 20).
- **HTTP/HTTPS (HyperText Transfer Protocol):** Para transferir páginas web y APIs.
- **FTP/SFTP (File Transfer Protocol):** Para mover archivos enteros.
- **SMTP/IMAP/POP3:** Para enviar y recibir correos electrónicos.
- **SSH/Telnet:** Para control de línea de comandos remota.

---

## 🗃️ Arquitectura Cliente-Servidor vs P2P

En la Capa 7, la forma en que los programas interactúan suele seguir dos modelos de arquitectura:

1. **Modelo Cliente-Servidor (Client-Server):**
   - Es el rey de Internet (usado por HTTP, DNS, Email).
   - El **Cliente** (tu PC, o más específicamente, tu navegador web) inicia la comunicación pidiendo un recurso.
   - El **Servidor** (una supercomputadora en un centro de datos, esperando pasivamente en un puerto) recibe la petición, procesa y responde enviando el recurso (la página web).
   - *Seguridad:* Es un modelo centralizado. Si el servidor cae (DDoS), nadie tiene acceso.

2. **Modelo Peer-to-Peer (P2P):**
   - Todos los dispositivos son clientes y servidores al mismo tiempo. Comparten los recursos de forma distribuida.
   - Usado en BitTorrent, algunas criptomonedas (Blockchain), y ciertos servicios de comunicación.
   - *Seguridad:* Es muy resiliente a ataques de denegación de servicio (si un peer cae, la red sigue), pero históricamente ha sido un vector gigante para la propagación de malware (ej. descargar archivos piratas infectados), ya que no hay un servidor central de confianza.

---

## ❓ ¿Por qué importa en Seguridad?

Los atacantes aman la Capa 7.
Hoy en día, casi todas las empresas tienen Firewalls de Capa 3 y 4 muy sólidos (bloquean IPs extrañas y cierran todos los puertos).
**¿Cuál es el único puerto que una empresa siempre tiene que dejar abierto a Internet para ganar dinero?** El puerto de su aplicación web (80/443).

Por lo tanto, los atacantes ya no atacan el puerto o el router, atacan la *lógica* de la aplicación de Capa 7.

### Ataques típicos de Capa 7 (Ejemplos):
- **Cross-Site Scripting (XSS) y SQL Injection:** El atacante usa comandos maliciosos dentro del protocolo HTTP para robar sesiones de usuarios o robar la base de datos completa. (Lo veremos a fondo en el [[09 - Web Security/00 - Overview|Módulo 09]]).
- **DDoS L7 (HTTP Flood):** En lugar de abrumar el ancho de banda del router con pings (L3), el atacante envía 10.000 peticiones de búsqueda muy complejas a la página web por segundo. Para el router o firewall tradicional, parecen conexiones de usuarios normales y legítimos. Sin embargo, el esfuerzo computacional de resolver esas 10.000 búsquedas en la base de datos funde la CPU del servidor web. Es mucho más difícil de detectar y mitigar.

Para defenderse en Capa 7 se necesitan herramientas "inteligentes" y muy específicas, como los **WAF (Web Application Firewalls)**, que leen el contenido profundo del mensaje HTTP y pueden distinguir una búsqueda normal de una maliciosa.

---

## 📌 Must Know (Imprescindible)
- La Capa 7 contiene los protocolos de red (ej. HTTP), no las aplicaciones de software (ej. Chrome).
- Entender el modelo Cliente-Servidor.
- Comprender por qué los ataques de Capa 7 son más complejos y difíciles de bloquear con firewalls tradicionales.

---

## 🔄 Preguntas de repaso
1. Identificá la afirmación correcta: "El programa Microsoft Outlook es un protocolo de Capa 7" o "Microsoft Outlook utiliza protocolos de Capa 7 (como SMTP/IMAP) para comunicarse".
2. Explicá cómo un ataque DDoS en Capa 7 (como un HTTP Flood que satura la CPU del servidor) es diferente en concepto a un ataque DDoS en Capa 3 (como un Ping Flood que satura el cable del router).
3. ¿Cuál es el propósito de un Web Application Firewall (WAF) en comparación con un Firewall tradicional de red?

**➡️ Siguiente nota:** [[19 - DNS en Profundidad]]
