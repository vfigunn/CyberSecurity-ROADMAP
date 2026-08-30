# 09 - Firmas Digitales (Integridad y No Repudio)

## 🎯 Objetivos
- Comprender cómo la criptografía asimétrica se puede usar en "Reversa".
- Entender el funcionamiento matemático de una Firma Digital.
- Identificar cómo esto resuelve la Integridad y el "No Repudio" (Non-Repudiation) de la Tríada CIA.

---

## 🧠 Concepto: La Asimetría Invertida

En la [[07 - Criptografía Asimétrica (RSA, Llaves Públicas)|Nota 07]] vimos que el uso normal de la criptografía asimétrica (para mantener secretos) es que **Cualquiera encripta con tu Llave Pública, y solo VOS desencriptas con tu Privada**.

Pero, ¿qué pasa si damos vuelta esa regla? (Recordemos que lo que una llave cierra, la otra lo abre).
¿Qué pasaría si vos (el autor) **encriptas un archivo utilizando tu propia Llave Privada (la secreta)**?
- Significa que cualquier persona en Internet podrá tomar tu Llave Pública (la cual compartiste en tu web) y desencriptar el archivo.
- Si cualquiera puede abrirlo, **¡Hemos destruido la Confidencialidad!** Ya no hay secretos.

Entonces, ¿para qué sirve encriptar algo que todos pueden abrir?
Sirve para **Garantizar la Identidad y la Autoría**. Si yo puedo abrir este paquete exitosamente usando la *Llave Pública de María*, significa (por matemática asimétrica pura) que este paquete tuvo que ser cerrado irremediablemente con la *Llave Privada de María*. 

Como María es la única que posee su llave privada, ella (y solo ella) pudo haber creado este archivo. Acabamos de conseguir el codiciado **No Repudio (Non-Repudiation)**: María nunca podrá decirle a la justicia "Ese contrato no lo hice yo", porque las matemáticas no mienten.

---

## 🖋️ La Firma Digital en Acción

Encriptar el archivo entero con tu llave privada es lento e ineficiente. La industria inventó un proceso elegante que combina la magia inversa asimétrica con el poder destructivo de los [[04 - Hashing I (Conceptos, MD5, SHA-256)|Hashes (Nota 04)]].

**El Proceso de Firmar (El Emisor):**
1. Escribís el PDF del contrato: "Le transfiero mi casa a Juan".
2. No encriptás el PDF. Simplemente le aplicás una licuadora SHA-256, obteniendo el Hash (Huella digital): `ab45...`.
3. Tomás tu **Llave Privada Secreta** y encriptás solamente el pequeño hash `ab45...`. A ese bloquecito encriptado se le llama la **Firma Digital**.
4. Le enviás a Juan el PDF de texto claro, pegándole al final la pequeña Firma Digital.

**El Proceso de Verificación (El Receptor - Juan):**
Juan necesita saber dos cosas: A) Que fuiste VOS el que envió el contrato (No Repudio), y B) Que un hacker no modificó el texto del PDF mientras viajaba por el correo (Integridad).
1. Juan separa el PDF claro de la Firma Digital.
2. Juan toma tu **Llave Pública** y la usa para desencriptar tu Firma Digital. 
   - *¿Pudo abrirla?* ¡Sí! Entonces se garantiza matemáticamente que fuiste vos (Autoría/No Repudio). De la firma desencriptada sale el **Hash Original de origen** (`ab45...`).
3. Ahora Juan duda: "¿El hacker habrá modificado el PDF en el camino?". Juan toma el PDF claro y le aplica el mismo algoritmo SHA-256 por su cuenta, generando un **Hash Recién Calculado Localmente**.
4. Juan compara los dos Hashes (El que desencriptó de tu firma vs El que calculó él).
   - Si ambos hashes dan `ab45...`, los pilares de la ciberseguridad se sostienen: **Identidad garantizada, Integridad intacta, No Repudio absoluto.**

---

## ❓ ¿Por qué importa en Seguridad?

Las firmas digitales no son solo para contratos de abogados. Se usan en el software a cada milisegundo:

- **Actualizaciones de SO (Windows Update):** Cuando Windows descarga un parche de seguridad `actualizacion.exe` de Internet, no lo instala a ciegas. Windows extrae la Firma Digital pegada al `.exe`, utiliza la Llave Pública de Microsoft (que viene guardada en la placa madre) y verifica el Hash. Si coincide, significa que el `.exe` realmente fue fabricado por los ingenieros de Microsoft (y no por un Hacker Chino de Estado) y que no fue alterado en tránsito. 
*(Si sos del Red Team y creás un troyano `.exe`, Windows saltará con alertas rojas diciendo que la herramienta "No está firmada", a menos que logres robarte la Llave Privada de un desarrollador legítimo).*

- **Bitcoin y Criptomonedas:** Las billeteras cripto son simplemente un par de Llaves (Pública y Privada). Cuando ordenás "Transferir 1 BTC", estás utilizando la magia de tu llave privada para *Firmar Digitalmente* esa orden antes de tirarla a la red de mineros.

---

## 📌 Must Know (Imprescindible)
- Qué es una Firma Digital técnicamente: **Un Hash que ha sido cifrado con la Llave Privada del autor**.
- En qué se diferencia encriptar datos vs firmar datos (La firma requiere encriptar el hash con la Privada, destruyendo la Confidencialidad a favor del No Repudio).
- Saber cómo el receptor verifica la firma (desencriptando la firma con la llave pública y comparando los Hashes).

---

## 🔄 Preguntas de repaso
1. Una corporación de software lanza un parche de seguridad de 500 Megabytes a través de Internet, y le "pega" su Firma Digital correspondiente para asegurar su legitimidad. Conceptualmente, el departamento de Criptografía de la empresa ¿usó su Llave Privada para encriptar los 500 MB del parche entero, o usó su Llave Privada para encriptar únicamente el Hash (ej. SHA-256) de ese parche?
2. Si un analista de SOC extrae la "Firma Digital" adjunta a un archivo malicioso, y luego intenta desencriptarla utilizando la Llave Pública del desarrollador legítimo (ej. Adobe), pero el programa le arroja un error avisándole que fue imposible desencriptar o acceder al Hash original con esa Llave Pública, ¿qué conclusión categórica puede sacar el analista sobre la autoría de esa firma?
3. En el proceso de validación final de un documento firmado digitalmente, el receptor logró desencriptar la firma con la Llave Pública del autor obteniendo el Hash original: `XYZ123`. Sin embargo, al pasar el archivo PDF por la función de Hashing localmente, el resultado dio `ABC999`. ¿Qué pilar de la tríada CIA (Confidencialidad, Integridad o Disponibilidad) acaba de colapsar, confirmando que el archivo es fraudulento?

**➡️ Siguiente nota:** [[10 - Infraestructura de Llave Pública (PKI y Certificados TLS)]]
