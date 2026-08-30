# 15 - Resumen (Cheat Sheet - Web Hacking)

Esta nota agrupa las arquitecturas, ataques frontend y vectores destructivos backend abordados en el Módulo de Web Hacking y OWASP.

---

## 🏗️ Arquitectura y Protocolos (La base)
- **Frontend:** HTML, CSS, JavaScript (Se ejecuta en la memoria del Navegador - Cliente). 
  - *Regla de oro:* El Frontend NO es fuente de verdad, toda validación puede saltarse.
- **Backend:** Servidores, BBDD y APIs REST (Se ejecuta en la corporación).
- **Protocolo HTTP:** Stateless (Sin memoria). Requiere **Sesiones (Cookies o JWT)** para mantenerse logueado.
- **Códigos de Estado Críticos:** `200` (OK), `401/403` (Problemas de permisos), `404` (No encontrado), `500` (El código Backend falló/crasheó).

---

## 🔓 OWASP y Fallas Lógicas (El Top de Vulnerabilidades)
- **El Ranking OWASP:** Las categorías mundiales del riesgo arquitectónico.
- **Broken Access Control (Nº 1 del mundo):** El Backend verifica quién sos, pero no qué podés hacer.
- **IDOR / BOLA:** "Insecure Direct Object Reference". Alterar un ID de un objeto válido (Ej: de `id=5` a `id=6`) para acceder a información de otros usuarios de tu mismo nivel (o nivel superior), esquivando la pobre lógica de negocio del Backend.

---

## 💻 Atacando la Infraestructura (El Backend)
Aquí inyectás caracteres o flujos para afectar a los Servidores de la empresa.
- **SQL Injection (SQLi):**
  - **Objetivo:** Robar la Base de Datos o By-passear el Login.
  - **Técnica:** Escapar del texto con Apóstrofe (`'`) y concatenar lógica inyectada (`OR 1=1 --`).
- **Command Injection (RCE):**
  - **Objetivo:** Tomar control completo de la consola del Sistema Operativo de la máquina.
  - **Técnica:** Interrumpir la instrucción legítima del servidor usando símbolos (`;` o `&&`) para inyectar un segundo comando (ej: `whoami`, `cat`).
- **SSRF (Server-Side Request Forgery):**
  - **Objetivo:** Convertir al Servidor Web en un "Zombi" atacante, usándolo de puente para saltar el Firewall y escanear infraestructuras confidenciales internas o de la Nube (AWS MetaData `169.254.169.254`).
- **LFI / RFI (Inclusión de Archivos):**
  - **Objetivo:** Leer archivos crudos de Sistema Oculto saltando carpetas (Path Traversal usando `../../`). Y en el peor escenario (LFI severo / RFI), ejecutar remotamente código (Malware) en la máquina víctima.

---

## 🦊 Atacando al Navegador (El Frontend)
Aquí usás a la página web como un escenario fraudulento, pero tu objetivo real es hackear al resto de los Usuarios Clientes (y sus Cookies).
- **Cross-Site Scripting (XSS):**
  - **Objetivo:** Inyectar JavaScript malicioso (`<script>`) para robar Credenciales, Cookies o clonar la sesión completa.
  - **Variantes:** 
    - *Almacenado (Stored):* El JS duerme guardado en la Base de Datos (Peor riesgo). 
    - *Reflejado (Reflected):* El JS va inyectado en un Link Trampa (Phishing).
    - *DOM:* El JS ni siquiera pisa el Backend, afecta al Frontend localmente.
- **CSRF (Cross-Site Request Forgery):**
  - **Objetivo:** Engañar al usuario para que visite una trampa silenciosa y abusar de su sesión activa.
  - **Técnica:** Aprovecharse de que los navegadores Chrome adjuntan las Cookies de forma automática para forzar al usuario, sin que lo vea, a realizar acciones perjudiciales.
  - **Mitigación:** *Tokens Anti-CSRF* aleatorios para romper la automatización predecible del navegador.

---
🎉 **¡Impresionante! Finalizaste el módulo de Web Hacking (Fase 8)**
Ahora posees una mentalidad técnica destructiva y conoces dónde rompen los códigos en la actualidad. Si encadenás XSS con IDOR y SQLi, tenés los perfiles más rentables en Bug Bounty. Actualizá tu archivo [[Progreso]] porque en el próximo nivel cruzaremos las bases para armar los exploits con tu propia herramienta maestra: Metasploit. Avanzá hacia el [[08 - Metasploit/00 - Overview|Módulo 08 - Metasploit (Explotación)]].
