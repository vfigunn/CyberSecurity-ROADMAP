# Respuestas Evaluación Módulo 06 - Windows & AD

A continuación se presentan las respuestas correctas de la evaluación del [[06 - Windows/14 - Evaluación|Módulo 06]], junto con la justificación técnica de cada una.

---

### Sección 1: Arquitectura y Sistema de Archivos

**1. C) `lsass.exe`**
> *Justificación:* El proceso `Local Security Authority Subsystem Service (LSASS)` almacena en memoria los tokens de sesión, credenciales en texto claro (dependiendo de la configuración del sistema) y hashes NTLM. Es el blanco número uno para el robo de credenciales mediante herramientas como Mimikatz.

**2. B) `HKEY_CURRENT_USER` (HKCU)**
> *Justificación:* Modificar la rama `HKLM` afecta a toda la máquina y requiere estrictos permisos de Administrador (Ring 3 elevado). Como el atacante es un usuario estándar, solo posee permisos de escritura sobre su propio entorno personal en el registro (HKCU), utilizando claves como `HKCU\Software\Microsoft\Windows\CurrentVersion\Run`.

**3. B) No podrá crearlo, porque en caso de conflicto, Windows siempre aplica el permiso más restrictivo resultante de la combinación.**
> *Justificación:* Aunque la red (Share) intente darle permisos de modificación, al cruzar la puerta física hacia el disco, NTFS evalúa que solo tiene Lectura. Las matemáticas de seguridad del S.O. priorizan el bloqueo más estricto entre los dos permisos solapados, negando la escritura.

---

### Sección 2: Fundamentos de Active Directory

**4. D) Forest (Bosque)**
> *Justificación:* Mientras que el Dominio representa una frontera administrativa (y por años se consideró la frontera de seguridad), la arquitectura de relaciones de confianza (Trusts) implica que un compromiso total de un dominio puede extenderse a otros dominios del mismo árbol. El único y verdadero límite de aislamiento absoluto de seguridad es el Bosque entero.

**5. C) Domain Controller (Controlador de Dominio)**
> *Justificación:* El Domain Controller es el "Rey" del ecosistema. Alberga el archivo maestro de base de datos de identidades, `NTDS.dit`, el cual contiene absolutamente todas las cuentas, grupos, políticas y los hashes de contraseñas de cada empleado y servicio. Su compromiso significa la toma total (Takeover) de la corporación.

**6. C) Organizational Unit (OU)**
> *Justificación:* Las OUs son los "contenedores" lógicos (carpetas) dentro de un dominio diseñados específicamente para agrupar recursos y delegar autoridad granular. Esto permite aplicar GPOs (Políticas) o dar permisos de administración a técnicos Junior sin otorgarles llaves sobre todo el Dominio.

---

### Sección 3: Autenticación, Políticas y PowerShell

**7. B) Porque en la arquitectura NTLM, poseer el Hash es funcionalmente idéntico a poseer la contraseña (ataque Pass-The-Hash).**
> *Justificación:* NTLM es un protocolo de Desafío/Respuesta, pero debido a un diseño heredado, la "Respuesta" se genera aplicando matemáticas directamente sobre el Hash de la contraseña. Por ende, quien obtenga el Hash desde la RAM (lsass) no necesita averiguar la contraseña en texto claro; solo necesita "pasar" el Hash al sistema para ser validado.

**8. B) Descargar TGS (Tickets de Servicio) encriptados y llevarlos a su computadora offline para crackear la contraseña de las Cuentas de Servicio.**
> *Justificación:* El Kerberoasting abusa de una funcionalidad lícita de Kerberos: pedir un boleto de servicio (TGS). Como una porción de ese ticket se encripta con el hash del servicio (SPN), el atacante puede exportar el ticket y, usando gran poder de cómputo (GPUs) en su casa sin alertar a la red, romperlo offline para descubrir contraseñas débiles.

**9. C) Group Policy Objects (GPOs)**
> *Justificación:* Las Políticas de Grupo son el motor de inyección de configuraciones masivo de Windows Server. Un atacante con privilegios administrativos que desee causar destrucción simultánea global sin tener que hackear "uno por uno" a 5,000 equipos, simplemente publicará una GPO maliciosa para que las 5,000 máquinas se infecten solas tras solicitar la actualización cíclica al DC.

**10. B) PowerShell permite ejecutar comandos (como `Invoke-WebRequest`) inyectando el código malicioso directamente en la Memoria RAM, sin escribir nunca un archivo `.exe` en el disco local.**
> *Justificación:* La táctica "Living off the Land" (LotL) con PowerShell omite por completo el disco rígido de la computadora. Al operar directamente en memoria RAM desde un ejecutable legítimo y nativo del S.O. (`powershell.exe`), los antivirus heurísticos tradicionales basados en firmas de archivos quedan completamente ciegos.
