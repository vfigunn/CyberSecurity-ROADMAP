# Lab 03.1 - Exploración y Permisos (Incident Response Simulada)

## 🎯 Objetivo
Aplicar tus conocimientos de navegación en la terminal, búsqueda en la jerarquía FHS y lectura de permisos (UGO / SUID). Vas a actuar como un Analista Defensivo que investiga un sistema Linux recién comprometido.

---

## 📋 Escenario
Un servidor web Linux (`Ubuntu Server`) alojado en la nube comenzó a comportarse de forma extraña. El administrador del sistema (el usuario `bob`) notó que la CPU estaba al 100% (minería de criptomonedas sospechada). Tu trabajo es investigar los rastros que dejó el atacante solo usando comandos de la terminal descritos a continuación (basado en transcripciones).

### 🛠️ Herramientas involucradas:
- `ls -la`, `pwd`, `cat`, `grep`, `find`, `chmod`, `ps aux`.

---

## 📝 Procedimiento (Tu Trabajo)

Lee cuidadosamente las evidencias simuladas (resultados de comandos) y respondé las preguntas a continuación.

### Evidencia A: Analizando al usuario y los procesos
Ingresás por SSH y obtenés tu propia terminal. Decidís revisar qué procesos sospechosos están corriendo.
Ejecutas: `ps aux | grep -i "miner"`
```text
root     14022  98.0  2.1  20480  5012 ?        R    14:02   3:00 /tmp/.hidden_miner
bob      15011   0.0  0.1   6100   800 pts/1    S+   14:15   0:00 grep -i miner
```

1. Analizando el resultado de `ps aux`, ¿qué comando deberías escribir inmediatamente en la terminal para destruir y erradicar el proceso malicioso que está minando criptomonedas sin piedad?
2. El proceso del atacante está ejecutándose desde el archivo `/tmp/.hidden_miner`. Teniendo en cuenta la filosofía de la carpeta `/tmp` en el sistema FHS (Nota 02), ¿por qué es lógico que un atacante elija descargar y correr su malware allí en lugar de en `/bin` o `/etc`?

### Evidencia B: Revisando configuraciones y Escalada de Privilegios
Sospechas que el atacante (que entró usando el usuario del servidor web, `www-data`) logró una forma de escalar privilegios para ejecutar su minero como el usuario Dios (`root`), tal como vimos en la Evidencia A.
Decidís buscar en todo el disco algún archivo (backdoor) al que el atacante le haya asignado el permiso especial **SUID** para ganar privilegios.

Ejecutás: `find / -perm -4000 2>/dev/null`
*(Muestra los resultados clásicos, y uno que destaca fuertemente al final)*:
```text
/usr/bin/passwd
/usr/bin/sudo
/usr/bin/su
/opt/backup_tool
```

Decidís inspeccionar el archivo raro `/opt/backup_tool` ejecutando `ls -l /opt/backup_tool`:
```text
-rwsr-xr-x 1 root root  14500 Ago 30 14:10 /opt/backup_tool
```

3. Explicá con tus palabras por qué los permisos exactos que muestra el `ls -l` de ese archivo confirman que el atacante logró crear un Backdoor con SUID activo.
4. ¿Qué parte específica del comando `find` (`2>/dev/null`) utilizaste en la Evidencia B para evitar que tu terminal se llene de mensajes molestos de "Permiso denegado"? ¿Cómo se llama coloquialmente ese destino?

### Evidencia C: Buscando puertas traseras en SSH
Sabés que un atacante a menudo instala su propia Llave Pública SSH para volver a entrar al servidor cuando quiera (sin necesitar contraseña, persistencia. Ver Nota 16).
Decidís revisar la carpeta secreta del usuario `bob` (que tiene UID 1000).

Ejecutas: `cat /home/bob/.ssh/authorized_keys`
```text
ssh-rsa AAAAB3Nza... (Llave legitima de Bob)
ssh-ed25519 AAAAC3Nza... hacker@kali (Llave insertada hoy a las 14:05)
```

5. En este momento, tu directorio actual (CWD o `pwd`) es `/var/log`. Si, sin cambiar de carpeta, quisieras eliminar/borrar el archivo entero `authorized_keys` que acabás de inspeccionar usando una **ruta absoluta**, ¿qué comando `rm` completo deberías escribir?
6. En Linux, ¿qué característica del nombre del archivo o carpeta (como la carpeta `.ssh`) hace que esté oculto por defecto cuando ejecutas un simple comando `ls`?

---

## 🔍 Resultado Esperado (Autoevaluación)

Revisá tus conclusiones técnicas y comandos con la solución propuesta.

> [!example]- Ver Respuestas del Laboratorio
> 1. `kill -9 14022`. El PID del proceso malicioso (del usuario root) es el 14022. Debemos usar la señal 9 (SIGKILL absoluto) para asegurarnos de que muera inmediatamente sin ejecutar rutinas de escape.
> 2. La carpeta `/tmp` es temporal y por defecto tiene permisos `777` (Lectura, Escritura y Ejecución para cualquier persona). Un atacante con usuario de bajos privilegios (`www-data`) no puede escribir en `/bin` (solo root puede), por lo tanto, `/tmp` es el único lugar donde tiene garantizado poder guardar (escribir) su archivo malicioso.
> 3. En la sección de permisos del usuario/dueño, en lugar de haber una `x` de ejecución, hay una `s` minúscula (`rws`). Esto indica el permiso SUID. Como el dueño del archivo es `root`, significa que cualquier usuario mortal que ejecute ese archivo, lo correrá temporalmente con los privilegios de `root` (Escalada perfecta).
> 4. Redirigimos el Standard Error (File Descriptor 2) hacia `/dev/null`, conocido coloquialmente como el agujero negro de Linux, para que los errores sean descartados en lugar de imprimirse en pantalla.
> 5. El comando sería: `rm /home/bob/.ssh/authorized_keys`. (Es absoluto porque empieza en la raíz `/`).
> 6. Cualquier archivo o carpeta cuyo nombre comience con un punto (`.`) en Linux se considera automáticamente oculto y solo se muestra si usas el modificador `ls -a`.

**➡️ Siguiente nota:** [[22 - Laboratorio 2 - Script de Auditoría Básico]]
