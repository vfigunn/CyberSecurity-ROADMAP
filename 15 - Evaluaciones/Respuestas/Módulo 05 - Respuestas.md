# Respuestas Evaluación Módulo 05 - Criptografía

A continuación se presentan las respuestas correctas de la evaluación del [[05 - Criptografía/14 - Evaluación|Módulo 05]], junto con la justificación técnica de cada una.

---

### Sección 1: Transformación de Datos y Hashes

**1. C) Base64 es un sistema de Encoding (codificación) que es públicamente reversible en instantes, no brinda Confidencialidad.**
> *Justificación:* El Encoding es meramente una traducción de formatos (Generalmente ASCII) para compatibilidad de transporte. No utiliza llaves criptográficas ni matemáticas complejas protectoras. Cualquier atacante puede tomar la cadena y usar la función de decodificación universal inversa (`base64 -d`) en 1 segundo.

**2. B) Integridad**
> *Justificación:* La función Hash produce una huella digital matemática de tamaño fijo. Gracias a la propiedad del "Efecto Avalancha", si un solo byte del archivo descargado fue corrompido, el Hash resultante cambiará totalmente, alertando de que la Integridad de los datos se rompió.

**3. C) Añadir una cadena de caracteres aleatorios (Salt) a cada contraseña antes de aplicarle el algoritmo hash.**
> *Justificación:* Las "Rainbow Tables" son diccionarios pre-calculados de palabras comunes. Al añadir una Sal (Salt) única por usuario (Ej: `password123+x9f$z`), el hash resultante se vuelve totalmente ajeno a la tabla precalculada, obligando al atacante a intentar romperla con fuerza bruta desde cero, perdiendo años de cómputo.

---

### Sección 2: Criptografía Simétrica y Asimétrica

**4. C) Utiliza la misma y única clave tanto para encriptar como para desencriptar, siendo extremadamente veloz.**
> *Justificación:* Ese es el concepto fundamental (y la razón de su nombre) del Cifrado Simétrico. Su baja carga matemática (comparado con la Asimétrica) lo hace ideal para procesar gigabytes de información en milisegundos (Full Disk Encryption / Tráfico HTTPS masivo con AES).

**5. C) La Llave Pública de Bob.**
> *Justificación:* La regla absoluta de la asimetría dice: "Lo que encriptas con una llave, solo se desencripta con su contraparte del mismo par". Si Alice encripta el paquete con la Pública de Bob, la matemática dicta que la **única llave en el universo entero** capaz de desencriptarlo es la Privada de Bob (la cual él protege celosamente).

**6. A) Es extremadamente lento y costoso computacionalmente en comparación con el cifrado Simétrico.**
> *Justificación:* Las operaciones matemáticas detrás de la asimetría (multiplicación y factorización de números primos de 4000 dígitos) requieren muchísima CPU. Si se utilizara para encriptar un disco de estado sólido, la computadora operaría a velocidades de tortuga.

---

### Sección 3: Arquitecturas y Firmas (PKI)

**7. C) Garantizar el No Repudio (Autoría) y la Integridad, ya que solo el autor posee la llave privada para crear esa firma.**
> *Justificación:* Al encriptar el hash con tu llave privada, te aseguras de que el mundo entero pueda desencriptarlo (con tu llave pública) y verificar tu firma. Esto destruye la Confidencialidad (pues es público), pero sella con matemática irrefutable tu Identidad (porque sos el único con la Privada necesaria para haber hecho esa firma inicial).

**8. C) Actuar como Notario (Tercero de confianza), firmando digitalmente el Certificado del Servidor para garantizar que la Llave Pública realmente le pertenece.**
> *Justificación:* Las CAs (como DigiCert o Let's Encrypt) operan como los "Gobiernos de Internet". Su firma inyectada en el Certificado Digital del Banco X le demuestra a los navegadores del mundo (Google Chrome, Firefox) que no se encuentran bajo un ataque de suplantación de identidad (Man-In-The-Middle).

**9. B) Diffie-Hellman (DH / ECDHE)**
> *Justificación:* Diffie-Hellman no es un algoritmo para encriptar un archivo. Es un algoritmo para "intercambiar y acordar" matemáticamente una llave secreta compartida (Simétrica) a través de un canal público, sin enviar la llave real. Es el motor detrás del Perfect Forward Secrecy en los navegadores modernos.

**10. D) Esteganografía**
> *Justificación:* La esteganografía ("seguridad por oscuridad") altera los componentes estructurales (Least Significant Bit - LSB) de archivos portaaviones (audios, imágenes) para esconder secretos dentro. Su objetivo no es cifrar el mensaje para volverlo ilegible, sino engañar al Blue Team haciéndoles creer que el mensaje secreto *no existe*.
