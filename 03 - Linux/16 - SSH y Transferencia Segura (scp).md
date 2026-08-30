# 16 - SSH y Transferencia de Archivos (SCP)

## 🎯 Objetivos
- Comprender el uso práctico de SSH para control remoto en el día a día.
- Conocer la diferencia entre autenticación por contraseña y por llave pública.
- Aprender a transferir archivos entre máquinas Linux a través de la red de forma encriptada usando `scp`.

---

## 🧠 Concepto: La terminal remota (SSH)

En la nota de protocolos [[22 - Protocolos de Correo y Transferencia|del módulo anterior]] explicamos que **SSH (Secure Shell - Puerto 22)** es el protocolo que usamos para administrar computadoras sin estar sentados físicamente frente a ellas, con todo el texto viajando de forma completamente cifrada por la red.

En el mundo Linux, SSH no es solo un concepto, es el aire que respira cualquier administrador de sistemas (SysAdmin), Analista Cloud o Atacante. Es una herramienta nativa; no necesitás descargar nada raro para usarla.

---

## 🛠️ Uso de SSH (Cliente)

Para conectarte desde tu máquina local (tu laptop) al Servidor remoto en la nube (ej. IP `192.168.1.100`), abrís tu terminal y usás el siguiente formato:
`ssh [usuario]@[ip_del_servidor]`

```bash
$ ssh juan@192.168.1.100
```
La terminal te pedirá la contraseña del usuario `juan` de esa máquina. Al introducirla, "te teletransportás". Todo comando (`ls`, `pwd`, `rm`) que escribas a partir de ahora, ocurrirá en el procesador y disco duro del servidor en la nube.
Para volver a tu máquina, simplemente escribís `exit`.

### Autenticación por Llaves Públicas (Public Key Auth)
Usar contraseñas es peligroso (pueden ser vulnerables a ataques de Fuerza Bruta). El estándar de oro en ciberseguridad es **nunca** usar contraseñas para SSH, sino usar un archivo criptográfico (Llave). 
1. Creás un par de llaves en tu laptop (Una pública para compartir, una privada súper secreta).
2. Pegás la llave pública dentro del servidor en un archivo llamado `~/.ssh/authorized_keys`.
3. Cuando hacés la conexión, el servidor y tu laptop intercambian matemáticas complejas. Si las llaves encajan, entrás sin escribir ninguna contraseña. 
*(Si sos un atacante y lográs robar la Llave Privada de un SysAdmin, tenés acceso garantizado e indetectable a toda la infraestructura sin necesitar saber su clave).*

---

## 📦 Transferencia Segura: `scp` (Secure Copy)

Estás conectado por SSH y acabas de comprimir un log de evidencia en un archivo `.tar.gz` (Nota anterior). ¿Cómo traés ese archivo físico desde el Servidor hacia tu laptop? 

No podés enviarlo por mail ni tenés un entorno gráfico para arrastrarlo con el mouse. 
La herramienta estándar de Linux es **`scp`**. Funciona exactamente igual que el comando local `cp` (Copiar), pero agregando el detalle de la red, y viajando mágicamente a través del túnel seguro y cifrado de SSH.

**Sintaxis base:** `scp [origen] [destino]`

### Caso A: Traer un archivo (Descargar / Pull)
Estás en la terminal normal de TU laptop. Querés traer el archivo `evidencia.log` que está en la carpeta `/var/log/` del servidor remoto `192.168.1.100` y guardarlo en tu carpeta actual local (`.`).
```bash
$ scp juan@192.168.1.100:/var/log/evidencia.log .
```

### Caso B: Enviar un archivo (Subir / Push)
Querés enviar tu script de automatización (`herramienta.sh`) desde tu PC local hacia la carpeta `/tmp/` del servidor web.
```bash
$ scp herramienta.sh juan@192.168.1.100:/tmp/
```

> **Aviso de Atacante (Pivoting):** `scp` requiere que tengas credenciales válidas SSH. Una vez que el Red Team obtiene una contraseña o llave de SSH, utilizarán `scp` constantemente para subir binarios ofensivos (herramientas de escaneo como Nmap portátil) a las máquinas internas de la empresa comprometida y usarlas como punto de salto (Pivoting).

---

## 📌 Must Know (Imprescindible)
- La sintaxis `ssh usuario@ip` para conectarse a un servidor.
- Comprender conceptualmente por qué usar Llaves SSH (Public Key Authentication) es más seguro que usar contraseñas (evita ataques de diccionario/fuerza bruta).
- Saber usar `scp` para mover archivos por la red, usando la sintaxis `scp origen destino` pero entendiendo el formato `usuario@ip:/ruta/al/archivo`.

---

## 🔄 Preguntas de repaso
1. Cuando ejecutas `ssh admin@10.0.5.20`, ¿en qué puerto (número y protocolo L4) esperás que el servidor remoto esté escuchando para aceptar tu conexión por defecto?
2. Escribí el comando exacto para Enviar un archivo llamado `payload.bin` que tenés en tu carpeta local, hacia la carpeta `/home/ubuntu/` del servidor remoto `200.50.10.1` utilizando el usuario `ubuntu`.
3. ¿Por qué el uso de "Autenticación por Llave Pública" (Public Key) mitiga casi al 100% el riesgo de que un atacante vulnere tu servidor SSH utilizando un ataque de Fuerza Bruta automatizado?

**➡️ Siguiente nota:** [[17 - Variables de Entorno y Alias]]
