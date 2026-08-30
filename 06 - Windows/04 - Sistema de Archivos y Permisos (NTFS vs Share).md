# 04 - Sistema de Archivos y Permisos (NTFS vs Share)

## 🎯 Objetivos
- Conocer NTFS, el sistema de archivos moderno de Microsoft.
- Comprender profundamente la diferencia entre Permisos Locales (NTFS) y Permisos de Red (Share Permissions).
- Entender cómo la herencia y los conflictos de permisos afectan a las corporaciones.

---

## 🧠 Concepto: NTFS (New Technology File System)

En los años 90 (con Windows 95), Microsoft usaba FAT32, un sistema sin reglas ni seguridad, donde cualquiera que prendiera la PC podía leer todos los archivos. 
NTFS es el sistema de archivos actual (desde Windows NT/XP). Trajo tres innovaciones absolutas a la empresa:
1. **Journaling (Diarios):** Recuperación ante fallas de energía; no más archivos corruptos por apagones.
2. **ADS (Alternate Data Streams):** Permite "esconder" archivos dentro de otros archivos (Muy usado por el malware antiguo).
3. **ACLs (Listas de Control de Acceso):** Un sistema granular de permisos. Ahora se puede decir: *"A esta carpeta solo puede entrar el Grupo 'Contadores' y pueden leer, pero no borrar"*.

---

## 🛡️ Los Dos Mundos de los Permisos

Este es el concepto más difícil de digerir para los administradores Junior, y la causa número uno de fugas de información interna en las empresas.
En una corporación, la Carpeta de "Recursos Humanos" no está en tu PC local. Está alojada en un Servidor en el sótano, y accedés a ella a través de la Red (La famosa "Unidad Compartida Z:").

Para que vos puedas abrir un archivo dentro de esa carpeta a través de la red corporativa, debes superar **DOS puertas de seguridad independientes**.

### 1. Puerta Exterior: Permisos de Recurso Compartido (Share Permissions)
Controlan quién puede "ver y entrar" a la carpeta desde el cable de red (desde afuera del servidor).
- Son permisos simples. Solo tienen 3 niveles: **Leer, Cambiar (Modificar), o Control Total**.
- Si el Servidor le dice a la puerta exterior: *"Compartir esta carpeta con Permiso de solo LECTURA para todo el mundo (Everyone)"*.

### 2. Puerta Interior: Permisos NTFS (Locales)
Controlan quién puede tocar los datos físicos alojados en el disco duro (desde adentro). 
- Son hiper granulares (Leer, Escribir, Modificar, Borrar subcarpetas, Cambiar Dueño, etc.).
- Si el disco duro del Servidor tiene un permiso NTFS que dice: *"El usuario Juan tiene CONTROL TOTAL sobre esta carpeta"*.

### 💥 La Regla del Conflicto (El Más Restrictivo Gana)
Si Juan intenta acceder a la carpeta desde otra computadora por la red:
- La Puerta Exterior (Share) le dice a Juan: "Tu permiso de entrada a través de la red es solo LECTURA".
- Juan logra pasar. Llega a la Puerta Interior (NTFS) del disco. El disco le dice a Juan: "Oh, tu permiso físico aquí es CONTROL TOTAL".

¿Puede Juan borrar el archivo?
**NO.** Las matemáticas de seguridad de Windows calculan ambos permisos y **siempre aplican el permiso más restrictivo de los dos combinados**. Como Juan entró a través de un canal (Share) que lo limitó a "Lectura", no puede escribir ni borrar, sin importar que su permiso físico NTFS dijera lo contrario. 

> *Nota de Blue Team:* Por esta complicación, la recomendación de Microsoft y la industria es siempre simplificar: A la "Puerta Exterior" (Share) se le da permiso "Control Total a Todo el Mundo", y la verdadera seguridad y los bloqueos a Juan se configuran minuciosamente en la "Puerta Interior" (NTFS).

---

## 🧬 La Herencia (Inheritance)

Por defecto, en NTFS, si creas una carpeta llamada "Secreta" dentro del "Disco C:\", los permisos de "Secreta" son copiados automáticamente de su padre ("Disco C:\"). 
Esto facilita la administración (Si das acceso a la carpeta "Finanzas", automáticamente los usuarios tendrán acceso a los miles de archivos y subcarpetas que contenga adentro).

Sin embargo, a veces es necesario **Romper la Herencia**. Por ejemplo, dentro de la carpeta general de "Finanzas" (donde entran todos), querés crear una subcarpeta de "Salarios de Directivos" donde solo entre el Gerente. En ese caso, desactivás la herencia de esa carpeta puntual, borrás los permisos copiados, y agregás únicamente al usuario del Gerente.

---

## 📌 Must Know (Imprescindible)
- Conocer la diferencia entre la solidez de **NTFS** y la falta de seguridad del obsoleto **FAT32** (usado hoy solo en pendrives y cámaras).
- Memorizar la regla de oro del acceso por red: Se evalúan los permisos de Share y los permisos NTFS de forma paralela, y **el más restrictivo de los dos es el que se aplica en la vida real.**
- Entender el concepto de Herencia de padre a hijo, y saber que puede ser desactivada para casos de seguridad puntual.

---

## 🔄 Preguntas de repaso
1. Un Administrador configura los permisos de la carpeta de red `\\Servidor\Proyecto_Alfa`. En la pestaña de Compartición de Red (Share), le otorga al grupo de empleados el permiso de "Cambiar/Modificar". En la pestaña de seguridad del disco duro (NTFS), le otorga al grupo el permiso de solo "Lectura". Si un empleado accede a la carpeta por la red e intenta guardar un documento nuevo adentro, ¿qué sucederá y por qué?
2. Un atacante extrae el Disco Duro físico de una laptop robada de un directivo, lo conecta mediante USB a su propia máquina Linux (Kali) e intenta leer los documentos. Sabiendo que los permisos NTFS actúan a nivel de Sistema Operativo, ¿podrá el atacante leer los archivos (asumiendo que el disco no estaba encriptado con Bitlocker ni tecnología criptográfica)?
3. Creas una carpeta llamada "Pública" y le das acceso de escritura a todos. Luego, adentro, creas una subcarpeta llamada "Privada". Sin que toques nada más, todos los usuarios pueden escribir en "Privada". ¿Qué propiedad automática del sistema de archivos NTFS provocó esto, y qué tendrías que hacer en las propiedades de la subcarpeta para evitarlo?

**➡️ Siguiente nota:** [[05 - Introducción a Active Directory (El Corazón Corporativo)]]
