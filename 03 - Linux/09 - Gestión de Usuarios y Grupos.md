# 09 - Gestión de Usuarios y Grupos

## 🎯 Objetivos
- Entender cómo Linux maneja las identidades de los usuarios.
- Conocer la diferencia entre un usuario estándar y el usuario `root` (Superusuario).
- Comprender el propósito del archivo `/etc/passwd`.
- Saber cómo utilizar el comando `sudo`.

---

## 🧠 Concepto: Sistema Multi-Usuario

Linux nació de Unix, un sistema diseñado en los 70 para grandes computadoras (Mainframes) a las que se conectaban docenas de personas al mismo tiempo usando terminales tontas.
Por diseño, **Linux es estrictamente multi-usuario**. Asume que la máquina no es tuya, sino que la compartes. Por lo tanto, el control de quién es quién y qué puede hacer cada uno es la base de toda su arquitectura de seguridad.

Para Linux, los usuarios y los grupos no son nombres (como "Juan"), sino números.
- **UID (User ID):** El número identificador del usuario.
- **GID (Group ID):** El número identificador del grupo al que pertenece.

---

## 👑 El usuario Dios: `root`

En Windows, existe la cuenta de "Administrador". En Linux, existe el **Superusuario**, siempre llamado **`root`**. Su UID es siempre **`0`**.

El usuario `root` tiene poder absoluto y divino sobre el sistema. Las reglas de seguridad y permisos de los archivos simplemente no se aplican a `root`. 
- Si un archivo está configurado para que "Nadie pueda leerlo", `root` puede leerlo.
- Si un programa crítico del sistema no se debe borrar, `root` puede borrarlo (y destruir el sistema).

**Seguridad:** En las prácticas modernas de ciberseguridad, **jamás** se inicia sesión directamente como el usuario `root` (por ejemplo, a través de [[16 - SSH y Transferencia Segura (scp)|SSH]]). Es tan peligroso que, si cometes un error de tipeo, podrías borrar un servidor en producción. Además, si todos los administradores usan la misma cuenta `root`, no hay forma de auditar *quién* de los tres cometió el error (Falta de No Repudio).

### La solución: `sudo` (Superuser DO)
En lugar de loguearte como el Dios `root`, inicias sesión con tu usuario mortal (ej. `juan`). Si necesitás hacer una tarea administrativa, usás el comando **`sudo`** antes de tu orden.
```bash
$ apt install nmap       # Esto falla, porque juan no tiene permiso para instalar cosas.
$ sudo apt install nmap  # Esto funciona. 
```
`sudo` te "presta" los poderes de `root` por un breve instante para ejecutar ese comando específico (te pedirá tu propia contraseña como confirmación). Todo queda registrado en los logs de seguridad, permitiendo saber que fue *Juan* quien usó los poderes de `root` a las 15:00 hs.

*(También existe el comando `su` (Switch User) que te permite cambiar de un usuario a otro en la terminal).*

---

## 🗃️ El Archivo `/etc/passwd`

Este es uno de los archivos más famosos (y más leídos) en la historia del Hacking y la administración de sistemas.
A pesar de su nombre histórico, hoy en día **no contiene contraseñas**, pero contiene el listado maestro de absolutamente todas las cuentas de usuario de la máquina (humanos y programas). 

Si haces un `cat /etc/passwd`, verás muchas líneas parecidas a esta:
`root:x:0:0:root:/root:/bin/bash`
`juan:x:1000:1000:Juan Perez,,,:/home/juan:/bin/bash`

¿Qué significan esos campos separados por dos puntos (`:`)?
1. **Nombre de usuario:** `juan`.
2. **Contraseña (`x`):** La `x` significa que la contraseña encriptada (hash) no está acá por seguridad. Está escondida en el archivo `/etc/shadow`, que solo `root` puede leer.
3. **UID:** `1000` (En Linux, los humanos suelen empezar a partir del UID 1000. Los menores a 1000 son usuarios falsos usados por programas y servicios).
4. **GID:** `1000`.
5. **Comentario/Descripción:** `Juan Perez,,,`.
6. **Carpeta Home:** `/home/juan`.
7. **Shell por defecto:** `/bin/bash` (El intérprete de comandos que se le abre cuando inicia sesión).

> *Truco de Red Team:* Si lográs leer este archivo en un servidor web vulnerado, inmediatamente obtenés una lista de nombres de usuario válidos, que luego podés usar para ataques de fuerza bruta contra el sistema de correos o el SSH de la empresa.

---

## 👨‍👩‍👧‍👦 Grupos

Para facilitar la administración de los permisos, Linux agrupa usuarios.
Si tenés 50 desarrolladores, no querés darle permisos a la carpeta de código a cada uno de los 50. Simplemente creás un Grupo llamado `devs`, metés a los 50 humanos ahí adentro, y le das permiso a la carpeta al grupo `devs`.

- Podés ver a qué grupos pertenece tu usuario escribiendo el comando `id` o `groups` en la terminal.

---

## 📌 Must Know (Imprescindible)
- La diferencia de poder entre un usuario estándar y el UID 0 (`root`).
- Para qué sirve el comando `sudo`.
- Conocer la ubicación y la información que provee el archivo `/etc/passwd`.

---

## 🔄 Preguntas de repaso
1. Si ejecutás un comando y la terminal te dice "Permission Denied" (Permiso Denegado) para modificar un archivo del sistema, ¿qué palabra clave podrías intentar poner al principio del comando para ejecutarlo con privilegios elevados?
2. ¿Por qué el archivo `/etc/passwd` es útil para un atacante durante la fase de Reconocimiento si hoy en día ya no contiene las contraseñas reales?
3. En el archivo `/etc/passwd`, ves una línea que termina en `/bin/false` en lugar de terminar en `/bin/bash` (el shell). Basado en tu sentido común, ¿qué le pasaría a ese usuario si intenta iniciar sesión por línea de comandos?

**➡️ Siguiente nota:** [[10 - Permisos de Archivos (chmod, chown)]]
