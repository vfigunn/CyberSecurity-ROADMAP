# 15 - Resumen (Cheat Sheet - Windows & AD)

Esta nota agrupa los conceptos arquitectónicos, protocolos de autenticación corporativos y vectores de ataque fundamentales del Ecosistema de Microsoft.

---

## 🏗️ Arquitectura Local y Persistencia
- **Rings (Modos):** Ring 3 (User Mode - Aplicaciones limitadas) vs Ring 0 (Kernel Mode - Control total de hardware).
- **El Registro (`regedit`):** La base de datos central de Windows.
  - **HKLM:** Afecta a toda la máquina. Requiere permisos de Admin.
  - **HKCU:** Afecta solo al usuario logueado. NO requiere admin.
- **Persistencia de Malware:** Esconderse para arrancar con la PC.
  - Vía Registro: Claves `Run` y `RunOnce`.
  - Vía Servicios: Se ocultan en background (muchos corren como `NT AUTHORITY\SYSTEM`).
  - Vía Tareas Programadas (`Taskschd`): Ejecuciones por tiempo (Similar a Cron).
- **Procesos críticos:** `smss.exe` (Inicio), `svchost.exe` (Contenedor de servicios), **`lsass.exe`** (El guardián de la RAM que contiene los hashes NTLM).

---

## 🔒 Sistema de Archivos (Permisos)
- **NTFS vs Share:** 
  - *Share (Red):* La puerta exterior (Acceso por red).
  - *NTFS (Local):* La puerta interior física.
- **Regla de conflicto:** Al acceder por red, se combinan ambos permisos y **GANA EL MÁS RESTRICTIVO**. (Ej. Modificar en red + Lectura en NTFS = El usuario solo tendrá permiso de Lectura).

---

## 🏢 Active Directory (Arquitectura Lógica)
La base de datos (Directorio) que rige al 95% de las empresas Fortune 500.
- **Jerarquía:** 
  - **OU (Organizational Unit):** Carpetas para ordenar usuarios/PCs y delegar poder.
  - **Dominio:** Límite administrativo local (ej. `empresa.com`).
  - **Árbol:** Dominios que comparten raíz (ej. `es.empresa.com`).
  - **Bosque (Forest):** El límite supremo de seguridad en AD.
- **El Controlador de Dominio (DC):** El servidor vital. Hospeda el archivo **`NTDS.dit`** (Donde están todos los hashes de las claves de la empresa). Si el DC cae, hay Replicación (varios DCs sincronizados).

---

## 🔑 Autenticación, Búsqueda y Administración
- **LDAP:** El "Buscador/Guía Telefónica" (Puerto 389). Se usa para leer la base de datos (Buscar correos, grupos). *Herramienta ofensiva: BloodHound (Mapea rutas de ataque).*
- **NTLMv2:** Protocolo heredado de Desafío/Respuesta. 
  - *Problema:* El Hash es equivalente a la clave.
  - *Ataque letal:* **Pass-The-Hash (PtH).** (Robar el hash de LSASS y presentarlo para loguearse).
- **Kerberos:** El estándar de Tickets (KDC, TGT, TGS). La clave (casi) nunca viaja.
  - *Ataque 1:* **Golden Ticket.** Falsificar tickets maestros robando la cuenta `krbtgt` del Controlador de Dominio.
  - *Ataque 2:* **Kerberoasting.** Solicitar tickets TGS lícitamente, llevárselos a casa y romper (crackear offline) las contraseñas de las Cuentas de Servicio.

---

## 🛡️ Políticas de Grupo y Fileless Malware
- **GPOs (Group Policy Objects):** Paquetes de reglas inyectadas desde el Controlador de Dominio hacia miles de computadoras (Endurecimiento).
  - *Vulnerabilidad:* Si el Red Team obtiene acceso de edición a una GPO central, puede inyectar un Ransomware que todas las computadoras de la empresa descargarán y ejecutarán por sí solas.
- **PowerShell:** Terminal orientada a Objetos (.NET). Pasaje de datos mediante pipes reales, no textos.
  - *Fileless Malware:* "Living off the Land" (LotL). Ejecutar código malicioso inyectándolo directamente a la RAM a través de PowerShell, sin guardar ningún ejecutable (`.exe`) en el disco duro, evadiendo Antivirus tradicionales.

---
🎉 **¡Felicitaciones por dominar la Infraestructura de Windows (Fase 7)!**
Ahora entendés cómo el Blue Team organiza a 10.000 empleados y cómo las APTs destruyen redes completas. Actualizá tu archivo [[Progreso]] y prepárate: cruzaremos la barrera web. En el [[07 - Web Hacking/00 - Overview|Módulo 07 - Web Hacking (OWASP)]] entenderemos cómo funcionan las páginas de Internet y cómo explotarlas (Inyecciones SQL, XSS, APIs).
