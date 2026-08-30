# 06 - Windows y Active Directory

Bienvenido al Módulo 06. Si alguna vez te preguntaste por qué los grupos de Ransomware logran apagar o secuestrar 10.000 computadoras de una empresa multinacional en menos de 1 hora, la respuesta está en este módulo: **Active Directory**.

Windows, en el hogar, es un sistema operativo diseñado para ser fácil de usar. Pero en el mundo corporativo, Windows es una bestia inmensa de administración centralizada. El 95% de las grandes empresas del planeta utilizan la arquitectura de Active Directory (AD) de Microsoft para gobernar su red.

Si sos del Blue Team, tu trabajo consistirá en monitorear los eventos de seguridad de AD y asegurar las Políticas de Grupo. Si sos del Red Team, tu objetivo final será robar un Ticket de Kerberos para convertirte en "Domain Admin" (El Dios de la red) y tomar control total del Bosque corporativo.

---

## 📚 Notas del Módulo

### Anatomía del Sistema (Máquina Local)
1. [[01 - Arquitectura de Windows (Procesos y Memoria)]]
2. [[02 - El Registro de Windows (Regedit)]]
3. [[03 - Servicios y Tareas Programadas]]
4. [[04 - Sistema de Archivos y Permisos (NTFS vs Share)]]

### Active Directory (El Corazón de la Empresa)
5. [[05 - Introducción a Active Directory]]
6. [[06 - Estructura Lógica (Dominios, Bosques y OUs)]]
7. [[07 - Controladores de Dominio (Domain Controllers)]]

### Protocolos de Confianza (Autenticación)
8. [[08 - Autenticación I (NTLM y el problema del Hash)]]
9. [[09 - Autenticación II (Kerberos y el Sistema de Tickets)]]
10. [[10 - Búsqueda y BBDD (Protocolo LDAP)]]

### Administración y Amenazas
11. [[11 - Políticas de Grupo (GPOs)]]
12. [[12 - PowerShell (Administración y Amenazas)]]

## 🔬 Práctica y Evaluación
13. [[13 - Ejercicios]]
14. [[14 - Evaluación]]

## 📝 Resumen
15. [[15 - Resumen]]

---

## 🎯 ¿Qué vas a lograr?

Al finalizar este módulo, vas a ser capaz de:
- Diferenciar instantáneamente un sistema doméstico aislado (Workgroup) de una red centralizada (Domain).
- Comprender profundamente dónde se esconde el Malware moderno (Registro, Servicios, Tareas Programadas).
- Explicar cómo el protocolo Kerberos permite autenticar usuarios sin enviar jamás sus contraseñas por la red.
- Entender cómo una sola Política de Grupo mal configurada puede comprometer 500 computadoras al mismo tiempo.

**➡️ Siguiente nota:** [[01 - Arquitectura de Windows (Procesos y Memoria)]]
