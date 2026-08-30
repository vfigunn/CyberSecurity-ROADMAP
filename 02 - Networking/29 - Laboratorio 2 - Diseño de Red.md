# Lab 02.2 - Diseño Seguro de Red (Network Architecture)

## 🎯 Objetivo
Aplicar conceptos de segmentación de redes (VLANs, Subnetting), direccionamiento IP y Controles de Seguridad perimetral (Firewalls) para diseñar una arquitectura de red básica y segura para una PYME.

---

## 📋 Escenario: "TechStart Inc."

Sos el Arquitecto de Seguridad recién contratado para "TechStart Inc.", una empresa de software pequeña. 
Actualmente, la empresa tiene 50 empleados. Tienen una "red plana" (un solo Switch gigante, todos conectados al mismo router) utilizando la red `192.168.1.0/24`.

El jefe te pide que rediseñes la red desde cero para solucionar problemas de seguridad, ya que la semana pasada un invitado conectó su laptop al Wi-Fi de visitas y, debido a un malware que tenía, infectó a todos los Servidores de Base de Datos de la empresa, deteniendo las operaciones 3 días.

### Requerimientos de la nueva red:
La empresa tiene 3 departamentos claramente definidos. Necesitan estar separados lógicamente para mejorar la seguridad.
1. **Desarrolladores (Devs):** Son 30 empleados. Necesitan salir a internet libremente.
2. **Servidores Internos (Servers):** Son 10 servidores (Bases de datos, Git, etc.). NO deben tener acceso directo a internet, pero los Desarrolladores necesitan acceder a ellos.
3. **Invitados (Wi-Fi Público):** Personas que visitan la oficina. Solo necesitan acceso a Internet. NO deben ver absolutamente nada de la red interna de la empresa.

---

## 🛠️ Procedimiento (Tu Trabajo)

Toma nota o dibuja tu respuesta. (En entornos profesionales, usarías herramientas como Microsoft Visio, Draw.io o Lucidchart para dibujar diagramas de red).

### Paso 1: Diseño de Segmentación (Capa 2 y Capa 3)
Para separar los tres departamentos, decidís usar [[23 - VLANs|VLANs]] y Subnetting usando rangos de IPs Privadas ([[09 - Direccionamiento IPv4]]). 
- Asigná a cada requerimiento (Devs, Servers, Invitados) un ID de VLAN (ej. VLAN 10).
- Asigná a cada requerimiento una Subred distinta en notación CIDR (podés usar redes pequeñas `/24` como la `192.168.10.0/24`, `192.168.20.0/24`, etc.).

### Paso 2: Ubicación del Firewall (Capa 3 / Perímetro)
Para que las VLANs se comuniquen, necesitás un dispositivo de Capa 3. Dado que queremos filtrar el tráfico, decidís instalar un Next-Generation Firewall (NGFW).
- ¿Dónde ubicarías lógicamente este Firewall en tu red? (¿Entre qué redes/VLANs y el Internet?).

### Paso 3: Creación de Reglas de Firewall (Políticas)
Un Firewall bloquea todo por defecto (Deny All). Debés escribir las reglas necesarias para cumplir estrictamente con los requerimientos del negocio, y nada más (Principio de Mínimo Privilegio).

Escribí las reglas en una tabla con el formato: `[Acción (Allow/Drop)] | [Red Origen] | [Red Destino] | [Puertos permitidos]`

**Reglas a crear:**
1. Regla para permitir a los Desarrolladores (Devs) conectarse a los Servidores internos (Servers). (Solo permitiremos el protocolo seguro SSH).
2. Regla para permitir a los Desarrolladores navegar por páginas web en Internet de forma segura.
3. Regla para permitir a los Invitados navegar por páginas web en Internet (HTTP y HTTPS) y resolver nombres DNS.
4. (Regla final explícita): Bloquear todo tráfico entre la red de Invitados y la red de Servidores.

---

## 📝 Resultado Esperado (Autoevaluación)

Revisá tu diseño propuesto contra esta solución estándar de la industria.

> [!example]- Ver diseño y respuestas
> **Paso 1: Segmentación (VLANs e IPs)**
> - **Devs:** VLAN 10 | Red: `10.0.10.0/24` (IPs: 10.0.10.1 al 254)
> - **Servers:** VLAN 20 | Red: `10.0.20.0/24`
> - **Invitados:** VLAN 30 | Red: `192.168.100.0/24` (Es buena práctica usar un rango totalmente diferente visualmente para redes no confiables).
> 
> **Paso 2: Ubicación del Firewall**
> El Firewall actuará como el núcleo (Router) de la red (Arquitectura Router-on-a-Stick o Core Firewall). Los Switches (Capa 2) se conectan al Firewall mediante puertos *Trunk*. Todas las subredes (VLAN 10, 20, 30) tienen su Default Gateway en el Firewall. El Firewall, por el otro lado, se conecta hacia el router del ISP / Internet. De esta forma, el Firewall audita **todo** tráfico que cruza entre VLANs y hacia internet.
> 
> **Paso 3: Reglas de Firewall (ACLs)**
> 
> | Regla # | Acción | Red Origen | Red Destino | Puertos / Protocolos | Justificación |
> | :--- | :--- | :--- | :--- | :--- | :--- |
> | **1** | ALLOW | VLAN 10 (Devs) | VLAN 20 (Servers) | TCP 22 (SSH) | Permite administración remota segura de los servidores a los Devs. |
> | **2** | ALLOW | VLAN 10 (Devs) | Internet (Cualquiera) | TCP 80 (HTTP), TCP 443 (HTTPS), UDP 53 (DNS) | Permite navegación web normal. |
> | **3** | ALLOW | VLAN 30 (Invitados) | Internet (Cualquiera) | TCP 80, TCP 443, UDP 53 | Permite navegación básica a invitados. |
> | **4** | DROP | VLAN 30 (Invitados) | VLAN 20 (Servers) | TODOS | Regla crítica. Aíslas el riesgo principal de la empresa. Ningún invitado tocará los servidores. |
> | **5** | DROP | TODOS | TODOS | TODOS | *Implicit Deny* (Bloqueo Implícito). Cualquier conexión que no haga match con la regla 1, 2 o 3 será destruida (Ej. un Dev intentando hacer Pings a un servidor, o un servidor intentando salir a internet, será bloqueado). |

**➡️ Siguiente nota:** [[30 - Ejercicios]]
