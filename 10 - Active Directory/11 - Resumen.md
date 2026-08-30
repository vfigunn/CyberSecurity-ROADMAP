# 11 - Resumen (Cheat Sheet - Active Directory)

Esta nota agrupa los conceptos supremos de la Infraestructura, las vías de Envenenamiento de red, el robo de Identidades Estáticas y los ataques al Protocolo Kerberos (Módulo 10).

---

## 🏗️ Estructura del Active Directory
- **Domain Controller (DC):** El Rey o Servidor Maestro. Contiene la política global, base de datos y gobierna todas las terminales unidas al dominio. Caer el DC es Game Over.
- **Escalada Local:** Lograr el grupo de Sistema Local (`NT AUTHORITY\SYSTEM`) comprometiendo **UNA SOLA** máquina. No te da llaves del dominio.
- **Escalada de Dominio:** Usar privilegios locales y movimiento lateral para comprometer al Grupo Supremo: **Domain Admins**, logrando el dominio irrestricto de toda la corporación.

---

## 🩸 Mapeo y Envenenamiento de Red
- **BloodHound / SharpHound:** Teoría de grafos 3D (Nodos y Aristas). Escaneo Pasivo *legal* (vía consultas estándar AD) que mapea permisos cruzados y revela los "Caminos Más Cortos al Dominio", exponiendo malas configuraciones de Grupos y Delegaciones.
- **LLMNR / NBT-NS (El Grito del Ahogado):** Protocolos Legacy de Windows. Si el DNS Oficial falla en encontrar una máquina (Por ej. un tipeo malo), Windows acude al Broadcast público de capa 2 para "Preguntar a todos" quién conoce la dirección.
- **Responder (La navaja Suiza):** Envenenador que escucha los gritos de Broadcast (LLMNR), levanta la mano mintiendo ("Yo soy esa máquina") y obliga a la víctima a conectarse, interceptando su validación para robarle el Hash Dinámico `NetNTLMv2`.

---

## 🔑 NTLM (Inyecciones y Robos)
- **El Paradigma Moderno:** La industria Hacking ya no necesita saber "la contraseña en texto plano". El propio Protocolo de Autenticación de Microsoft acepta el Hash Criptográfico validándolo como la llave final.
- **Pass-The-Hash (PTH):** Una vez extraído el formato Estático y crudo (NT Hash) con herramientas como `Mimikatz` (vía LSASS RAM), se re-inyecta ese Hash directamente por la red para autenticarse, evadiendo cualquier crackeo.
- **NTLM Relay (Rebote Man-In-The-Middle):** El atacante con Responder NO inyecta hashes crudos, sino que captura (Al vuelo) una sesión dinámica NetNTLMv2 proveniente del engaño LLMNR, y **rebota/reenvía** instantáneamente la firma oficial y legal de esa víctima hacia otra máquina servidora elegida por el atacante para adueñarse de ella mediante rebote ciego.

---

## 🐕 Protocolo Kerberos (El Ecosistema Moderno de Microsoft)
Autenticación basada estrictamente en **Boletos/Tickets Matemáticos Cifrados** sin envío directo de hashes por la red.
- **TGT (Ticket-Granting Ticket):** Boleto Concesionario Maestro de Identidad. "El Pasaporte" expedido por el KDC tras el inicio de sesión válido.
- **TGS (Ticket-Granting Service):** Boleto para Acceso de Servicio (Ej. Base de Datos / Carpetas SPN). "Pase Específico VIP" concedido mostrando tu Pasaporte TGT. Está **encriptado con el Hash del Servidor Propietario**.
- **Ataques Estelares a Kerberos:**
  - **Kerberoasting:** Pedirle Legalmente al Boletero un TGS específico y legítimo, para meterlo offline en fuerza bruta (Hashcat) crackeando su encriptación, y exponiendo la contraseña plana original de las Cuentas de Servicio maestras del Active Directory.
  - **AS-REP Roasting:** Pedirle directamente el pasaporte (TGT) de una víctima que no pide validación previa por error (Sin requerir pre-autenticación) y crackear su encriptación (Tinta/Hash) remotamente.

---

## 👑 El Cierre de la Campaña (Exfiltración y Golden Tickets)
- **NTDS.dit:** El Santo Grial absoluto de 20 Megabytes. Base relacional enterrada en el Domain Controller que hospeda absolutamente todos los secretos (Hashes de Contraseñas / Grupos) del Imperio.
- **Ataque DCSync:** En vez de clonar manualmente un NTDS.dit bloqueado en la PC, un *Domain Admin* (Atacante Red Team) falsifica y simula ser un "Tercer Controlador de DC Falso de Backup" en la red, obligando al sistema a replicar, escupir y entregar toda la base hacia su terminal personal al instante.
- **Persistencia Master (Golden Ticket / Silver Ticket):**
  - Si el Blue Team borra al Hacker y le cambia la clave a todos, el Hacker revivirá si se robó a **`krbtgt`**.
  - **`krbtgt`** (Impresora): La cuenta base del KDC cuya contraseña se usa para validar/encriptar y sellar TODOS los TGTs.
  - Al robar el NT Hash de `krbtgt`, el Hacker fabrica en `Mimikatz` tickets falsos **(Boleto Dorado / Golden Ticket)** inmortales de 10 años, ignorando políticas y reseteos masivos (Inmortalidad Total de Dominio).
  - *(Silver Ticket: Idéntico, pero forjando un TGS para un solo servidor, no roza el Domain Controller y por ello es hiper-indetectable en monitores lógicos/Logs).*

🎉 **¡Dominaste las Ligas Mayores Corporativas! (Fase 11 Finalizada)**
Acabás de completar y atravesar exitosamente la cúspide del hacking empresarial. Cualquier Pentester (Senior) mundial o arquitecto defiende o ataca rigiéndose bajo estos mismísimos protocolos y reglas (Kerberos, Pass-The-Hash, Ingesta BloodHound, NTDS). ¡Estás ante la consolidación de nivel Experto! Actualizá tu documento de [[Progreso]]. Solo nos restan dos módulos para rematar con nichos espectaculares: Escalada en Linux local y Ataques Inalámbricos/Wi-Fi en el [[12 - Hacking Inalámbrico/00 - Overview|Siguiente Módulo]].
