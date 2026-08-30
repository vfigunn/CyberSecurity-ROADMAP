# Lab 12.1 - Asaltando el Wi-Fi Corporativo

## 🎯 Objetivo
Visualizar el flujo cronológico de un Asalto Inalámbrico (Red Team WPA2). Entender cómo combinar las herramientas de Kali Linux (Aircrack-ng) con tácticas de Ingeniería Social pasiva (Evil Twin) para by-passear contraseñas complejas desde el estacionamiento de la empresa.

---

## 📜 El Escenario (Operación "Aire Abierto")

Fuiste contratado por la Junta Directiva de la empresa "TechCorp". 
Te prohíben entrar físicamente al edificio. Tu objetivo es vulnerar su perímetro desde la calle y demostrar si su seguridad inalámbrica sirve o no.

### Fase 1: El Radar Ciego (Airmon-ng y Airodump-ng)
**Día 1 - 09:00 AM (En el estacionamiento):**
- Estás en tu auto con tu computadora y una Antena USB Alfa Network de alto alcance.
- Ejecutás `airmon-ng start wlan0` para convertir tu antena en un espía ciego (Modo Monitor).
- Ejecutás `airodump-ng wlan0mon`. Tu pantalla se llena de BSSIDs y nombres de redes de los edificios vecinos.
- Identificás tu objetivo: `TechCorp_WPA2` (BSSID: `00:11:22:AA:BB:CC`), operando en el Canal 11.
- Enfocás el radar: `airodump-ng -c 11 --bssid 00:11... -w Captura_TechCorp wlan0mon`. Ahora estás grabando el aire exclusivamente en ese canal.

### Fase 2: Forzando el Handshake (Aireplay-ng)
**Día 1 - 09:15 AM:**
- A través del radar, ves que hay 3 empleados (Clientes) ya conectados a la red trabajando pacíficamente.
- Como ya están conectados, no van a generar un Handshake de inicio de sesión. Tenés que patearlos.
- Abrís una segunda terminal y cargás el cañón de inyección: `aireplay-ng -0 10 -a 00:11... -c [MAC_del_Empleado] wlan0mon`.
- Inyectás 10 paquetes masivos de "Deauth" (Desautenticación). El celular del empleado en el 2do piso se desconecta.
- En 1 segundo, el celular del empleado intenta reconectarse por instinto. 
- ¡BINGO! Tu primer terminal (Airodump-ng) muestra el texto mágico arriba: `[ WPA Handshake: 00:11:22... ]`. Ya capturaste la firma criptográfica en el aire.

### Fase 3: La Trituradora (Fuerza Bruta)
**Día 1 - 10:00 AM:**
- Apagás las antenas. No necesitás más interacción física.
- En Kali Linux ejecutás `aircrack-ng Captura_TechCorp.cap -w rockyou.txt`.
- La herramienta (usando tu placa de video) prueba 5 millones de contraseñas por segundo.
- **Fracaso.** Pasan 3 horas y Aircrack-ng dice *"Passphrase not found"*. TechCorp usa una contraseña aleatoria de 40 caracteres con símbolos (Imposible de crackear con fuerza bruta matemática).

### Fase 4: Cambio de Estrategia (Evil Twin y Portal Cautivo)
**Día 2 - 08:30 AM (Plan B):**
- La matemática falló. Vamos a atacar la estupidez de los usuarios.
- Desde tu auto, disparás un ataque `aireplay-ng` infinito y permanente en bucle (DDoS) contra el router de TechCorp. **La oficina entera se queda sin Wi-Fi.** Los empleados están furiosos y desesperados.
- Encendés una *segunda antena* tuya, y creás un Access Point Falso (Rogue AP) usando la herramienta `Wifiphisher`. Lo llamás exactamente igual: `TechCorp_WPA2`. Y no le ponés contraseña para conectarse.
- Los celulares de los empleados desesperados ven que el router original está muerto, ven tu router abierto (con el mismo nombre) y se conectan a vos mágicamente.

### Fase 5: El Engaño Psicológico
**Día 2 - 08:45 AM:**
- Los empleados están conectados a tu Antena, pero no les das Internet.
- Un empleado abre Chrome para ver las noticias. Tu servidor DNS interno redirige su tráfico a una página web falsa que vos creaste alojada en tu propia computadora.
- La página web ocupa toda la pantalla del empleado, tiene el Logo Perfecto de TechCorp y dice: *"Actualización Urgente del Firmware del Router. Por favor, introduzca la contraseña oficial del Wi-Fi de TechCorp para reanudar su conexión a Internet"*.
- El empleado, irritado por no poder trabajar, escribe su super contraseña de 40 caracteres y le da Enter.
- ¡Boom! La contraseña real y plana te llega directamente a tu terminal de Kali Linux en formato texto.

---

## 📝 Conclusión del Laboratorio
El Hacking Inalámbrico moderno te enseña que **las murallas matemáticas de WPA2 son perfectas, pero los humanos son extremadamente débiles**.
Un Red Teamer no se rinde ante una clave larga. Aplica Ingeniería Social local (Gemelos Malvados) explotando la dependencia tecnológica del usuario y las fallas del diseño (Deauth Masivo sin encriptación) del estándar Wi-Fi original.

**➡️ Siguiente nota:** [[07 - Ejercicios]]
