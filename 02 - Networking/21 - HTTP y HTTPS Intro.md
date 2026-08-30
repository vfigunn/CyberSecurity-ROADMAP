# 21 - HTTP y HTTPS (Introducción)

## 🎯 Objetivos
- Entender cómo funciona el protocolo HTTP a un nivel alto.
- Conocer la diferencia crítica entre HTTP y HTTPS en términos de la Tríada CIA.
- Comprender el ciclo de Petición-Respuesta (Request-Response).

*(Nota: En el **Módulo 09 - Web Security** profundizaremos a nivel experto en cabeceras, códigos de estado, cookies y vulnerabilidades complejas. Esta nota es para entender su lugar en la red).*

---

## 🧠 Concepto: HTTP (HyperText Transfer Protocol)

El protocolo **HTTP** (Puerto TCP `80` por defecto) es el idioma que hablan los navegadores (Clientes) y los servidores web. 
Es un protocolo basado en texto y funciona estrictamente bajo el modelo **Cliente-Servidor** mediante un ciclo de *Request* (Petición) y *Response* (Respuesta).

### El ciclo Petición - Respuesta

1. **Request (El Cliente pide):** Escribís `google.com` en el navegador. (Después de que DNS resolvió la IP, y TCP hizo el Three-Way Handshake), tu navegador envía texto plano que dice algo como:
   ```http
   GET /index.html HTTP/1.1
   Host: www.google.com
   User-Agent: Mozilla Firefox
   ```
2. **Response (El Servidor responde):** El servidor recibe ese texto, procesa la petición y responde enviando el archivo solicitado junto con un "Código de Estado":
   ```http
   HTTP/1.1 200 OK
   Content-Type: text/html

   <html><body>Hola Mundo</body></html>
   ```

*(Códigos de Estado rápidos: 200 = Éxito, 404 = No encontrado, 500 = Error interno del servidor, 403 = Prohibido).*

---

## 🛑 El problema de seguridad con HTTP

HTTP fue creado en los inicios de la web para compartir documentos entre universidades. Su mayor debilidad hoy en día es que **transmite todos los datos en texto claro (Plaintext)**.

Si ingresas a `http://banco.com` y pones tu usuario y contraseña, ese texto (`usuario=admin&pass=12345`) viaja desde tu computadora, pasando por el router Wi-Fi de la cafetería, pasando por múltiples routers del ISP, hasta llegar al banco, **sin ningún tipo de ocultamiento**.

Si un atacante en la cafetería está "escuchando" el tráfico Wi-Fi (Wiretapping/Sniffing) utilizando herramientas como [[27 - Análisis de Tráfico (Wireshark y tcpdump)|Wireshark]], verá tu contraseña pasar por la pantalla como si la estuviera leyendo de un libro abierto. (Pérdida absoluta de **Confidencialidad**). Además, el atacante podría interceptar la respuesta del banco y modificar tu saldo antes de que llegue a tu pantalla (Pérdida de **Integridad**).

---

## 🔒 HTTPS (HTTP Secure)

Para solucionar esto, se creó **HTTPS** (Puerto TCP `443`). 
HTTPS no es un protocolo totalmente nuevo; es exactamente el mismo HTTP de siempre, pero **encapsulado dentro de un túnel cifrado**. Ese túnel es provisto por otro protocolo llamado **TLS** (Transport Layer Security, antes conocido como SSL).

### ¿Cómo protege TLS/HTTPS la comunicación?

Utiliza criptografía avanzada (que veremos en el [[06 - Cryptography/13 - TLS en Profundidad|Módulo 06]]) para garantizar:

1. **Confidencialidad (Cifrado):** Nadie en el medio del camino (ni el hacker del café, ni tu proveedor de internet) puede leer qué página exacta estás visitando dentro del sitio ni qué datos envías. Solo ven un flujo de datos aleatorios e indescifrables que van desde tu IP hacia la IP del servidor.
2. **Integridad (Hashing):** Si el hacker intenta modificar aunque sea una sola letra del paquete cifrado en el medio del camino, tu navegador se dará cuenta de que fue alterado y romperá la conexión (mostrando un error rojo de seguridad).
3. **Autenticidad (Certificados Digitales):** Cuando entrás a `https://banco.com`, el servidor te envía un Certificado (validado por una autoridad superior) que prueba criptográficamente que ellos realmente son el Banco, y no un servidor falso creado por un atacante.

---

## ❓ ¿Por qué importa en Seguridad?

La transición global de HTTP a HTTPS ha sido la mayor victoria de seguridad en la historia de Internet. 
Hoy en día, Chrome y Firefox marcan las páginas HTTP como **"No Seguras"** para disuadir a los usuarios de usarlas.

- **Para el Blue Team:** HTTPS es genial para proteger al usuario, pero es un **dolor de cabeza para los Firewalls defensivos corporativos**. Si un empleado descarga un virus a través de HTTPS, el Firewall de la empresa no puede ver el virus, porque está cifrado. Para solucionar esto, las empresas hacen "Inspección TLS/SSL" (un ataque Man-In-The-Middle intencional y autorizado por la empresa) para abrir el túnel, revisar que no haya virus, volver a cifrarlo y enviárselo al empleado.
- **Para el Red Team:** Si auditas un sistema y ves paneles de control o formularios de login utilizando puerto 80 (HTTP), se reporta como una vulnerabilidad crítica inmediatamente (CWE-319: Cleartext Transmission of Sensitive Information).

---

## 📌 Must Know (Imprescindible)
- HTTP = Puerto 80, texto plano (inseguro).
- HTTPS = Puerto 443, usa TLS para garantizar Confidencialidad, Integridad y Autenticidad.
- El ciclo de Petición/Respuesta básico (Cliente envía `GET`, Servidor responde `200 OK`).

---

## 🔄 Preguntas de repaso
1. Si un atacante intercepta una conexión HTTPS entre vos y Wikipedia usando un analizador de paquetes (Sniffer), ¿qué información de la comunicación podrá leer?
2. ¿Por qué el uso generalizado de HTTPS representa un desafío para los controles de seguridad detectivos (como un IDS o un Firewall) de una red corporativa?
3. Cuando un servidor responde a una petición HTTP, además de enviar el archivo (ej. una foto), ¿qué otro dato importante devuelve para indicar si la petición tuvo éxito o falló?

**➡️ Siguiente nota:** [[22 - Protocolos de Correo y Transferencia]]
