# 22 - Protocolos de Correo, Transferencia y Administración

## 🎯 Objetivos
- Conocer los protocolos fundamentales para el funcionamiento del correo electrónico (SMTP, IMAP, POP3).
- Diferenciar los protocolos de transferencia de archivos (FTP, TFTP, SFTP) y los de administración remota (Telnet, SSH).
- Identificar cuáles son inherentemente inseguros (y no deben usarse) y cuáles son sus reemplazos modernos seguros.

---

## 📧 Protocolos de Correo Electrónico

Enviar y recibir un email no utiliza un solo protocolo. Es un sistema distribuido que usa diferentes protocolos para cada parte del viaje.

### 1. SMTP (Simple Mail Transfer Protocol) - *Puerto 25 (TCP)*
- **Para qué sirve:** Para **ENVIAR** correos. 
- **Cómo funciona:** Cuando le das a "Enviar" en tu aplicación (Outlook, Thunderbird), tu aplicación usa SMTP para enviar el correo a tu servidor (ej. Servidor de Gmail). Luego, tu servidor usa SMTP para enviárselo al servidor del destinatario (ej. Servidor de Hotmail).
- *Seguridad:* El SMTP original era texto plano y permitía falsificar al remitente (Email Spoofing) fácilmente. Hoy se usan versiones seguras (SMTPS en puerto 465/587) y controles en el [[19 - DNS en Profundidad|registro DNS]] como SPF y DMARC para validar identidades.

### 2. POP3 y IMAP (Recepción)
Sirven para que tu aplicación (Cliente) descargue los correos desde tu servidor (ej. Gmail) hacia tu computadora.
- **POP3 (Post Office Protocol v3) - *Puerto 110 (TCP)*:**
  - *Modelo antiguo.* Descarga los correos del servidor a tu PC local y, típicamente, **los borra del servidor**. Si abrís el correo en tu celular y lo descarga por POP3, luego no lo vas a poder ver en tu PC.
- **IMAP (Internet Message Access Protocol) - *Puerto 143 (TCP)*:**
  - *Modelo moderno.* Sincroniza la vista. Los correos se mantienen en el servidor. Si lees un correo en tu celular, se marca como leído en tu PC porque ambas aplicaciones miran la misma bandeja en el servidor web.

*(Tanto POP3 como IMAP tienen versiones seguras con TLS en los puertos 995 y 993 respectivamente).*

---

## 📁 Protocolos de Transferencia de Archivos

Mover archivos grandes por la red es la tarea de estos protocolos.

### 1. FTP (File Transfer Protocol) - *Puertos 20 y 21 (TCP)*
- *El abuelo de las transferencias.* Usa el puerto 21 para comandos ("Logueame como admin") y el puerto 20 para enviar los datos reales.
- **Riesgo Crítico:** Todo (incluyendo los usuarios y contraseñas) viaja en **texto plano**. Si un atacante intercepta la red, obtiene credenciales en segundos.
- **Veredicto en Seguridad:** **No debe usarse en internet bajo ninguna circunstancia** para datos sensibles.

### 2. TFTP (Trivial FTP) - *Puerto 69 (UDP)*
- Es la versión "Lite". Utiliza [[16 - TCP vs UDP|UDP]] en lugar de TCP, por lo que es más rápido y tiene menos sobrecarga. No requiere usuario ni contraseña (cualquiera puede leer/escribir si conoce el nombre del archivo).
- *Uso:* Se usa solo en LANs hiper-seguras para tareas muy específicas, como equipos de red que descargan su sistema operativo (PXE boot) al encenderse o para respaldar la configuración de un router.

### 3. SFTP (SSH File Transfer Protocol) - *Puerto 22 (TCP)*
- **El reemplazo seguro.** Proporciona transferencia de archivos, pero empaquetada dentro de una conexión cifrada por **SSH**. Todos los datos y credenciales están cifrados y a salvo de ataques de sniffing.
- *(No confundir con FTPS, que es el FTP viejo envuelto en TLS).*

---

## 🖥️ Administración Remota de Servidores

¿Cómo administras un servidor Linux que está físicamente a 500 kilómetros de distancia? Usás la línea de comandos remota.

### 1. Telnet - *Puerto 23 (TCP)*
- El protocolo original de administración de consola remota.
- **Riesgo Crítico:** Al igual que FTP, todo lo que tipeas viaja en texto plano. En un router configurado con Telnet, el comando de contraseña de administrador (`enable secret 1234`) viaja por el cable de tal forma que cualquiera puede leerlo.
- **Veredicto en Seguridad:** Absolutamente obsoleto e inaceptable en redes modernas. (Solo se lo encuentra a veces en dispositivos IoT horriblemente diseñados).

### 2. SSH (Secure Shell) - *Puerto 22 (TCP)*
- El estándar de la industria hoy en día. Reemplazó a Telnet.
- Proporciona una sesión de consola encriptada mediante criptografía de clave pública. Incluso si un atacante captura todo el tráfico entre tu computadora y el servidor, solo verá datos indescifrables.
- Además de dar consola, SSH puede hacer "túneles" (Port Forwarding), enviando otros protocolos inseguros por dentro de su conexión segura.

---

## 📌 Must Know (Imprescindible)
- Conocer los protocolos inseguros (Telnet, FTP, POP3/IMAP sin TLS) y por qué son peligrosos (transmiten en texto plano).
- Conocer sus alternativas seguras (SSH, SFTP).
- Saber que SMTP es para enviar (push) y que IMAP/POP3 es para recuperar (pull).

---

## 🔄 Preguntas de repaso
1. Explicá la diferencia funcional principal entre utilizar POP3 o IMAP para configurar la cuenta de correo corporativa en el teléfono de un empleado.
2. Como pentester, descubrís que un servidor tiene el puerto 21 (FTP) abierto y requiere autenticación. ¿Qué vulnerabilidad inherente tiene esto si un atacante ya está realizando ARP Spoofing en esa misma red?
3. ¿Por qué SFTP es considerado seguro y cuál es su relación con el protocolo de administración de servidores más famoso?

**➡️ Siguiente nota:** [[23 - VLANs]]
