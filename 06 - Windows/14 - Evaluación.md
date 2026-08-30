# 14 - Evaluación del Módulo 06 (Windows & AD)

## 📝 Instrucciones
Evaluación final del módulo. Múltiples preguntas basadas en arquitecturas de Microsoft, ataques de Active Directory y manejo de procesos.
Anotá tus respuestas y luego verificalas con el solucionario.

> **Importante:** Las respuestas correctas y explicaciones están en: `[[29 - Evaluaciones/Respuestas/Módulo 06 - Respuestas]]`.

---

## 🎯 Sección 1: Arquitectura y Sistema de Archivos

**1. Un atacante quiere extraer en texto plano las contraseñas de los usuarios que han iniciado sesión recientemente en una máquina local. ¿A qué proceso crítico de la memoria RAM de Windows (en Ring 3) deberá inyectarle código su herramienta ofensiva?**
A) `smss.exe`
B) `svchost.exe`
C) `lsass.exe`
D) `services.msc`

**2. En la base de datos central de Windows (El Registro), un Malware busca esconderse para asegurar que se ejecutará en cada inicio de la computadora, pero el atacante solo logró obtener acceso mediante un "Usuario Estándar" sin privilegios administrativos. ¿En qué Clave (Hive) del registro alojará obligatoriamente su puerta trasera?**
A) `HKEY_LOCAL_MACHINE` (HKLM)
B) `HKEY_CURRENT_USER` (HKCU)
C) `HKEY_CLASSES_ROOT` (HKCR)
D) `HKEY_USERS` (HKU)

**3. Un usuario intenta acceder a una carpeta por la Red. Sus Permisos Share (de red) le otorgan `Modificar`. Sin embargo, sus Permisos NTFS (locales en el disco) le otorgan únicamente `Lectura`. ¿Qué sucederá cuando intente crear un archivo nuevo dentro de la carpeta?**
A) Podrá crearlo, porque los Permisos Share son los que dominan las peticiones de red.
B) No podrá crearlo, porque en caso de conflicto, Windows siempre aplica el permiso más restrictivo resultante de la combinación.
C) El sistema lanzará un error de "Pantalla Azul" por conflicto.
D) Podrá crearlo, pero se borrará automáticamente al cerrar sesión.

---

## 🎯 Sección 2: Fundamentos de Active Directory

**4. ¿Cómo se denomina al límite de seguridad máximo absoluto en la arquitectura lógica de Active Directory (El límite de confianza más alto que existe)?**
A) Domain (Dominio)
B) Organizational Unit (Unidad Organizativa)
C) Tree (Árbol)
D) Forest (Bosque)

**5. Los grupos de Ransomware a menudo comprometen servidores poco importantes y escalan privilegios lateralmente hasta llegar al servidor de infraestructura más importante de toda la empresa, donde roban el archivo `NTDS.dit`. ¿Cómo se llama ese servidor sagrado?**
A) Exchange Mail Server
B) IIS Web Server
C) Domain Controller (Controlador de Dominio)
D) File Server

**6. Como administrador, querés delegar permisos específicos al Soporte Técnico para que solo puedan resetear contraseñas de las personas de Ventas, sin afectar al resto de la compañía. ¿Qué objeto lógico (una especie de "carpeta" virtual) utilizarías en Active Directory para agrupar a los empleados de Ventas y asignar la política?**
A) Árbol (Tree)
B) Root Domain
C) Organizational Unit (OU)
D) GPO

---

## 🎯 Sección 3: Autenticación, Políticas y PowerShell

**7. En el protocolo NTLMv2, ¿Por qué es posible que un atacante se autentique exitosamente contra un servidor sin tener la más mínima idea de cuál es la contraseña en texto claro del usuario?**
A) Porque el servidor siempre acepta conexiones desde IPs de la red local.
B) Porque en la arquitectura NTLM, poseer el Hash es funcionalmente idéntico a poseer la contraseña (ataque Pass-The-Hash).
C) Porque NTLM envía las contraseñas sin cifrar por la red.
D) Porque el servidor utiliza Criptografía Asimétrica (RSA).

**8. Dentro del protocolo de Autenticación Kerberos, un atacante logra realizar el ataque conocido como "Kerberoasting". ¿Qué logra exactamente el atacante con esta técnica?**
A) Falsificar un Golden Ticket utilizando la cuenta `krbtgt`.
B) Descargar TGS (Tickets de Servicio) encriptados y llevarlos a su computadora offline para crackear la contraseña de las Cuentas de Servicio.
C) Denegar el servicio (DDoS) del Key Distribution Center (KDC).
D) Crear un nuevo Bosque (Forest) malicioso.

**9. Eres parte del Blue Team y descubrís que 5,000 computadoras acaban de modificar su clave de registro de Auto-Arranque de forma simultánea, infectándose con Ransomware en menos de 90 minutos. Asumiendo que el atacante ya tenía privilegios de Domain Admin, ¿Qué herramienta nativa de administración de Windows Server utilizó para orquestar este ataque masivo?**
A) BloodHound
B) Task Scheduler Local
C) Group Policy Objects (GPOs)
D) Ping Sweep

**10. El "Fileless Malware" (Malware sin archivos) ha revolucionado la industria. ¿Qué característica fundamental de PowerShell permite que este ataque sea tan devastador contra los Antivirus tradicionales basados en firmas de discos duros?**
A) PowerShell está escrito en C++.
B) PowerShell permite ejecutar comandos (como `Invoke-WebRequest`) inyectando el código malicioso directamente en la Memoria RAM, sin escribir nunca un archivo `.exe` en el disco local.
C) PowerShell puede leer el archivo NTDS.dit sin necesidad de permisos.
D) PowerShell bloquea las actualizaciones de Windows Defender.

---

> **¿Terminaste?**
> Buscá el archivo en la carpeta de evaluaciones para comparar tus resultados (Módulo 06 - Respuestas).

**➡️ Siguiente nota:** [[15 - Resumen]]
