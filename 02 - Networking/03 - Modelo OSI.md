# 03 - El Modelo OSI

## 🎯 Objetivos
- Entender qué es un modelo de red y por qué es necesario.
- Conocer las 7 capas del Modelo OSI en orden.
- Comprender el concepto de "Encapsulación" de datos.

---

## 🧠 Concepto

A finales de los 70s, la comunicación entre computadoras era un caos. Si comprabas computadoras IBM, solo hablaban con IBM. Si comprabas computadoras DEC, solo hablaban con DEC. No existía un estándar universal.

Para solucionar esto, la ISO (Organización Internacional de Normalización) creó el **Modelo OSI (Open Systems Interconnection)** en 1984.

**El Modelo OSI es un marco teórico de 7 capas que describe cómo deben viajar los datos desde un programa de software en una computadora A hasta un programa de software en una computadora B, a través de cualquier medio de red.**

Es importante aclarar: *Nadie "instala" el modelo OSI.* Es un modelo conceptual, un mapa mental para ayudar a fabricantes e ingenieros a diseñar protocolos que sean compatibles entre sí.

---

## 📚 Las 7 Capas del Modelo OSI

Se leen de abajo hacia arriba (desde el cable hasta la aplicación del usuario).

| Capa (Layer) | Nombre | Función Simplificada | PDU (Datos) | Dispositivos / Protocolos Típicos |
| :--- | :--- | :--- | :--- | :--- |
| **L7** | **Aplicación** | Interfaz directa con el software (Navegador, Email). | Datos | HTTP, FTP, DNS, SMTP |
| **L6** | **Presentación** | Traduce, formatea y cifra/descifra los datos. | Datos | SSL/TLS, JPEG, ASCII |
| **L5** | **Sesión** | Inicia, mantiene y cierra la conexión (sesión) entre las apps. | Datos | NetBIOS, RPC |
| **L4** | **Transporte** | Segmenta los datos, controla el flujo y corrige errores end-to-end. | **Segmento** (o Datagrama) | **TCP, UDP** |
| **L3** | **Red** | Enrutamiento lógíco. Decide el mejor camino de origen a destino final. | **Paquete** | **IP (IPv4/IPv6), Routers** |
| **L2** | **Enlace de Datos** | Direccionamiento físico (MAC) dentro de la misma red local (LAN). | **Trama** (Frame) | **MAC, Ethernet, Switches** |
| **L1** | **Física** | Transmisión de bits crudos (0s y 1s) por el cable o el aire. | **Bits** | Cables, Hubs, Señales de Wi-Fi |

> *Mnemotecnia para recordar de abajo hacia arriba (en inglés):* **P**lease **D**o **N**ot **T**hrow **S**ausage **P**izza **A**way (Physical, Data Link, Network, Transport, Session, Presentation, Application).

---

## 📦 El concepto de Encapsulación

¿Cómo funciona la comunicación usando este modelo? Usamos la **Encapsulación**.
Imaginá que querés mandar una carta (el dato de la Capa 7).

1. Escribís la carta (**L7 - Datos**).
2. La metés en un sobre y le ponés tu nombre y el de la persona a nivel departamento (**L4 - Puerto / Segmento**).
3. Metés ese sobre en un sobre más grande que tiene el código postal de la ciudad de destino (**L3 - Dirección IP / Paquete**).
4. Metés ese sobre en uno más grande que dice "Al buzón de la esquina de mi calle" (**L2 - Dirección MAC / Trama**).
5. Se lo das al cartero físico para que se lo lleve caminando (**L1 - Bits en el cable**).

Cada capa agrega su propia "cabecera" (Header) con información de control antes de pasarlo a la capa inferior. Cuando el mensaje llega a la PC destino, ocurre el proceso inverso (Desencapsulación): cada capa lee su cabecera, la descarta, y pasa el contenido a la capa superior.

---

## ❓ ¿Por qué importa en Seguridad?

El Modelo OSI es el lenguaje universal que hablamos en Ciberseguridad e IT. 
Los atacantes no atacan "la computadora", atacan una capa específica. Y los defensores deben colocar controles en capas específicas.

- Si alguien corta el cable de fibra óptica con una pala, es un ataque de Denegación de Servicio en **Capa 1** (Física).
- Si un atacante engaña a los switches de tu red local, es un ataque en **Capa 2** (Ej: ARP Spoofing).
- Si te hacen un DDoS saturando el router con millones de paquetes IP falsos, es un ataque en **Capa 3** (Red).
- Si hacen un escaneo para ver si tenés el puerto 22 abierto, operan en **Capa 4** (Transporte).
- Si te hacen un SQL Injection en tu página web, es un ataque en **Capa 7** (Aplicación).

Un **Firewall L3** solo puede ver IPs y bloquearlas. Un **Firewall L7** (como un WAF - Web Application Firewall) puede entender si el tráfico HTTP contiene un ataque web complejo, es decir, es más "inteligente" (y más caro) porque "ve" más arriba en el modelo OSI.

---

## 📌 Must Know (Imprescindible)
- Conocer los nombres y números de las capas 1, 2, 3, 4 y 7.
- Entender qué es un Paquete (Capa 3) frente a una Trama (Capa 2).
- Comprender el proceso de Encapsulación.

## 💡 Good to Know (Bueno saberlo)
- Hoy en día, las capas 5 (Sesión) y 6 (Presentación) del modelo OSI suelen combinarse dentro de la Capa 7 en implementaciones prácticas, ya que la aplicación moderna (como un navegador) maneja la sesión y el cifrado por su cuenta.

---

## 🔄 Preguntas de repaso
1. Si un analista de SOC (Blue Team) dice: "Estamos sufriendo un ataque en Capa 3", ¿qué dispositivo y qué protocolo de los mencionados en la tabla probablemente estén bajo ataque?
2. Describí brevemente el proceso de "Encapsulación" de arriba hacia abajo.
3. Un Firewall que bloquea la IP `192.168.1.50`, ¿en qué capa del modelo OSI está trabajando principalmente?

**➡️ Siguiente nota:** [[04 - Modelo TCP-IP]]
