# 07 - Web Hacking (OWASP)

Bienvenido al Módulo 07. Hasta este momento de la carrera, hemos estudiado cómo asegurar y comprometer la infraestructura de red pura (Sistemas Operativos, Enrutadores, Active Directory). 
Pero hoy en día, las empresas no instalan programas en la computadora de sus empleados. Todo está en la Nube. Todo es una "Página Web" o una "API".

El Web Hacking (Seguridad de Aplicaciones Web / AppSec) es el arte de encontrar fallas lógicas en el código que el programador subió a Internet. Es el terreno de juego del **Bug Bounty**, donde hackers éticos ganan recompensas de miles de dólares por reportar un solo error que permite comprar un iPhone en la tienda de Apple por $0 dólares.

---

## 📚 Notas del Módulo

### Anatomía de la Web
1. [[01 - Arquitectura Web (Frontend, Backend, APIs)]]
2. [[02 - El Protocolo HTTP (Peticiones, Respuestas, Headers)]]
3. [[03 - El Proyecto OWASP (El Top 10)]]

### Ataques contra el Backend (Servidor)
4. [[04 - Inyección I (SQL Injection - SQLi)]]
5. [[05 - Inyección II (Command Injection)]]
6. [[11 - Inclusión de Archivos (LFI, RFI, Path Traversal)]]

### Ataques contra el Frontend (Navegador y Usuario)
7. [[06 - Cross-Site Scripting (XSS)]]
8. [[07 - Manejo de Sesiones (Cookies vs Tokens JWT)]]
9. [[09 - Ataques de Suplantación (CSRF)]]

### Fallas Lógicas y de Arquitectura API
10. [[08 - Control de Acceso Roto (IDOR - BOLA)]]
11. [[10 - Falsificación del Lado del Servidor (SSRF)]]

## 🔬 Práctica y Evaluación
12. [[12 - Laboratorio Teórico - Cazando Bugs (Bug Bounty)]]
13. [[13 - Ejercicios]]
14. [[14 - Evaluación]]

## 📝 Resumen
15. [[15 - Resumen]]

---

## 🎯 ¿Qué vas a lograr?

Al finalizar este módulo, vas a ser capaz de:
- Leer y modificar una petición HTTP "cruda" interceptada en el aire.
- Diferenciar instantáneamente un ataque XSS (que ataca al navegador) de un ataque SQLi (que ataca a la base de datos).
- Entender cómo funcionan las *Cookies* y los *Tokens (JWT)*, y cómo los atacantes se roban las sesiones activas.
- Conocer las vulnerabilidades de diseño lógico que no dependen de código malicioso, sino de reglas de negocio mal programadas (como IDOR).

**➡️ Siguiente nota:** [[01 - Arquitectura Web (Frontend, Backend, APIs)]]
