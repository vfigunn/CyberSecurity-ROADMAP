# 05 - Hashing II (Colisiones y Contraseñas)

## 🎯 Objetivos
- Entender el concepto de "Colisión" y por qué eso destruye a un algoritmo de Hashing (el caso de MD5).
- Comprender cómo los atacantes rompen los hashes usando diccionarios.
- Aprender cómo la industria de la programación soluciona este fallo utilizando *Salt* y *Pepper*.

---

## 💥 Colisiones (Rompiendo las matemáticas)

En la nota anterior dijimos que las huellas digitales son únicas, y que un hash te asegura que el archivo no fue manipulado.

Pero pensemos con lógica matemática extrema: 
Si un archivo puede tener un tamaño "infinito" (existen infinitas combinaciones de datos posibles en el universo), pero el hash resultante (el MD5) siempre genera un output de tamaño fijo limitado a 32 caracteres (128 bits, que equivalen a 340 undecillones de combinaciones)... 
Tarde o temprano, por pura matemática, **van a existir dos archivos distintos en el universo que al pasar por la licuadora generen la misma y exacta huella digital.**

A este evento catastrófico se le llama **Colisión**.

### El peligro de la Colisión
Durante años se creyó que las probabilidades de que esto pasara eran casi nulas. Pero con el avance del poder de cómputo (Tarjetas Gráficas - GPUs), los hackers encontraron cómo forzar que esto suceda.

**Cómo funciona el ataque (El fin de MD5):**
1. Vos tenés un contrato digital en PDF que dice "Le pagaré $10 dólares a Juan". Aplicás MD5. Da el hash: `7x8y9z`.
2. Un atacante toma tu contrato original y crea una versión fraudulenta que dice "Le pagaré $1.000.000 a Juan".
3. El atacante usa su supercomputadora, y empieza a inyectar "basura invisible" (espacios en blanco) al final del contrato fraudulento hasta que, de casualidad matemática (Colisión), su PDF falso genera el exacto y mismo hash `7x8y9z`.
4. El sistema verificador de la empresa revisa la firma y asume que el PDF del millón de dólares es el original. Jaque mate a la Integridad.

Por esto mismo MD5 y SHA-1 fueron erradicados. Hoy por hoy, las computadoras son tan poderosas que pueden forzar colisiones en esos algoritmos fácilmente. (SHA-256 sigue siendo seguro, por ahora).

---

## 🔐 Hashing de Contraseñas (Cracking)

Cuando te registrás en Facebook con la contraseña "gatito123", Mark Zuckerberg (Facebook) **NO guarda la palabra "gatito123" en su base de datos.** Si lo hicieran, un empleado de Facebook podría leerla.
En su lugar, el servidor aplica un algoritmo de Hashing y guarda en la base de datos la huella digital: `d3a2...`.
La próxima vez que intentes loguearte, Facebook toma lo que escribiste, lo vuelve a hashear en el momento, y compara si los dos Hashes son idénticos. Si lo son, pasás.

Si un hacker se roba la base de datos, solo verá columnas con números incomprensibles (Hashes). ¿Cómo hace el hacker para saber que el hash `d3a2` significa "gatito123" si los hashes son teóricamente irreversibles?

### El Ataque de Diccionario (Rainbow Tables)
El atacante no aplica matemáticas hacia atrás (porque es imposible).
El atacante usa un archivo de texto gigantesco (un Diccionario de millones de contraseñas comunes).
1. Lee la palabra "123456" de su diccionario local.
2. Le aplica la matemática del hash. Da `e10a...`
3. Compara el `e10a...` con todos los millones de hashes robados de la base de datos. Si alguno coincide (Hit), el atacante sabe automáticamente que ese usuario estaba usando la contraseña "123456".

Esto se hace a miles de millones de intentos por segundo usando placas de video. Las **Rainbow Tables** son bases de datos pre-calculadas colosales donde la gente ya calculó los hashes (MD5, SHA1) de absolutamente todas las palabras del abecedario, ahorrando tiempo.

---

## 🧂 Salar la carne (Salt & Pepper)

Para arruinarle el negocio al hacker, los programadores defensivos (Blue Team) utilizan la **Salt (Sal)**.

Cuando el usuario crea la contraseña "gatito123", antes de aplicarle la licuadora (el Hashing), el servidor genera 20 caracteres totalmente aleatorios (ej. `%h71@!Lm...`) que actúan como "Sal".
El servidor "pega" esa Sal a la contraseña del usuario (quedando: `gatito123%h71@!Lm...`) y recién ahí mete todo en la licuadora.

**¿Por qué esto destruye al atacante?**
- Porque la Sal es **única para cada usuario**. (El usuario 1 tiene una sal, el usuario 2 tiene otra distinta).
- Cuando el atacante robe la base de datos e intente usar sus diccionarios o Rainbow Tables pre-calculados, fallará miserablemente, porque los hashes no provienen solo de la palabra "gatito123", sino de "gatito123+una_sal_aleatoria", lo que cambia todo el hash por completo (Efecto Avalancha).

*(Existe algo llamado **Pepper (Pimienta)**, que es similar a la Sal, pero en lugar de guardarse públicamente en la misma base de datos, el servidor lo guarda escondido en un archivo de configuración separado, añadiendo una capa más de pesadilla para el atacante).*

> **Nota de Seguridad del Software:** Por la enorme evolución del hardware (las placas de video), usar algoritmos rápidos (como MD5 o SHA256) para contraseñas es peligroso. Los atacantes pueden adivinarlas rápido. La industria actual usa algoritmos específicamente diseñados para ser lentos, devoradores de RAM, y que incluyen Sal y procesos múltiples obligatoriamente (Algoritmos *Key Derivation*).
> **Estándares modernos de contraseñas:** **Argon2** (el mejor), **Bcrypt**, y **Scrypt**.

---

## 📌 Must Know (Imprescindible)
- Concepto de Colisión (dos archivos distintos generando el mismo Hash).
- Los atacantes no "desencriptan" hashes. Los rompen mediante comparaciones por Fuerza Bruta, Diccionarios y Tablas precalculadas.
- La función vital de la **Sal (Salt)**: Añadir entropía/basura aleatoria por cada usuario antes de aplicar el hash, volviendo inútiles a las tablas Rainbow pre-calculadas de los atacantes.
- En la programación moderna, Bcrypt y Argon2 son obligatorios para guardar passwords; MD5/SHA256 ya no alcanzan.

---

## 🔄 Preguntas de repaso
1. Según tu propio entendimiento de las matemáticas del Hashing, explicá lógicamente por qué es una certeza absoluta (y no una mera probabilidad) de que el problema teórico de las Colisiones exista en cualquier algoritmo que produzca una salida de tamaño fijo (por más seguro que parezca).
2. Si un desarrollador web te dice con orgullo: "Estoy seguro frente a los robos de bases de datos porque no guardo contraseñas en texto plano, sino que aplico el modernísimo algoritmo SHA-256 a la contraseña del usuario y guardo esa huella digital", ¿por qué deberías reprobar su auditoría de seguridad explicando el ataque que un hacker realizaría usando sus placas de video (GPU)?
3. Si un atacante roba tanto la tabla de la Base de Datos que contiene los Hashes, como así también la tabla adyacente que guarda la "Sal (Salt)" en texto claro (pública) correspondiente a cada usuario, ¿por qué el sistema sigue estando protegido frente a los ataques pre-calculados masivos de las Rainbow Tables, a pesar de que el atacante conozca la Sal de todos?

**➡️ Siguiente nota:** [[06 - Criptografía Simétrica (AES, DES)]]
