# 02 - Descubrimiento y Hacking Web

Agrupa a los Proxies, Fuzzers y Escáneres Web. Estas herramientas atacan el Puerto 80 (HTTP) o 443 (HTTPS) con el fin de encontrar carpetas ocultas, APIs secretas y vulnerabilidades (SQLi / XSS).

---

### 8. Burp Suite
- **¿Qué es?:** El Proxy Interceptor comercial número 1 del mundo. Te sentás en el medio entre tu Navegador Firefox y el Servidor. Frenás las peticiones web en vivo, modificás parámetros invisibles (Precios, IDs) y las mandás.
- **Uso:** Interfaz Gráfica (GUI). La herramienta principal de todo Bug Bounty Hunter. *(Módulo Repeater, Intruder, Proxy).*

### 9. Gobuster
- **¿Qué es?:** Una herramienta rapidísima (Escrita en Go) para Fuzzing. Descubre carpetas ocultas y archivos (`/admin`, `/login.php`) en servidores web aplicando ataques de diccionario ciegos a lo bestia.
- **Comando Básico (Carpetas):**
  `gobuster dir -u http://<IP> -w /usr/share/wordlists/dirb/common.txt`
- **Buscar extensiones puntuales:**
  Añadir `-x php,txt,html` *(Fuerza a buscar archivos que terminen en esas extensiones).*

### 10. Ffuf (Fuzz Faster U Fool)
- **¿Qué es?:** El sucesor moderno de Gobuster. También escrito en Go, es ridículamente rápido y ultra-personalizable. Sirve para fuzzing de carpetas, subdominios y parámetros web.
- **Comando Básico:**
  `ffuf -w diccionario.txt -u http://<IP>/FUZZ` *(Reemplazará la palabra FUZZ por cada palabra del diccionario).*

### 11. Nikto
- **¿Qué es?:** Un escáner web histórico. Escanea rápidamente un servidor y te avisa si está corriendo software obsoleto (Ej: Apache versión vieja), o si le faltan cabeceras de seguridad. Hace mucho ruido.
- **Comando Básico:**
  `nikto -h http://<IP>`

### 12. SQLMap
- **¿Qué es?:** La herramienta suprema (y destructiva) para detectar y explotar inyecciones SQL de forma 100% automatizada. Literalmente puede vaciar bases de datos enteras con un comando si la web es vulnerable.
- **Comando Básico (Volcar bases):**
  `sqlmap -u "http://<IP>/producto.php?id=1" --dbs` *(Busca todas las bases de datos)*.
  `sqlmap -u "http://<IP>/producto.php?id=1" -D base1 -T usuarios --dump` *(Vacía la tabla de usuarios)*.

### 13. WPScan (WordPress Security Scanner)
- **¿Qué es?:** Herramienta dedicada exclusivamente a hackear sitios hechos con WordPress (Que componen el 40% de Internet). Enumera plugins viejos, usuarios y hace fuerza bruta.
- **Comando Básico (Enumerar usuarios y plugins):**
  `wpscan --url http://sitio.com --enumerate u,vp`

### 14. OWASP ZAP (Zed Attack Proxy)
- **¿Qué es?:** La alternativa 100% gratuita y Open Source a Burp Suite, mantenida por la fundación OWASP. También intercepta tráfico y realiza escaneos automatizados geniales de vulnerabilidades web (Spiders).
- **Uso:** Interfaz Gráfica (GUI) / Automatización en DevSecOps.

### 15. Wfuzz
- **¿Qué es?:** El Fuzzer clásico de Python. Excelente para descubrir "Parámetros escondidos" en una URL (Ej. `http://sitio.com/pagina?FUZZ=1`).
- **Comando Básico:**
  `wfuzz -c -z file,diccionario.txt --hc 404 http://<IP>/pagina.php?FUZZ=1` *(Oculta todos los errores 404 Not Found).*

**➡️ Siguiente nota:** [[03 - Fuerza Bruta y Cracking]]
