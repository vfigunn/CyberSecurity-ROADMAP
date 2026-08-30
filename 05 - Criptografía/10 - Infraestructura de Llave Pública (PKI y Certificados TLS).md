# 10 - Infraestructura de Llave Pública (PKI) y Certificados (HTTPS)

## 🎯 Objetivos
- Resolver el último defecto crítico de las comunicaciones seguras (El ataque del Impostor).
- Entender qué es un Certificado Digital y qué es una Autoridad Certificadora (CA).
- Comprender cómo funciona la arquitectura PKI que le da vida al candadito verde (HTTPS/TLS) en tu navegador web.

---

## 🧠 El Problema: El Hacker Disfrazado

Hasta la nota de [[08 - El Intercambio de Llaves (Diffie-Hellman)|Diffie-Hellman]] vimos cómo dos partes pueden hablar de forma encriptada e inquebrantable a través de Internet. Pero falta la pieza más importante del rompecabezas.

**Escenario (El Man-in-the-Middle Supremo):**
1. Ingresás a `www.banco.com`. 
2. Le pedís la **Llave Pública Asimétrica** al servidor del banco para poder armar el canal seguro HTTPS.
3. Un Hacker (Hacker-Bob), que interceptó tu cable Wi-Fi, corta el mensaje. El Hacker *intercepta* la verdadera Llave Pública del banco, la tira a la basura, y te envía a ti (la víctima) **una Llave Pública falsa, generada por el Hacker**, diciendo: *"Hola, soy el banco, esta es mi Llave Pública"*.
4. Tu navegador (Google Chrome), en la ignorancia total, utiliza la llave pública del Hacker para encriptar tu contraseña, y la envía a la red creyendo que estás seguro.
5. El Hacker la recibe, y como la encriptaste con su llave pública falsa, él usa su propia Llave Privada para abrirla. Te roba la contraseña, se ríe, la vuelve a encriptar con la llave verdadera del banco y la reenvía para que no notes nada.

*Falla Crítica:* La matemática de las llaves funcionó perfecto, el candado cerró... pero **¿Cómo diablos sabés que la Llave Pública que te llegó pertenece legítimamente a la persona que dice ser, y no a un impostor?**

---

## 📜 Certificados Digitales (El DNI de Internet)

Para solucionar el fraude de identidad, la industria inventó el **Certificado Digital**. 

En la vida real, si Juan te dice "Soy un Oficial de Policía, tomá mis armas (Llave Pública)", no le creés por fe. Le pedís su placa (Credencial o DNI). Le creés a esa placa porque está *sellada* por el Gobierno de tu país, que es una entidad central en la que todos confiamos ciegamente.

Un Certificado (típicamente formato **X.509**) es un simple archivo de texto que dice:
- *"La Llave Pública `9x8y7z` que tienes enfrente pertenece absoluta y legalmente a `www.banco.com`."*
- Y abajo, lleva la [[09 - Firmas Digitales (Integridad y No Repudio)|Firma Digital de la Nota 09]] de un "Gobierno de Internet".

### La Autoridad Certificadora (CA)
En Internet no hay un gobierno central, hay empresas gigantes llamadas **Autoridades Certificadoras (CA)** (ej. DigiCert, Let's Encrypt, VeriSign). Estas empresas son los notarios del mundo cibernético. Tienen su propio par de llaves (Pública y Privada). Sus llaves privadas están escondidas en cajas fuertes blindadas bajo tierra militarizadas (literalmente).

**¿Cómo obtiene el Banco su certificado?**
1. El Banco genera su propio par de llaves. 
2. El Banco va físicamente (o legalmente) a DigiCert (la CA), le entrega su Llave Pública y sus documentos de registro de empresa.
3. DigiCert verifica los papeles y dice: *"Doy fe notarial de que esta Llave Pública es del Banco verdadero"*. 
4. DigiCert crea el archivo Certificado X.509, y usando su inmensa **Llave Privada Maestra**, le aplica su **Firma Digital** al archivo entero.

### Validando la cadena de confianza en Chrome
1. Entrás al `banco.com`.
2. El servidor ya no te envía solo su Llave Pública. Te envía su **Certificado X.509 completo** (que lleva adentro su llave y la Firma de DigiCert).
3. Tu navegador (Chrome) recibe el certificado, extrae la Firma Digital de DigiCert, y necesita desencriptarla para verificar su Hash (Integridad).
4. ¿De dónde saca Chrome la Llave Pública de DigiCert para hacer esto? **Todas las Llaves Públicas de todas las CA del planeta ya vienen pre-instaladas en el código fuente de tu Sistema Operativo (Windows/macOS) desde la fábrica (Root Store)**.
5. Chrome verifica matemáticamente la firma de la CA. Si la matemática cuadra, enciende el **Candadito Verde (Conexión Segura - HTTPS)** y recién ahí usa la Llave Pública del banco. Si la matemática falla, o si el certificado fue expedido a nombre de `hacker.com`, Chrome te lanza la temida pantalla roja: *ERR_CERT_AUTHORITY_INVALID*.

*(A toda esta macro-arquitectura de Servidores Web, CAs Notarias, y computadoras con llaves raíz pre-instaladas se le conoce formalmente como PKI - Public Key Infrastructure).*

---

## 📌 Must Know (Imprescindible)
- Qué soluciona un Certificado (Ataques MitM o Impostores: Ataques a la Identidad y Autenticidad, no al cifrado en sí).
- Qué contiene mínimamente un Certificado X.509: (El Nombre del Dominio, La Llave Pública del Dueño, y La Firma Digital de la CA que lo emitió).
- Qué es una Autoridad Certificadora (CA): La entidad de confianza que emite y firma los certificados.
- Saber que las Llaves Públicas de las CA Raíz ya están instaladas "físicamente" en tu PC.

---

## 🔄 Preguntas de repaso
1. Un atacante (Man-in-the-Middle) intercepta tu conexión a `google.com` y te envía su propia Llave Pública. Para que tu navegador Chrome le crea y le entregue un "candadito verde" HTTPS, el atacante debería adjuntar un Certificado válido a nombre de `google.com` firmado por una CA famosa como DigiCert. Explicá lógicamente por qué es teóricamente imposible que el atacante (suponiendo que no hackeó las bóvedas físicas de DigiCert) pueda falsificar la Firma Digital de DigiCert sobre ese certificado fraudulento.
2. Basado en el concepto de la cadena de confianza PKI (Public Key Infrastructure), ¿qué componente crucial (relacionado con las Autoridades Certificadoras) debe estar pre-instalado en el Sistema Operativo del cliente para que su navegador web pueda validar matemáticamente la firma de un certificado recibido por Internet?
3. Si entrás a la página web de tu banco y tu navegador (Firefox) te despliega una advertencia de seguridad roja a pantalla completa diciendo "El certificado no es válido" (o su fecha está expirada), ¿a qué vector de ataque clásico estás siendo vulnerable si ignorás la alerta, clickeas "Continuar de todos modos" e ingresás tu contraseña? *(Pista: Involucra que alguien en el medio te pase una llave falsa).*

**➡️ Siguiente nota:** [[11 - Esteganografía (Ocultando a plena vista)]]
