# 13 - Servicios y Systemd (`systemctl`)

## 🎯 Objetivos
- Entender qué es un Servicio (o Demonio/Daemon).
- Conocer la arquitectura moderna de Systemd.
- Aprender a iniciar, detener y revisar el estado de los servicios usando `systemctl`.

---

## 🧠 Concepto: Demonios (Daemons)

En la [[12 - Gestión de Procesos (ps, top, kill)|nota anterior]] hablamos de procesos que corren en segundo plano (Background). 
Los **Servicios** (históricamente llamados *Daemons* o Demonios en el mundo Unix) son un tipo muy especial de proceso en segundo plano.

- **Definición:** Un Daemon es un programa que se ejecuta de forma continua, invisible y en segundo plano, esperando que ocurra un evento para actuar.
- *Ejemplo:* El servidor web (Apache o Nginx) es un Daemon. Está silenciosamente corriendo en la RAM, sin que nadie lo toque, esperando a que llegue una conexión HTTP al Puerto 80 para despachar la página web. Si nadie se conecta, él sigue ahí esperando 24/7.
- *(Por convención, el nombre de los Daemons suele terminar con la letra 'd', como `sshd`, `httpd`, `systemd`).*

### Systemd (El jefe de todos los demonios)
Cuando encendés un servidor Linux, el Kernel es lo primero que carga. Inmediatamente después, el Kernel carga el proceso inicial. 
Hoy en día, casi todas las distribuciones usan **Systemd** como ese proceso Padre inicial (Tiene el **PID 1** absoluto). El trabajo de Systemd es arrancar, organizar y supervisar a todos los demás servicios del sistema.

---

## 🛠️ Administrando Servicios (`systemctl`)

Para interactuar con Systemd y controlar los servicios de tu servidor, utilizamos el comando **`systemctl`** (System Control).
(Nota: Muchos de estos comandos requieren usar `sudo` porque afectan a todo el servidor).

Supongamos que estamos administrando el servicio de acceso remoto seguro: `ssh` (o `sshd`).

### 1. Ver el estado de un servicio
Este es el comando más común para verificar si un servidor está caído.
```bash
$ systemctl status ssh
```
- Te dirá si está **`active (running)`** en verde, o **`inactive (dead)`**. También te mostrará las últimas 10 líneas de logs de ese servicio específico (ideal para ver rápidamente por qué un servicio falló al arrancar).

### 2. Detener e Iniciar en el momento
Si querés apagar temporalmente el servicio web para mantenimiento, o prenderlo.
```bash
$ sudo systemctl stop ssh
$ sudo systemctl start ssh
```
- *Truco (Reiniciar):* Si cambiaste un archivo de configuración en `/etc/ssh/`, los cambios no surten efecto solos. Tenés que reiniciar el servicio para que lea la configuración nueva: `sudo systemctl restart ssh`.

### 3. Habilitar y Deshabilitar (Arranque Automático)
Este concepto es vital. Iniciar un servicio con `start` solo lo prende **ahora**. Si reiniciás el servidor entero (reboot), el servicio arrancará apagado.
Para decirle a Systemd que querés que un servicio inicie automáticamente cada vez que se enciende la máquina (Autostart), tenés que **habilitarlo (enable)**.
```bash
$ sudo systemctl enable ssh
$ sudo systemctl disable ssh
```

---

## ❓ ¿Por qué importa en Seguridad?

Los atacantes aman los servicios, especialmente para **Persistencia**.

Si un atacante hackea un servidor, su mayor miedo es que el administrador reinicie la máquina (porque al reiniciar, la RAM se vacía y el malware desaparece).
Para lograr "Persistencia", el atacante creará su propio archivo de servicio Systemd malicioso (ej. un Reverse Shell) y lo habilitará con `systemctl enable backd.service`.
De esta forma, cuando el administrador reinicie la máquina para actualizarla, **el propio sistema operativo iniciará automáticamente el malware del atacante** (PID 1 ordenando su ejecución).

El Blue Team debe cazar (Threat Hunting) constantemente los servicios habilitados que parezcan sospechosos en sus servidores críticos.

---

## 📌 Must Know (Imprescindible)
- Qué es un Daemon (Servicio).
- El rol de Systemd como el "Jefe" (PID 1).
- La diferencia fundamental entre `start` (Iniciar ahora) y `enable` (Iniciar al arrancar el sistema siempre).
- Cómo ver si un servicio está funcionando (`systemctl status`).

---

## 🔄 Preguntas de repaso
1. Instalaste un nuevo servidor de bases de datos. Lo arrancaste usando `sudo systemctl start mysql` y funciona perfectamente. Sin embargo, ese fin de semana la empresa sufre un corte de luz, los servidores se apagan y al volver la electricidad se encienden solos. El lunes, te llaman diciendo que la base de datos está apagada. ¿Qué paso te saltaste en la configuración?
2. Si sospechás que el servidor de correo electrónico (postfix) intentó arrancar pero falló por un error de configuración, ¿qué comando usarías que te muestra tanto si está apagado, como las últimas líneas de error que emitió al intentarlo?
3. En el contexto de ciberseguridad ofensiva, ¿qué significa el término "Persistencia" y por qué los atacantes interactúan con `systemctl` para lograrla?

**➡️ Siguiente nota:** [[14 - Gestión de Paquetes (apt, dpkg)]]
