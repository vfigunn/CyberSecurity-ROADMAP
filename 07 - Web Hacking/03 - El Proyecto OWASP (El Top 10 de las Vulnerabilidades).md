# 03 - El Proyecto OWASP (El Top 10)

## 🎯 Objetivos
- Conocer la Biblia de la seguridad de aplicaciones web.
- Entender cómo OWASP define los estándares que todas las empresas buscan auditar.
- Conocer a vuelo de pájaro las categorías de vulnerabilidades más letales del mundo actual antes de profundizarlas en las próximas notas.

---

## 🧠 Concepto: ¿Qué es OWASP?

A diferencia de la red interna de Windows, donde un hacker se esconde usando malware, la Web es pública. Todos pueden acceder a la página principal de un banco desde su celular en pijama. El problema es que los programadores de ese banco, al diseñar la página, comenten los mismos errores lógicos una y otra vez.

**OWASP (Open Worldwide Application Security Project)** es una fundación mundial sin fines de lucro. 
Su misión no es crear herramientas de hackeo, sino documentar metódicamente (basado en miles de incidentes reales) cuáles son los peores errores arquitectónicos que cometen las empresas.

Cada pocos años, publican el famosísimo documento **OWASP Top 10**. 
No es una lista de "10 vulnerabilidades exactas" (como "El bug de Apache 2012"). Es una lista de las **10 Categorías de riesgo arquitectónico** más grandes del mercado actual.

*(Aviso: Si te contratan en cualquier empresa del mundo para hacer Pentesting Web, el entregable/reporte obligatorio dirá: "Esta aplicación fue auditada bajo el marco de trabajo del OWASP Top 10").*

---

## 📜 El Resumen del OWASP Top 10 (Versión Actual / 2021)

*En las próximas notas profundizaremos en el funcionamiento técnico (y la explotación real) de estas categorías. Aquí te presento el mapa para que vayas familiarizando los términos.*

1. **A01: Broken Access Control (Control de Acceso Roto)**
   - *El problema #1 del planeta.* Las personas pueden entrar donde no deberían. Ej: Iniciar sesión como usuario normal, y forzar la URL a `/admin_panel` para gobernar la página (IDOR).
2. **A02: Cryptographic Failures (Fallos Criptográficos)**
   - Visto en el módulo pasado. Transmitir contraseñas sin HTTPS, usar MD5 para Hashes, o el terrible error de *Codificar (Base64)* la información de la tarjeta de crédito en lugar de *Encriptarla (AES)*.
3. **A03: Injection (Inyección)**
   - El favorito histórico del Red Team (Antiguo Rey, hoy puesto 3). Los atacantes inyectan código malicioso (SQL o Comandos de Linux) en las barras de búsqueda y el servidor web los obedece por error. (SQLi, Command Injection, XSS).
4. **A04: Insecure Design (Diseño Inseguro)**
   - Novedad reciente. Trata sobre errores de lógica de negocios irrecuperables. (Ej: Si el sitio web permite adivinar la respuesta secreta "Nombre de tu mascota", el diseño está mal de raíz sin importar el código).
5. **A05: Security Misconfiguration (Mala Configuración de Seguridad)**
   - Bases de datos MongoDB expuestas a Internet sin contraseña por defecto, o páginas de error que le devuelven al hacker el código fuente crudo en pantalla (Stack Traces).
6. **A06: Vulnerable and Outdated Components (Componentes Vulnerables y Desactualizados)**
   - El código que hizo tu empresa es perfecto. Pero la librería (paquete de Python/NPM) que importaste para calcular la fecha, fue hackeada el mes pasado y no aplicaste el parche (Log4j es el caso más famoso).
7. **A07: Identification and Authentication Failures (Fallos de Autenticación)**
   - El manejo desastroso del Login (Sesiones). Permitir que un hacker lance 1 millón de contraseñas por segundo a la pantalla de login (Fuerza Bruta) porque el sitio no tiene captchas ni bloqueos (Rate Limiting).
8. **A08: Software and Data Integrity Failures (Fallos de Integridad)**
   - Instalar actualizaciones automáticas sin que el sistema valide la Firma Digital (Hashes) del parche, permitiendo Man-in-the-Middle o manipulaciones de los *pipelines* de desarrollo (CI/CD).
9. **A09: Security Logging and Monitoring Failures (Fallas de Registro/Monitoreo)**
   - Un hacker extrae 500.000 tarjetas de crédito de la base de datos a las 4 AM. El ataque tomó 5 horas. Pero como la empresa no guardaba logs (registros) ni tenía alertas, el equipo de defensa se enteró 6 meses después porque lo leyeron en el diario.
10. **A10: Server-Side Request Forgery (SSRF)**
    - Manipular a un servidor web para que haga una solicitud (HTTP) *en tu nombre* hacia las redes ultra-secretas internas de la empresa, saltándose los Firewalls perimetrales. 

---

## 📌 Must Know (Imprescindible)
- Saber qué es el **OWASP Top 10** (El estándar mundial de los 10 riesgos más críticos en diseño y desarrollo de aplicaciones web).
- Familiarizarse con los términos de oro que te acompañarán el resto de tu carrera en Seguridad Web: **Broken Access Control, Injection, Cryptographic Failures, SSRF**.

---

## 🔄 Preguntas de repaso
1. Una aplicación web recién lanzada funciona perfectamente a nivel de código. Sin embargo, en el portal de registro, si un atacante teclea repetidamente "12345" en el campo de contraseña, el sistema no exige letras mayúsculas, números ni longitud mínima. Además, permite 500 intentos por minuto sin bloqueo. ¿En qué categoría específica del OWASP Top 10 se engloba este problema estructural?
2. Explicá por qué el "Control de Acceso Roto" (Broken Access Control) ha desplazado a las Inyecciones (Injection) del primer lugar histórico del Top 10, considerando que los lenguajes de programación modernos (y los Frameworks web) resuelven mucha seguridad técnica automáticamente, pero no pueden adivinar las reglas de negocio de una empresa.
3. Un atacante descubre que una página web desarrollada en PHP utiliza un servidor Apache instalado en 2015, el cual nunca fue actualizado (no se le aplicaron parches) debido a que los administradores tenían "miedo de romper algo". Al explotar una vulnerabilidad pública (CVE) de ese Apache antiguo, logra acceso. Según OWASP, ¿a qué categoría corresponde este incidente, tan común en la industria corporativa?

**➡️ Siguiente nota:** [[04 - Inyección I (SQL Injection - SQLi)]]
