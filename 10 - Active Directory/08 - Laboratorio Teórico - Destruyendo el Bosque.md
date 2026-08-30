# Lab 10.1 - Destruyendo el Bosque (Campaña AD Completa)

## 🎯 Objetivo
Visualizar el flujo natural, cronológico y despiadado de un Operador de Red Team (o un Ransomware de Estado) atacando una red corporativa de 5.000 equipos gobernada por Active Directory, aplicando cada concepto de la Fase 11 en cadena.

---

## 📜 El Escenario (Operación "Caída del Rey")

Estás contratado físicamente. Te sentaste en el Lobby del "Banco Mundial".
Enchufaste un cable de red a un puerto de pared libre que estaba escondido detrás de una maceta en la recepción. (No tenés usuario, no tenés clave, solo tenés acceso ciego a la red local).

### Fase 1: El Ruido en el Cable (Responder)
**Día 1:** Arrancás tu computadora de ataque (Kali Linux). 
El Firewall no te va a dejar hacer escaneos masivos, así que decidís ser un "Fantasma de Red". Encendés la herramienta `Responder` y te quedás escuchando pasivamente la red.
- A las 10:45 AM, un desarrollador de IT escribe mal el nombre de una impresora: `\\Print-Serverr`.
- El DNS falla. Windows lanza un grito de auxilio LLMNR/NBT-NS a todo el piso.
- Tu `Responder` escucha el grito. Le responde en milisegundos: *"Acá está el Print-Server, pasame tu credencial"*.
- El desarrollador se conecta a tu máquina. ¡Boom! Obtuviste en tu pantalla el inmenso **Hash NetNTLMv2** del desarrollador.

### Fase 2: Crackeando o Rebotando (NTLM Relay)
**Día 2:** Ese Hash NetNTLMv2 robado por Responder es dinámico, no podés usarlo directamente.
- Metés el Hash en tu trituradora gráfica (Hashcat) en tu casa. El desarrollador usaba una contraseña penosa: *"Banco2024!"*. ¡Se crackeó en 2 minutos!
- Al día siguiente, volvés al Banco, y te logueás en la red como si fueras el Desarrollador. 
*(Escalada Local Lograda. Sos un usuario Estándar de la Red).*

### Fase 3: Dibujando el Laberinto (BloodHound)
**Día 4:** Sos un Desarrollador, pero no sos Administrador de Dominio (El Rey). No sabés dónde está la bóveda.
- Usás tus permisos de desarrollador para subir y ejecutar `SharpHound.exe`.
- SharpHound escanea pasivamente todo el Active Directory y te dibuja el mapa (Grafo) de los 5.000 empleados en BloodHound.
- Al analizar el mapa 3D, descubrís una vulnerabilidad espectacular: **Tu cuenta de desarrollador tiene permisos para delegar y pedir Tickets Kerberos (SPN) de un Servidor de Backup Antiguo.**

### Fase 4: Solicitud de Entradas (Kerberoasting)
**Día 5:** Usás el camino que te marcó BloodHound.
- Con la herramienta `Impacket`, le hablás al Controlador de Dominio (Boletero). Le decís: *"Soy el Desarrollador, pasame un ticket TGS para ir al Servidor de Backup"*.
- El DC te entrega el Boleto TGS encriptado.
- Te lo llevás a tu casa. Como el Boleto TGS está encriptado directamente con el **Hash de la Cuenta de Servicio de Backup**, usás Hashcat de nuevo. La clave del Servidor de Backup se rompe.
- ¡Resulta que el Servidor de Backup pertenecía, lógicamente, al grupo global de *"Administradores de Dominio"* para poder hacer copias!

### Fase 5: Jaque Mate (DCSync)
**Día 7:** Ya sos Dios en la red. Tenés las llaves del Administrador de Dominio.
- Te conectás por red. No atacás el disco local porque te va a frenar el EDR.
- Ejecutás un ataque pasivo de réplica: **DCSync**.
- Tu máquina maliciosa le grita al Controlador de Dominio original: *"Hola, soy el DC Secundario, sincronizame tu Base de Datos entera"*.
- El DC te vomita el archivo `NTDS.dit` completo. Acabás de extraer 5.000 contraseñas y hashes de la empresa en 15 segundos.

### Fase 6: Inmortalidad (Golden Ticket)
**Día 8:** Entregás tu reporte al Banco, demostrando que la red cayó entera.
- Les explicás que si fueras un cibercriminal ruso (Ransomware), en el Día 7 habrías agarrado el Hash robado de la cuenta mágica `krbtgt`, e impreso un **Golden Ticket (Boleto Dorado)** de Kerberos que dura 10 años.
- Este billete falso te permitiría volver a entrar al banco por la puerta grande en el año 2030, sin contraseña, burlando cualquier sistema biométrico que instalen.

---

## 📝 Conclusión del Laboratorio
El Hacking Interno es un juego de dominó. El Blue Team del banco tenía las murallas (Firewalls exteriores) perfectamente diseñadas. Pero la podredumbre estaba adentro: Permitir LLMNR, dejar Cuentas de Servicio débiles, y no auditar quién tenía los permisos en cascada con BloodHound, causó el colapso y posterior extorsión económica global (Ransomware) por un solo empleado que escribió mal la palabra "Impresora".

**➡️ Siguiente nota:** [[09 - Ejercicios]]
