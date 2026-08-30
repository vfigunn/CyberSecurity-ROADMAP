# 01 - La Anatomía del Dominio y Escalada de Privilegios

## 🎯 Objetivos
- Refrescar la estructura jerárquica del Active Directory.
- Conocer la meta final de todo atacante de redes internas.
- Diferenciar la Escalada de Privilegios "Local" versus la de "Dominio".

---

## 🧠 Concepto: El Reino y la Corona

En la [[06 - Windows/08 - Active Directory (La Red Corporativa)|Nota de Active Directory de Windows]], aprendimos que AD es la libreta de direcciones global. 
Todas las computadoras de la empresa ("El Dominio") no son islas independientes; obedecen ciegamente a un Rey: **El Controlador de Dominio (Domain Controller o DC)**.

El DC es el servidor supremo. Contiene la base de datos maestra con TODOS los usuarios y TODOS los hashes (contraseñas) de todos los empleados y directivos de la empresa.

### La Meta Final (El Domain Admin)
El atacante nunca se conforma con hackear la PC de la secretaria. Su meta en una red interna siempre es llegar al DC. 
Una vez que el atacante logra capturar (hackear) al Controlador de Dominio, el atacante obtiene el grupo supremo: **El Domain Admin (Administrador del Dominio)**.

Ser *Domain Admin* equivale a ser el Dios de la red. Podés leer los correos del CEO, cambiar la contraseña de todos, borrar computadoras, instalar cámaras web remotas sin permisos, y robar la base de datos financiera. Todo, en un solo clic.

---

## 🪜 El Camino hacia la Corona: La Escalada de Privilegios

Rara vez un atacante ingresa a la red corporativa directamente como un *Domain Admin* (Porque los administradores protegen esas cuentas con su vida).
El atacante ingresa como la recepcionista o como un invitado (Usuario de Bajos Privilegios). Para llegar al DC, necesita escalar (Subir de nivel).

Hay dos tipos de escalada obligatorias en el mundo del Pentesting:

### 1. Escalada de Privilegios LOCAL (Dueño de la Máquina)
Es el paso 1. El atacante vulnera la PC de la recepcionista. Pero su virus corre con permisos de "Usuario Estándar". Esto significa que no puede robar los Hashes locales ni instalar programas.
Para escalar a **Administrador Local (SYSTEM)**, el atacante abusa de errores dentro de esa única máquina:
- **Misconfigurations (Mala configuración):** Descubre que la recepcionista tiene permisos de escritura en un archivo ejecutable del antivirus. El atacante reemplaza el EXE del antivirus por su virus. Como el antivirus arranca con permisos SYSTEM, al arrancar, el virus obtiene privilegios SYSTEM.
- **Kernel Exploits:** El atacante detecta que el sistema Windows 10 no está actualizado. Ejecuta un Exploit público (Ej. PrintNightmare o LocalPotato) que corrompe la memoria del sistema operativo y le otorga instantáneamente la consola de Administrador (SYSTEM).

*(Nota: Ser SYSTEM te hace dueño de ESA computadora local, pero no te hace dueño del Dominio. Para el servidor, seguís siendo la recepcionista).*

### 2. Escalada de Privilegios de DOMINIO (Dueño de la Red)
Es el paso 2 y el hito definitivo. Ahora que el atacante es dueño absoluto de la computadora de la recepcionista, empieza a buscar formas de engañar al **Controlador de Dominio**.
- **Movimiento Lateral:** Como no puede saltar directo al Rey, salta primero a la PC de un Desarrollador, luego al Servidor de Archivos, y luego al Servidor de IT.
- Durante los saltos (que veremos en este módulo mediante BloodHound, Kerberos y NTLM), el atacante va robando pedazos de Hashes hasta encontrar la computadora donde el *Domain Admin* dejó su cuenta logueada por error. ¡La roba, y la red cae!

---

## 📌 Must Know (Imprescindible)
- **Domain Controller (DC):** El servidor maestro. Su compromiso equivale a la caída completa y catastrófica de toda la empresa (Game Over).
- **Escalada Local:** Lograr pasar de "Usuario Común" a "NT AUTHORITY\SYSTEM" en **una sola** computadora para poder apagar su antivirus local y robar sus secretos en memoria. (Ej: Abuso de servicios mal configurados o Fallas del Kernel).
- **Escalada de Dominio:** Salir de la computadora local (Lateral Movement) para engañar al Rey (DC) y robarle al Active Directory la credencial suprema de **Domain Admin**.

---

## 🔄 Preguntas de repaso
1. Lograste vulnerar la laptop de un pasante de Marketing que no estaba actualizada mediante un ataque de Phishing. Desde tu conexión remota a su computadora, intentás volcar (extraer) los Hashes NTLM de su disco duro usando Mimikatz, pero la consola te arroja: "Access Denied: Se requieren privilegios Administrativos". ¿A qué proceso o tipo de Escalada te estás enfrentando y debes superar como requisito inicial obligatorio?
2. Siguiendo el caso anterior: Una vez que superás la barrera y te convertís en el Dios local (`SYSTEM`) de la laptop del pasante de Marketing. ¿Podés ejecutar un comando para borrar inmediatamente a todos los usuarios de la corporación del Servidor Central? ¿Por qué?
3. En la taxonomía del Blue Team (Defensiva). ¿Por qué se dice universalmente que el grupo "Domain Admins" (Administradores de Dominio) debe tener la menor cantidad de usuarios humanos posibles y jamás deben utilizar esas cuentas para navegar por Internet o leer su correo electrónico diario? (Asociá tu respuesta a las metas del Red Team).

**➡️ Siguiente nota:** [[02 - Mapeo y Reconocimiento (BloodHound)]]
