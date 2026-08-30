# 25 - Dispositivos de Seguridad en Red

## 🎯 Objetivos
- Entender el rol de los Firewalls en una red.
- Diferenciar entre Stateless, Stateful y Next-Gen Firewalls (NGFW).
- Comprender la diferencia entre IDS e IPS.
- Conocer la función de un Proxy.

---

## 🧱 1. Firewalls

El **Firewall** (Cortafuegos) es el guardián de la red. Se ubica en los bordes de la red (Perímetro) o entre diferentes zonas internas (ej. entre la [[23 - VLANs|VLAN]] de Invitados y la de Servidores).
Su trabajo es inspeccionar el tráfico que intenta cruzar y, basándose en reglas predefinidas, decidir si **Permite (Allow)** o **Deniega/Descarta (Drop/Deny)** el tráfico.

### Evolución de los Firewalls

1. **Packet Filtering (Stateless) - Primera Generación:**
   - Inspeccionan paquetes individualmente en Capa 3 y Capa 4 (IPs y Puertos).
   - Son "tontos" y no tienen memoria (Stateless). Si permiten que tu PC le haga una petición web a Google (Puerto 80 saliente), no recuerdan que lo hiciste. Por lo tanto, tendrías que crear *otra regla* permitiendo explícitamente el tráfico entrante desde Google. Fueron reemplazados rápido.

2. **Stateful Inspection - Segunda Generación:**
   - Es el estándar clásico de los últimos 25 años. Tienen memoria. 
   - Mantienen una **Tabla de Estado (State Table)**. Si permiten que la PC origine una conexión (TCP) hacia afuera a Google, el Firewall anota esa conexión en su memoria y permite *automáticamente* que la respuesta de Google vuelva a entrar. El tráfico no solicitado desde el exterior (conexiones nuevas iniciadas desde Internet) es bloqueado por defecto.
   - *Límite:* Operan hasta Capa 4. Solo ven IPs y Puertos. Si permiten el puerto 80, dejan pasar *cualquier cosa* por el puerto 80 (incluyendo un malware disfrazado de página web).

3. **Next-Generation Firewalls (NGFW) - Capa 7:**
   - Son los firewalls modernos corporativos (Palo Alto, Fortinet, Check Point).
   - Inspeccionan hasta la **Capa de Aplicación (L7)**. Ya no les importa solo el puerto; inspeccionan el *contenido* del paquete (Deep Packet Inspection - DPI).
   - Tienen consciencia de la Aplicación: Pueden bloquear el *chat* de Facebook, pero permitir navegar por el *muro* de Facebook, sin importar qué puertos se usen.
   - Integran prevención de intrusiones, descifrado SSL/TLS, y antivirus en la misma caja física.

---

## 🚨 2. IDS e IPS (Sistemas de Intrusión)

A diferencia del Firewall (que se basa en reglas como "Permitir IP X"), los IDS/IPS buscan comportamiento malicioso o firmas de ataques conocidos dentro del tráfico permitido. (Funcionan igual que un antivirus, pero para la red).

1. **IDS (Intrusion Detection System):**
   - Es un control **Detectivo**.
   - Funciona como una cámara de seguridad. "Escucha" o recibe una copia de todo el tráfico que pasa por la red. Si ve un ataque (ej. alguien haciendo Nmap o lanzando un Exploit a la base de datos), el IDS no hace nada para frenarlo. Su trabajo es **generar una alarma** enviando un log al [[14 - SOC & SIEM|SIEM]] para que un analista lo investigue.
   - *Ventaja:* Como solo es un observador pasivo, si el IDS se rompe o se equivoca (Falso Positivo), la red sigue funcionando y el tráfico legítimo no es afectado.

2. **IPS (Intrusion Prevention System):**
   - Es un control **Preventivo**.
   - Funciona "In-Line" (el cable entra al IPS y sale del IPS). El tráfico DEBE pasar a través de él.
   - Si ve un ataque, el IPS **destruye el paquete malicioso** e interrumpe la conexión inmediatamente.
   - *Desventaja:* Si se equivoca al clasificar algo legítimo como ataque (Falso Positivo), bloqueará al usuario legítimo interrumpiendo el negocio de la empresa. Su calibración es muy delicada.

*(Hoy en día, casi no se compran IDS/IPS por separado; vienen integrados como módulos dentro de los NGFW).*

---

## 🕵️‍♂️ 3. Proxies (Forward Proxy)

Un **Proxy** actúa como un intermediario o "corredor" entre los usuarios de una red interna e Internet.

Si querés entrar a `youtube.com`:
1. Tu PC no se conecta a YouTube. Se conecta al Servidor Proxy interno y le dice: *"Por favor, traeme el video de Youtube"*.
2. El Proxy va a Internet, se conecta a YouTube usando **su propia IP**, descarga el video, lo escanea en busca de malware, y te lo entrega.
3. YouTube nunca supo quién eras; solo vio la IP del Proxy.

**Funciones de seguridad de un Proxy corporativo:**
- **Filtrado de Contenido / Web Filtering:** El proxy bloquea sitios no permitidos por la política (ej. Pornografía, sitios de apuestas, servidores de malware conocidos).
- **Aislamiento:** El usuario no toca directamente el Internet, el Proxy lo hace por él, previniendo infecciones directas.
- **Auditoría:** El Proxy mantiene un registro exacto (Log) de cada URL (Capa 7) que cada empleado visitó a cada minuto.

---

## 📌 Must Know (Imprescindible)
- Diferencia fundamental entre Firewall (reglas L3/L4) y NGFW (inspección L7).
- Diferencia crítica entre IDS (Detectivo/Alarma) e IPS (Preventivo/Bloqueo).
- El concepto de un Firewall "Stateful" (con memoria de conexiones).
- La función de un Proxy (Filtrado de contenido web, intermediario).

---

## 🔄 Preguntas de repaso
1. Un analista de redes se queja de que configurar un firewall antiguo de *Packet Filtering* es frustrante, porque tiene que escribir dos reglas para cada conexión (una de ida y una de vuelta). ¿Qué tecnología de firewalls (Generación 2) solucionó este problema y cómo?
2. Un banco implementa un dispositivo de seguridad. Un día el dispositivo falla (por error de software) identificando el tráfico legítimo de los clientes hacia el Home Banking como un ataque de DDoS, y bloquea todo acceso al sitio web durante 2 horas. ¿Qué tipo de dispositivo era probablemente, un IDS o un IPS? Justificá.
3. Si la política de una empresa requiere bloquear el acceso específicamente a la URL `www.ejemplo.com/juegos` (pero permitir el resto de `www.ejemplo.com`), ¿qué tecnología deberías usar: un Firewall tradicional (L3/L4) o un Web Proxy / NGFW (L7)? ¿Por qué?

**➡️ Siguiente nota:** [[26 - Herramientas de Red en CLI]]
