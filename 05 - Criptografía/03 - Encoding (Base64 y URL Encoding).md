# 03 - Encoding (Base64 y URL Encoding)

## 🎯 Objetivos
- Comprender el propósito técnico de usar Encoding en redes y desarrollo.
- Identificar visualmente la codificación **Base64** (el formato más usado por el Red Team).
- Entender el concepto de **URL Encoding** (imprescindible en Web Pentesting).

---

## 🧠 Concepto: La Torre de Babel de las Computadoras

Como vimos en la [[02 - Diferencia entre Encoding, Hashing y Encripción|nota anterior]], el Encoding NO tiene fines de seguridad ni requiere llaves secretas.

¿Por qué existe entonces?
Las computadoras se comunican en código binario puro (Bytes). Pero protocolos antiguos como el SMTP (Correo Electrónico, [[02 - Networking/22 - Protocolos de Correo y Transferencia|Módulo 02]]) o el HTTP, fueron diseñados originalmente para transmitir únicamente *texto imprimible en inglés (ASCII)*, como letras y números.

¿Qué pasa si querés adjuntar una fotografía `.jpg` (que está hecha de Bytes locos e impronunciables) en un correo electrónico? Si la enviás en su forma original, los routers y servidores de correo antiguo verían caracteres extraños que no entienden y la descartarían.

Ahí entra el **Encoding**. Toma los bytes de la foto `.jpg` y los traduce forzosamente a letras legibles del abecedario (A-Z, a-z, 0-9). De esa forma, la foto viaja disfrazada de "texto puro", el servidor de correo la acepta felizmente, y el servidor de destino la decodifica para volver a formar la foto.

---

## 📜 1. Base64 Encoding

Es el rey indiscutido del Encoding. Se llama así porque utiliza exactamente **64 caracteres** para realizar la traducción (Las 26 letras mayúsculas + 26 letras minúsculas + 10 números + 2 símbolos extras como `+` y `/`).

### ¿Cómo identificar el Base64 visualmente?
Esto es vital para un analista de seguridad (Blue Team):
1. Solamente contiene letras y números sin acentos ni espacios.
2. No tiene significado en ningún idioma humano (ej. `SGVsbG8gV29ybGQ=`).
3. **La Pista Maestra (El Padding):** Si ves un texto extraño que termina con uno o dos signos de igual (**`=`** o **`==`**), hay un 95% de probabilidades de que sea Base64. (El signo igual se usa como relleno matemático al final del bloque para que los números cuadren).

### Uso en Seguridad (Ofensiva/Defensiva)
Los atacantes aman Base64 para la **Evasión de Antivirus (Obfuscation)**.
Si el atacante manda un malware de Bash Scripting con el comando `rm -rf /`, el antivirus o el IPS lo leerán, reconocerán el comando destructivo y lo bloquearán.

El atacante puede pasar su comando por Base64:
`echo "rm -rf /" | base64` 
Y obtiene el texto: `cm0gLXJmIC8K`

El atacante envía ese texto. El Antivirus ve `cm0gLXJmIC8K`, asume que no es nada peligroso y lo deja pasar. Cuando llega al servidor víctima, el script malicioso lo decodifica y lo ejecuta. El Blue Team pasa horas decodificando Base64 en reversa para saber qué atacó sus servidores.

---

## 🌍 2. URL Encoding (Percent-encoding)

Este es el segundo tipo de codificación que dominarás, indispensable si vas a estudiar Seguridad Web.

Las URLs (links de páginas web) tienen caracteres reservados que cumplen funciones estructurales. Por ejemplo, el signo de interrogación `?`, el Ampersand `&`, el Espacio en blanco ` `, y la barra `/`.
Si un atacante intenta inyectar un código malicioso (SQL Injection) en la barra de búsqueda de una página web, su código probablemente contenga espacios y barras. Si los envía tal cual, el servidor web se confundirá y romperá la conexión HTTP.

El **URL Encoding** reemplaza los caracteres peligrosos (o los espacios) con un signo de porcentaje (`%`) seguido de dos números hexadecimales.

**Ejemplos clásicos (Memorizar):**
- El **Espacio en blanco** se convierte en **`%20`**. (Ej: `hola%20mundo`).
- La barra diagonal (`/`) se convierte en **`%2F`**.
- El signo igual (`=`) se convierte en **`%3D`**.

> *Dato de Pentester:* Si en un análisis de tráfico de red interceptás una petición a un servidor web que dice:
> `GET /login.php?user=admin%27%20OR%201%3D1`
> Automáticamente tenés que saber leer a través del URL Encoding. Tu mente debe traducir los `%20` a espacios y ver la sentencia clara: `admin' OR 1=1`. Estás viendo un intento de inyección de base de datos en vivo.

---

## 📌 Must Know (Imprescindible)
- El Encoding solo adapta datos para el transporte (compatibilidad), no los encripta.
- Poder reconocer Base64 a simple vista (terminación característica con `=` o `==`).
- Reconocer qué es el URL Encoding (caracteres `%%`) y saber de memoria que `%20` representa un Espacio.

---

## 🔄 Preguntas de repaso
1. Interceptás un tráfico malicioso y ves un archivo adjunto que contiene un bloque de texto que dice `UEsDBBQAAAAIA...QQAAAA==`. Explicá cómo deducís visualmente de qué formato se trata y por qué estás completamente seguro de que podrás revelar su contenido sin necesidad de poseer ninguna llave secreta.
2. Un atacante intenta evadir un filtro web introduciendo el payload de Inyección de Comandos `cat /etc/passwd`. Sabiendo cómo funciona el URL Encoding básico para los caracteres especiales, ¿cómo se vería específicamente el espacio en blanco (entre 'cat' y la barra) luego de pasar la cadena por URL Encoding?
3. En tus propias palabras, explicá por qué un sistema de correo (SMTP tradicional) necesita utilizar algoritmos como Base64 para poder enviar un archivo PDF adjunto a otra persona, considerando cómo funcionan las computadoras internamente.

**➡️ Siguiente nota:** [[04 - Hashing I (Conceptos, MD5, SHA-256)]]
