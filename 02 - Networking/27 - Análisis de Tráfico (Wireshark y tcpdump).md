# 27 - Análisis de Tráfico (Wireshark y tcpdump)

## 🎯 Objetivos
- Entender el concepto de "Sniffing" y captura de paquetes.
- Diferenciar cuándo usar tcpdump y cuándo usar Wireshark.
- Conocer la interfaz básica de Wireshark.

---

## 🧠 Concepto: Escuchando la red (Sniffing)

Como aprendimos, la red es un medio compartido por el que viajan [[06 - Capa de Enlace de Datos y MAC (L2)|Tramas]] y [[08 - Capa de Red e IP (L3)|Paquetes]]. 
Normalmente, la tarjeta de red de tu computadora ignora los paquetes que no van dirigidos a ella (a su dirección MAC). Sin embargo, si ponemos la tarjeta de red en **Modo Promiscuo** (Promiscuous mode), obligamos a la tarjeta a "leer" y capturar **todos** los paquetes que pasen por el cable o el aire, sin importar a quién vayan dirigidos.

Esto se llama **Sniffing** (Husmeo de red). 
- Los analistas defensivos lo usan para investigar ataques o problemas de red.
- Los atacantes lo usan para robar contraseñas en texto plano o espiar conversaciones (Wiretapping lógico).

---

## 🛠️ Herramientas de Análisis (Packet Sniffers)

Las dos herramientas supremas en este campo son Wireshark y tcpdump. Ambas capturan el tráfico y suelen guardar los resultados en un archivo con extensión **`.pcap`** (Packet Capture), que es el formato estándar mundial para esto.

### 1. `tcpdump` (El rastreador de terminal)
- Es una herramienta de línea de comandos (CLI), nativa y casi omnipresente en servidores Linux/Unix.
- **Uso:** Ideal para servidores sin interfaz gráfica o conexiones SSH lentas. Entras al servidor, ejecutás tcpdump para capturar el tráfico durante 5 minutos y guardarlo en un archivo `.pcap`.
- *Ejemplo básico:* `tcpdump -i eth0 -w captura.pcap` (Captura todo en la interfaz eth0 y lo guarda en captura.pcap).

### 2. Wireshark (El microscopio gráfico)
- Es un software con interfaz gráfica (GUI) increíblemente poderoso. Es estándar de la industria.
- **Uso:** Mientras tcpdump sirve para *capturar*, Wireshark se usa para *analizar*. Descargás el archivo `.pcap` a tu PC de escritorio, lo abrís con Wireshark, y el software despedaza el archivo mostrándote los datos diseccionados por cada capa del [[03 - Modelo OSI|Modelo OSI]].

---

## 🔎 La anatomía de Wireshark

Cuando abrís una captura en Wireshark, la pantalla se divide en tres paneles clave:

1. **Panel Superior (Lista de Paquetes):**
   - Muestra cada paquete capturado como una línea de texto. Ves el Número, Tiempo, IP de Origen, IP de Destino, Protocolo y longitud. 
   - Es una locura leer paquete por paquete, por lo que este panel tiene una **Barra de Filtros** vital (Ej: escribir `http` solo te muestra paquetes de ese protocolo, o `ip.addr == 192.168.1.1` te muestra solo el tráfico de esa IP).
2. **Panel Central (Detalles del Paquete):**
   - Al hacer clic en un paquete en el panel superior, este panel te muestra la Encapsulación OSI.
   - Podés desplegar como si fueran carpetas: "Frame (L2)", "Internet Protocol (L3)", "Transmission Control Protocol (L4)", etc., y ver qué bits exactos viajaban en las cabeceras.
3. **Panel Inferior (Bytes del Paquete):**
   - Muestra los datos crudos en formato Hexadecimal (los 0s y 1s de la Capa 1 interpretados).

---

## ❓ ¿Por qué importa en Seguridad?

Analizar `.pcaps` es una de las habilidades principales de un analista de [[14 - SOC & SIEM|SOC]], un Responder de Incidentes (IR) y un Analista Forense (DFIR). 

- Si el Firewall emite una alerta diciendo *"Posible inyección SQL detectada"*, el analista descarga la captura de red de ese momento exacto, la abre en Wireshark y busca probar si el ataque fue real o un falso positivo, leyendo el texto exacto que el atacante envió al servidor.
- **Falta de visibilidad por Cifrado:** Como vimos, el tráfico [[21 - HTTP y HTTPS Intro|HTTPS (TLS)]] va cifrado. En Wireshark, el panel central mostrará la IP y el Puerto, pero si mirás el contenido de los datos, solo verás texto aleatorio (basura). Wireshark no puede romper el cifrado militar por sí solo. 

---

## 📌 Must Know (Imprescindible)
- Qué es el Modo Promiscuo.
- Diferencia de uso principal entre tcpdump (CLI, captura) y Wireshark (GUI, análisis profundo).
- Qué es un archivo `.pcap`.

## 💡 Good to Know (Bueno saberlo)
- A los archivos grandes de red se los suele llamar "Network Traffic Dumps" o simplemente "PCAPs". En ejercicios de CTF (Capture The Flag) y entrevistas laborales de ciberseguridad, es muy común que te den un archivo `.pcap` y te pidan "encontrar qué contraseña robó el hacker".

---

## 🔄 Preguntas de repaso
1. Si sos administrador y tenés que capturar el tráfico del servidor de la empresa (un Linux puro sin interfaz gráfica de usuario) para investigar un problema, ¿qué herramienta de las mencionadas utilizarías?
2. Explicá cómo interfiere el protocolo HTTPS en el trabajo de análisis con Wireshark si el analista está intentando leer un archivo descargado por un empleado.
3. ¿Cuál es el propósito del panel de "Barra de Filtros" en Wireshark?

**➡️ Siguiente nota:** [[28 - Laboratorio 1 - Análisis de Paquetes]]
