# 15 - Archivos Comprimidos (`tar`, `gzip`)

## 🎯 Objetivos
- Diferenciar el concepto de "Empaquetado" vs "Compresión".
- Aprender a empaquetar y comprimir directorios usando `tar`.
- Entender por qué un atacante (o un SysAdmin) utiliza extensivamente estos comandos para mover información (Exfiltración/Backups).

---

## 🧠 Concepto: Dos procesos diferentes

En el mundo Windows, si usás WinRAR o ZIP para comprimir una carpeta con fotos, el programa hace dos cosas a la vez: agrupa las 10 fotos juntas, y usa algoritmos matemáticos para achicar su tamaño.
En la filosofía Unix/Linux (donde cada herramienta hace una sola cosa perfecta, ver [[08 - Redirecciones y Pipes|Nota 08]]), estos son dos pasos separados:

1. **Empaquetado (Archiving):** Agarra 100 archivos sueltos y carpetas, y los "pega" juntos en un único archivo grande y macizo. No achica el tamaño. La herramienta maestra para esto es **`tar`** (Tape Archive, nombrado así porque antes se guardaban en cintas magnéticas).
2. **Compresión (Compression):** Toma un archivo (por ejemplo, el bloque inmenso que creó `tar`) y aplica matemáticas para reducir su peso en megabytes. Las herramientas comunes son **`gzip`** (extensión `.gz`), `bzip2` o `xz`.

Por suerte, el comando `tar` moderno nos permite llamar a los algoritmos de compresión internamente (mediante Flags), haciendo los dos pasos de una sola vez para nuestra comodidad.

---

## 🛠️ Usando `tar`

Los archivos finales suelen tener la extensión **`.tar.gz`** (o simplemente `.tgz`). Esto se conoce coloquialmente como un "Tarball".

La sintaxis del comando `tar` se basa totalmente en un bloque de letras (flags) que definen qué querés hacer (Crear o Extraer).

### 1. Comprimir (Crear un Tarball)
Supongamos que sos un atacante y has recopilado 50 archivos PDF de secretos corporativos de una carpeta, y te los querés enviar a tu servidor. 50 transferencias levantarían alarmas, así que los vas a agrupar y comprimir en un solo archivo pequeño primero.

**Comando:** `tar -czvf secretos.tar.gz /home/usuario/Documentos`

¿Qué significan las letras? (Es vital entenderlas para leer manuales técnicos):
- **`c` (Create):** Crea un nuevo archivo.
- **`z` (Gzip):** Comprímelo matemáticamente usando el algoritmo Gzip.
- **`v` (Verbose):** (Parlanchín). Te va imprimiendo en pantalla los nombres de los archivos a medida que los procesa, para que sepas que no se tildó.
- **`f` (File):** Indica que lo que viene inmediatamente después de esta letra es el *nombre del archivo final*. (Por eso la `f` siempre debe ir última en el bloque de letras).

### 2. Descomprimir (Extraer un Tarball)
Acabás de descargar el código fuente de una nueva herramienta de Hacking y viene en formato `herramienta.tar.gz`. Tenés que extraerla en tu carpeta actual.

**Comando:** `tar -xzvf herramienta.tar.gz`

- **`x` (eXtract):** Extrae el archivo en lugar de crearlo.
- El resto de las letras (`z`, `v`, `f`) hacen lo mismo que arriba.

*(Truco: Memorizalo mentalmente como "Czevef" para Crear y "Xzevef" para Extraer).*

---

## ❓ ¿Por qué importa en Seguridad?

- **Para el Defensor (Blue Team / IT):** Esta es la herramienta principal para hacer Backups. Antes de aplicar un parche crítico a un servidor web o tocar el archivo `/etc/passwd`, un SysAdmin responsable hará una copia empaquetada de toda la carpeta de configuración en segundos para poder restaurarla si algo sale mal.
- **Para el Atacante (Red Team):** Como dijimos, es la herramienta perfecta para la **Exfiltración de Datos**. Empaquetar y comprimir una base de datos gigante no solo ahorra tiempo de transferencia para evadir detección, sino que enviar un solo archivo TCP levanta muchas menos alarmas en el Firewall y en el IDS que realizar 10.000 conexiones separadas para extraer archivos sueltos.

---

## 📌 Must Know (Imprescindible)
- La diferencia filosófica entre empaquetar (`tar` base) y comprimir (`gzip`).
- Saber de memoria los flags `-czvf` (Crear) y `-xzvf` (Extraer).
- Entender el concepto y uso del flag `-v` (Verbose) que se repite en cientos de herramientas de Linux.

---

## 🔄 Preguntas de repaso
1. Estás leyendo el historial de comandos de un atacante (Post-Explotación) y ves que ejecutó: `tar -czf backup.tar.gz /var/log/`. ¿Qué estaba haciendo el atacante y por qué faltar la letra `v` no afectó el resultado del archivo creado?
2. Descargaste una herramienta defensiva llamada `ids-monitor.tar.gz`. Escribí el comando exacto para extraer todo su contenido en tu carpeta actual.
3. (Integración) Si intentaras hacer el comando anterior en una carpeta del sistema protegida como `/etc/` o `/opt/`, y no tuvieras permisos, ¿qué comando tendrías que anteponer al tuyo para que funcione (pidiéndote contraseña)?

**➡️ Siguiente nota:** [[16 - SSH y Transferencia Segura (scp)]]
