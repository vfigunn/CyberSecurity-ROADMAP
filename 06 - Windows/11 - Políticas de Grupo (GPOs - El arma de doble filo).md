# 11 - Políticas de Grupo (GPOs - El arma de doble filo)

## 🎯 Objetivos
- Entender el sistema de administración masiva de Microsoft (GPOs).
- Conocer por qué las GPO son la línea de defensa final (Endurecimiento) para el Blue Team.
- Identificar cómo los atacantes (Ransomware) arman las GPOs en tu contra.

---

## 🧠 Concepto: Controlando el Enjambre

En las notas anteriores explicamos que, si tu empresa tiene 10.000 computadoras, Active Directory centraliza la base de datos de Usuarios para que puedan iniciar sesión en cualquier PC.

Pero, ¿qué pasa con el control físico y lógico de esas 10.000 máquinas? 
- Querés bloquear el Panel de Control en las computadoras del sector de Alumnos de un colegio.
- Querés instalar Chrome y desplegarlo en las 10.000 máquinas automáticamente.
- Querés prohibir que los usuarios usen pendrives USB (Para evitar que inserten malware).
- Querés cambiar el fondo de pantalla corporativo.

No vas a ir máquina por máquina haciendo esos ajustes de registro. Tampoco vas a crear un Script de Python y correrlo en cada PC.
Acá entra la genialidad administrativa de Windows: **Group Policy Objects (GPOs)** o Políticas de Grupo.

---

## ⚙️ ¿Cómo funciona una GPO?

Una GPO es un "Paquete de reglas" que configurás cómodamente desde el Controlador de Dominio y se las inyectás de forma remota y forzada a miles de computadoras o usuarios al mismo tiempo.

**El Flujo (La aplicación):**
1. En tu servidor (DC), abrís el editor de políticas y creás una GPO llamada `Bloquear-USB`. Adentro de la GPO, modificás la regla pertinente.
2. "Enlazás" (Vinculás) esa GPO a la [[06 - Estructura Lógica (Dominios, Bosques y OUs)|Unidad Organizativa (OU) de "Alumnos"]].
3. Todas las computadoras encendidas en la red corporativa preguntan cíclicamente al Controlador de Dominio (cada 90-120 minutos, en un proceso invisible llamado `gpupdate`): *"¿Hay alguna regla nueva para mí?"*.
4. Las computadoras de los alumnos descubren la nueva política, la descargan, **hacen las modificaciones profundas en sus propios Registros Locales**, e inmediatamente el puerto USB deja de funcionar. Todo sin que te hayas levantado de la silla.

### Blue Team: Endurecimiento (Hardening)
Las GPOs son la principal herramienta de ciberseguridad corporativa. Con un clic podés:
- Forzar la encriptación BitLocker en todas las laptops de la empresa (Para que no extraigan los discos).
- Bloquear la ejecución del temido comando de PowerShell desde usuarios estándar.
- Establecer políticas de contraseñas de alta seguridad (Bloquear cuentas luego de 3 intentos fallidos de login).

---

## 💥 Red Team: Armando la GPO (GPO Abuse)

Al igual que muchas herramientas administrativas, las GPOs se crearon para ayudar, pero son el arma de destrucción masiva definitiva de las Amenazas Persistentes (APTs) y del Ransomware.

Si un atacante hackeó a una empresa y logró robar privilegios máximos (Domain Admin) o logró conseguir permisos de edición específicos sobre una Política de Grupo importante...

1. El Atacante crea una nueva Política de Grupo llamada `Actualizacion-Legitima`.
2. Adentro de la GPO, el atacante programa una "Tarea de Inicio" (Startup Script) que ejecuta silenciosamente un archivo `.exe` malicioso alojado en su propio servidor.
3. El atacante aplica esta GPO a la Organización entera (A nivel del **Dominio**).
4. El atacante simplemente se sienta a esperar y tomar un café. 
5. Durante los próximos 90 minutos (y al día siguiente, cuando las 10.000 PCs se prendan a las 9 AM), todas las máquinas de la compañía le consultarán al Controlador de Dominio, descargarán la política corrupta del hacker, y **ellas mismas ejecutarán el Malware (Ransomware) de forma automática con privilegios máximos (SYSTEM)**.

El 95% de los incidentes mundiales donde multinacionales se despertaron un lunes con todas sus computadoras encriptadas al mismo tiempo (apagando trenes, hospitales, y petroleras) se originaron porque los hackers abusaron de las GPOs de Active Directory para enviar el misil.

---

## 📌 Must Know (Imprescindible)
- Qué es una **GPO (Group Policy Object)**: El método nativo de Microsoft para administrar y forzar reglas de seguridad sobre enjambres de computadoras desde un único punto.
- Comprender su función dual: Es la herramienta fundamental de **Hardening (Defensa)** de la empresa, pero es el vector letal (Distribución masiva) utilizado por los atacantes en la etapa final de un **Ransomware de Dominio**.
- Recordar que las GPOs se aplican sobre las carpetas virtuales vistas en notas anteriores (Unidades Organizativas - OUs).

---

## 🔄 Preguntas de repaso
1. Estás trabajando en una corporación. Se requiere deshabilitar el acceso al Panel de Control a todos los empleados del "Piso 5", pero los Gerentes (que también están en el Piso 5) deben conservar acceso completo. Asumiendo que todos los Empleados están dentro de la Unidad Organizativa (OU) `Empleados_P5`, y los Gerentes están en la OU `Gerentes_P5`. ¿Cómo utilizarías las Políticas de Grupo (GPO) para resolver esta problemática sin instalar nada extra?
2. Desde la perspectiva del atacante, tras ganar acceso de "Domain Admin" en una multinacional de 50,000 máquinas distribuidas en 3 países, ¿por qué el atacante no utilizaría una herramienta o script personal para intentar conectarse directamente a cada una de las 50,000 IPs una por una para inyectar su Ransomware, y en su lugar crearía y publicaría una GPO maliciosa?
3. Un atacante crea una GPO y la programa para que inyecte una Tarea Programada (Task Scheduler) maliciosa en todas las computadoras. Sin embargo, no recibe ninguna conexión por parte de las víctimas durante más de 1 hora, a pesar de que las máquinas están encendidas. Teniendo en cuenta la arquitectura de sondeo (polling) de Windows, explicá por qué las máquinas demoran en aplicar las nuevas reglas, y qué comando de consola podría correr el administrador local para forzar esa actualización inmediatamente (`gp...`).

**➡️ Siguiente nota:** [[12 - PowerShell (Administración y Amenazas)]]
