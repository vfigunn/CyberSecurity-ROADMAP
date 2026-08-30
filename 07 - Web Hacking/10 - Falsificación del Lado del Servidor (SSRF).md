# 10 - Falsificación del Lado del Servidor (SSRF)

## 🎯 Objetivos
- Diferenciar drásticamente CSRF (Nota 09) de SSRF.
- Entender cómo SSRF logra vulnerar el perímetro de seguridad corporativo (Firewalls) desde adentro.
- Conocer cómo SSRF se usa como misil contra la infraestructura de la Nube (AWS/Azure).

---

## 🧠 Concepto: Convirtiendo al Servidor en un Zombi

En la nota pasada vimos CSRF (Falsificación de la petición del lado del Cliente), donde la víctima sos Vos y tu navegador, porque hiciste clic en el link del gatito.
**SSRF (Server-Side Request Forgery)** es la peor pesadilla arquitectónica de una empresa. La víctima del engaño **es el propio Servidor Web de la corporación (El Backend)**.

En un SSRF exitoso, logramos "controlar" la mente del Servidor Web, forzándolo a que realice peticiones HTTP, descargas, o se comunique con sistemas internos *en nuestro nombre*. Como la petición sale desde adentro de la empresa (del mismísimo servidor), saltamos el Firewall perimetral como si nada.

---

## 💥 El Ataque y el Abuso de Confianza

Los servidores web modernos (Backend) suelen tener funcionalidades que descargan archivos de Internet.
Imaginá un sitio de traducción de PDFs o un conversor de imágenes. La web tiene un campo que dice: 
> `[Ingrese la URL de la imagen a descargar:] http://imagenes.com/foto.jpg`

Cuando enviás ese texto, el servidor Backend en EE.UU. lee la URL, usa su propio poder de procesamiento para conectarse a `imagenes.com`, descarga la foto y te la procesa en la pantalla. Todo legítimo.

**El Ataque de Inyección de URL (SSRF):**
El Atacante (Red Team) descubre este campo. Y le da una orden letal:
> `[Ingrese la URL a descargar:] http://127.0.0.1:8080/admin_panel_oculto`
*(Recordatorio de Redes: 127.0.0.1 es 'localhost'. Significa "Yo mismo" en cualquier computadora).*

**La Destrucción (Explotando la Intranet Ciega):**
- El Servidor Web, tonto y obediente, no va a Internet. Se da la vuelta e intenta descargar una página web alojada en sí mismo (puerto 8080), que nunca fue expuesta a Internet porque es el panel privado de los administradores.
- Como la conexión se originó **desde la computadora del propio Servidor** hacia su propio interior, las reglas del Firewall la aprueban instantáneamente por ser tráfico hiper-confiable.
- El servidor descarga la página privada (con configuraciones sensibles) y se la devuelve al atacante en su casa mostrándosela en su navegador.

Con esta misma técnica, el atacante puede escanear IPs internas de la empresa (Ej. `http://192.168.1.15`), buscando servidores de bases de datos que no están expuestos en Internet, y usará al Servidor Web hackeado como un puente para leer la red interna a su antojo.

---

## ☁️ SSRF vs La Nube (AWS Metadata)

Si te toca un objetivo que vive en Cloud Computing (Amazon Web Services, Microsoft Azure, Google Cloud), encontrar un SSRF equivale a ganar el juego entero al instante (Game Over y Severidad Crítica).

Las computadoras alquiladas en la nube de Amazon (AWS EC2) tienen un secreto: **La URL mágica interna de Metadatos**.
Existe una IP súper específica (solo accesible desde el interior de esa computadora) que guarda las llaves secretas corporativas: `http://169.254.169.254/`

Si un atacante en un Bug Bounty inyecta un SSRF con esta URL:
> `http://169.254.169.254/latest/meta-data/iam/security-credentials/rol-admin`
El Servidor Web (que vive adentro de Amazon AWS) ejecutará la petición hacia su propia capa de nube. Amazon le devolverá los Tokens (Llaves de Acceso Crítico) de su cuenta corporativa. El Servidor le entregará esas llaves al atacante, quien podrá iniciar sesión en la cuenta global de la empresa y borrar todos sus servidores en cuestión de minutos (Caso real: Robo a Capital One).

---

## 📌 Must Know (Imprescindible)
- **SSRF (Server-Side Request Forgery)** se trata de inyectar o manipular URLs para engañar a un Servidor (Backend) y forzarlo a realizar peticiones HTTP hacia destinos internos (o de Nube) elegidos por el atacante.
- El peligro extremo radica en **Saltar el Firewall**. Las aplicaciones internas ciegas asumen que cualquier petición originada por el Servidor Web Maestro (Localhost) es confiable y segura.
- Conocer la IP mágica que hace mortales los SSRF en entornos de infraestructura en la Nube (AWS): **`169.254.169.254`** (El servicio de metadatos).

---

## 🔄 Preguntas de repaso
1. Una aplicación web de diseño gráfico te permite "Cargar tu imagen desde Google Drive" pegando el link del archivo en la caja de texto, por lo que el Backend procede a conectarse y descargar esa imagen. A la hora de modelar la arquitectura de amenazas, diferenciá concretamente por qué este comportamiento del servidor lo pone en riesgo de SSRF pero no lo pone en riesgo de CSRF (visto en la nota anterior).
2. Estás ejecutando una prueba de concepto SSRF inyectando la URL interna `http://192.168.10.55/panel_admin` en una barra vulnerable, pero la aplicación no te muestra ninguna página en respuesta. Sin embargo, mediante un cronómetro, notás que si ponés una IP de un servidor apagado (`192.168.10.99`) la página demora 15 segundos en cargar; mientras que si ponés la IP `.55` la página te tira el error de inmediato en menos de 1 segundo (Respuesta rápida). ¿Qué información de inteligencia ofensiva podrías deducir sobre la red interna (Intranet) del objetivo basándote en la diferencia de tiempos de procesamiento (Ataque Ciego)?
3. Si un equipo del Red Team localiza una vulnerabilidad de SSRF en una aplicación financiera que está hosteada al 100% en contenedores EC2 dentro de Amazon Web Services (AWS). ¿A qué IP de red interna estándar (la cual debés memorizar) dirigirían inmediatamente su SSRF para forzar al servidor a vomitar las credenciales hiper-secretas de acceso a la nube, logrando la escalada de privilegios máxima?

**➡️ Siguiente nota:** [[11 - Inclusión de Archivos (LFI / RFI y Directory Traversal)]]
