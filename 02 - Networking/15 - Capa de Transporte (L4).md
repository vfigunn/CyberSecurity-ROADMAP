# 15 - Capa de Transporte (Capa 4 / L4)

## 🎯 Objetivos
- Comprender la función de la Capa de Transporte en los modelos OSI y TCP/IP.
- Entender la diferencia entre comunicación "Host-to-Host" y "Process-to-Process".
- Conocer la introducción a los Puertos y la Segmentación.

---

## 🧠 Concepto

Subimos de nivel en el modelo OSI. 
- La Capa 2 (Switch/MAC) lleva los datos a la computadora correcta en la misma habitación.
- La Capa 3 (Router/IP) lleva los datos a la computadora correcta en cualquier parte del mundo.

Cuando el paquete IP llega finalmente a la computadora correcta (L3: *Host-to-Host*)... ¿ahora qué? Tu computadora puede tener 50 aplicaciones diferentes abiertas (Google Chrome, Spotify, un juego online, un servidor de correo). 
**¿A qué aplicación debe entregarle los datos tu sistema operativo?**

Este es el trabajo de la **Capa de Transporte (Layer 4)**. Su PDU se denomina **Segmento** (o Datagrama).
La Capa 4 es responsable de la comunicación **Process-to-Process** (de aplicación origen a aplicación destino). Separa los datos de Spotify de los datos de Google Chrome.

---

## 🚪 El concepto de los Puertos (Ports)

¿Cómo hace la Capa 4 para separar este tráfico? Usando **Puertos (Ports)**.
(Profundizaremos en esto en la nota 17, pero aquí va la base conceptual).

La Capa 4 le asigna un número a cada aplicación. Es como un sistema de apartamentos:
- La Dirección IP (L3) es la dirección del edificio de departamentos. (Te hace llegar al lugar).
- El **Puerto (L4)** es el número del departamento exacto. (Te hace llegar a la persona correcta).

Si el servidor web de Google recibe datos en el **Puerto 80**, sabe que esos datos son para su programa de servidor web. Si los recibe en el **Puerto 25**, sabe que son para su programa de correo electrónico.

---

## 🪚 Segmentación y Reensamblaje

A la red (cables y routers) no le gustan los archivos grandes. No podés enviar un archivo de video de 1 Gigabyte "de un solo saque" por la red. Si el router recibe 1 GB entero, se atasca, y si ocurre un error eléctrico que arruina el último bit, tendrías que enviar todo el gigabyte de nuevo.

La Capa 4 (específicamente el protocolo TCP) se encarga de la **Segmentación**.
1. Toma ese video inmenso de 1 GB.
2. Lo corta en "pedacitos" diminutos (Segmentos), generalmente de unos 1500 bytes cada uno.
3. Le pone a cada pedacito un "Número de Secuencia" (ej. Pieza 1 de 1000, Pieza 2 de 1000).
4. Envía estos pedazos sueltos por Internet.

Como Internet es caótico (Capa 3 hace el "mejor esfuerzo"), los paquetes pueden viajar por distintos caminos y llegar al destino desordenados (ej. llega la pieza 4 antes que la 3). 
La Capa 4 en la computadora receptora toma todos estos pedazos, usa los números de secuencia para ordenarlos (Reensamblaje), e invoca la magia para reconstruir el archivo de video original antes de pasárselo a la aplicación (Capa 7) para que puedas verlo.

---

## Dos protocolos que gobiernan la Capa 4

En esta capa reinan dos protocolos con filosofías totalmente opuestas sobre cómo manejar estos segmentos:

1. **TCP (Transmission Control Protocol):** Obsesionado con el control, la fiabilidad y asegurarse de que ningún dato se pierda.
2. **UDP (User Datagram Protocol):** Relajado, increíblemente rápido, al que no le importa si algún dato se pierde por el camino.

En la [[16 - TCP vs UDP|próxima nota]] veremos a fondo esta épica batalla de diseño.

---

## ❓ ¿Por qué importa en Seguridad?

La Capa 4 es, probablemente, la capa técnica donde los analistas de seguridad pasan más tiempo.

- **Firewalls y Control de Acceso:** Los firewalls corporativos no solo bloquean IPs (L3), bloquean *Puertos* (L4). (Ej: "Permitir la IP X hacia mi servidor, pero *solo* a través del Puerto 443").
- **Escaneo de Puertos (Port Scanning):** Es la actividad ofensiva por excelencia. Un atacante (o un pentester) usa herramientas (como Nmap) para probar los 65.535 puertos de un servidor y descubrir cuáles están "abiertos" y qué aplicaciones están escuchando detrás de ellos. Así descubre la Superficie de Ataque.
- **Detección de intrusiones:** Muchos ataques o conexiones de malware dejan huellas inconfundibles al comportarse de forma anómala (ej. abriendo puertos raros) que el Blue Team puede detectar.

---

## 📌 Must Know (Imprescindible)
- La función de la Capa 4: Comunicación Process-to-Process (Aplicación a Aplicación) utilizando Puertos.
- La responsabilidad de la Capa 4 en la segmentación de datos.
- La PDU se llama *Segmento* (en TCP) o *Datagrama* (en UDP).

## 💡 Good to Know (Bueno saberlo)
- La dirección combinada de una IP y un Puerto (ej. `192.168.1.50:80`) se denomina **Socket**. Lo veremos pronto.

---

## 🔄 Preguntas de repaso
1. Si la Capa 3 se encarga de llevar la información desde la PC origen a la PC destino, ¿por qué necesitamos la Capa 4?
2. Aplicando la analogía del edificio (IP) y departamento (Puerto), ¿qué pasaría si un atacante escanea todos los "departamentos" de tu computadora y encuentra que todos tienen la puerta cerrada (Puertos cerrados)?
3. ¿Cómo hace el sistema receptor para reconstruir un archivo grande a partir de miles de segmentos pequeños que pueden haber llegado desordenados?

**➡️ Siguiente nota:** [[16 - TCP vs UDP]]
