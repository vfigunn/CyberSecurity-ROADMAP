# Respuestas Evaluación Módulo 10 - Active Directory

A continuación se presentan las respuestas correctas de la evaluación del [[10 - Active Directory/10 - Evaluación|Módulo 10]], junto con la justificación técnica de cada una.

---

### Sección 1: Reconocimiento y Envenenamiento

**1. B) BloodHound (Mediante el recolector SharpHound)**
> *Justificación:* Al adentrarte a ciegas en un corporativo masivo de 5.000 usuarios, encontrar a "quién atacar" de manera manual, sin generar ruido excesivo (Auditoría), es casi imposible. El recolector `SharpHound` consulta legal y orgánicamente al servidor central por todos los parámetros de seguridad permitidos, y `BloodHound` cruza esos datos mediante Teoría de Grafos 3D descubriendo qué rutas lógicas o saltos (Pathways) conectan a un usuario ordinario con la joya suprema (El Administrador de Dominio).

**2. B) Windows activa los protocolos LLMNR / NBT-NS y comienza a gritar y preguntar en "Broadcast" a todos los equipos locales si alguno conoce o sabe quién es `Cerrvidor-Files`.**
> *Justificación:* Las corporaciones, por motivos de compatibilidad nativa extrema para entornos de oficinas sin infraestructura DNS madura ("Computadoras Peer-to-Peer caseras"), mantienen activados de fábrica los Protocolos Base de Multidifusión local (Link-Local Multicast Name Resolution). Estos "Gritos" ensordecedores a la red en la Capa 2 abren la puerta gigante que todo cibercriminal espera pacientemente explotar para realizar inyecciones.

**3. D) NetNTLMv2**
> *Justificación:* Cuando engañamos a la víctima y la forzamos a intentar validar su identidad contra nuestra máquina espía (Responder), Windows NO viaja y entrega su Hash NTLM estático crudo, mucho menos el texto plano. Entrega un hash en formato Dinámico (Hash sobre Hash / Challange-Response) conocido como NetNTLMv2. Este Hash no puede ser reciclado directamente (Pass-The-Hash), sino que se destina para ser atacado offline (Cracking mediante fuerza bruta / GPU Hashcat) o ser rebotado en vivo hacia terceros (NTLM Relaying).

---

### Sección 2: Protocolos, Relay y Kerberoasting

**4. B) Pass-The-Hash (PTH)**
> *Justificación:* En Ciberseguridad Ofensiva "AD-Centric", descubrir la contraseña original real en texto (Ej. Password123) carece de practicidad en muchos casos. Gracias a la flaqueza del protocolo Base Microsoft, la autenticación y validez corporativa final descansa netamente en la matemática de su Hash. Inyectar o "Pasar el Hash Estático NT" crudo vía consola (PTH) confunde al servidor, el cual aprueba legalmente el ticket como si tú fueses el mismísimo dueño biológico tecleándolo.

**5. B) El intercambio de Boletos o Tickets TGT y TGS encriptados.**
> *Justificación:* Ante los constantes robos en memoria (PTH) y rebotes ciegos en red de NTLM (Relay), Kerberos se convirtió en el Rey de la Industria. Con Kerberos, tú no vas a tu computadora y gritas tus contraseñas por el aire a ciegas; le pides un Boleto Central al DC que prueba tu identidad (TGT), y con él reclamas Pases de acceso individual/específico y encriptado hacia cada sala de servidores (TGS), aislando masivamente los robos cruzados en red.

**6. B) Kerberoasting**
> *Justificación:* En lugar de romper software (Exploit de Buffer Overflow), Kerberoasting es el rey del "Abuso por Diseño Legítimo" (Misconfiguration-Abuse). Todo empleado está moralmente capacitado para solicitarle al Controlador de Dominio un TGS apuntando hacia un servicio público de alto valor (Base de Datos/Service Principal Name o SPN). Como el Controlador entrega este TGS validado matemáticamente (es decir, **encriptado en base al Hash NT maestro** del propio servicio SQL), el atacante desvía el vuelo, y mediante procesamiento masivo tritura y rompe la debilidad de la encriptación externa obteniendo su interior llano (Offline SPN-Cracking / Kerberoasting).

---

### Sección 3: Extracción Total y Persistencia Absoluta

**7. C) `NTDS.dit` (Active Directory DB)**
> *Justificación:* Si estás en un juego de Capture The Flag (CTF), o ejecutando una simulación de Red Team millonaria en la vida real, el botín y bandera final siempre tiene el mismo nombre. Obtener y extraer a tu terminal el archivo Base (Directory Information Tree o `.dit`), equivale a la destitución inmediata y Game Over corporativo masivo porque el 100% de la identidad biológica de las máquinas y humanos corporativos conviven centralizados allí.

**8. B) `krbtgt` (Key Distribution Center Service Account)**
> *Justificación:* Si tú intentaras generar "Tu propio Billete Dólar falso o tu TGT en casa", el escáner del Banco lo rechazaría por no poseer la tinta magnética validada. Kerberos (Como Boletero), valida todos los boletos chequeando que posean la misma firma oficial base. Dicha firma oficial es el Hash encriptado criptográficamente derivado eternamente de una cuenta oculta no interactiva: La cuenta Maestra de `krbtgt`. Falsificando su firma (Hash) se imprime la puerta trasera más legendaria e inmortal: El Golden Ticket.

**9. B) Porque el Silver Ticket (Al falsificar un TGS directo a un servidor específico) saltea, ignora, y **NUNCA entra en contacto directo con el Controlador de Dominio (DC)** (El cual es el punto de vigilancia supremo donde el Blue Team audita todos los pases), mientras que el Golden Ticket es global y muy ruidoso.**
> *Justificación:* El Golden Ticket es masivo y global, requiriendo solicitar su validación y paso inicial (TGT) por el Gran DC. El Silver Ticket, por el contrario, requiere que le robes directamente la firma al servidor específico en cuestión (Ej. File Server / SPN account) y fabriques un ticket pase TGS que ataca y viaja directamente desde ti hacia él. A los ojos del Gran Controlador de Dominio principal, este tráfico periférico (Lateral), evadirá drásticamente sus métricas y registros (SIEM).
