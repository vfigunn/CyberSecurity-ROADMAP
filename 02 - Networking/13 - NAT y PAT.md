# 13 - NAT y PAT

## 🎯 Objetivos
- Entender el problema de las IPs Públicas vs Privadas que resuelve NAT.
- Conocer cómo NAT y PAT traducen direcciones.
- Comprender por qué NAT *no* es una herramienta de seguridad (aunque muchos crean que sí).

---

## 🧠 Concepto: El problema a resolver

En la nota [[09 - Direccionamiento IPv4]] aprendimos que las computadoras de nuestra red local (nuestra casa, o la oficina) tienen **IPs Privadas** (Ej: `192.168.1.50`). 
También aprendimos que los routers de Internet están programados para bloquear y descartar cualquier paquete que tenga una IP privada. 

Entonces, ¿cómo puede tu computadora navegar por Internet si su IP está "prohibida" en la red pública?

La respuesta es **NAT (Network Address Translation - Traducción de Dirección de Red)**. Es un servicio que se ejecuta en tu [[12 - Routers y Tablas de Enrutamiento|Router]].

---

## 🔄 ¿Cómo funciona NAT/PAT?

Cuando vos contratás internet, el proveedor (ISP) te regala **UNA (1)** sola dirección IP Pública válida en todo el mundo. Esa IP se le asigna a tu Router en la interfaz que da a la calle. 
En la interfaz que da a tu casa, el Router maneja la red privada (`192.168.x.x`).

Cuando tu PC (`192.168.1.50`) envía un paquete a Google:
1. El paquete llega a tu Router (tu *Default Gateway*).
2. El Router detiene el paquete, borra la "IP de Origen" privada (`192.168.1.50`) y la reemplaza por su propia IP Pública (`203.0.113.1`).
3. El Router anota en una tabla de memoria temporal: *"El paquete privado .50 me pidió que enviara esto a Google"*.
4. El paquete viaja por Internet con una IP pública válida y legal.
5. Google responde y le manda la página web a tu IP Pública (tu Router).
6. Tu Router recibe el paquete de Google, mira su tabla de memoria y dice: *"Ah, esta respuesta era para la PC `192.168.1.50`"*.
7. Vuelve a cambiar la IP, esta vez la de destino, y se la entrega a tu PC.

A los ojos de Google, todas las consultas (las tuyas, las de tu mamá, las de tu Smart TV) provienen exactamente de la **misma computadora**. Google nunca ve tus IPs privadas.

### PAT (Port Address Translation)
Si tres computadoras en tu casa abren google.com a la misma vez, ¿cómo sabe el router a quién entregarle cada respuesta si las tres salidas tienen la misma IP pública?
Para esto usa **PAT**. Además de cambiar la IP, el Router le asigna un número de "puerto" al azar diferente a la comunicación de cada PC. Así sabe exactamente a quién devolverle cada paquete en base al puerto de respuesta. A esto se le suele llamar *NAT Overload* y es lo que usan todos los routers caseros.

---

## ❓ NAT no es Seguridad

Existe un mito gigantesco en IT: *"NAT me protege porque oculta mis IPs internas de Internet"*.

Es cierto que, gracias a NAT, alguien desde Internet no puede iniciar una conexión directamente hacia tu computadora (porque tu IP privada no es enrutable). Pero **eso no es seguridad, es un efecto secundario de intentar solucionar la escasez de direcciones de IPv4.**

- El trabajo de bloquear conexiones no deseadas es del **Firewall** (Políticas de filtrado L3/L4), no de NAT.
- Si instalas [[11 - IPv6]] (donde NAT no existe y tu PC tiene IP Pública global), ¿estás inseguro? No, siempre y cuando tengas un buen Firewall activado.
- Además, NAT no te protege de las verdaderas amenazas modernas. Si vos iniciás la conexión (haciendo clic en un enlace de Phishing o descargando un troyano), el Router traducirá tu conexión normalmente, y el malware entrará directo a tu PC sin que a NAT le importe, abriendo el camino para el atacante (Reverse Shell).

---

## 🚪 Port Forwarding (Reenvío de Puertos)

A veces, *queremos* que alguien desde Internet pueda entrar a nuestra red privada. Por ejemplo, si tenés un servidor web o las cámaras de seguridad alojadas en tu casa.

Como Internet solo conoce tu IP Pública (la de tu Router), si alguien intenta accederla, el Router rechazará la conexión porque no sabrá a quién mandársela.
Para solucionar esto configuramos **Port Forwarding (NAT Estático)**. Le decimos al Router permanentemente: *"Cualquier conexión nueva que venga desde Internet por el puerto 80 (Web), no la rechaces, reenvíala directamente a la PC con IP Privada `192.168.1.10`"*.

> **Peligro:** El Port Forwarding perfora el perímetro. Es la principal causa por la que redes hogareñas y de pequeñas empresas son hackeadas (especialmente cuando exponen puertos de administración remota como RDP o SSH sin contraseñas fuertes).

---

## 📌 Must Know (Imprescindible)
- Por qué existe NAT (Para permitir que redes con IPs privadas naveguen por Internet público).
- Qué es el Port Forwarding (y por qué es peligroso si se configura mal).
- Entender que NAT NO es un control de seguridad (El control de seguridad es el Firewall).

---

## 🔄 Preguntas de repaso
1. Explicá paso a paso cómo tu computadora (con IP Privada) logra leer esta página web alojada en Internet utilizando el concepto de NAT.
2. ¿Por qué el protocolo IPv6 elimina la necesidad de utilizar NAT?
3. Un atacante en Rusia quiere conectarse al servidor de cámaras de seguridad dentro de una empresa, el cual tiene una IP Privada. ¿Por qué el atacante no puede simplemente escribir esa IP Privada en su navegador? ¿Qué configuración en el router de la empresa permitiría (por error o necesidad) que el atacante logre acceder?

**➡️ Siguiente nota:** [[14 - Protocolo ICMP]]
