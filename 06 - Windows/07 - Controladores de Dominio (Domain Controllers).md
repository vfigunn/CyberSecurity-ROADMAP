# 07 - Controladores de Dominio (Domain Controllers - DC)

## 🎯 Objetivos
- Entender qué es un DC físicamente y por qué es el servidor más crítico de cualquier empresa.
- Conocer la diferencia con los servidores miembro.
- Aprender cómo se mantiene vivo el bosque cuando hay más de un Controlador de Dominio (Replicación).

---

## 🧠 Concepto: El Rey del Castillo

En la introducción, llamamos al Controlador de Dominio "El Rey". 
Físicamente, un Controlador de Dominio (DC) es un servidor Windows Server (2016, 2019, 2022) que tiene instalado y encendido el "Rol" (Software) de **Active Directory Domain Services (AD DS)**.

Desde el segundo en que le instalás ese software, el servidor deja de ser una máquina normal y se convierte en la computadora más importante, delicada y protegida de toda la empresa.

### ¿Qué hace el DC?
- Almacena el archivo **`NTDS.dit`**. Esta es literalmente "La Base de Datos". Es el archivo físico que contiene los usuarios, los grupos y los **hashes de las contraseñas de todos los empleados de la empresa**. (Si el atacante roba este archivo, hackeó la compañía entera).
- Procesan los *Logins* de toda la red. Si el DC se apaga o se rompe, absolutamente nadie en la empresa puede iniciar sesión, ni leer su correo, ni imprimir (porque no hay quien valide las credenciales).
- Controla el tiempo de toda la empresa (Sincronización de relojes NTP), vital para la seguridad de la criptografía y el protocolo Kerberos.

---

## 🖥️ Servidores Miembro vs DCs

En una red empresarial típica de Active Directory, no todos los servidores son DCs.
Supongamos que la empresa tiene 50 Servidores:

- **2 Servidores** son Controladores de Dominio (DCs). Solo se dedican a la seguridad y a validar contraseñas. Nadie los usa para trabajar, y no tienen ni Word ni Excel instalados.
- Los **48 Servidores restantes** se llaman **Servidores Miembro (Member Servers)**. Son servidores normales que se unieron al Dominio. 
  - 10 de ellos son Servidores de Bases de Datos (SQL).
  - 10 son Servidores Web (IIS).
  - 28 son Servidores de Archivos (Carpetas compartidas).

> **Ataque de Escalada (Ruta Crítica):** 
> 1. Un atacante hace Phishing a un empleado e infecta su PC. 
> 2. Desde la PC, el atacante roba la contraseña de un Administrador de Base de Datos. 
> 3. El atacante salta lateralmente hacia el Servidor Miembro (SQL) usando esa contraseña.
> 4. Estando en el Servidor Miembro, descubre una mala configuración y escala privilegios hasta tomar control absoluto de ese servidor de base de datos.
> 5. Finalmente, usa esa posición de poder para atacar a la joya de la corona: **El Controlador de Dominio**. Si lo logra, los 50 servidores (y la empresa) le pertenecen.

---

## 🔄 Replicación (Alta Disponibilidad)

Si el Controlador de Dominio es el corazón de la empresa y se corta la luz, ¿la empresa quiebra?
No. Por diseño, **Active Directory NUNCA opera con un solo Controlador de Dominio.**

Siempre hay como mínimo dos (o cien, en empresas multinacionales). Todos son "reyes" y todos tienen la copia exacta de la base de datos `NTDS.dit`.
Microsoft implementó un sistema llamado **Multi-Master Replication** (Replicación Multi-Maestro):

- Si el usuario de Recursos Humanos cambia su contraseña en París, el Controlador de Dominio de París actualiza la base de datos.
- Automáticamente, a través de la red, el Controlador de Dominio de París le avisa al Controlador de Dominio de Nueva York: *"Ey, hubo un cambio, anotá la nueva contraseña"*.
- Todos los Controladores se mantienen sincronizados.
- Si el Controlador de París explota o se apaga, las computadoras de los usuarios de París, al ver que el Rey no responde, automáticamente redireccionarán sus preguntas de contraseña al Controlador de Dominio de Nueva York a través de Internet (gracias al DNS). La empresa sigue funcionando.

---

## 📌 Must Know (Imprescindible)
- Un **Domain Controller (DC)** es el servidor que hospeda la base de datos de identidades.
- El archivo más sagrado de toda la empresa se llama **`NTDS.dit`** (Allí viven los Hashes de contraseñas de toda la corporación).
- Para evitar que la caída de un servidor paralice a la empresa, AD usa **Múltiples DCs con Replicación** (copias exactas y sincronizadas).

---

## 🔄 Preguntas de repaso
1. Una corporación bancaria internacional sufre una severa caída en su data center de Londres, lo que provoca que su único Controlador de Dominio físico en esa región se apague por completo. Sin embargo, los empleados de Londres aún pueden iniciar sesión en sus computadoras y la red continúa operando casi sin interrupciones. Sabiendo que la empresa posee data centers en otras partes del mundo, ¿qué concepto de la arquitectura de Active Directory permitió salvar la continuidad del negocio?
2. Un grupo de Ransomware (Red Team) irrumpe en la red corporativa. Tras 3 días de Movimiento Lateral, logran comprometer y extraer silenciosamente un archivo llamado `NTDS.dit` alojado en la ruta `C:\Windows\NTDS\`. ¿Qué tipo de servidor acaban de hackear y por qué el analista forense declararía una violación total de confidencialidad de identidades para toda la empresa?
3. En una empresa pequeña de 20 computadoras, el administrador decide utilizar el único Controlador de Dominio que poseen para instalar, además de AD, un servidor web Apache y un servidor de archivos públicos donde los empleados comparten PDFs pesados de proyectos. ¿Por qué esto es considerado una pésima práctica (Anti-patrón) de arquitectura y un enorme riesgo de seguridad?

**➡️ Siguiente nota:** [[08 - Autenticación I (NTLM y el problema del Hash)]]
