# 06 - Búsqueda de Archivos (find, locate)

## 🎯 Objetivos
- Aprender a localizar archivos y carpetas perdidos u ocultos en el sistema.
- Conocer la diferencia de funcionamiento entre `locate` (rápido pero desactualizado) y `find` (lento, pero poderoso y en tiempo real).

---

## 🧠 Concepto

A diferencia de Windows, donde tenés una barra de búsqueda en cada ventana del Explorador, en la terminal de Linux tenés que usar comandos para encontrar un archivo cuando no sabés en qué carpeta está escondido. Esto es el pan de cada día en ciberseguridad (ej. buscar dónde se instaló un malware, o buscar dónde guarda las contraseñas un servidor web).

Existen dos herramientas principales, con filosofías totalmente opuestas.

---

## 1. `locate` (La búsqueda indexada)

`locate` es como buscar un libro en el catálogo de una biblioteca usando la computadora del bibliotecario. 
El comando no va a revisar los estantes físicos de la biblioteca; simplemente consulta una base de datos interna (`updatedb`) que tiene un índice de todos los archivos del sistema.

- **Ventaja:** Es absurdamente rápido (encuentra cosas en milisegundos).
- **Desventaja:** La base de datos suele actualizarse solo una vez al día. Si creás un archivo malicioso ahora mismo y un analista usa `locate` un minuto después, el comando dirá que el archivo no existe. (Podés forzar la actualización ejecutando `sudo updatedb`).

**Uso básico:**
```bash
$ locate contraseñas.txt
/home/usuario/Documentos/contraseñas.txt
```

---

## 2. `find` (El sabueso implacable)

`find` es la herramienta suprema. A diferencia de locate, `find` es una búsqueda **en vivo y en tiempo real**. Recorre el disco duro, carpeta por carpeta, buscando lo que le pidas.

- **Ventaja:** Siempre es exacto. Es increíblemente poderoso (podés buscar por tamaño, permisos, fecha, dueño).
- **Desventaja:** Puede ser lento si buscás en todo el disco duro completo (desde `/`).

**Sintaxis principal de `find`:**
`find [Dónde buscar] [Qué buscar]`

### A) Buscar por nombre exacto (`-name`)
Quiero buscar un archivo llamado `config.php` empezando desde el Directorio Raíz (`/`) hacia abajo.
```bash
$ find / -name config.php
```
*(Si no sabés si está en mayúsculas o minúsculas, podés usar `-iname` para ignorar el caso).*

### B) Búsqueda con Comodines (Wildcards - `*`)
El asterisco (`*`) significa "cualquier cosa".
- Si buscás `*.jpg`, encontrará todo lo que termine en `.jpg`.
- Si buscás `backup*`, encontrará todo lo que empiece con "backup" (ej. backup1, backup_base_de_datos).
```bash
$ find /var -name "*.log"
```
*(Este comando buscará todos los archivos de logs dentro de la carpeta /var).*

### C) Buscar por otras propiedades (Avanzado)
En ciberseguridad, solemos buscar archivos que cumplan condiciones sospechosas.
- **Por tamaño (`-size`):** Buscar archivos mayores a 50 Megabytes.
  `find / -size +50M`
- **Por usuario (`-user`):** Buscar archivos que le pertenezcan al usuario "root" pero que estén escondidos en la carpeta de un usuario normal.
  `find /home/juan -user root`

> **Nota sobre el ruido:** Cuando haces un `find /`, el comando intentará entrar a carpetas restringidas de administradores y te llenará la pantalla de errores diciendo "Permiso denegado" (Permission denied). Más adelante, en la nota de [[08 - Redirecciones y Pipes|Redirecciones]], aprenderemos cómo enviar esos errores "a la basura" para que no nos molesten.

---

## 📌 Must Know (Imprescindible)
- La diferencia fundamental: `locate` usa una base de datos desactualizada, `find` busca en tiempo real.
- La sintaxis de `find` para buscar por nombre y el uso del comodín asterisco `*`.

---

## 🔄 Preguntas de repaso
1. Descargaste un archivo desde internet hace 10 minutos, pero no recordás en qué carpeta de tu sistema lo guardaste. ¿Por qué el comando `locate` probablemente no te ayude a encontrarlo de inmediato?
2. Escribí el comando exacto de `find` para buscar en la carpeta actual (`.`) todos los archivos que terminen con la extensión `.pdf`.
3. ¿Qué parte del disco duro revisará el siguiente comando? `find /home/maria/ -name "secreto.txt"`

**➡️ Siguiente nota:** [[07 - Búsqueda en Textos (grep)]]
