# 02 - Diferencia entre Encoding, Hashing y Encripción

## 🎯 Objetivos
- Erradicar el error técnico más común en la industria tecnológica.
- Entender claramente los tres conceptos que transforman los datos, sus objetivos y sus capacidades de reversibilidad.

---

## 🧠 Concepto: La Trilogía de Transformación

A simple vista, si ves el texto `eXlPZGVk` en la pantalla, parece que los datos han sido "encriptados" porque son ilegibles para un humano. 
**Este es el error que cuesta millones a las empresas.** Que algo sea incomprensible no significa que sea seguro.

Existen tres formas fundamentales de transformar los datos en informática, y cada una tiene un objetivo drásticamente distinto.

---

## 1. Encoding (Codificación)

**Objetivo:** Transformar los datos para garantizar la **Compatibilidad** (Disponibilidad), nunca la seguridad.
- **¿Qué hace?** Cambia el formato de los datos usando un diccionario público para que puedan viajar sin problemas a través de sistemas que no soportan ciertos caracteres (como fotos enviadas por correo electrónico, o espacios en blanco en una URL).
- **¿Tiene llave secreta?** NO.
- **¿Es reversible?** SÍ, de forma inmediata. Cualquier persona del planeta puede decodificarlo usando el mismo diccionario público.

> **Analogía:** Es como traducir la palabra "Perro" al Código Morse (`.--. . .-. .-. ---`). No estás escondiendo el mensaje; cualquiera con un manual de Código Morse puede volver a leer la palabra "Perro".

---

## 2. Hashing (Resumen Matemático)

**Objetivo:** Garantizar la **Integridad**. Asegurar que el archivo no ha sido modificado.
- **¿Qué hace?** Pasa el archivo original (ej. una película de 4GB o la palabra "hola") a través de una licuadora matemática, destruyendo el archivo original y escupiendo una "Huella Digital" de tamaño fijo (ej. 32 caracteres).
- **¿Tiene llave secreta?** NO.
- **¿Es reversible?** **¡Absolutamente NO!** Un hash es matemáticamente unidireccional (One-Way). Es imposible volver a construir la vaca (archivo original) a partir de la carne picada (el hash resultante).

> **Analogía:** Si pasás una novela entera por una trituradora de papel y pesás los pedazos, obtendrás "500 gramos" (Tu Hash). Si alguien añade una hoja falsa a la novela, pesará "505 gramos" (Cambió el hash, hubo manipulación). Pero es imposible usar el número "500 gramos" para reconstruir el texto exacto de la novela.

---

## 3. Encripción / Cifrado (Encryption)

**Objetivo:** Garantizar la **Confidencialidad**. Asegurar que solo las personas autorizadas puedan leer el mensaje.
- **¿Qué hace?** Transforma el texto plano en texto cifrado (Ciphertext) utilizando matemáticas extremadamente complejas, que solo pueden ser revertidas si posees la contraseña o "Llave" (Key).
- **¿Tiene llave secreta?** **SÍ.** Es el único de los tres métodos que utiliza llaves criptográficas.
- **¿Es reversible?** SÍ, pero es **condicional**. Solo es reversible si (y solo si) conocés la Llave Secreta correcta.

> **Analogía:** Poner el documento dentro de una caja fuerte de titanio, cerrarla, y tirar la caja fuerte en medio de la plaza del pueblo. Cualquiera puede ver la caja (texto cifrado), pero sin la llave secreta, es imposible leer el documento de adentro.

---

## ❓ El Error Letal (Por qué importa en Seguridad)

El error clásico del programador web principiante (Junior):
*"Tengo que proteger las contraseñas de los usuarios en la Base de Datos. No quiero guardarlas en texto claro. Voy a 'Encriptarlas' pasándolas a Base64 (Encoding)."*

Si el atacante hackea la base de datos y roba los textos en Base64, el programador creerá que está a salvo porque "los datos están protegidos". 
El atacante copiará los datos, abrirá Google, buscará "Base64 Decoder", pegará el texto y **obtendrá todas las contraseñas reales de los clientes en 2 segundos**. 

Codificar (Encoding) **no es** asegurar.
La Encripción requiere secretismo (Llaves). El Hashing es destructivo. El Encoding es simplemente traducción pública.

---

## 📌 Must Know (Imprescindible)
- **Encoding = Compatibilidad**. Es público y 100% reversible (Ej. Base64).
- **Hashing = Integridad**. Es unidireccional y 0% reversible. La huella digital destruye el original (Ej. MD5, SHA256).
- **Encripción = Confidencialidad**. Es reversible solo mediante el uso de una Llave Secreta.

---

## 🔄 Preguntas de repaso
1. Una herramienta de seguridad toma el archivo `malware.exe`, lo procesa y devuelve la cadena alfanumérica `5e884898da28047151d0e56f8dc6292773603d0d6aabbdd62a11ef721d1542d8`. Sabiendo que a partir de esa cadena de texto es matemáticamente imposible volver a recrear o ejecutar el `malware.exe`, ¿qué proceso de los tres se le aplicó al archivo original?
2. Si un desarrollador usa el algoritmo estándar `AES-256` para ocultar información, ¿qué elemento adicional e indispensable debe poseer un receptor autorizado para poder revertir el proceso y leer el mensaje original?
3. Un atacante se infiltra en una red y captura tráfico que contiene credenciales. Observa que el formato del usuario y la contraseña es `admin:password`, pero está representado como `YWRtaW46cGFzc3dvcmQ=`. Él reconoce esto como una técnica para evitar errores de caracteres (como los dos puntos) en la transmisión HTTP, y logra convertirlo a texto legible instantáneamente. ¿Qué técnica aplicó el servidor web, confirmando que cometió una negligencia grave de seguridad?

**➡️ Siguiente nota:** [[03 - Encoding (Base64 y URL Encoding)]]
