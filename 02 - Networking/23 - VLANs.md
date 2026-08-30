# 23 - VLANs (Virtual LANs) y Trunking

## 🎯 Objetivos
- Entender qué es una VLAN y el problema que resuelve.
- Comprender el concepto de Dominio de Broadcast (y cómo las VLANs lo dividen).
- Conocer qué es un puerto Troncal (Trunk) vs un puerto de Acceso.
- Entender el Ataque "VLAN Hopping".

---

## 🧠 Concepto: El problema de la red plana

Imaginá una empresa con 100 computadoras conectadas a múltiples [[07 - Switches y ARP|Switches]]. Todos los switches están conectados entre sí, formando una única gran red (una Red Plana). 

Esto trae dos problemas enormes:
1. **Ruido (Rendimiento):** Cuando la computadora 1 grita pidiendo una dirección IP (Broadcast [[20 - DHCP|DHCP]]), o pregunta por una dirección MAC (Broadcast ARP), ese grito lo escuchan las otras 99 computadoras. Todo este grupo se llama **Dominio de Broadcast**. Si tenés 1,000 computadoras gritando al mismo tiempo, la red colapsa.
2. **Seguridad:** El departamento de Recursos Humanos (HR), los Servidores Financieros, y el Wi-Fi de los invitados están en el mismo Switch físico. Un invitado puede hacer un escaneo y comunicarse directamente con los Servidores Financieros usando protocolos de Capa 2 (esquivando los Firewalls).

---

## ✂️ La solución: VLANs (Redes de Área Local Virtuales)

Una **VLAN** permite "dividir" un Switch físico en múltiples Switches lógicos (virtuales). 
Es decir, tomamos un switch de 24 puertos y lo configuramos por software:
- Puertos del 1 al 8: **VLAN 10** (Recursos Humanos).
- Puertos del 9 al 16: **VLAN 20** (Servidores Financieros).
- Puertos del 17 al 24: **VLAN 30** (Invitados).

**Regla de oro de las VLANs:** Un mensaje enviado por una computadora en la VLAN 10 *nunca, bajo ninguna circunstancia* saldrá por los puertos configurados para la VLAN 20 o 30, aunque estén en el mismo aparato de metal.

- Las VLANs reducen los Dominios de Broadcast. Un grito en la VLAN 10 solo lo escuchan las PC de HR.
- **Mejoran la Seguridad:** Si un invitado en la VLAN 30 quiere hackear a Finanzas, como el Switch aísla físicamente el tráfico, el único camino que tiene el invitado es ir hacia un **[[12 - Routers y Tablas de Enrutamiento|Router]]** (o Firewall). El Router sí puede conectar distintas VLANs (Enrutamiento Inter-VLAN). Pero al pasar por el Router, el Firewall puede detener el tráfico diciendo: *"Los invitados no tienen permitido el paso hacia los servidores"*.
- *Implementar VLANs es aplicar [[10 - Attack Surface|Segmentación de Red]].*

---

## 🔌 Puertos de Acceso vs. Puertos Trunk (Troncales)

Si tenés un Switch en el Piso 1 y otro Switch en el Piso 2, y querés que Recursos Humanos (VLAN 10) exista en ambos pisos... ¿cómo haces para conectar ambos switches? ¿Tirás 3 cables distintos, uno para cada VLAN? Sería carísimo e ineficiente.

Acá entra el concepto de **Trunking (Troncal)** (Estándar IEEE 802.1Q).

1. **Puertos de Acceso (Access Ports):** Son los puertos donde se conectan las computadoras finales. Estos puertos pertenecen a **UNA sola VLAN**. La computadora no tiene idea de que las VLANs existen; el switch le quita esa etiqueta antes de entregarle los datos.
2. **Puertos Troncales (Trunk Ports):** Es un puerto especial que conecta un Switch con otro Switch (o con un Router). Por este puerto **viaja el tráfico de TODAS las VLANs mezclado**. 

*¿Cómo sabe el Switch 2 a qué VLAN pertenecía el paquete cuando le llega por el cable Trunk?* 
El Switch 1, antes de enviar la Trama (Frame), le "pega una etiqueta" (VLAN Tag / 802.1Q) en la cabecera de Capa 2 que dice: `[Soy de la VLAN 10]`. El Switch 2 recibe la trama, lee la etiqueta, la arranca y se la envía a la computadora correspondiente en el puerto de HR.

---

## ☠️ Ataque de Salto de VLAN (VLAN Hopping)

Un Switch mal configurado puede ser explotado. El ataque más famoso es el *VLAN Hopping*.

Por defecto, muchos Switches antiguos vienen configurados con sus puertos en modo "Dinámico". Esto significa que si conectás un cable, el switch intenta negociar qué tipo de conexión sos.
- Un atacante (que debería estar en el puerto de Acceso de Invitados) conecta su laptop y le envía al Switch mensajes especiales diciéndole: *"Hola, soy otro Switch"*.
- Si el Switch está mal configurado, le cree al atacante y convierte el puerto del atacante en un **Puerto Trunk**.
- Al ser un puerto Trunk, el switch le empieza a enviar al atacante el tráfico de **TODAS** las VLANs de la empresa. Además, el atacante puede fabricar paquetes con la etiqueta `[VLAN 20]` (Servidores) y enviarlos; el switch los procesará legítimamente y los dejará entrar a la red segura, puenteando cualquier Firewall.

*Defensa (Hardening):* Todo administrador de redes debe desactivar la negociación dinámica de puertos (DTP en Cisco) y forzar todos los puertos orientados a usuarios a que sean estrictamente "Puertos de Acceso".

---

## 📌 Must Know (Imprescindible)
- Qué es una VLAN y cómo segmenta la red (rompe los Dominios de Broadcast).
- La diferencia fundamental entre un Puerto de Acceso (1 VLAN, computadoras) y un Trunk (Múltiples VLANs, entre Switches).
- Que las computadoras normales no ven las "etiquetas" (Tags 802.1Q) de las VLANs.
- Por qué las VLANs mejoran la seguridad al forzar el tráfico a pasar por un Router L3 (Firewall).

---

## 🔄 Preguntas de repaso
1. Si dos computadoras están conectadas físicamente al mismo Switch, pero configuradas en puertos con VLANs distintas, ¿pueden comunicarse entre ellas usando solo el Switch? Explicá.
2. ¿Cómo soluciona un puerto Trunk el problema de tener que conectar varios switches que comparten las mismas múltiples VLANs?
3. ¿Cómo funciona conceptualmente el ataque de VLAN Hopping (mediante negociación dinámica de puertos)?

**➡️ Siguiente nota:** [[24 - Wireless Networking]]
