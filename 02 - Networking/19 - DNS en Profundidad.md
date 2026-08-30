# 19 - DNS en Profundidad (Domain Name System)

## 🎯 Objetivos
- Entender el propósito crítico del protocolo DNS.
- Comprender la arquitectura jerárquica de los servidores DNS.
- Conocer los registros DNS más importantes (A, AAAA, MX, CNAME, TXT).
- Entender los abusos de DNS y ataques (DNS Spoofing, DNS Tunneling, Amplificación).

---

## 🧠 Concepto: La guía telefónica de Internet

Los humanos somos malos recordando números (IPs). Recordar `8.8.8.8` o `142.250.78.46` es difícil; recordar `google.com` es fácil.
Los enrutadores de Internet, en la Capa 3, solo entienden direcciones IP, no entienden letras ni palabras.

El **DNS (Domain Name System)** (Capa 7, operando comúnmente en el [[17 - Puertos y Sockets|Puerto 53 UDP/TCP]]) es el servicio de directorio global que traduce los nombres de dominio legibles por humanos (`www.google.com`) a direcciones IP legibles por máquinas (`142.250.78.46`). Sin DNS, Internet como lo conocemos dejaría de existir al instante.

---

## 🏗️ La Arquitectura Jerárquica de DNS

DNS no es un solo servidor gigante que tiene todas las IPs del mundo. Es una base de datos distribuida a nivel mundial que funciona como un árbol invertido. 

Cuando tu navegador necesita saber la IP de `www.google.com.ar`, el proceso (simplificado) es:

1. **Tu PC / Caché:** Se fija si ya preguntó esto hace poco y lo tiene guardado en su memoria local. Si no, le pregunta al Resolutor de tu proveedor de internet (ISP) o al que tengas configurado (como `8.8.8.8`).
2. **DNS Resolver:** El resolutor inicia la búsqueda. Va a la cima del mundo: los *Root Servers*.
3. **Root Servers (Servidores Raíz - `.`):** Son 13 grupos de servidores globales hiper-protegidos que saben dónde están todos los dominios de primer nivel (Top-Level Domains). El Root no sabe la IP de google, pero le dice al Resolver: *"No lo sé, pero andá a preguntarle al servidor que maneja todos los dominios `.ar`"*.
4. **TLD Servers (Top-Level Domain):** El servidor `.ar` sabe dónde están registrados los dominios `.com.ar`. Dice: *"Andá a preguntarle al Servidor Autoritativo de Google"*.
5. **Authoritative Name Server:** Estos servidores son los dueños de la verdad para ese dominio específico. El servidor de Google responde: *"Sí, yo administro `google.com.ar`. La IP para `www` es `142.250.78.46`"*.
6. El Resolver se lo envía a tu PC, tu PC lo guarda en memoria, y se conecta al servidor web.

Este proceso (Resolución Recursiva) pasa en milisegundos.

---

## 🗂️ Tipos de Registros DNS (DNS Records)

Cuando sos dueño de un dominio, tenés que crear registros en tu Servidor Autoritativo. En ciberseguridad, a menudo investigamos estos registros usando la herramienta [[26 - Herramientas de Red en CLI|dig o nslookup]].

- **A (Address Record):** Traduce un nombre a una dirección **IPv4**. (El más común).
- **AAAA (Quad-A Record):** Traduce un nombre a una dirección **IPv6**.
- **CNAME (Canonical Name):** Un alias. Apunta un dominio a *otro* dominio. (Ej. `www.ejemplo.com` apunta a `servidor-web-2.amazon.com`).
- **MX (Mail Exchange):** Indica qué servidor es responsable de recibir los correos electrónicos (`@`) de ese dominio.
- **TXT (Text Record):** Permite a los dueños dejar texto libre. Hoy se usa intensamente para la seguridad de correos (SPF, DKIM, DMARC) y para verificar la propiedad de un dominio.
- **NS (Name Server):** Indica qué servidores son los autoritativos para el dominio.

---

## ☠️ Ataques y Abusos de DNS

Dado que DNS es fundamental y suele estar permitido salir a internet desde cualquier red corporativa a través del firewall (Puerto 53), es un vector de ataque enorme.

### 1. DNS Spoofing / DNS Cache Poisoning
Un atacante en la red local o que compromete un servidor DNS puede insertar respuestas falsas. 
- *Situación:* Pedís la IP de `banco.com`.
- *Ataque:* El servidor envenenado responde con la IP `100.100.100.100` (que pertenece a un servidor clonado falso que hizo el atacante).
- *Resultado:* Ingresas tu usuario y contraseña creyendo que es tu banco, pero se las estás dando al atacante (Ataque Phishing avanzado). *(El control contra esto se llama DNSSEC).*

### 2. Amplificación DNS (Ataque DDoS)
Se basa en que DNS usa UDP (ver [[16 - TCP vs UDP]]). 
- El atacante falsifica su IP (pone la IP de la víctima, ej: Servidor de Xbox).
- El atacante envía una petición corta a miles de servidores DNS en internet pidiendo *"Dame TODOS los registros de X dominio gigante"*.
- Los servidores DNS, pensando que la víctima lo pidió, le envían archivos gigantes de respuesta a la víctima simultáneamente, hundiendo su conexión a internet.

### 3. DNS Tunneling y C2 (Exfiltración)
Al igual que con [[14 - Protocolo ICMP|ICMP Tunneling]], si un Firewall bloquea las salidas de un malware hacia internet, el malware puede esconder sus secretos (archivos robados o comandos C2) codificándolos dentro de peticiones DNS inofensivas.
- *Ejemplo:* El malware en una PC interna hace un *query* (consulta) DNS por el dominio ficticio: `contrasena-administrador-es-12345.dominio-del-hacker.com`. 
- El servidor DNS interno buscará eso en internet hasta llegar al Servidor Autoritativo del Hacker. ¡El hacker acaba de recibir la contraseña, escondida en una petición DNS "legítima" que cruzó el Firewall sin problemas!

---

## 📌 Must Know (Imprescindible)
- Qué hace DNS y cuál es su puerto (53 TCP/UDP).
- Diferencia entre registros A, AAAA, MX, y CNAME.
- Cómo funciona conceptualmente el DNS Tunneling y por qué evade firewalls.

## 💡 Good to Know (Bueno saberlo)
- Modificar el archivo `hosts` en Windows (`C:\Windows\System32\drivers\etc\hosts`) o en Linux (`/etc/hosts`) permite "anular" el DNS y decirle a la computadora qué IP tiene un dominio sin preguntarle a internet. Es lo primero que revisa un Blue Team para ver si un malware desvió el tráfico, o lo primero que modifica un Red Team.

---

## 🔄 Preguntas de repaso
1. Si un usuario intenta enviar un correo electrónico a `ventas@empresa.com`, ¿qué registro DNS buscará su servidor de correo para saber a qué servidor entregarle el mensaje?
2. Explicá cómo un atacante utiliza un servidor DNS público para realizar un ataque DDoS de "Amplificación" contra una tercera víctima.
3. El archivo `hosts` local de tu PC es consultado *antes* o *después* de enviarle la petición al servidor DNS de Internet? (Podés inferirlo del "Good to Know").

**➡️ Siguiente nota:** [[20 - DHCP]]
