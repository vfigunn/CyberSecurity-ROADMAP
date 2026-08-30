# 11 - Esteganografía (Ocultando a plena vista)

## 🎯 Objetivos
- Conocer una técnica alternativa y complementaria a la criptografía.
- Entender el concepto de "Ocultar la existencia del mensaje".
- Aprender las técnicas básicas (Manipulación del LSB y archivos adjuntos) usadas frecuentemente por el Malware moderno.

---

## 🧠 Concepto: Seguridad por Oscuridad

La Criptografía es excelente protegiendo secretos (Confidencialidad), pero tiene un "problema" en ciertos entornos restrictivos: **Llama muchísimo la atención**.

Si enviás un archivo encriptado masivo a través del Firewall de una empresa, los sistemas de seguridad defensivos (IPS/DLP) lo detectarán de inmediato. Verán un bloque de basura ilegible y dirán: *"No puedo leer esto, pero sé que es un archivo encriptado. Probablemente es un robo de datos (Exfiltración), lo voy a bloquear"*.

La **Esteganografía** (del griego "escritura oculta") es el arte de ocultar no solo el contenido del mensaje, sino **ocultar el hecho de que el mensaje siquiera existe**. El objetivo es que los sensores de seguridad vean pasar un archivo totalmente inocente y lo dejen salir de la empresa sin sospechar nada. 

---

## 🖼️ Técnicas Digitales Comunes

Aunque se puede ocultar información en archivos de audio (.wav) o de video (.mp4), el portador universal de la esteganografía en ciberseguridad es la **Imagen (.jpg / .png)**.

### 1. El Bit Menos Significativo (LSB - Least Significant Bit)
Esta es la magia negra del hacking. 
Las imágenes digitales están formadas por millones de píxeles. El color de cada píxel se define por valores matemáticos (Bytes) que van del 0 al 255 en el canal RGB (Rojo, Verde, Azul).

Por ejemplo, un píxel es Rojo puro, con valor binario: `11110000`.
Si el atacante toma tu secreto (ej. el plano de un avión robado), lo divide en puros Unos y Ceros, y reemplaza sutilmente el **último bit** (el menos significativo) de cada píxel de una foto inocente de un paisaje por los bits del plano secreto.

- El píxel original era: `11110000`
- El píxel inyectado es: `11110001`
- **El resultado en la vida real:** Matemáticamente la imagen cambió. Pero para el ojo humano (e incluso para la mayoría de los análisis automatizados del Firewall), el tono de Rojo del píxel cambió de forma tan insignificante (1/255 de diferencia) que la foto del paisaje se ve absolutamente perfecta y normal.

El atacante envía la foto del paisaje inocente por correo a su cuenta personal. El Firewall la analiza, ve que es solo una foto de 3 Megabytes, no detecta encriptación, y la deja salir. El atacante llega a su casa, usa una herramienta de ingeniería inversa, extrae el último bit de cada píxel, y rearma el archivo del plano de avión robado. (Típicamente, el mensaje escondido primero se Encripta con AES y *luego* se le aplica Esteganografía LSB).

### 2. Archivos Empaquetados / Final del archivo (EOF)
Mucho más simple que el LSB (y más fácilmente detectable por Blue Teams).
Cada archivo tiene una estructura que le dice a la computadora dónde empieza y dónde termina. El Final de un Archivo de imagen (JPG) tiene una firma hexadecimal llamada EOF (End Of File).

Un atacante puede tomar un archivo ZIP (con malware adentro) y usar la consola de Linux para literalmente "pegarlo" al final del código hexadecimal de la imagen JPG, por debajo de la firma de cierre.
Cuando el usuario víctima hace doble clic en la imagen, el visor de Windows lee la foto hasta el cierre EOF y se detiene (mostrando la foto normal y creyendo que ahí terminó todo). Pero el malware oculto (el ZIP) viaja pasivamente adherido y puede ser extraído si se le pasa la herramienta adecuada.

---

## ❓ ¿Por qué importa en Seguridad?

La Esteganografía es una pesadilla para el Blue Team y un arma elegante para las Amenazas Persistentes Avanzadas (APTs).

- **Comunicaciones C2 (Command & Control):**
  Un malware (troyano) infecta tu máquina empresarial. Necesita recibir las órdenes del atacante (Ej. "Inicia el ataque de Ransomware"). Pero los servidores de la empresa bloquean el tráfico sospechoso.
  El malware simplemente se conecta a la cuenta de Twitter/X del atacante de forma legítima, y descarga el último "Meme" (imagen en PNG) que el atacante publicó en la red social. El malware analiza matemáticamente los píxeles (LSB) del meme, extrae el comando encriptado ("iniciar ataque"), y lo ejecuta. Para el SOC de la empresa, simplemente pareció que un empleado abrió Twitter y descargó un meme. Esto es 100% indetectable en la capa de red tradicional.

---

## 📌 Must Know (Imprescindible)
- La principal diferencia táctica: La criptografía oculta *qué significa* el mensaje (su legibilidad); la esteganografía oculta *el hecho de que existe* un mensaje.
- Entender el concepto general de alterar el **Least Significant Bit (LSB)** para esconder datos masivos dentro de los píxeles de una imagen sin alterar su visualización.
- Saber que en un entorno forense riguroso, las técnicas básicas de esteganografía pueden ser detectadas (usando esteganálisis y comparando los hashes originales vs los alterados de la foto).

---

## 🔄 Preguntas de repaso
1. Una corporación bancaria implementó un avanzado sistema Data Loss Prevention (DLP) que bloquea y alerta sobre cualquier correo electrónico saliente que contenga archivos encriptados (como ZIPs con contraseña o bloques PGP) detectando su entropía. Describí conceptualmente qué técnica utilizaría un empleado malicioso (Insider Threat) para evadir este filtro y enviar el código fuente robado al exterior utilizando fotos de sus vacaciones sin llamar la atención.
2. Si aplicás Esteganografía (usando el método LSB) para inyectar un documento de Word oculto dentro de una fotografía `.jpg` llamada `vacaciones.jpg`. ¿Qué pilar fundamental (de la Tríada CIA y las reglas del Hashing) de la imagen `vacaciones.jpg` se destruye automáticamente durante el proceso, permitiendo a un investigador forense (Blue Team) notar que la foto fue manipulada si poseía el MD5 original?
3. En tus propias palabras, explicá por qué los grupos cibercriminales avanzados (APTs) prefieren transmitir las órdenes a su Malware utilizando esteganografía a través de fotos alojadas en redes sociales públicas (como Twitter, Reddit o Imgur) en lugar de enviar las órdenes directamente por un canal TCP desde un servidor con IP estática que ellos controlan.

**➡️ Siguiente nota:** [[12 - Laboratorio - Cracking de Hashes (John The Ripper simulado)]]
