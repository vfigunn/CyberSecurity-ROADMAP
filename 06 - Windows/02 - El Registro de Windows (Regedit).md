# 02 - El Registro de Windows (Regedit)

## 🎯 Objetivos
- Entender qué es y por qué es el Sistema Nervioso Central de Windows.
- Conocer la estructura jerárquica de Hives (Claves) y sus usos.
- Comprender por qué es el escondite favorito del Malware para lograr la "Persistencia".

---

## 🧠 Concepto: La base de datos definitiva

En [[03 - Linux/05 - La Jerarquía de Archivos (FHS)|Linux]], dijimos que toda la configuración del sistema (DNS, usuarios, programas instalados) se guardaba en archivos de texto plano desparramados dentro de la carpeta `/etc`.

**Windows no usa archivos de texto esparcidos.** 
Microsoft decidió que TODA la configuración, absolutamente TODO sobre cómo se ve, cómo arranca, qué programas están instalados, qué drivers funcionan, y hasta qué fondo de pantalla tenés, se guarda en una gigantesca y compleja **Base de Datos central jerárquica**.

A esta base de datos se le llama el **Registro de Windows**.
La herramienta oficial para visualizarlo y modificarlo (extremadamente peligrosa si se usa mal) se llama **`regedit.exe`**.

---

## 📂 La Estructura del Registro

El Registro parece un árbol de carpetas normal de Windows, pero no son carpetas físicas en tu disco duro, son "Claves" virtuales de base de datos.
La base se divide en 5 ramas principales (llamadas **Hives** o Raíces). 
Como Analista, solo te importarán las dos más grandes: **HKLM** y **HKCU**.

### 1. HKLM (`HKEY_LOCAL_MACHINE`)
Esta clave afecta a la **Máquina completa y a todos sus usuarios**. 
- Todo lo que instales acá, lo sufrirán todos. 
- Guarda la configuración de la red, los programas instalados para toda la PC, y los servicios de arranque de Windows.
- Requiere privilegios de Administrador para ser modificada.

### 2. HKCU (`HKEY_CURRENT_USER`)
Esta clave afecta **únicamente al usuario que está logueado en ese momento**.
- Guarda las preferencias visuales (fondos de pantalla, colores), el historial de navegadores integrado, y los programas que ese usuario específico decidió arrancar junto con Windows.
- El usuario normal puede modificar esta rama sin requerir permisos de Administrador.

*Las otras tres (HKCR, HKU, HKCC) son sub-ramas técnicas u ocultas que se enlazan dinámicamente, rara vez atacadas directamente.*

---

## 🪱 El Malware y la "Persistencia"

En Ciberseguridad Ofensiva (Red Team/Malware), el objetivo no es solo infectar la PC, es **asegurarse de que el virus vuelva a arrancar automáticamente mañana cuando el usuario reinicie la computadora**. A esto se le llama **Lograr Persistencia**.

El Registro de Windows es el lugar número uno del planeta para esconder Persistencia. Existen claves específicas diseñadas para que los programas se ejecuten automáticamente (Auto-Start Extensibility Points - ASEP).

Las dos rutas más famosas (y escaneadas por el Blue Team) son las claves **`Run`** y **`RunOnce`**:

> `HKLM\Software\Microsoft\Windows\CurrentVersion\Run`
> `HKCU\Software\Microsoft\Windows\CurrentVersion\Run`

- Si un Malware crea un pequeño valor dentro de esa ruta y escribe la dirección `"C:\Temp\virus.exe"`, Windows ejecutará ese virus silenciosamente cada vez que se prenda la PC.
- Si el atacante logró ser Administrador, lo esconderá en la clave `HKLM` (Se ejecuta prenda quien prenda la PC).
- Si el atacante no tiene permisos, lo esconderá en la clave `HKCU` (Se ejecuta solo cuando la víctima se loguea en su cuenta).

---

## 🛡️ Análisis Forense (DFIR)

Para un investigador forense (Blue Team), el Registro es una mina de oro de evidencia.
Aunque el atacante haya borrado un archivo de Word confidencial, o desinstalado su herramienta de Hacking, el Registro guarda huellas imborrables.

- **Historial de Ejecución (`UserAssist`):** El registro guarda la hora exacta, el día y la cantidad de veces que hiciste doble clic en CUALQUIER archivo `.exe` a lo largo de los años. 
- **Dispositivos USB (`USBSTOR`):** Cada vez que conectás un Pendrive (incluso uno que conectaste hace 4 años por 5 minutos y nunca más), el Registro guarda el número de serie de hardware exacto de ese Pendrive. Ideal para juicios y fuga de datos corporativos.

---

## 📌 Must Know (Imprescindible)
- Diferencia crítica: **HKLM** afecta a todos y requiere admin. **HKCU** afecta al usuario actual y no requiere admin.
- La utilidad del Registro en el mundo de la ciberseguridad: Se usa intensamente para **Persistencia** de malware (Clave `Run`) y para obtención de evidencia **Forense** histórica.

---

## 🔄 Preguntas de repaso
1. Un atacante diseña un pequeño Malware (Payload) a través de un documento de Office malicioso y logra infectar a la secretaria de recursos humanos (que tiene un usuario estándar sin privilegios de administrador). Para que el virus inicie automáticamente con Windows mañana, ¿en qué "Hive" principal (HKLM o HKCU) intentará el Malware inyectar su ruta en la clave de auto-arranque `Run`?
2. Como investigador forense (Blue Team) de un robo corporativo, te dicen que el sospechoso copió los planos del nuevo motor en un pendrive a las 3:00 AM de ayer. ¿Por qué utilizarías una herramienta para analizar el Registro de la computadora, si el sistema de archivos tradicional (`C:\`) no guarda el historial de hardware enchufado?
3. Habiendo estudiado Linux, compará conceptualmente dónde irías en un servidor Ubuntu para cambiar el puerto de escucha del servidor Web (Apache/Nginx) versus dónde irías a buscar esa configuración en un servidor Windows si estuviera nativamente integrado.

**➡️ Siguiente nota:** [[03 - Servicios y Tareas Programadas]]
