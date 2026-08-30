# 06 - OSINT e Ingeniería Social

Acá entramos a la "Fase 1" del Hacking. Antes de lanzar un exploit, los hackers perfilan la psicología humana de los empleados y extraen correos corporativos buscando en bases públicas de Internet sin tocar la empresa (OSINT - Inteligencia de Fuentes Abiertas).

---

### 36. Maltego
- **¿Qué es?:** La plataforma de OSINT comercial más potente. Dibuja un Grafo interactivo en pantalla. Ponés el nombre "Juan Pérez" y el programa bucea en Internet (con APIs) devolviéndote su Twitter, las empresas donde trabajó, y con qué otras personas se relaciona. Todo de forma gráfica.
- **Uso:** (Interfaz Gráfica - GUI). Viene instalado en Kali Linux como `Maltego Community Edition`.

### 37. theHarvester
- **¿Qué es?:** El clásico para recolectar correos electrónicos. Le pasás el nombre de una empresa, y escrapea (revisa) Google, LinkedIn, y Bing de forma masiva para darte una lista de todos los empleados y subdominios que encontró.
- **Comando Básico:**
  `theHarvester -d empresa.com -l 500 -b google`

### 38. Recon-ng
- **¿Qué es?:** Un framework de OSINT construido en Python con una interfaz idéntica a Metasploit. Posee "módulos" para buscar fugas de datos, puertos públicos e información geográfica (requiere configurar claves de APIs de distintos servicios para brillar).
- **Comando Básico (Iniciar el framework):**
  `recon-ng`

### 39. Sherlock
- **¿Qué es?:** La herramienta definitiva para buscar a una persona. Le pasás un "Nombre de Usuario" y Sherlock revisa en 300 redes sociales diferentes (Tiktok, Reddit, foros de programación) para ver en cuáles de ellas existe una cuenta con ese mismo nombre, descubriendo la huella digital completa del usuario.
- **Comando Básico:**
  `sherlock nombre_usuario`

### 40. Shodan
- **¿Qué es?:** Es el "Google de los Hackers". No es un programa de consola, es una página web (`shodan.io`) o herramienta por API. Mientras Google busca texto en páginas, Shodan escanea Internet y busca el hardware: (Cámaras de seguridad sin clave, semáforos expuestos a la red, y servidores de bases de datos olvidados).
- **Uso Común (Dorking):**
  Buscar `port:"3306" country:"US"` te listará todas las bases de datos de Estados Unidos accidentalmente abiertas al mundo.

### 41. Social-Engineer Toolkit (SET)
- **¿Qué es?:** La caja de herramientas creada por Dave Kennedy para atacar al humano (Ingeniería Social). Automatiza la creación de ataques de Phishing, clonación de páginas de logueo (Ej. Te crea un clon idéntico de Facebook en 3 clics) y ataques por códigos de barras o SMS.
- **Comando Básico (Lanzar interfaz de menú):**
  `setoolkit`

### 42. GoPhish
- **¿Qué es?:** El framework corporativo para simular ataques de Phishing dentro de empresas. Permite programar envíos de correos trampa masivos, y te genera un Dashboard interactivo que te avisa "Qué empleado abrió el correo" y "Qué empleado hizo clic en el enlace falso".
- **Uso:** (Aplicación Web que se lanza localmente y se usa en el navegador).

**➡️ Siguiente nota:** [[07 - Forense y Esteganografía]]
