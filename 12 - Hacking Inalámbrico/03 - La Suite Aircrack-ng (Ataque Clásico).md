# 03 - La Suite Aircrack-ng (Ataque Clásico)

## 🎯 Objetivos
- Conocer la herramienta pilar del Hacking Inalámbrico presente en Kali Linux.
- Aprender el flujo de 4 comandos esenciales (`airmon-ng`, `airodump-ng`, `aireplay-ng`, `aircrack-ng`).
- Entender el Ataque de Desautenticación (Forzando el Handshake).

---

## 🧠 Concepto: El Arsenal Aircrack-ng

En la nota pasada dijimos que para capturar el Handshake, el hacker tenía que tener la suerte de estar ahí en el momento exacto en el que el empleado llega y se conecta.
Pero los hackers no tienen paciencia. Si llegaste a las 11:00 AM y todos ya están conectados... **Los expulsás de la red a la fuerza, haciendo que se reconecten.**

Todo el ataque se ejecuta utilizando la suite de software `Aircrack-ng` (Preinstalada en todos los Linux de Hacking). Son 4 programas (herramientas) que cumplen tareas separadas.

---

## 🛠️ El Flujo del Ataque (Los 4 pasos)

### 1. Activar el Oído (`airmon-ng`)
Tenés que transformar tu antena normal (`wlan0`) en un Modo Monitor (Antena de espionaje).
> `airmon-ng start wlan0`
*(Ahora tu placa pasó de llamarse `wlan0` a `wlan0mon` indicando que es un Monitor activo).*

### 2. Escanear el Radar (`airodump-ng`)
Prendés el radar para ver qué redes Wi-Fi flotan por tu cuadra.
> `airodump-ng wlan0mon`
La pantalla negra se llena de BSSID (Direcciones MAC) y Nombres de redes. 
Elegís a tu víctima: *"Ahí está el Banco, funciona en el Canal 6"*.
Reiniciás el radar pero **fijando la puntería solo en ese canal**, y le decís al programa que empiece a grabar en un archivo `.cap` todo lo que escucha.
> `airodump-ng -c 6 --bssid [MAC_del_Banco] -w Captura_Banco wlan0mon`
*(Ahora estás escuchando pasivamente, esperando el Handshake).*

### 3. La Patada de Red (Desautenticación con `aireplay-ng`)
Como nadie se está conectando, usás el cañón de inyección de paquetes. 
Apuntás un misil sordo al Celular del empleado. Le inyectás "Paquetes de Deauth" al aire.
> `aireplay-ng -0 5 -a [MAC_del_Banco] -c [MAC_del_Empleado] wlan0mon`
**¿Qué pasa acá?** Tu antena falsifica la MAC del Banco y le grita al celular: *"¡Ey, soy el Router, salí de la red, te desconecto!"*. 
El celular del empleado pierde el Wi-Fi por 1 segundo. El celular inmediatamente, de forma automática, dice: *"Uy, se cortó, me vuelvo a conectar rapidísimo"*.
Al volverse a conectar... **¡El celular y el Router vuelven a generar el 4-Way Handshake en el aire!**
Tu radar (`airodump-ng`), que estaba grabando en segundo plano, te avisa arriba a la derecha: `[WPA Handshake: Capturado!]`. Ya ganaste la mitad de la batalla.

### 4. La Trituradora Criptográfica (`aircrack-ng`)
Cerrás todo. Te llevás tu archivo `.cap` (que contiene el saludo encriptado) a tu casa.
Iniciás la trituradora pasándole un Diccionario Gigante de contraseñas, y el archivo que grabaste.
> `aircrack-ng Captura_Banco.cap -w diccionario_mundial.txt`
El programa probará contraseñas sin parar (Sin interactuar con el router, puramente matemático). Si la contraseña del banco era débil (Ej: "Bancazo123"), la herramienta explotará el saludo y te mostrará la clave en pantalla (KEY FOUND!).

---

## 📌 Must Know (Imprescindible)
- **airodump-ng:** El Radar (Para escuchar los BSSIDs, clientes conectados y atrapar el `.cap`).
- **aireplay-ng:** El Cañón Inyector (Para mandar paquetes de de-autenticación al aire y forzar la desconexión del cliente, obligándolo a generar un Handshake de re-conexión automatizado).
- **aircrack-ng:** El Triturador (Herramienta offline que toma el .cap y el diccionario de palabras, rompiendo la fuerza bruta por CPU/GPU).

---

## 🔄 Preguntas de repaso
1. Al realizar una auditoría (Pentesting Inalámbrico), enciendes el radar principal (Airodump-ng) para escanear las redes del edificio, pero el programa colapsa o no devuelve ningún resultado (No logra encontrar la placa). Como paso obligatorio anterior a lanzar tu radar, ¿Qué herramienta de la suite debiste haber utilizado para "matar" los procesos restrictivos de red de Linux y alterar tu tarjeta inalámbrica `wlan0` a Modo Promiscuo Inalámbrico (`wlan0mon`)?
2. Estás espiando pasivamente una red corporativa muy estable. Nadie nuevo llega ni se conecta. Sabiendo que el Handshake (La información cifrada para iniciar sesión en WPA2) solo ocurre una única vez al conectarse por primera vez; ¿Qué herramienta ofensiva agresiva (aireplay-ng) y qué "Tipo de Paquetes específicos 802.11" debés falsificar e inyectar al aire para interrumpir y causar que los teléfonos se reconecten automáticamente, vomitando el Handshake hacia tu radar?
3. Sabiendo cómo opera internamente el Comando #4 de la trituración final (`aircrack-ng`); ¿Por qué se dice que el atacante (tras haber robado y grabado exitosamente el archivo `.cap` en la calle) "desconecta" su antena, regresa a su casa, y ni siquiera necesita estar a 10 kilómetros del edificio objetivo ni tener Internet para poder adivinar la contraseña oficial del Wi-Fi?

**➡️ Siguiente nota:** [[04 - Ataques de Denegación (Deauth) y Evil Twin]]
