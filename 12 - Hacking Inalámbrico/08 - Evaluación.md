# 08 - Evaluación del Módulo 12 (Wi-Fi y Mobile)

## 📝 Instrucciones
Evaluación final del módulo. Múltiples preguntas basadas en los estándares 802.11, la inyección del aire, y las mecánicas para subvertir contraseñas WPA2.
Anotá tus respuestas y luego verificalas con el solucionario.

> **Importante:** Las respuestas correctas y explicaciones están en: `[[15 - Evaluaciones/Respuestas/Módulo 12 - Respuestas]]`.

---

## 🎯 Sección 1: Teoría Inalámbrica y El Aire

**1. Sos un consultor de Seguridad Ofensiva (Red Team) y te entregan una laptop genérica con Windows 10 para comenzar la auditoría de intrusión perimetral de un Shopping Center (Robo de contraseñas de las tiendas). Te rehusás y explicás que el 90% de las tarjetas o placas integradas base de las laptops bloquean y capan funciones de espionaje 802.11 vitales y mandatarias. ¿Qué "Modo" o estado específico debés obligatoriamente poder habilitar en tu placa (usando adaptadores USB Alfa / Atheros) para que la antena abandone su filtro y logre "capturar/escuchar" masivamente todo el espectro de tráfico de red alienígena ajeno a ti?**
A) Modo Promiscuo Inalámbrico / Modo Monitor
B) Modo de Cifrado Maestro (WEP Master)
C) Modo Administrado (Managed Mode / Normal)
D) Routing BGP Global

**2. Para engañar y visualizar el objetivo correcto en la terminal de Kali Linux, necesitás discernir entre el nombre "Público y falsificable" del Router contra su Huella Digital de Hardware. ¿Qué siglas se utilizan técnicamente en las herramientas como `airodump-ng` para referirse a la "Dirección MAC" inalterable y cruda del Acces Point emisor, en contraposición al nombre comercial visible (Ej. "Fibertel-Visitantes")?**
A) PUID y ESSID
B) IPv4 Route y DNS Sec
C) BSSID (Dirección MAC) frente al ESSID (Nombre en texto)
D) VLAN Tag e IEEE Name

**3. Te percatás de que la red objetivo que debes vulnerar no es del obsoleto y fracturable formato de 1999 (WEP), sino que opera firmemente utilizando WPA2-PSK (AES). A nivel puramente conceptual e inicial, ¿Qué bloque lógico o "archivo/firma criptográfica" imperativo estás forzado a intentar capturar volando por el aire para luego podértelo llevar a tu computadora de escritorio en tu casa e intentar atacarlo con diccionarios de Fuerza Bruta para desvelar la contraseña real?**
A) El ARP Spoofing File
B) El 4-Way Handshake
C) El Payload Meterpreter .exe
D) El Ticket Kerberos de 10 Años

---

## 🎯 Sección 2: El Ataque Práctico

**4. Estás aparcado en tu auto enfrente de la corporación intentando interceptar el WPA2 "Handshake". Tu radar principal espía (`airodump-ng`) está grabando pasivamente en el Canal correcto, pero ves que hace dos horas no entra ni sale nadie nuevo de las oficinas (Todos los clientes están conectados a sus teléfonos fijos desde las 09 AM). ¿Qué debes inyectar artificial y violentamente (utilizando `aireplay-ng`) con el objeto final de interrumpir la calma temporalmente, causar pánico y forzar la re-conexión automática de un cliente/celular hacia la antena central?**
A) Inyección masiva de Malware .apk al celular.
B) Ataque de Paquetes "Deauth" (Desautenticación)
C) Ejecutar un script de SQLi contra la base de datos de la operadora del ISP.
D) Intercepción BGP del router a nivel mundial.

**5. Realizaste las fases anteriores a la perfección. Regresás a tu "Laboratorio u Oficina" (Offline/Sin estar cerca de la empresa ya) y tenés en tu poder el archivo `.cap` conteniendo el tan ansiado y cifrado Handshake corporativo en tus manos. ¿Qué programa o aplicación mundial es el núcleo y trituradora criptográfica local final encargada de procesar ese archivo e iterar diccionarios millonarios (`rockyou.txt`) a base de GPU para adivinar y exponer en pantalla la clave de "Texto Plano"?**
A) Hashcat (o Aircrack-ng puro)
B) Responder / NTLM Relay
C) Wireshark Master Viewer
D) Cobalt Strike Team Server

**6. Si una corporación usa una política de clave de "64 Caracteres Aleatorios generados en bóveda" para su router de oficina, el equipo de Pentesting no va a malgastar un millón de años ejecutando GPU/Fuerza Bruta contra el Handshake capturado, porque la matemática los destruiría. ¿Qué vector o táctica paralela, centrada 100% en la Ingeniería Social al empleado desesperado, implementa el Atacante combinando un Router Falso Gratuito + Portal Cautivo para evadir la matemática y forzar a que sea el humano quien revele la información confidencial?**
A) Ataque por Diccionario Básico en Linux
B) Ataque "Evil Twin" (Gemelo Malvado)
C) Pass-The-Ticket
D) Kerberoasting de Red Inalámbrica

---

## 🎯 Sección 3: Dispositivos Móviles (Mobile Top 10)

**7. Un Arquitecto de Software programa una Aplicación Bancaria móvil oficial para un importante banco, la cual se distribuirá en Play Store. Para facilitar el sistema y la base de datos en teléfonos Android, el arquitecto guarda el "Token VIP de Sesión de Transferencias" del cliente en un archivo local SQLite `.db` con el texto "Crudo" (Sin cifrado) en lo más profundo de las carpetas internas de la App, creyendo firmemente que el formato Sandboxing normal de Android lo vuelve inalcanzable para otros procesos. ¿De qué vulnerabilidad fundamental listada y clasificada por OWASP Mobile Top 10 estamos hablando?**
A) M2 - Insecure Data Storage (Almacenamiento Local Inseguro)
B) M7 - Client Side Injection
C) XSS Reflejado en HTML5
D) M10 - Vulnerabilidad de Nmap y BGP

**8. Enlazado al caso anterior, el Blue Team Móvil justifica las decisiones del programador esgrimiendo: *"No pasa nada si el Token de sesión bancario se almacena sin encriptar, porque el usuario y dueño normal de su celular estándar (Comprado en fábrica) no posee privilegios administrativos lógicos para navegar carpetas /data internas o abrir bases de datos propias de otra App"*. ¿Cuál es la táctica o procedimiento técnico agresivo (cuyo nombre varía entre el SO de Google o el de Apple) que un ladrón aplicaría sobre el teléfono robado, garantizándole escapar del Sandboxing, lograr los privilegios de un "Dios absoluto" y poder visualizar toda memoria para robar el token plano?**
A) Descompilarlo en Wireshark
B) Instalar un Antivirus de prueba (Avast).
C) Realizar "Rooting" (Android) o "Jailbreak" (iOS) en el dispositivo local.
D) Usar Bloodhound Local

---

> **¿Terminaste?**
> Buscá el archivo en la carpeta de evaluaciones para comparar tus resultados (Módulo 12 - Respuestas).

**➡️ Siguiente nota:** [[09 - Resumen]]
