# 04 - Explotación y Active Directory

Las armas de destrucción masiva. Estas herramientas se usan para lanzar el "Exploit", tomar el control de la computadora enemiga y escalar privilegios dentro de las redes corporativas de Microsoft (Módulo 10).

---

### 22. Metasploit Framework (MSF)
- **¿Qué es?:** El framework de ataque más grande del mundo. Posee miles de exploits listos para disparar y payloads avanzados como `Meterpreter` para espiar la RAM y mover archivos.
- **Flujo de comandos básico (En MSFconsole):**
  1. `search eternalblue` *(Busca un exploit)*.
  2. `use exploit/windows/smb/ms17_010_eternalblue` *(Lo selecciona)*.
  3. `set RHOSTS <IP_Víctima>` *(Apunta el cañón)*.
  4. `run` o `exploit` *(Dispara)*.

### 23. SearchSploit
- **¿Qué es?:** Una herramienta de consola (Buscador offline de Exploit-DB). Si ves un servicio viejo (Ej. ProFTPd 1.3.3c) ejecutándose, usás esta herramienta para buscar el archivo de código en C o Python que lo vulnera al instante.
- **Comando Básico:**
  `searchsploit ProFTPd 1.3.3c`

### 24. Impacket
- **¿Qué es?:** Una colección masiva de scripts de Python para hackear protocolos internos de Windows (SMB, WMI, Kerberos). Es obligatoria para Hacking de Active Directory.
- **Herramientas Clave de Impacket:**
  - `psexec.py usuario:clave@<IP>` *(Obtiene consola remota instantánea).*
  - `GetNPUsers.py` *(Para Ataques AS-REP Roasting).*
  - `GetUserSPNs.py` *(Para Ataques Kerberoasting).*
  - `secretsdump.py` *(Para volcar todos los hashes NTDS.dit y de memoria remota).*

### 25. Responder
- **¿Qué es?:** El envenenador de red definitivo de redes Windows. Escucha gritos perdidos (LLMNR / NBT-NS), engaña a las víctimas y roba sus Hashes NetNTLMv2. (Ver Nota 03 del Módulo 10).
- **Comando Básico (Modo Escucha en tu interfaz de red):**
  `responder -I eth0 -dwv`

### 26. Mimikatz
- **¿Qué es?:** Creada por Benjamin Delpy. La peor pesadilla de Microsoft. Es un ejecutable que corre en memoria en la PC de la víctima y extrae contraseñas crudas y Hashes directos desde el proceso de autenticación de Windows (`LSASS.exe`).
- **Comandos Básicos (Requiere ser Administrador local SYSTEM):**
  1. `privilege::debug` *(Eleva poder).*
  2. `sekurlsa::logonpasswords` *(Extrae las claves).*

### 27. BloodHound
- **¿Qué es?:** El mapa del tesoro corporativo. Toma información de la red y dibuja mediante *Teoría de Grafos 3D* los caminos (Vulnerabilidades de permisos) más cortos hacia el Controlador de Dominio.
- **Recolección (Se corre en la PC de la víctima):**
  `SharpHound.exe -c All`
- **Análisis:**
  Se arrastra el `.zip` generado a la Interfaz Gráfica de BloodHound.

### 28. Chisel
- **¿Qué es?:** Túneles (Pivoting). Si hackeaste el Servidor A, y querés atacar al Servidor B (que está escondido y no tiene salida a Internet), usás Chisel. Empaqueta el tráfico como si fuera Web (HTTP) para evadir los firewalls y que tu Kali Linux acceda a redes muertas e internas.
- **Uso:** (Cliente en la víctima / Servidor en Kali Linux).

**➡️ Siguiente nota:** [[05 - Inalámbrico y Redes]]
