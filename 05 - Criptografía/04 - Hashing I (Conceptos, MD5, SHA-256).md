# 04 - Hashing I (Conceptos y Algoritmos)

## 🎯 Objetivos
- Entender el concepto matemático detrás de las Funciones Hash y sus características principales.
- Conocer la diferencia entre MD5, SHA-1 y la familia SHA-256 (el estándar moderno).
- Identificar por qué es el mecanismo vital para verificar la **Integridad** (Tríada CIA) en descargas y análisis forense.

---

## 🧠 Concepto: La Licuadora Unidireccional

En la [[02 - Diferencia entre Encoding, Hashing y Encripción|Nota 02]], describimos al Hash como una licuadora matemática. 
Una función Hash es un algoritmo que toma cualquier entrada (el Input, también llamado Mensaje), aplica complejas operaciones matemáticas que destruyen el dato original, y genera una salida (el Output, también llamado Hash, Digest o Huella Digital).

Esa "Huella Digital" tiene reglas estrictas e inquebrantables.

### Las 3 Reglas de Oro del Hashing:
1. **Longitud Fija Absoluta:**
   No importa si el input es la letra "A", o si el input es una película en 4K que pesa 10 Gigabytes. Si utilizás el algoritmo MD5 para ambas cosas, el output generado **siempre tendrá exactamente la misma longitud** (ej. 32 caracteres).

2. **Unidireccionalidad (One-Way):**
   Es matemáticamente irreversible. Dada la huella digital (el output), es teóricamente imposible realizar ingeniería inversa y aplicar las matemáticas hacia atrás para recuperar el archivo de la película o la contraseña original. (Es como intentar "des-mezclar" el café con leche para separar la leche del café).

3. **El Efecto Avalancha:**
   Un cambio microscópico en el Input cambia dramáticamente el Output.
   Si calculás el hash de un documento PDF de 1.000 páginas y te da `abcd...`, y luego entrás al documento y le agregás **un solo punto final** (.) en la última página, el hash entero cambiará radicalmente (dando ej. `z9x4...`). Ningún carácter del hash original se conservará.

---

## ⚖️ Hashing en Acción: Integridad

**Escenario Clásico:**
Vas a instalar Kali Linux en tu PC principal. Descargás la imagen `.iso` de 3 Gigabytes desde un servidor en Internet. 
¿Cómo estás seguro de que, durante la descarga, un atacante no hizo "Man-in-the-Middle" (Modificó los paquetes en el aire) y le inyectó un malware a tu instalador de Kali?

1. En la página web oficial, los creadores de Kali aplicaron el algoritmo SHA-256 a su archivo original en su servidor, y publicaron el hash (la huella) como texto: `a1b2c3d4...`
2. Cuando el archivo `.iso` termina de descargar en tu PC, abris tu terminal y utilizás un programa para aplicarle matemáticamente el algoritmo SHA-256 tú mismo al archivo descargado.
3. El programa te escupe el hash calculado localmente. **Compráras tu hash con el hash de la web**.
   - Si son idénticos: Estás 100% seguro de que el archivo que tenés es el original y perfecto (Integridad garantizada).
   - Si difieren: Alguien o algo (un error de red, o un hacker) modificó un byte del archivo. Borrás el instalador de inmediato porque está corrupto o infectado.

*Este proceso es el pan de cada día en el análisis de Malware (DFIR). Los analistas generan un hash del malware que encontraron, y buscan ese hash en bases de datos mundiales (como VirusTotal) para ver si otros investigadores ya analizaron esa misma arma.*

---

## 🛠️ Algoritmos Comunes

Existen múltiples "marcas de licuadoras", desarrolladas a lo largo de las décadas.

- **MD5 (Message Digest 5):**
  Creado en 1992. Genera un hash de **32 caracteres** (128 bits). 
  *Estado actual:* **ROTO Y OBSOLETO** para seguridad, porque es vulnerable a colisiones (ver próxima nota). Aún se usa mucho, pero solo para verificar errores de descarga rápidos no relacionados a ciberseguridad.
- **SHA-1 (Secure Hash Algorithm 1):**
  Genera un hash de **40 caracteres** (160 bits). Fue el rey de Internet durante los años 2000.
  *Estado actual:* **ROTO.** En 2017 Google comprobó que también sufre de vulnerabilidad de colisiones y fue formalmente deprecado.
- **SHA-2 / SHA-256:**
  Es la familia de algoritmos moderna (creada por la NSA). Genera hashes masivos, típicamente de **64 caracteres** (256 bits).
  *Estado actual:* **EL ESTÁNDAR DORADO.** Es lo que utiliza la red Bitcoin y lo que debes usar si vas a diseñar software hoy en día.
- *(También existe SHA-3 y algoritmos específicos para guardar contraseñas que veremos en la próxima nota).*

---

## 📌 Must Know (Imprescindible)
- Las 3 reglas de oro: Salida de tamaño fijo, Unidireccional (irreversible) y el Efecto Avalancha ante cambios mínimos.
- Saber que el principal pilar de la tríada CIA que defiende el hash es la **Integridad**.
- Memorizar que el algoritmo **MD5 y SHA-1 están rotos**, y el estándar actual seguro es la familia **SHA-256**.

---

## 🔄 Preguntas de repaso
1. Aplicás el algoritmo SHA-256 sobre un archivo de texto que contiene la palabra "Gato" y el output genera 64 caracteres. Luego, aplicás el mismo algoritmo sobre una base de datos de empleados de 50 Gigabytes. Sabiendo cómo operan los hashes (las 3 reglas de oro), ¿cuántos caracteres exactos de longitud tendrá el output de la base de datos?
2. Estás redactando las políticas de ciberseguridad para una empresa que almacena reportes médicos de alta sensibilidad. El equipo de desarrolladores antiguos usaba el algoritmo MD5 para generar firmas de integridad de los documentos cada vez que los modificaban. ¿Qué recomendación arquitectónica estricta deberías hacerle al equipo sobre ese algoritmo y por cuál lo reemplazarías?
3. Un analista de malware descarga un supuesto instalador de Adobe Acrobat de la PC de un sospechoso. Pasa el archivo por el comando de hashing local y la huella digital resultante no coincide en lo absoluto con la huella digital que la empresa Adobe publicó en su sitio oficial. Explicá cómo el concepto del "Efecto Avalancha" te permite estar 100% seguro de que el archivo descargado no es el instalador puro original de Adobe.

**➡️ Siguiente nota:** [[05 - Hashing II (Colisiones, Salt y Pepper)]]
