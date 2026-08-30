# 03 - Fuerza Bruta y Cracking

Cuando ya sabemos que hay un servicio corriendo (ej: SSH o FTP) o nos robamos una base de datos llena de "Hashes", necesitamos herramientas que iteren miles de contraseñas por segundo para adivinarlas. Se dividen en Online y Offline.

---

## ⚡ Cracking ONLINE (Atacando servicios vivos)
Estas herramientas intentan iniciar sesión disparando diccionarios directamente a través de la red (Mucho ruido, alto riesgo de bloqueo de IP).

### 16. Hydra
- **¿Qué es?:** El crackeador de inicios de sesión en red más rápido y famoso. Soporta más de 50 protocolos (SSH, FTP, HTTP, RDP, MySQL).
- **Comando Básico (Crackear SSH sabiendo el usuario 'admin'):**
  `hydra -l admin -P /usr/share/wordlists/rockyou.txt ssh://<IP>`
- **Comando Básico (Crackear Formulario Web):**
  `hydra -l admin -P rockyou.txt <IP> http-post-form "/login.php:user=^USER^&pass=^PASS^:F=Contraseña Incorrecta"`

### 17. Medusa
- **¿Qué es?:** Una alternativa a Hydra. Más estable en conexiones paralelas pero un poco más antigua.
- **Comando Básico (FTP):**
  `medusa -u admin -P rockyou.txt -h <IP> -M ftp`

### 18. CrackMapExec (CME)
- **¿Qué es?:** (Crackeo de Redes Internas y Windows). La navaja suiza de Active Directory (Módulo 10). Permite testear millones de contraseñas contra SMB/WinRM en milisegundos y ejecutar código remotamente sin compilar malware.
- **Comando Básico (Password Spraying en AD):**
  `crackmapexec smb <IP_Rango> -u ListaUsuarios.txt -p 'Verano2024!'` *(Prueba una misma clave débil contra todos los usuarios del dominio para no bloquear las cuentas)*.

---

## 💻 Cracking OFFLINE (Triturando Hashes)
Te robaste la BBDD. Ya no dependés de la red (Nadie te bloquea). Tu límite de velocidad depende puramente del poder de tu CPU o Tarjeta Gráfica (GPU).

### 19. Hashcat
- **¿Qué es?:** El crackeador Offline más avanzado y rápido del planeta. Utiliza las Placas de Video (GPU Nvidia/AMD) para crackear millones de Hashes (Contraseñas encriptadas) por segundo.
- **Comando Básico (Crackear Hashes MD5 / Modo 0):**
  `hashcat -m 0 -a 0 hashes.txt rockyou.txt` *(Prueba las palabras del rockyou contra los hashes del archivo)*.
- **Identificar el código del tipo de Hash:** Buscar en la wiki de Hashcat. Ej: `1000` es NTLM, `1800` es SHA-512(Unix).

### 20. John The Ripper (JTR)
- **¿Qué es?:** El padre de los crackeadores Offline. Mejor optimizado para funcionar usando el Procesador (CPU). Es espectacular para crackear Zips con contraseña o archivos protegidos.
- **Comando Básico (Autodetecta el tipo de Hash):**
  `john --wordlist=rockyou.txt hashes.txt`
- **Crackear un archivo .ZIP (Requiere 2 pasos):**
  1. `zip2john secreto.zip > hash.txt` *(Extrae el hash del zip)*.
  2. `john --wordlist=rockyou.txt hash.txt`

### 21. CeWL
- **¿Qué es?:** Un generador de diccionarios. No crackea nada, pero crea las armas. Escanea la página web de tu víctima (ej. banco.com) y fabrica un diccionario con todas las palabras de esa web. Vital si la empresa usa contraseñas basadas en sus propios productos.
- **Comando Básico:**
  `cewl -w diccionario_custom.txt -d 2 http://empresa.com`

**➡️ Siguiente nota:** [[04 - Explotación y Active Directory]]
