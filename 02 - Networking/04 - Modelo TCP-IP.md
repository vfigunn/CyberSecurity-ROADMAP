# 04 - El Modelo TCP/IP

## 🎯 Objetivos
- Entender qué es el modelo TCP/IP y en qué se diferencia del Modelo OSI.
- Conocer la correspondencia entre las capas de ambos modelos.
- Comprender por qué TCP/IP es el estándar de facto (real) de Internet.

---

## 🧠 Concepto

Mientras que el [[03 - Modelo OSI|Modelo OSI]] es el estándar "teórico" que usamos para explicar redes y seguridad en una clase, **el Modelo TCP/IP es el protocolo que realmente hace funcionar a Internet.**

TCP/IP (Transmission Control Protocol / Internet Protocol) fue desarrollado en los años 70 por el Departamento de Defensa de EE. UU. (DARPA) con un objetivo militar: crear una red tan robusta que pudiera sobrevivir a un ataque nuclear y seguir enrutando información. 

Dado que fue creado *antes* del Modelo OSI (y centrado en la practicidad más que en la teoría estricta), tiene menos capas y es más pragmático.

---

## 📚 Las 4 Capas del Modelo TCP/IP (y su equivalencia con OSI)

El modelo TCP/IP original tiene 4 capas. Agrupa varias funciones que el Modelo OSI separa.

| Capa TCP/IP | Equivalente en Modelo OSI | Función | Protocolos Típicos |
| :--- | :--- | :--- | :--- |
| **4. Capa de Aplicación** | Capas 7, 6 y 5 (Aplicación, Presentación, Sesión) | Maneja el software de alto nivel, el cifrado y la lógica de la sesión. | HTTP, HTTPS, DNS, SMTP, SSH, FTP |
| **3. Capa de Transporte** | Capa 4 (Transporte) | Asegura la comunicación entre aplicaciones (Host to Host), puertos y control de errores. | **TCP, UDP** |
| **2. Capa de Internet** | Capa 3 (Red) | Enrutamiento de paquetes a través de diferentes redes para llegar al destino. | **IPv4, IPv6**, ICMP (Ping) |
| **1. Capa de Acceso a la Red (Link)** | Capas 2 y 1 (Enlace de Datos y Física) | Trata con el hardware físico y el direccionamiento local de la red LAN (MAC). | Ethernet, Wi-Fi (802.11), ARP |

> *Nota importante:* Existe una versión actualizada del modelo TCP/IP (a veces llamada "Modelo Híbrido" o de 5 capas) que separa la capa de Acceso a la Red en Física y Enlace de Datos, igual que OSI. Esto se hace porque la tecnología Ethernet (L2) y el cable (L1) son muy distintos. Pero conceptualmente, la tabla de arriba es el estándar clásico.

---

## ⚖️ OSI vs TCP/IP: ¿Cuál uso?

En la práctica profesional de ciberseguridad y redes, **usamos una mezcla de ambos dialectos**.

- **Hablamos usando la numeración de OSI:** Un ingeniero de redes dirá "Tenemos un problema en Capa 2" (refiriéndose a un Switch, del modelo OSI), no dirá "Tenemos un problema en la capa de Acceso a la Red del modelo TCP/IP".
- **Implementamos protocolos TCP/IP:** Las computadoras modernas, servidores y firewalls ejecutan el "Stack (Pila) TCP/IP". Nadie instala la "Pila OSI".

Por lo tanto: Usamos los números y conceptos de OSI para categorizar, pero usamos los protocolos reales del conjunto TCP/IP para trabajar.

---

## ❓ ¿Por qué importa en Seguridad?

Toda la seguridad de Internet (y sus vulnerabilidades) están atadas al diseño de TCP/IP.
Como fue diseñado en los años 70 en un entorno académico y militar de confianza cerrada, **la seguridad no estaba integrada por defecto**.

El modelo original asumía que si estabas conectado a la red, eras alguien "bueno". 
- El protocolo IP no verifica si quien envía el paquete realmente es quien dice ser (lo que permite *IP Spoofing*).
- Los protocolos de aplicación antiguos (HTTP, FTP, Telnet) enviaban contraseñas en texto plano.

La ciberseguridad moderna ha pasado décadas intentando agregar parches de seguridad (como TLS/HTTPS) encima de un modelo que fue diseñado para ser rápido y resiliente, pero no seguro frente a actores maliciosos.

---

## 📌 Must Know (Imprescindible)
- TCP/IP es el conjunto de protocolos real que impulsa Internet; OSI es el modelo teórico.
- Conocer la Capa de Internet (IP) y la Capa de Transporte (TCP/UDP) del modelo TCP/IP.
- Saber que la Capa de Aplicación de TCP/IP engloba las capas 5, 6 y 7 de OSI.

## 💡 Good to Know (Bueno saberlo)
- El término "Stack TCP/IP" se refiere a la implementación en código que tiene tu sistema operativo (Windows, Linux, macOS) para procesar estas capas.

---

## 🔄 Preguntas de repaso
1. Según la tabla de equivalencias, ¿qué capas del Modelo OSI se combinan para formar la Capa de Aplicación en el Modelo TCP/IP?
2. ¿Por qué en la industria de la ciberseguridad seguimos usando el vocabulario de "Capa 2" o "Capa 3" del Modelo OSI, a pesar de que el modelo real implementado es TCP/IP?
3. ¿Cuál fue el objetivo de diseño principal (filosofía) detrás de la creación de TCP/IP por DARPA, y por qué causó problemas de seguridad en el futuro?

**➡️ Siguiente nota:** [[05 - Capa Física (L1)]]
