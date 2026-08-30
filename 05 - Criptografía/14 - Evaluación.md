# 14 - Evaluación del Módulo 05 (Criptografía)

## 📝 Instrucciones
Evaluación final del módulo. Múltiples preguntas basadas en arquitecturas criptográficas, el uso adecuado de los algoritmos y los estándares de seguridad modernos.
Anotá tus respuestas y luego verificalas con el solucionario.

> **Importante:** Las respuestas correctas y explicaciones están en: `[[15 - Evaluaciones/Respuestas/Módulo 05 - Respuestas]]`.

---

## 🎯 Sección 1: Transformación de Datos y Hashes

**1. Un programador guarda las contraseñas en la base de datos transformándolas a Base64. Un auditor le reporta esto como una vulnerabilidad Crítica (Severidad Alta). ¿Cuál es la justificación técnica del auditor?**
A) Base64 es un algoritmo de hashing obsoleto, debería usar SHA-256.
B) Base64 genera colisiones matemáticas frecuentemente, mezclando usuarios.
C) Base64 es un sistema de Encoding (codificación) que es públicamente reversible en instantes, no brinda Confidencialidad.
D) Base64 consume demasiado poder de CPU (procesamiento) del servidor.

**2. ¿Cuál es el principal pilar de la tríada CIA que se busca proteger cuando aplicamos una función Hashing (como SHA-256) sobre un archivo descargado de Internet?**
A) Confidencialidad
B) Integridad
C) Disponibilidad
D) No Repudio

**3. Si un atacante tiene acceso a una tabla de contraseñas hasheadas con MD5 puro, podrá romperlas fácilmente usando Rainbow Tables (Tablas pre-calculadas). ¿Cuál es el mecanismo técnico defensivo estándar para arruinar las tablas del atacante?**
A) Comprimir las contraseñas en un ZIP antes de hashearlas.
B) Encriptar el hash resultante con AES-256.
C) Añadir una cadena de caracteres aleatorios (Salt) a cada contraseña antes de aplicarle el algoritmo hash.
D) Reemplazar MD5 por el algoritmo de Encoding URL.

---

## 🎯 Sección 2: Criptografía Simétrica y Asimétrica

**4. ¿Cuál de las siguientes afirmaciones describe mejor al cifrado Simétrico (Ej. AES)?**
A) Utiliza una Llave Pública para cifrar y una Privada para descifrar.
B) Es ideal para firmar documentos digitalmente.
C) Utiliza la misma y única clave tanto para encriptar como para desencriptar, siendo extremadamente veloz.
D) No sufre del "problema de distribución de la llave" a través de Internet.

**5. En un sistema de Criptografía Asimétrica, si Alice quiere enviarle a Bob un archivo ultra secreto, ¿Qué llave exacta debe utilizar Alice para ENCRIPTAR el archivo asegurándose de que solo Bob pueda leerlo?**
A) La Llave Pública de Alice.
B) La Llave Privada de Alice.
C) La Llave Pública de Bob.
D) La Llave Privada de Bob.

**6. El cifrado asimétrico (RSA) es brillante, pero tiene un defecto fundamental que impide utilizarlo para encriptar, por ejemplo, un disco duro entero de 1 Terabyte. ¿Cuál es ese defecto?**
A) Es extremadamente lento y costoso computacionalmente en comparación con el cifrado Simétrico.
B) Sus llaves caducan cada 24 horas por seguridad.
C) Los algoritmos asimétricos están completamente rotos y obsoletos.
D) Solo funciona a través de Internet, no en archivos locales.

---

## 🎯 Sección 3: Arquitecturas y Firmas (PKI)

**7. ¿Cuál es el objetivo matemático de firmar digitalmente un archivo (encriptar el hash del archivo con la Llave Privada del autor)?**
A) Garantizar que nadie más pueda leer el contenido del archivo (Confidencialidad absoluta).
B) Reducir el peso del archivo para un envío por correo más rápido.
C) Garantizar el No Repudio (Autoría) y la Integridad, ya que solo el autor posee la llave privada para crear esa firma.
D) Evitar que el archivo sea borrado por un Antivirus.

**8. En la infraestructura del candado verde (HTTPS/TLS) de la web, ¿qué rol cumple una Autoridad Certificadora (CA) como DigiCert?**
A) Encriptar personalmente el tráfico entre el usuario y el servidor web.
B) Guardar copias de seguridad de las contraseñas de todos los usuarios del mundo.
C) Actuar como Notario (Tercero de confianza), firmando digitalmente el Certificado del Servidor para garantizar que la Llave Pública realmente le pertenece.
D) Proveer los servidores DNS para enrutar el tráfico web.

**9. Para resolver el problema del intercambio de llaves simétricas a través de un canal inseguro (evitando que un atacante las intercepte), y para garantizar el "Perfect Forward Secrecy" en HTTPS, ¿Qué algoritmo matemático se utiliza masivamente en la actualidad?**
A) Base64
B) Diffie-Hellman (DH / ECDHE)
C) MD5
D) LSB (Esteganografía)

**10. Un atacante ocultó los planos de un malware dividiendo su código y reemplazando el último bit (LSB) de los píxeles de una fotografía inofensiva de un gato, evadiendo los Firewalls de la empresa. ¿Cómo se llama esta técnica de seguridad por oscuridad?**
A) Criptografía Simétrica
B) PKI (Public Key Infrastructure)
C) Salting
D) Esteganografía

---

> **¿Terminaste?**
> Buscá el archivo en la carpeta de evaluaciones para comparar tus resultados (Módulo 05 - Respuestas).

**➡️ Siguiente nota:** [[15 - Resumen]]
