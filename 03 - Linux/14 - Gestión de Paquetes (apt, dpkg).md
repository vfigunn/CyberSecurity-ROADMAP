# 14 - Gestión de Paquetes (`apt`, `dpkg`)

## 🎯 Objetivos
- Entender qué son los Repositorios y cómo Linux instala software de manera segura (mucho antes que las "App Stores").
- Conocer la diferencia entre un gestor de paquetes de bajo nivel (`dpkg`) y alto nivel (`apt`).
- Aprender los comandos básicos para actualizar e instalar software.

---

## 🧠 Concepto: Repositorios (Repositories)

En Windows tradicional, cuando querés un programa, lo buscás en Google, entrás a una página web de dudosa procedencia, descargás un archivo `.exe`, le hacés doble clic, y rezás para que no tenga malware (Siguiente, Siguiente, Siguiente, Finalizar). Es un modelo inherentemente inseguro (y por eso los Antivirus se volvieron millonarios).

En Linux (desde los 90s), el concepto es radicalmente distinto y más seguro. Utilizan **Repositorios**.
Los repositorios son servidores oficiales en Internet controlados por los creadores de la Distribución (ej. por Canonical para Ubuntu, o por Offensive Security para Kali). Estos servidores contienen miles de programas pre-aprobados, testeados, y **firmados criptográficamente** (Capa de Autenticidad).

Para instalar un programa (Paquete), vos no abrís el navegador. Le ordenás a un comando en la terminal que se conecte a ese repositorio oficial seguro, descargue el programa y lo instale por vos.

---

## 🛠️ Herramientas: La familia Debian/Ubuntu/Kali

*(Existen otras familias como RedHat/Fedora que usan `yum` o `dnf`, pero en la inmensa mayoría del mundo de la ciberseguridad usarás distros derivadas de Debian).*

El ecosistema usa archivos comprimidos con extensión **`.deb`** (el equivalente a los `.msi` o `.exe` de instalación).

### 1. `dpkg` (El trabajador de bajo nivel)
Si alguien te pasó un archivo `.deb` en un pendrive (sin usar los repositorios de Internet), usás `dpkg` para instalarlo a la fuerza localmente.
- **Instalar:** `sudo dpkg -i programa_descargado.deb`
- **Problema de `dpkg`:** No resuelve **Dependencias**. Si el programa necesita una librería gráfica que tu sistema no tiene, la instalación fallará miserablemente.

### 2. `apt` / `apt-get` (El administrador inteligente)
`apt` (Advanced Package Tool) fue creado para solucionar el problema de `dpkg`. `apt` habla con los servidores de Internet, busca el programa y, maravillosamente, **calcula, descarga e instala todas las dependencias adicionales que necesite de forma automática**.

**Ciclo de vida normal de un administrador:**

1. **Actualizar el catálogo (`apt update`)**
   Este es el comando más importante. NO instala ni actualiza los programas de tu PC. Solo se conecta a los servidores de internet, descarga la "Lista de catálogo/precios" de las versiones más nuevas de todos los programas del mundo, y la guarda en tu PC. Siempre se ejecuta primero.
   ```bash
   $ sudo apt update
   ```

2. **Actualizar el sistema (`apt upgrade`)**
   Ahora sí, compara la lista nueva (que descargaste con el paso 1) con los programas que tenés instalados. Si encuentra que hay versiones viejas en tu PC, las actualiza automáticamente instalando los parches de seguridad.
   ```bash
   $ sudo apt upgrade
   ```

3. **Instalar un programa nuevo (`apt install`)**
   ```bash
   $ sudo apt install nmap
   ```

4. **Desinstalar un programa (`apt remove`)**
   Borra el binario del programa (pero a veces deja sus archivos de configuración de la carpeta `/etc` por las dudas). Si querés borrarlo y erradicar todo rastro (limpieza profunda), usás `purge`.
   ```bash
   $ sudo apt purge nmap
   ```

---

## ❓ ¿Por qué importa en Seguridad?

- **Gestión de Vulnerabilidades (Patch Management):** Uno de los controles más críticos que exige cualquier normativa internacional (como ISO 27001) es mantener los servidores actualizados. Un atacante siempre probará exploits antiguos (1-Day exploits) con la esperanza de que el administrador se haya olvidado de hacer `apt update && apt upgrade`.
- **Envenenamiento de Repositorios:** Si un atacante logra comprometer la lista de repositorios locales (ubicada en el archivo `/etc/apt/sources.list`) y la apunta hacia el servidor malicioso del atacante... la próxima vez que el administrador inocente intente instalar o actualizar un programa usando `apt`, estará instalando software troyanizado directamente desde el servidor del hacker.

---

## 📌 Must Know (Imprescindible)
- El concepto superior de seguridad de usar Repositorios frente a descargar instaladores `.exe`.
- La diferencia crítica entre `apt update` (Actualizar lista) y `apt upgrade` (Actualizar el software en sí).
- Cómo instalar un programa (`apt install`).

---

## 🔄 Preguntas de repaso
1. Querés instalar un editor de texto nuevo (`nano`), así que ejecutás `sudo apt install nano`. La terminal te lanza un error diciendo que el archivo "No pudo ser encontrado en el repositorio 404 Not Found". Tu compañero te dice que debes ejecutar un comando previo para refrescar la lista de links de descarga antes de intentar instalar. ¿Qué comando es ese?
2. Te han pasado una herramienta de hacking altamente customizada en un archivo llamado `herramienta.deb` a través de un chat seguro, ya que no existe en los repositorios oficiales de Kali. ¿Qué herramienta o comando deberías usar para instalar ese archivo local manualmente?
3. Explicá por qué el sistema de dependencias automáticas de `apt` es una ventaja masiva frente a la instalación manual con `dpkg`.

**➡️ Siguiente nota:** [[15 - Archivos Comprimidos (tar, gzip)]]
