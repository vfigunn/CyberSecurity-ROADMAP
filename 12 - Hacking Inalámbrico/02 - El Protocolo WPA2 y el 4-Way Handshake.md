# 02 - El Protocolo WPA2 y el 4-Way Handshake

## 🎯 Objetivos
- Entender por qué el protocolo WEP murió y WPA2 es el estándar moderno.
- Aprender qué ocurre matemáticamente en el aire cuando alguien pone la clave del Wi-Fi.
- Conocer la meta final del atacante: La Captura del "Handshake".

---

## 🧠 Concepto: La Evolución de las Contraseñas

En 1999, las redes Wi-Fi usaban la seguridad **WEP (Wired Equivalent Privacy)**. 
Era un desastre de diseño criptográfico. En WEP, las matemáticas de la contraseña se repetían estadísticamente. 
- *El Ataque:* El hacker prendía su Antena en Modo Monitor. Dejaba la computadora capturando el aire por 15 minutos, y mágicamente podía calcular la contraseña real sin importar su longitud. (Por suerte, WEP está extinto en la actualidad).

Hoy el mundo usa **WPA2** (Y su sucesor, WPA3).
WPA2 solucionó la matemática. Ya no podés simplemente "deducir" la contraseña escuchando pasivamente la red. El algoritmo (AES) es impenetrable.

Para romper un WPA2, el atacante no ataca al Router todo el tiempo. **El atacante debe atacar el momento exacto en el que el empleado llega a la oficina y se conecta por primera vez al Wi-Fi**. Ese segundo se conoce como el "Handshake".

---

## 🤝 El Saludo Inalámbrico (4-Way Handshake)

Cuando tu celular se conecta a la red Wi-Fi de tu casa, no manda la contraseña "Password123" en texto plano volando por el aire (Si lo hiciera, cualquier hacker en Modo Monitor la podría leer).

WPA2 utiliza un apretón de manos de 4 pasos (4-Way Handshake) entre el Router y tu Celular para validar que ambos tienen la contraseña, pero sin decirla en voz alta.

**Cómo funciona (El concepto lógico):**
1. **Paso 1 (Router a Celular):** El Router genera un número aleatorio larguísimo (`Random_R`) y se lo tira al aire al celular.
2. **Paso 2 (Celular a Router):** El celular agarra el `Random_R`, le suma su propio `Random_C`, y mete todo eso junto a la Contraseña Oficial del Wi-Fi (Que el usuario ya tipeó en pantalla) en una licuadora criptográfica de Hashing (MIC). El celular le devuelve la respuesta sellada al Router por el aire.
3. **Paso 3 y 4:** El router valida que la matemática coincida, instalan las llaves de encriptación y el celular obtiene acceso a Internet.

**El Ataque (La Captura):**
Al hacker de afuera no le importan los 4 pasos. El hacker, con su antena en Modo Monitor, sabe que toda esa matemática viajó por el aire frente a su casa.
El hacker guarda esa porción de paquetes cifrados (El archivo de captura `.cap`) en su disco duro. **Acaba de atrapar el "Handshake" en el aire.**

---

## 🔨 La Fuerza Bruta Offline

El Hacker ya atrapó la conversación encriptada del inicio de sesión (El archivo `.cap`). Pero todavía NO tiene la contraseña en texto plano, ni puede conectarse al Wi-Fi gratis.
Ese Handshake no se puede revertir con ingeniería inversa.

Para descubrir la clave, el hacker se va a su casa. Apaga su Antena.
Abre su máquina trituradora (`Hashcat` / `Aircrack-ng`) con una lista de diccionarios mundiales masiva (`rockyou.txt`).
- La trituradora prueba la palabra "Primavera". Le aplica la matemática del Handshake (Paso 1 y 2). Si el resultado de la trituradora da exactamente igual al resultado del archivo `.cap` que capturó en el aire, **entonces la palabra probada era la contraseña real del Wi-Fi**.
- Si el usuario tenía una contraseña como "Auto2020", el ataque por fuerza bruta la adivinará en 5 minutos y el Hacker ya tiene la contraseña del edificio.

*(Es por esto que hoy en día, hackear una red WPA2 es un 100% dependiente de qué tan tonto o complejo fue el usuario al elegir su contraseña. Si el usuario puso un password de 24 caracteres aleatorios, el Handshake no se crackeará nunca y el Wi-Fi es invulnerable).*

---

## 📌 Must Know (Imprescindible)
- **El 4-Way Handshake:** El intercambio crítico de paquetes en WPA2 que ocurre cada vez que un dispositivo válido intenta autenticarse e iniciar sesión en el Router.
- El atacante **NO ataca al protocolo WPA2 de frente**. Su única táctica es capturar ese apretón de manos crítico en el aire, llevarlo a su casa, e intentar romper el cifrado subyacente haciendo que su placa de video adivine la contraseña (Fuerza Bruta de Diccionario).

---

## 🔄 Preguntas de repaso
1. Históricamente, las redes `WEP` cayeron en el olvido porque permitían a un atacante quebrar la contraseña matemática sin necesitar de un diccionario (crackeo pasivo/deducción mediante inyección de paquetes IVs). En las redes corporativas actuales basadas en el estándar `WPA2-PSK`, ¿por qué la técnica de escuchar el aire y deducir la contraseña ya no es viable, y a qué archivo gigantesco con millones de contraseñas mundiales (`rockyou.txt`) debe acudir el atacante para tener éxito?
2. Un atacante se estaciona a las 11:00 AM fuera de la empresa en su auto y enciende su Antena USB en Modo Monitor. Todos los empleados de la empresa llegaron a trabajar a las 09:00 AM y ya tienen sus celulares y computadoras firmemente conectados al Wi-Fi (No hay nuevos inicios de sesión). Sabiendo cómo funciona WPA2 y el intercambio de paquetes; explicá lógicamente por qué el atacante, aunque se quede capturando el aire 5 horas seguidas, jamás obtendrá el preciado "Handshake" en estas condiciones (A menos que aplique la técnica agresiva del próximo módulo).
3. Para la validación criptográfica WPA2; Contrastá y explicá con tus palabras si la contraseña en texto plano (Ej. `"ClaveCorporativa2026!"`) viaja literalmente por el aire de la habitación para ir desde el celular hasta el Router; o si en cambio ambos dispositivos utilizan la contraseña para firmar y sellar (Hash/MIC) variables dinámicas que envían de forma encriptada, ocultando así la clave plana del hacker que espía el medio inalámbrico.

**➡️ Siguiente nota:** [[03 - La Suite Aircrack-ng (Ataque Clásico)]]
