# 06 - La Caída del DC (DCSync y NTDS.dit)

## 🎯 Objetivos
- Entender el evento final "Game Over" (La caída total de la red).
- Conocer dónde vive la joya más valiosa de una empresa: el archivo `NTDS.dit`.
- Aprender la táctica de extracción pasiva invisible: DCSync.

---

## 🧠 Concepto: Alcanzando el Trono

Durante todas las notas anteriores (BloodHound, Pass-The-Hash, Kerberoasting), el atacante estuvo escalando la montaña pacientemente (Moviéndose Lateralmente).
Finalmente, el atacante (Red Team) logra su objetivo. Mediante el robo de un ticket y el salto entre 3 computadoras, el atacante **logra robar y autenticarse con la cuenta del "Domain Admin" (El Dios de la red)**.

El Red Team levanta los brazos. Oficialmente la empresa está comprometida. 
Pero el trabajo del atacante no termina ahí. ¿Qué hace el Hacker cuando obtiene el poder absoluto? **Extrae el tesoro para irse.**

---

## 🗄️ El Santo Grial: El Archivo NTDS.dit

Cada vez que el equipo de IT crea un usuario nuevo en la empresa, cada vez que le cambian la contraseña al CEO, todo ese texto se guarda en algún lugar físico de un disco duro.

Ese lugar es un pequeño y único archivo de Base de Datos relacional, enterrado en lo más profundo del disco `C:\` del **Controlador de Dominio (DC)**.
El archivo se llama **`NTDS.dit`** (Ubicado usualmente en `C:\Windows\NTDS\ntds.dit`).

Este archivo de 20 Megabytes es el objetivo económico y táctico número uno en Ciberseguridad Ofensiva Mundial. Contiene el nombre y los **Hashes NTLM (Las contraseñas cifradas) de ABSOLUTAMENTE TODOS los miembros, directivos, sistemas operativos, servidores y cuentas históricas de toda la multinacional global**. Quien tiene el `NTDS.dit`, posee la identidad absoluta de la corporación.

*(Nota: Windows no es tonto. Este archivo está bloqueado a nivel Kernel. No podés simplemente copiarlo y pegarlo ni siendo el Dios supremo (System) porque Windows no lo permite mientras la PC esté encendida. Para copiarlo, los atacantes requieren usar tácticas avanzadas de clonación "Volume Shadow Copies (VSS)" o la táctica que explicaremos a continuación).*

---

## 🔄 El Robo de Coronas: El Ataque DCSync

Para robar las contraseñas sin lidiar con copiar un archivo bloqueado en el disco local, los atacantes modernos utilizan la red, abusando de un privilegio funcional propio del **Active Directory**.

**La Función Legítima (Sincronización DC a DC):**
Las corporaciones tienen más de 1 Controlador de Dominio (Tienen un DC en Nueva York, y un Backup DC en Madrid). 
Cuando un empleado cambia su clave en Nueva York, Nueva York tiene que avisarle a Madrid: *"Ey, actualizaron una clave, tomá todos los datos nuevos (Sincronización)"*.
El protocolo nativo de Windows (RPC) permite que un DC "Le exija la BBDD a otro DC".

**La Falsificación Hacker (El Ataque DCSync):**
1. El atacante, que ya consiguió ser "Domain Admin", no ataca directamente al disco duro.
2. Saca su clásica herramienta maliciosa `Mimikatz` (o `Impacket` desde Kali Linux).
3. Utilizando los poderes robados del Domain Admin, la herramienta del hacker falsifica su identidad y **simula ser un "Tercer Controlador de Dominio recién instalado"**.
4. Le envía un mensaje al DC de la empresa desde su computadora atacante: *"¡Hola! Soy el nuevo Controlador de Dominio Backup. Necesito Sincronizar mis bases. ¡Dame una copia entera, por red, del archivo NTDS.dit con absolutamente todos los Hashes!"*.
5. El DC es engañado, lee los permisos (y nota que tenés perfil de Domain Admin, que sí tiene autoridad para pedir esto `DS-Replication-Get-Changes`), y **vomita/transfiere por cable de red la totalidad de los Hashes de la empresa hacia la computadora personal del hacker en su casa**.

¡Extracción completa y perfecta sin haber copiado ni tocado el archivo protegido localmente!

---

## 📌 Must Know (Imprescindible)
- **`NTDS.dit`:** La Base de Datos principal del Active Directory alojada en el Domain Controller. Contiene la información y los Hashes criptográficos de absolutamente cada cuenta del dominio entero de la corporación.
- **DCSync:** Un ataque post-explotación (requiere altos privilegios previos como *Domain Admin* u otros derechos de Replicación). Permite a un atacante en la red engañar al DC forzándolo a *"Sincronizar"* los datos, vomitando masivamente la Base de Datos NTDS por la red hacia la terminal del hacker (evitando la necesidad de robar el archivo bloqueado físico).

---

## 🔄 Preguntas de repaso
1. Al realizar una auditoría (Pentesting Avanzado), y mediante movimientos laterales previos a través de Pivoting, lográs volverte dueño (conseguir el token y contraseña) de una cuenta de usuario perteneciente al grupo "Domain Admins". Tu objetivo como líder del Red Team es extraer y exportar de forma veloz y silenciosa todos los Hashes históricos (NTLM) de la Base de Datos central del Directorio Activo, incluyendo la cuenta del CEO y el Directorio de Tecnología. ¿Cuál es la ruta absoluta original de Windows (la carpeta C:\...) en donde se aloja el archivo físico que los contiene?
2. Al intentar acceder, abrir o comprimir en un `.zip` este preciado archivo físico para robártelo hacia el disco rígido de tu computadora, te enfrentás repetidamente al error *"Acceso Denegado / The file is in use by another process"*. Sabiendo la interacción del sistema operativo local (Kernel/LSA) con los archivos de base de datos nucleares vivos, ¿Por qué Windows bloquea rígidamente su lectura tradicional de copiar/pegar y por qué esto forzó históricamente el uso del servicio `VSS` (Volume Shadow Copies) para clonarlo temporalmente?
3. Para saltarse los monitoreos locales de lectura física de discos que implementan los modernos Antivirus/EDR (como el SentinelOne del Blue Team). Decidís ejecutar un comando de Inyección e Impersonación utilizando la suite de `Mimikatz` / `Impacket` para solicitarle una réplica de red remota a través del propio protocolo legítimo y nativo del Active Directory (DRSR). ¿Cómo se le denomina oficialmente a este ataque devastador (donde tu PC atacante finge ser otro DC) y qué permiso fundamental abusa en el Dominio?

**➡️ Siguiente nota:** [[07 - Persistencia Absoluta (Golden y Silver Tickets)]]
