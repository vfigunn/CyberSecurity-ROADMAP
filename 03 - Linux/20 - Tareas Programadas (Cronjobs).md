# 20 - Tareas Programadas (`cron`)

## 🎯 Objetivos
- Entender el propósito del demonio `cron` en sistemas Linux/Unix.
- Aprender a interpretar y editar el archivo `crontab`.
- Conocer la sintaxis de tiempo (los 5 asteriscos) que definen cuándo se ejecuta una tarea.

---

## 🧠 Concepto: El Despertador del Servidor

Ya aprendiste a escribir scripts maravillosos en [[18 - Bash Scripting (Variables y Condicionales)|Bash]]. Por ejemplo, un script llamado `backup_seguro.sh` que comprime y encripta la base de datos de tu empresa.

El problema es que vos no querés conectarte por SSH todos los días a las 3:00 AM para ejecutar manualmente tu script.
Querés que el servidor lo haga solo. Para esto existe el servicio/demonio **`cron`**.

`cron` es un servicio de Linux que corre eternamente en segundo plano (PID de Systemd). Su única función es despertar cada minuto, mirar el reloj, mirar una lista de tareas (llamada `crontab`), y si coincide la hora, ejecutar el comando.

---

## 🛠️ Administrando el Crontab

Cada usuario del sistema puede tener su propia lista de tareas.
- Para **editar** tu lista de tareas, usás el comando:
  ```bash
  $ crontab -e
  ```
- Para **listar** (ver) tus tareas programadas sin editarlas:
  ```bash
  $ crontab -l
  ```

*(Como administrador `root`, podés ver/editar el crontab de otros usuarios usando el flag `-u`: `sudo crontab -u maria -l`).*

---

## ⏳ La Sintaxis del Crontab (Los 5 asteriscos)

Al abrir el crontab, vas a escribir una línea por cada tarea. La sintaxis de cada línea siempre consta de **6 partes**: las primeras 5 indican *CUÁNDO* se ejecutará, y la sexta indica *QUÉ* se ejecutará.

`* * * * * comando_a_ejecutar`

¿Qué representa cada uno de esos 5 asteriscos? (De izquierda a derecha):
1. **Minuto** (0 - 59)
2. **Hora** (0 - 23, formato 24hs)
3. **Día del Mes** (1 - 31)
4. **Mes** (1 - 12)
5. **Día de la Semana** (0 - 7, donde 0 y 7 son Domingo).

**El comodín Asterisco (`*`):**
Un asterisco significa "Cualquier" o "Todos los...".
Si los 5 valores son asteriscos, el comando se ejecutará cada minuto, de cada hora, de cada día.

### Ejemplos vitales
- Ejecutar un backup todos los días a las **3:30 AM**:
  `30 3 * * * /opt/scripts/backup.sh` *(Minuto 30, Hora 3, de cualquier día, de cualquier mes).*

- Ejecutar un chequeo de seguridad los **Lunes (1) a las 9:00 AM**:
  `0 9 * * 1 /opt/scripts/chequeo_seguridad.sh`

- Ejecutar un comando **cada 5 minutos**:
  `*/5 * * * * ping -c 1 8.8.8.8 > /tmp/ping.log` *(La barra `/` significa repetición por intervalos).*

---

## ❓ ¿Por qué importa en Seguridad?

Los Cronjobs son esenciales tanto para el bien como para el mal.

### 🛡️ El Blue Team (Automatización de Seguridad)
El equipo de IT usa crontabs constantemente para:
- Ejecutar Backups automáticos de bases de datos y configuraciones.
- Rotar y comprimir archivos de logs (`logrotate`) para que no llenen el disco duro.
- Ejecutar scripts de auditoría periódicos.

### ☠️ El Red Team (Persistencia)
Como vimos en la nota de [[13 - Servicios y Systemd (systemctl)|Servicios]], la persistencia es clave para un hacker. Si un atacante entra a tu servidor y ejecuta su malware, cuando reinicies el servidor, el malware morirá.
Una técnica avanzadísima (pero muy común) es que el atacante **agregue una línea maliciosa al archivo crontab de un usuario sin privilegios o de `root`**.
- *Técnica:* El atacante escribe: `* * * * * /tmp/.malware.sh` en el crontab.
- *Efecto:* Cada 1 minuto (60 segundos), el sistema operativo obligatoriamente "revive" el malware del atacante. Si el Blue Team encuentra el proceso en memoria (usando `top`) y lo destruye (`kill -9`), el proceso volverá a la vida mágicamente en menos de un minuto gracias a `cron`. 
Para erradicar la infección, el Blue Team debe encontrar y borrar la entrada oculta en el crontab antes de matar el proceso.

---

## 📌 Must Know (Imprescindible)
- Qué es `cron` y para qué sirve.
- El comando `crontab -e` (para editar) y `-l` (para ver).
- Entender la anatomía de los 5 asteriscos: Minuto, Hora, Día de Mes, Mes, Día de Semana.

---

## 🔄 Preguntas de repaso
1. Descubriste una línea en un servidor que dice: `0 12 1 * * /bin/vaciar_registros.sh`. Si hoy es 15 de marzo, ¿cuándo será la próxima vez (fecha y hora aproximada) que se ejecute este comando?
2. ¿Por qué agregar una tarea en el `crontab` es una de las técnicas de "Persistencia" preferidas por los desarrolladores de Malware en entornos Linux?
3. Si un comando (`ping`) produce un Output que no querés que se guarde en ningún lado (ni se te envíe por mail cada vez que corre el cron), ¿qué operador de redirección y hacia qué "archivo/destino especial" deberías usar al final del comando en la línea del crontab? *(Pista: Revisá la Nota 08 de Redirecciones).*

**➡️ Siguiente nota:** [[21 - Laboratorio 1 - Exploración y Permisos]]
