# 07 - Forense y Esteganografía

Las últimas armas de nuestro arsenal se enfocan en ocultar datos (Esteganografía) o en el trabajo de los analistas de Blue Team/DFIR que intentan descubrir qué hizo el hacker analizando imágenes de discos duros y memorias RAM infectadas.

---

### 43. Exiftool
- **¿Qué es?:** Herramienta para leer y editar los metadatos de los archivos (Especialmente fotos). Una foto JPG inocente subida a Internet contiene en sus metadatos la "Latitud y Longitud del GPS exacta de la casa" donde fue tomada, y la marca de la cámara. Exiftool extrae esa inteligencia.
- **Comando Básico:**
  `exiftool imagen.jpg`

### 44. Steghide
- **¿Qué es?:** Esteganografía pura. Permite ocultar un archivo de texto secreto (Ej. contraseñas robadas) adentro de los píxeles de una foto `.jpg` de un perrito. A simple vista la foto es normal, pero si tenés la clave, Steghide desempaca el texto desde adentro de la imagen.
- **Ocultar un archivo en una foto:**
  `steghide embed -cf foto_perro.jpg -ef secreto.txt`
- **Extraer el secreto de una foto infectada:**
  `steghide extract -sf foto_perro.jpg`

### 45. Binwalk
- **¿Qué es?:** La herramienta definitiva de Ingeniería Inversa y Forense para diseccionar firmwares de routers o archivos corruptos. Escanea un archivo y te dice: *"Esto parece una simple foto, pero adentro tiene inyectado código en Zip y un sistema de archivos de Linux embebido"*. Y con un comando lo extrae.
- **Comando Básico (Escanear y Extraer todo lo que encuentre adentro):**
  `binwalk -e archivo_sospechoso.bin`

### 46. Autopsy
- **¿Qué es?:** El programa líder gratuito para Digital Forensics (DFIR). Se le carga la "Clonación/Imagen" del disco duro del empleado que cometió el fraude, y Autopsy grafica todos sus correos eliminados, fotos borradas, e historial de navegación recuperado del disco rígido mediante una interfaz web preciosa.
- **Uso:** (Se ejecuta el servidor local y se visualiza en Interfaz Gráfica).

### 47. Volatility (Framework)
- **¿Qué es?:** Análisis Forense de Memoria RAM. Si el malware del hacker no guardó archivos en el disco, sino que vive solo en la RAM (Ej. Fileless Malware), tomás un volcado de la RAM (`.vmem`) y lo pasás por Volatility para diseccionar el ataque en el aire y extraer procesos ocultos.
- **Comando Básico (Listar procesos corriendo en ese momento):**
  `volatility -f memoria_ram.vmem --profile=Win10x64_18362 pslist`

### 48. CyberChef
- **¿Qué es?:** No es de consola, es una Web App alojada localmente (La Navaja Suiza de la Criptografía y Codificación). El hacker o analista pega un texto raro en Base64 o en Hexadecimal, y aplica "Recetas" arrastrando cajas gráficamente para decodificarlo, desencriptarlo, o transformar datos en segundos sin tirar una sola línea de código Python.
- **Uso:** (Aplicación Web).

### 49. HashID
- **¿Qué es?:** Simple pero vital. Si robás o encontrás un Hash (Ej: `$1$g3Bf...`), no vas a saber con qué algoritmo crackearlo en Hashcat si no sabés de qué tipo es. Le pasás el hash a HashID y te adivinará si es MD5, SHA-256 o bcrypt.
- **Comando Básico:**
  `hashid '$1$g3Bf...'`

### 50. Tmux / Terminator
- **¿Qué es?:** El multiplicador de fuerza de todo Hacker. Cuando estás en medio de un ataque, necesitás correr Nmap en un lugar, escuchar Reverse Shells en otro, y tener Metasploit abierto. En lugar de tener 5 ventanas flotando, Tmux (o Terminator) divide tu consola en cuadrículas manejables, y mantiene los procesos corriendo en segundo plano incluso si perdés tu conexión SSH al servidor de ataque.
- **Comando Básico (Tmux):**
  `tmux` *(Y luego atajos de teclado como Ctrl+B, % para dividir pantalla verticalmente).*

---

🎉 **FIN DEL ARSENAL.**
Ya tenés en tu poder la biblioteca táctica rápida. Estos nombres (Impacket, Mimikatz, Burp, BloodHound, Nmap, Hashcat) son el lenguaje diario del 100% de los foros de hacking del mundo.
Usalos sabiamente y mantenelos a mano para tus desafíos en la consola.
