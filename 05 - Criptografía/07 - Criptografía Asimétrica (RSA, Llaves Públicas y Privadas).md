# 07 - Criptografía Asimétrica (RSA y Llaves Públicas)

## 🎯 Objetivos
- Descubrir la genialidad matemática de poseer Dos Llaves (una Pública y una Privada).
- Entender cómo soluciona el problema de distribución de la nota anterior.
- Conocer los algoritmos RSA y ECC (Curvas Elípticas).

---

## 🧠 Concepto: La Magia de las Dos Llaves

En la década de los 70, la criptografía se revolucionó para siempre con la invención del sistema de **Llave Pública (Asimétrico)**.

En este sistema, ya no existe una sola llave compartida. Cada usuario (vos, Amazon, tu banco) genera **un PAR de llaves matemáticamente conectadas**:
1. **La Llave Privada (Private Key):** La mantenés oculta en lo más profundo de tu computadora, encriptada, y *jamás en toda tu vida la compartís con nadie*, ni siquiera con tu banco. Si alguien la roba, tu identidad digital está arruinada.
2. **La Llave Pública (Public Key):** Es como tu número de teléfono. La publicás en Internet, la pegás en tu frente, se la enviás a todos tus contactos y enemigos. No importa quién la tenga.

### La Regla de Oro Unidireccional:
> **Cualquier cosa que encriptes con la Llave Pública, SOLO puede ser desencriptada con su correspondiente Llave Privada.**

(E, interesantemente, viceversa: lo que encriptes con la Privada, solo se abre con la Pública).

---

## 🛡️ Solucionando el envío por Internet (Confidencialidad)

Volvamos al problema de comprar un libro en Amazon de la [[06 - Criptografía Simétrica (AES, DES)|Nota 06]]. 

1. Amazon generó su par de llaves. Mantiene la Privada oculta en un búnker.
2. Tu navegador web le pide a Amazon: *"Che, mandame tu Llave Pública"*. Amazon te la envía a través de Internet sin miedo.
3. Si un Hacker intercepta esa Llave Pública de Amazon en el aire, **no sirve de nada**. La regla dice que la llave pública NO sirve para desencriptar mensajes. Solo sirve para encriptarlos (Cerrar el candado).
4. Tu navegador usa la Llave Pública de Amazon para **encriptar (cerrar)** los datos de tu tarjeta de crédito. Una vez cerrado el candado, ni siquiera vos podés volver a abrirlo (porque la Llave Pública no abre).
5. Enviás el paquete cifrado por el aire. Si el Hacker lo intercepta, solo tiene el paquete y la llave pública, así que está atrapado.
6. El paquete llega a Amazon, y el servidor **utiliza su Llave Privada ultra-secreta** para abrirlo y leer tu tarjeta.

*El problema de distribución desapareció. Nunca tuviste que compartir un secreto con Amazon para hablar de forma segura.*

---

## ⚠️ La Gran Desventaja (Velocidad)

Si este sistema es tan perfecto, ¿por qué no encriptamos tu disco duro con él?
Porque las matemáticas detrás de las llaves asimétricas (cálculos de números primos gigantescos) son brutalmente lentas. 

**La Criptografía Asimétrica es entre 1.000 y 10.000 veces más lenta que la Simétrica (AES).** 
Si intentás cifrar un archivo de 2 GB con tu Llave Pública, tu computadora tardaría semanas.

*(En la próxima nota veremos cómo unimos lo mejor de los dos mundos para crear el Internet actual).*

---

## 🛠️ Algoritmos Asimétricos

- **RSA (Rivest-Shamir-Adleman):**
  El algoritmo más famoso del mundo. Su seguridad se basa en un problema matemático hasta ahora irresoluble: la Factorización de números primos colosales. Usa llaves inmensas (2048 o 4096 bits) porque las llaves más chicas ya fueron rotas.
- **ECC (Criptografía de Curva Elíptica):**
  El reemplazo moderno. Usa problemas matemáticos geométricos (Curvas en gráficos). La magia de ECC es que **con una llave pequeñita de 256 bits, te ofrece la misma seguridad que el mastodonte de RSA de 3072 bits**. Al usar llaves más chicas, requiere mucha menos CPU, por lo que es vital para la seguridad en celulares y el Internet de las Cosas (IoT).

---

## 📌 Must Know (Imprescindible)
- Las dos llaves: Privada (El secreto absoluto que desencripta) y Pública (La de distribución masiva que encripta hacia ti).
- La relación cruzada: Lo que una encripta, la otra lo desencripta. 
- Comprender exactamente la secuencia lógica de cómo Juan le envía un mensaje confidencial a María (Juan utiliza *la llave pública de María* para cerrarlo, y María usa su *propia llave privada* para abrirlo).
- La Criptografía Asimétrica (RSA/ECC) es brillante pero **excesivamente lenta** comparada con AES.

---

## 🔄 Preguntas de repaso
1. Ana quiere enviar un archivo médico altamente confidencial a Bob a través de una red comprometida (Internet). Ana y Bob poseen, cada uno, su propio par de llaves (Pública y Privada). De las 4 llaves que existen en el escenario, ¿qué llave exacta debe usar Ana para ENCRIPTAR el archivo asegurándose de que, una vez cerrado, solamente Bob en todo el planeta pueda leerlo?
2. Si un ciberatacante irrumpe en el servidor web de una empresa y logra robar el archivo que contiene la Llave Pública de ese servidor, ¿por qué ese robo no representa una brecha de Confidencialidad sobre los datos encriptados de los clientes que ya habían viajado hacia el servidor?
3. En la industria actual de desarrollo de aplicaciones para teléfonos móviles (Mobile App Security), ¿por qué los ingenieros prefieren utilizar el algoritmo asimétrico ECC en lugar del tradicional algoritmo RSA para realizar cálculos criptográficos dentro del celular del usuario?

**➡️ Siguiente nota:** [[08 - El Intercambio de Llaves (Diffie-Hellman)]]
