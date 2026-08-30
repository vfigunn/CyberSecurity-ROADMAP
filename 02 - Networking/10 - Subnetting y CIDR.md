# 10 - Subnetting y CIDR (La Máscara de Subred)

## 🎯 Objetivos
- Entender para qué sirve la Máscara de Subred.
- Comprender el formato CIDR (ej. `/24`).
- Conocer los conceptos básicos de Subnetting (Subdivisión de redes).

---

## 🧠 Concepto: La Máscara de Subred

En la nota anterior vimos que una IP `192.168.1.50` tiene una porción de Red (calle) y una porción de Host (casa). Pero mirando solo la IP, es imposible saber dónde termina la "calle" y dónde empieza la "casa". 

Para eso existe la **Máscara de Subred (Subnet Mask)**. Trabaja siempre en pareja con la IP.
Es otro número de 32 bits (4 octetos) que se alinea debajo de la IP.

- Los **unos binarios (255 decimal)** en la máscara significan: "Esta parte de la IP corresponde a la Red".
- Los **ceros binarios (0 decimal)** en la máscara significan: "Esta parte de la IP corresponde al Host".

**Ejemplo clásico de casa:**
- IP: `192.168.1.50`
- Máscara: `255.255.255.0`
*Traducción:* Los primeros tres números (`192.168.1`) son la red (la calle). El último número (`50`) es la PC de Juan (la casa). 
Si la PC de María quiere estar en la misma red local que Juan para que puedan pasarse archivos directamente (sin usar un router), la PC de María DEBE empezar con `192.168.1.X`.

---

## ✂️ CIDR Notation (Classless Inter-Domain Routing)

Escribir `255.255.255.0` todo el tiempo es molesto. Se inventó una forma rápida llamada **Notación CIDR** (o "slash notation").
Simplemente contás cuántos "unos" binarios consecutivos tiene la máscara. 

- `255.0.0.0` tiene 8 bits en "uno". Se escribe **`/8`** (Red enorme, 16 millones de PCs).
- `255.255.0.0` tiene 16 bits en "uno". Se escribe **`/16`** (Red mediana, 65,000 PCs).
- `255.255.255.0` tiene 24 bits en "uno". Se escribe **`/24`** (Red chica, 254 PCs. La clásica red de tu casa).

Si en un reporte de ciberseguridad dice que hay que bloquear la red `10.50.0.0/16`, ya sabés a qué tamaño de red se refieren.

---

## 🔪 Subnetting (Básico)

**Subnetting** es el proceso de "pedir prestados" bits de la porción de Host (la parte de las casas) para dárselos a la porción de Red (las calles), creando así múltiples subredes más pequeñas a partir de una red original grande.

### ¿Por qué lo hacemos? (Importancia crítica en Seguridad)

1. **Rendimiento:** Como vimos en L2, las computadoras hacen ruido (Broadcasts ARP, DHCP). Si metés 1,000 computadoras en una sola red `/16`, el ruido de broadcast constante satura la red y todo se vuelve lento. El Subnetting corta esa gran red en 4 redes de 250 equipos, confinando el ruido (Achica el *Broadcast Domain*).
2. **Seguridad (Segmentación de Red):** Este es el punto vital. Si tenés a Recursos Humanos, Servidores Críticos de Base de Datos y la red de Invitados (Wi-Fi Público) en la misma subred... un invitado puede escanear y atacar el servidor directamente con herramientas L2 (ARP Spoofing) o L3.
   - Mediante el Subnetting, ponemos a cada departamento en su propia Subred diferente.
   - *Regla de oro:* **El tráfico NO PUEDE pasar de una subred a otra sin atravesar un Router o Firewall L3.** 
   - Por lo tanto, el Firewall ahora puede intervenir y decir: "La subred Invitados NO tiene permiso para hablar con la subred Servidores". Has aplicado **Defensa en Profundidad** (Segmentación).

---

## 🔢 Direcciones de Red y Broadcast

En cualquier subred, hay dos direcciones IP que están reservadas (no se le pueden dar a una computadora):
1. **La dirección de Red:** La primera IP de la subred (donde todos los bits de host son 0). Se usa para nombrar a la red entera en las tablas de enrutamiento. (Ej. `192.168.1.0`)
2. **La dirección de Broadcast:** La última IP de la subred (donde todos los bits de host son 1). Se usa para enviar un mensaje a *todos* los equipos de esa subred al mismo tiempo. (Ej. `192.168.1.255`)

Por lo tanto, una red `/24` matemáticamente tiene 256 IPs (0 al 255), pero solo tiene **254 IPs utilizables** para hosts.

---

## ❌ Errores comunes

- **Equivocarse con la máscara:** Si le configuras la IP `192.168.1.50` a la PC-A con máscara `/24` (255.255.255.0) y a la PC-B la IP `192.168.1.60` con máscara `/30` (255.255.255.252), sus matemáticas fallarán, creerán que están en redes distintas y no se comunicarán directamente, frustrando al usuario (o al atacante que configuró mal su máquina de pentesting).

---

## 📌 Must Know (Imprescindible)
- Qué es la Máscara de Subred y cómo divide la porción de red de la de host.
- Qué significa la notación CIDR `/24`, `/16` y `/8`.
- Por qué dividimos en subredes (Segmentación por seguridad y reducción de dominios de broadcast).
- Saber que la primera IP es la de Red y la última la de Broadcast.

## 💡 Good to Know (Bueno saberlo)
- A nivel técnico avanzado y certificaciones de redes (CCNA), tenés que saber hacer cálculos de subnetting manuales y mentalmente usando binario. En la práctica real de ciberseguridad, casi todos usamos calculadoras de IP online, pero entender el *concepto* de por qué segmentamos es innegociable.

---

## 🔄 Preguntas de repaso
1. Si un analista de SOC (Blue Team) te dice "Investigá los logs de la subred 192.168.5.0/24", ¿aproximadamente a cuántos dispositivos vivos máximos tendrás que revisar?
2. ¿Por qué poner los servidores corporativos en la misma subred que la red Wi-Fi de invitados es una pésima práctica de ciberseguridad?
3. En la subred 10.0.0.0/24, ¿cuál es la dirección IP de Broadcast?

**➡️ Siguiente nota:** [[11 - IPv6]]
