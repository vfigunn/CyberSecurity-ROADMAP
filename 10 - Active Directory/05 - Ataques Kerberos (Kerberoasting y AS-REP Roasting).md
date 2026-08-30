# 05 - Ataques Kerberos (Kerberoasting y AS-REP Roasting)

## 🎯 Objetivos
- Entender por qué NTLM (Nota 04) es obsoleto y Kerberos es el rey moderno.
- Conocer la arquitectura del Perro de Tres Cabezas (Kerberos / TGT / TGS).
- Aprender los dos ataques de Active Directory más importantes de la industria para robar identidades: Kerberoasting y AS-REP.

---

## 🧠 Concepto: El Boletero del Cine (Kerberos)

Microsoft entendió que el protocolo antiguo **NTLM** era un desastre (porque permitía el Pass-The-Hash tan fácilmente enviando los hashes crudos por la red).
En los Active Directory modernos, el 99% de las computadoras utilizan el protocolo de red súper seguro llamado **Kerberos** (Nombrado así por el perro de 3 cabezas que custodia el infierno griego).

Kerberos elimina por completo el envío de contraseñas. Funciona puramente con **Tickets (Billetes)** y matemáticas criptográficas.

### El Flujo Normal (La Autenticación Infranqueable):
1. **AS (Authentication Service):** El usuario prende la PC el lunes. Pone su clave. La PC le pide al Controlador de Dominio (DC) un "Boleto Maestro". El DC le entrega el **TGT (Ticket-Granting Ticket)** o Boleto Concesionario (La entrada al Cine).
2. **TGS (Ticket-Granting Service):** El usuario ahora quiere ir al Servidor de Archivos (Quiere entrar a la Sala 5 de películas). El usuario le presenta su Boleto Maestro (TGT) al DC, y le dice *"Me autorizas a la Sala 5?"*.
3. El DC le dice *"Sí"*. Y le genera un segundo boleto hiper-específico (El Boleto de Sala 5) llamado **TGS**. Ese boleto TGS está **Cifrado (Criptografía) con la clave privada del Servidor de Archivos (Sala 5)**.
4. El usuario viaja por la red con su boleto, se lo entrega al Servidor de Archivos, el Servidor lo desencripta con su propia clave, confirma que el DC lo firmó, y lo deja pasar.

*(Magia de Seguridad: El atacante en el medio de la red jamás puede robar ni ver contraseñas, solo ve Boletos inentendibles encriptados volando por el cable de red).*

---

## 🔥 El Ataque 1: Kerberoasting (El Talón de Aquiles)

Kerberos es ultra-seguro, excepto por una falla lógica monstruosa en la manera en la que los Servidores interactúan.

Como dijimos en el *Paso 3*, cualquier usuario normal y corriente de la red con una cuenta básica, puede pedirle al DC (Boletero) un Ticket TGS para ir a visitar cualquier Servicio/Servidor de la corporación.
¡Y el Controlador de Dominio te lo entrega sin cuestionar, **cifrado (encriptado) con el Hash de contraseña del usuario dueño de ese servicio (Cuenta de Servicio)**!

