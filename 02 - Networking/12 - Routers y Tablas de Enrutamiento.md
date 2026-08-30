# 12 - Routers y Tablas de Enrutamiento

## 🎯 Objetivos
- Entender la función de un Router en la Capa 3.
- Comprender qué es una Tabla de Enrutamiento y cómo los routers toman decisiones.
- Diferenciar el Enrutamiento Estático del Dinámico (y por qué los atacantes atacan a este último).
- Entender el concepto de Puerta de Enlace Predeterminada (Default Gateway).

---

## 🚦 El Router: El policía de tráfico de Internet

En la [[07 - Switches y ARP|Nota 07]] vimos que el **Switch** (Capa 2) conecta computadoras *dentro de la misma red local (LAN)* usando direcciones MAC.
El **Router** (Capa 3) es el dispositivo que conecta **diferentes redes entre sí** usando direcciones IP. Internet, en su esencia más pura, es simplemente millones de Routers pasándose paquetes unos a otros.

Si tu computadora (`192.168.1.50`) quiere enviarle un paquete al servidor de Google (`8.8.8.8`), tu computadora se da cuenta (gracias a su [[10 - Subnetting y CIDR|Máscara de Subred]]) que Google **no** está en su misma red. Por lo tanto, no puede enviárselo directamente. 
Se lo envía a su **Default Gateway (Puerta de enlace predeterminada)**. 
El Default Gateway es simplemente la dirección IP de la "puerta de salida" de tu red hacia el mundo exterior, que casi siempre es la IP local de tu Router.

---

## 🗺️ La Tabla de Enrutamiento (Routing Table)

Cuando el Router recibe tu paquete dirigido a `8.8.8.8`, ¿qué hace con él? Consulta su **Tabla de Enrutamiento**. 

La tabla de enrutamiento es como un mapa de carreteras. Le dice al router: *"Si recibís un paquete que va a la red X, envialo por el puerto Y hacia el siguiente Router Z"*.

Si el Router no sabe cómo llegar a una red en particular, buscará en su tabla la "Ruta por Defecto" (`0.0.0.0/0` en IPv4). La ruta por defecto significa: *"Si no tenés ni idea de a dónde va este paquete, tiráselo al Router de tu Proveedor de Internet (ISP), que él es más grande y sabrá qué hacer"*. 
Y así, de Router en Router (Hop by Hop), el paquete llega a Google.

---

## 🛣️ Enrutamiento Estático vs Dinámico

¿Cómo aprende el Router los caminos para llenar su tabla de enrutamiento? Hay dos formas:

### 1. Enrutamiento Estático
Un ser humano (Administrador de Red) se conecta al Router y escribe a mano el comando: *"Para ir a la Red B, usá el cable 2"*.
- *Ventaja:* Es 100% seguro contra manipulaciones externas (nadie puede envenenar la tabla) y no consume recursos.
- *Desventaja:* No escala. Si tenés una red con 500 routers y un cable se rompe, todos los paquetes se perderán hasta que un humano reconfigure todos los routers a mano.

### 2. Enrutamiento Dinámico (BGP, OSPF)
Los Routers "hablan" entre ellos utilizando protocolos especiales. Se cuentan los chismes: *"Hola Router 2, yo conozco el camino a la Red C y tardo 5 milisegundos en llegar"*. Los Routers usan matemáticas complejas para calcular automáticamente la mejor ruta y actualizar sus tablas.
- *Ventaja:* Si un cable se rompe, los Routers se avisan entre ellos, recalculan el mapa en milisegundos y desvían el tráfico automáticamente. Es la única forma de que Internet funcione.
- *Desventaja:* Riesgos de seguridad.

---

## ❓ ¿Por qué importa en Seguridad?

Los atacantes aman los protocolos de Enrutamiento Dinámico, y el mayor ejemplo de esto son los ataques contra **BGP (Border Gateway Protocol)**.

BGP es el protocolo de enrutamiento que usan los enormes Routers de Internet (los de los ISPs). Funciona en base a la confianza. Si el ISP de un país, por error o malicia, le anuncia al mundo: *"¡Hey! ¡La ruta más rápida para llegar a Google, Facebook y Amazon pasa por mis servidores!"*, los Routers de todo el mundo le van a creer y empezarán a enviarle todo el tráfico (esto se conoce como **BGP Hijacking**).

Ha sucedido múltiples veces que tráfico sensible de EE.UU. o Europa fue redirigido a través de servidores en China o Rusia durante horas debido a anuncios BGP "erróneos" o manipulados intencionalmente para espiar o censurar (Ataque Man-in-the-Middle a escala global).

---

## 📌 Must Know (Imprescindible)
- La función de un Router (Capa 3) y cómo difiere de un Switch (Capa 2).
- Qué es una Tabla de Enrutamiento y el concepto de "Ruta por Defecto" (`0.0.0.0/0`).
- Entender la función del Default Gateway.

## 💡 Good to Know (Bueno saberlo)
- La mayoría de los Routers caseros (los que te da tu proveedor de internet) son dispositivos todo-en-uno: Tienen un Router (para hablar con internet), un Switch de 4 puertos (para conectar PCs por cable) y un Access Point (para dar Wi-Fi). En las empresas, estos tres dispositivos se compran por separado para mayor rendimiento.

---

## 🔄 Preguntas de repaso
1. Si tu PC tiene IP `192.168.1.100/24` y quiere hablar con la IP `192.168.1.105`, ¿el tráfico necesita pasar por el Router? ¿Por qué?
2. Si tu PC quiere hablar con `8.8.8.8`, ¿necesita pasar por el Router?
3. ¿Qué es un ataque de BGP Hijacking y cómo afecta a la confidencialidad de la información?

**➡️ Siguiente nota:** [[13 - NAT y PAT]]
