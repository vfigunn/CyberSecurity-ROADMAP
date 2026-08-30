# 12 - Gestión de Procesos (`ps`, `top`, `kill`)

## 🎯 Objetivos
- Entender qué es un Proceso y un PID.
- Aprender a listar los procesos en ejecución (El equivalente al Administrador de Tareas).
- Saber cómo destruir procesos congelados o maliciosos.

---

## 🧠 Concepto: Procesos y el PID

Un programa (como Firefox o un malware) guardado en el disco duro no hace nada, es solo un archivo inerte.
Cuando lo ejecutás (doble clic o escribiendo su nombre en la terminal), el Kernel lo carga en la Memoria RAM y le da vida. En ese momento deja de ser un "programa" y se convierte en un **Proceso**.

Para no confundirse, Linux le asigna a cada proceso vivo un número de identificación único llamado **PID (Process ID)**. 

### Procesos en segundo plano (Background)
A veces, iniciás un proceso en la terminal que tarda mucho tiempo (ej. un escaneo con Nmap de 2 horas). Si lo dejás corriendo, tu terminal quedará bloqueada y no podrás escribir más comandos hasta que termine.
- Si le agregás el símbolo **`&`** al final del comando (ej. `nmap 10.0.0.0/8 &`), el proceso se iniciará en *segundo plano* (Background). Se ejecutará invisiblemente, y la terminal te devolverá el control instantáneamente para que sigas trabajando.

---

## 🛠️ Herramientas de Gestión de Procesos

¿Cómo sabemos qué está corriendo en la máquina en este momento?

### 1. `ps` (Process Status)
Toma una "fotografía" instantánea de los procesos actuales. Si lo ejecutás solo, te mostrará únicamente los procesos de la pequeña terminal que tenés abierta (casi nada).
Para ver TODO lo que ocurre en el sistema (todos los usuarios, servicios, etc.), usamos el flag histórico:
```bash
$ ps aux
```
- **`a`**: Muestra procesos de todos los usuarios.
- **`u`**: Muestra el nombre del usuario dueño del proceso, el uso de CPU y de RAM.
- **`x`**: Muestra incluso los procesos que corren "sin estar atados a una terminal" (como los demonios/servicios del sistema).

> **El combo mágico del Threat Hunting:** Si querés saber si el proceso de "apache" (Servidor Web) está corriendo, hacés un *Pipe* hacia *Grep* (ver [[08 - Redirecciones y Pipes|Nota 08]]):
> `$ ps aux | grep apache`

### 2. `top` / `htop`
Mientras que `ps` es una foto estática, **`top`** es un video en vivo. Te muestra una tabla que se actualiza cada segundo, ordenada por el proceso que está consumiendo más CPU o RAM.
*(Para salir de la vista de `top`, presionás la tecla `q` de Quit).*

- `htop` es la versión colorida, moderna y más fácil de leer. Viene instalada por defecto en Kali Linux.

---

## 🔪 Matando Procesos (`kill`)

Si un proceso se congela, o si descubrís un malware de Criptominería robándote el 100% de la CPU en `top`, tenés que destruirlo.
A pesar del nombre agresivo, el comando `kill` en realidad sirve para enviar "Señales" a los procesos. Hay dos señales que tenés que saber:

1. **Señal 15 (SIGTERM - Terminate):** Es la forma educada.
   Le dice al proceso: *"Por favor, cerrate y guardá tus archivos"*.
   Es el comportamiento por defecto de `kill`. Necesitás saber el **PID** del proceso (lo sacás con `ps aux`).
   ```bash
   $ kill 4095
   ```

2. **Señal 9 (SIGKILL - Kill absoluto):** Es la forma violenta.
   A veces un proceso está tan congelado o es tan malicioso que ignora la Señal 15. La Señal 9 no le habla al proceso; le habla directamente al Kernel de Linux y le ordena *"Sacá a este proceso de la memoria RAM inmediatamente sin avisarle"*.
   ```bash
   $ kill -9 4095
   ```

---

## ❓ ¿Por qué importa en Seguridad?

La supervisión de procesos es el día a día de la Respuesta a Incidentes (IR).
- Un atacante sube un script (`backdoor.py`), lo ejecuta en segundo plano (`&`), y borra el archivo del disco duro para esconderse.
- Un Blue Team inexperto buscará el archivo en el disco duro con `find` y no hallará nada. Un Blue Team profesional hará un `ps aux` y verá que en la RAM el proceso de Python sigue vivo y conectado hacia afuera, y lo neutralizará con un `kill -9`.

---

## 📌 Must Know (Imprescindible)
- Qué es un PID (Identificador de Proceso).
- Recordar de memoria el comando `ps aux` para ver todo.
- La diferencia entre el comando `kill` educado y el violento (`kill -9`).

---

## 🔄 Preguntas de repaso
1. Si ejecutás un comando y necesitás recuperar inmediatamente el control de tu terminal para escribir otro comando, sin cerrar el primero, ¿qué símbolo debés añadir al final de tu orden inicial?
2. Un empleado te dice que su servidor Linux está extremadamente lento (posiblemente un DDoS de Capa 7 o un proceso corrupto). ¿Qué comando usarías para ver en tiempo real, de manera interactiva, qué programa se está consumiendo el 100% de la memoria RAM?
3. Encontraste un proceso llamado `minero_oculto` con el PID 8832 mediante `ps aux`. Intentaste enviar un comando `kill 8832`, pero el proceso sigue activo. ¿Qué comando alternativo intentarías a continuación?

**➡️ Siguiente nota:** [[13 - Servicios y Systemd (systemctl)]]
