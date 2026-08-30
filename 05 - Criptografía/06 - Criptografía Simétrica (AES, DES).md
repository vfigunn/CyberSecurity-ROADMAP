# 06 - Criptografía Simétrica (AES, DES)

## 🎯 Objetivos
- Entender cómo funciona la criptografía de llave compartida (Simétrica).
- Conocer los algoritmos históricos (DES) y el estándar inquebrantable actual (AES).
- Identificar el defecto fundamental (la paradoja) de este sistema al usarlo en redes.

---

## 🧠 Concepto: Una llave para gobernarlos a todos

La **Criptografía Simétrica** es el método de encriptación más antiguo del mundo (usado desde los tiempos de Julio César). 
La regla es simple: **La misma llave exacta que se utiliza para Cerrar (Encriptar) la caja, es la que se utiliza para Abrirla (Desencriptar).**

Si querés enviarle un documento secreto a un amigo, ambos deben poseer una copia de la misma llave (Contraseña).
1. Vos aplicás la matemática simétrica y la contraseña al documento. Se vuelve ilegible (`Texto Cifrado`).
2. Lo enviás por Internet.
3. Tu amigo recibe el archivo, le aplica la misma contraseña, y recupera el documento (`Texto Plano`).

### Las dos ventajas absolutas
1. **Velocidad:** Las matemáticas detrás del cifrado simétrico son muy simples para una computadora. Son extremadamente veloces.
2. **Eficiencia:** Es la única forma de encriptar grandes volúmenes de datos (ej. un disco rígido entero de 2 Terabytes, o una base de datos entera) sin que la computadora se congele tratando de procesarlo. 

---

## 🛠️ Los Algoritmos Simétricos

Existen algoritmos de "Flujo" (que encriptan bit por bit, ideales para transferencias en vivo como llamadas de voz) y algoritmos de "Bloque" (que encriptan bloques de texto enteros a la vez, ideales para archivos).

- **DES (Data Encryption Standard):**
  Creado en los años 70. Usaba una llave cortísima (56 bits).
  *Estado actual:* **MUERTO.** Fue roto a finales de los 90. Una computadora de hoy en día puede probar todas las combinaciones posibles de la llave (Fuerza Bruta) en pocas horas y abrir el archivo sin saber la contraseña. (Luego se inventó el 3DES para parcharlo, que también está obsoleto).
- **AES (Advanced Encryption Standard):**
  Adoptado por el gobierno de EE. UU. en 2001, disponible típicamente en 128, 192 o **256 bits**.
  *Estado actual:* **EL REY ABSOLUTO.** Es el estándar mundial. AES-256 es matemáticamente inquebrantable en la actualidad (y probablemente lo siga siendo hasta la era de la computación cuántica madura). Si usás AES-256, la única forma en que un hacker va a leer tus archivos es robándote la contraseña, no rompiendo la matemática.

---

## ⚠️ El Problema de la Distribución de la Llave

Si el sistema Simétrico es tan rápido e inquebrantable (AES), ¿por qué no lo usamos para todo en Internet?

**La Paradoja del envío:**
Imaginá que querés comprar un libro en Amazon. Para que Amazon no vea los datos de tu tarjeta de crédito, tu navegador va a encriptar tu tarjeta usando AES y una Llave inventada. 
El problema es que Amazon necesita esa misma Llave para desencriptar el mensaje y cobrarte. 
¿Cómo le enviás tu Llave secreta a Amazon a través de Internet? 
- Si la enviás por Internet en texto claro, un Hacker (Man-in-the-Middle) que esté espiando la red interceptará tu llave en el aire. Y cuando luego envíes tu tarjeta encriptada, el Hacker usará la llave que acaba de robar para abrir el paquete. 
- Si encriptás la llave antes de enviarla... ¡Amazon no podrá abrirla!

Este es el **Problema de la Distribución de la Llave**. El cifrado Simétrico es brillante, pero requiere que las dos partes (Vos y Amazon) se reúnan en persona, en un callejón oscuro, para pasarse la contraseña anotada en un papel antes de hablar por Internet. Esto, a escala global, es imposible.

*La solución a este problema revolucionó la historia humana y la veremos en la próxima nota: La Criptografía Asimétrica.*

---

## 📌 Must Know (Imprescindible)
- La definición de **Simétrico**: Misma llave para bloquear y desbloquear.
- Su mayor cualidad es la **Velocidad** (Ideal para cifrar discos enteros).
- El estándar inquebrantable de la industria a día de hoy es **AES (256)**.
- Comprender el defecto estructural en su uso para redes (El problema de la distribución segura de la llave compartida a través de un canal inseguro).

---

## 🔄 Preguntas de repaso
1. Si un administrador de IT debe elegir un algoritmo criptográfico para encriptar completamente (Full Disk Encryption) un disco duro externo de 4 Terabytes, ¿por qué es mandatorio utilizar un algoritmo Simétrico en lugar de uno Asimétrico (el cual estudiaremos en la próxima nota)?
2. Explica conceptualmente por qué se dice que el cifrado de Julio César (donde cada letra se desplaza 3 posiciones en el abecedario: la A se vuelve D, la B se vuelve E) es un ejemplo puro de Criptografía Simétrica primitiva.
3. Un oficial militar en un país envía a su comandante en otro país un archivo PDF cifrado militarmente con AES-256 a través de un correo electrónico interceptado por espías. Los espías logran obtener el archivo cifrado y también interceptan un segundo correo donde el oficial le enviaba la contraseña ("1234SuperSeguro") al comandante. Explicá cómo este escenario ejemplifica el defecto catastrófico de la criptografía simétrica cuando se opera a distancia.

**➡️ Siguiente nota:** [[07 - Criptografía Asimétrica (RSA, Llaves Públicas)]]
