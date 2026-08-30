# Respuestas Evaluación Módulo 12 - Hacking Inalámbrico

A continuación se presentan las respuestas correctas de la evaluación del [[12 - Hacking Inalámbrico/08 - Evaluación|Módulo 12]], junto con la justificación técnica de cada una.

---

### Sección 1: Teoría Inalámbrica y El Aire

**1. A) Modo Promiscuo Inalámbrico / Modo Monitor**
> *Justificación:* Los hardwares de consumo general están diseñados para el "Modo Administrado" (Filtro por MAC). Si querés realizar Hacking o Auditoría, necesitás forzosamente una tarjeta especializada (Famosamente chips Realtek/Atheros) cuyas funciones permitidas en Kali Linux posibilitan desbloquear el "Modo Monitor", permitiendo a la antena captar y engullir pasivamente indiscriminadamente todos los Handshakes voladores del entorno local, ignorando por completo el concepto de destinación del paquete.

**2. C) BSSID (Dirección MAC) frente al ESSID (Nombre en texto)**
> *Justificación:* El engaño al ojo humano radica en el texto (ESSID), donde tú podrías ver 10 redes llamándose "Cafeteria-Publico". Sin embargo, tu herramienta ofensiva Inalámbrica (`airodump` y el cañón de inyección) enfocarán sus matemáticas puramente al Localizador de Identificación de Hardware o Base Service Set Identifier (`BSSID`), el cual se manifiesta globalmente como la "Dirección MAC Exclusiva y cruda" (Ej. 0A:11:BB:CC) del Router o AP del objetivo a atacar.

**3. B) El 4-Way Handshake**
> *Justificación:* La criptografía del Wi-Fi Protocol Access 2 (WPA2) impidió que los atacantes deduzcan los vectores crudos en el aire sin diccionario como solía pasar en 1999 (WEP). Actualmente el foco masivo inicial de TODO atacante inalámbrico, es posicionar su radar en el instante exacto en que un usuario auténtico "Inicia Sesión", logrando interceptar los "4 pasos matemáticos del Apretón de Manos (Handshake)", siendo el único archivo/objeto `.cap` exportable del cual se depende estrictamente para adivinar el password en casa usando GPU.

---

### Sección 2: El Ataque Práctico

**4. B) Ataque de Paquetes "Deauth" (Desautenticación)**
> *Justificación:* El estándar 802.11 no incluyó (hasta WPA3) ninguna medida de Encriptación Estricta sobre las tramas básicas de Administración/Gestión (Management Frames). Por ende, tu herramienta `aireplay-ng` simplemente inyecta y falsifica un aluvión de paquetes ciegos fingiendo ser la MAC del router, obligando al sistema y al celular a sufrir un Drop/Desconexión ("Deauth"), facilitando masivamente tu obtención automática de un nuevo Handshake fresco cuando la víctima asustada vuelva a conectar sola 2 segundos después.

**5. A) Hashcat (o Aircrack-ng puro)**
> *Justificación:* Obtuviste el Handshake (El Enigma), pero carecés de la clave real. La herramienta offline por antonomasia y excelencia destructiva para iterar e inyectar velocidad masiva contra Hashes de WPA y Hashes en general de Active Directory (Empleando procesadores de Tarjetas Gráficas de altísimo flujo de cálculo en vez del CPU), se denomina `Hashcat` (Junto con las opciones clásicas de la suite `aircrack-ng`).

**6. B) Ataque "Evil Twin" (Gemelo Malvado)**
> *Justificación:* La criptografía irrompible de 64 caracteres fuerza al Atacante a evadir el sistema (Hardware/WPA2) y asestar el golpe contra "La Capa Humana" (Ingeniería Social). El Evil Twin (Gemelo) clona y enarbola un punto de acceso impostor idéntico con el objetivo de engañar al usuario incomunicado de la empresa (Tras un DDoS de Deauth) obligándolo de forma pacífica y creíble (Portal Web Cautivo Falso) a depositar libremente su colosal y monstruosa contraseña original, sin tener que aplicar ni 1% de fuerza bruta.

---

### Sección 3: Dispositivos Móviles (Mobile Top 10)

**7. A) M2 - Insecure Data Storage (Almacenamiento Local Inseguro)**
> *Justificación:* En Programación y Arquitectura Móvil Empresarial, es fundamental asumir un ecosistema híbrido hostil en el "Bolsillo" del usuario (Bajo la doctrina de la Defensa en Profundidad). Abandonar Archivos SQL/Texto expuestos localmente sin utilizar una capa de cifrado "App-Level" adicional interna (AES256 EncryptedDB), bajo la excusa de creer que los muros orgánicos nativos de Android resguardarán el entorno de agresiones de memoria (Mobile Top 10 / Insecure Data Storage), desencadena siempre vulneraciones de credenciales VIP de altísimo impacto y robo de identidad si el usuario sufriera un robo.

**8. C) Realizar "Rooting" (Android) o "Jailbreak" (iOS) en el dispositivo local.**
> *Justificación:* El Sandbox (Caja de Arena Aislante) oficial tanto en plataformas Google como de Apple sirve y rige para el "Usuario Normal y Regular/Standard". El Atacante Mobile (O un propietario incauto) destraba deliberadamente, mediante hazañas (Exploits LPE de Kernel o Bootrom) su celular para obtener privilegios máximos omnipotentes o "Status de Superusuario Absoluto". Las aplicaciones Financieras o de Bancos incorporan medidas reactivas anti-ejecución bloqueando y cerrando su uso de emergencia si el Teléfono Móvil local expone signos evidentes de hallarse operando bajo estado de **Jailbroken** o **Rooted**.