**El Flujo Ofensivo (Kerberoasting):**
1. El atacante usa su cuenta básica de Empleado Raso (Vulnerada).
2. Con una herramienta (Ej: `Rubeus` en C# o `Impacket` en Linux), le pregunta y le exige legalmente al DC que le imprima 50 boletos (TGS) para 50 Servicios ultra-críticos de la empresa (Servicio SQL, Servicio Web).
3. El DC se los entrega. El hacker agarra todos esos boletos encriptados, y en lugar de usarlos en la red, **se los guarda en el bolsillo, se desconecta de la empresa y se los lleva a su casa (Offline)**.
4. El atacante sabe que la encriptación matemática de esos boletos (Tickets TGS) *ES EXACTAMENTE* el Hash de contraseña del dueño del servidor SQL. 
5. Mete los boletos en la trituradora gráfica en su casa (`Hashcat`), crackea la protección de la encriptación (fuerza bruta), y consigue en texto plano las contraseñas de las Cuentas de Servicio maestras de los servidores, destrozando la empresa de forma legal sin levantar alarmas de red, porque simplemente "pidió un ticket al boletero".

---

## 🍖 El Ataque 2: AS-REP Roasting

El Kerberoasting clásico se enfoca en robar tickets del Paso 3 (El TGS del Servidor de Archivos).
Pero existe un ataque mucho más tonto y grave: El **AS-REP Roasting**. Ocurre en el Paso 1 (Cuando pedimos el Boleto Maestro TGT a la mañana).

Normalmente, para que el DC te entregue el Boleto TGT, tenés que pasar por la *"Pre-Autenticación"* (Es decir, probar primero que sabés tu contraseña).
Sin embargo, algunos Administradores de AD configuran cuentas súper viejas o de sistemas legacy y les tildan por error una casilla llamada: **"Do not require Kerberos preauthentication"** (No requerir pre-autenticación).

**El Flujo Ofensivo (Extracción instantánea):**
1. El atacante, sin tener la contraseña, le grita al DC: *"¡Che, pasame el Boleto TGT inicial de la cuenta del Gerente Viejo!"*.
2. Como esa cuenta tiene la Pre-Autenticación desactivada (Misconfiguration), el DC no hace ninguna pregunta y te vomita el Ticket Maestro por la cara, directamente encriptado con la contraseña del Gerente.
3. El atacante agarra la respuesta (AS-REP), se lo lleva a su casa en su bolsillo, y lo crackea offline de la misma forma que el Kerberoasting, obteniendo la contraseña del empleado instantáneamente y desde afuera del Active Directory.

---

## 📌 Must Know (Imprescindible)
- **TGT (Ticket-Granting Ticket):** El Boleto de Identidad base. Lo obtenés probando tu contraseña inicial y te permite pedir acceso a todas las demás cosas.
- **TGS (Ticket-Granting Service):** El Boleto específico de un recurso (El pase de Sala). Lo solicitás demostrando que tenés tu TGT. Viene encriptado con la clave del servidor destino.
- **Kerberoasting:** El ataque donde cualquier usuario válido le pide legalmente al DC un Ticket TGS para un servicio de alto nivel, lo extrae del entorno, y crackea la encriptación (El Hash del dueño) fuera de línea.
- **AS-REP Roasting:** El abuso de una mala configuración ("No requerir pre-autenticación Kerberos"), que permite al atacante extraer Boletos Maestros (TGT) de cuentas antiguas sin saber su clave original para crackearlos offline.

---

## 🔄 Preguntas de repaso
1. Al auditar la arquitectura de red (Blue Team), notás que el protocolo original `NTLM` sufre de un problema masivo debido a que es susceptible de rebotes (NTLM Relay) e inyecciones directas estáticas (Pass The Hash). Describí por qué la arquitectura que impuso el protocolo más moderno (`Kerberos`) logra neutralizar gran parte del Relay e Inyecciones, mencionando la diferencia criptográfica de viajar con "Tickets Encriptados y Validados por Servicios Centralizados (KDC)" en vez de "Suministrar Respuestas/Hashes NTLM constantes".
2. Sos el analista ofensivo (Red Team) y lograste vulnerar la cuenta del guardia de seguridad (sin ningún tipo de privilegio especial). Decidís ejecutar la técnica "Kerberoasting" utilizando la herramienta de Python `Impacket-GetUserSPNs`. El DC te devuelve exitosamente 5 Tickets TGS que pertenecen a 5 Servidores SQL Corporativos de alto valor (Cuentas de Servicio 'SPN'). Explicá por qué en este momento preciso el Firewall / IDS de la empresa no saltará bloqueándote, basándote en la "legalidad de la petición" de Kerberos.
3. Contrastando los dos perfiles de Roasting (Extraer tickets a la RAM local y llevarlos a tu casa para meterlos en la máquina trituradora `Hashcat` y forzar contraseñas). ¿En qué se diferencia absolutamente el pre-requisito obligatorio (La "Pre-Autenticación") para poder hacer un "Kerberoasting" (Que ataca la etapa 3 / TGS) vs un ataque directo "AS-REP Roasting" (Que ataca la cuenta en etapa 1 / TGT)?

**➡️ Siguiente nota:** [[06 - La Caída del DC (DCSync y NTDS.dit)]]
