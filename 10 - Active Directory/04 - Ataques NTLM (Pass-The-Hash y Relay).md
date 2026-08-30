# 04 - Ataques NTLM (Pass-The-Hash y Relay)

## 🎯 Objetivos
- Romper el mito de que "Siempre necesitamos la contraseña en texto plano".
- Entender cómo funciona la autenticación NTLM en sistemas Microsoft.
- Dominar el Arte del PTH (Pass-The-Hash) y NTLM Relaying.

---

## 🧠 Concepto: El Hash *ES* la llave

En la nota anterior, aprendimos que si un Hash es ultra-complejo (30 caracteres), es matemáticamente imposible crackearlo con placas de video (Fuerza Bruta) y nunca vamos a descubrir la contraseña en "Texto Plano".

¿Significa que perdimos el juego? **NO.**
La falla arquitectónica y gigantesca de los protocolos antiguos de Microsoft (como **NTLM** y **NTLMv2**) radica en cómo validan a los usuarios.
Cuando intentás iniciar sesión en una computadora de la red, el protocolo NTLM de Windows **no te pide tu contraseña en texto plano**. No le interesa qué letras digitaste. Windows lo que verdaderamente valida contra su base de datos es el **Hash** generado de esa contraseña.

**Deducción Hacker (El Pass-The-Hash):**
Si Windows no requiere el texto plano, sino que internamente usa el "Hash" como si fuera la llave real de la puerta... Si yo (como atacante) ya me robé el Hash, **no necesito crackearlo para descubrir su texto. ¡Simplemente puedo inyectar el Hash crudo en la red, pasárselo (Pass it) al servidor, y el servidor creerá que soy el dueño legítimo de la cuenta!**

---

## 🚪 Técnica 1: Pass-The-Hash (PTH)

1. El atacante logra vulnerar el "Servidor A" donde trabaja el Administrador de Dominio. 
2. Utiliza una herramienta legendaria (la más famosa de Windows) llamada **`Mimikatz`** (O su versión en Meterpreter: `hashdump`). `Mimikatz` entra a la Memoria RAM de Windows (`LSASS.exe`) y extrae los Hashes NTLM (En formato NT) crudos de todos los que iniciaron sesión ahí.
3. El atacante agarra ese texto (Ej: `NTLM Hash: a3b2c1d9...`). ¡No lo intenta crackear, porque es muy complejo!
4. El atacante usa herramientas (como `Impacket` en Linux o `CrackMapExec`) para **"Pasar el Hash"** (PTH) e inyectarlo en su intento de conexión contra el "Servidor B" (El Controlador de Dominio).
5. El Servidor B lee el Hash que el atacante acaba de inyectar, dice *"Sí, este es el hash oficial del Admin"*, y le otorga una Reverse Shell privilegiada instantánea.

*(Limitación: Para hacer Pass-The-Hash tradicional y reciclar Hashes estáticos, el atacante requiere tener el formato NT Hash limpio, lo cual suele exigir haber comprometido previamente una máquina).*

---

## 🔁 Técnica 2: NTLM Relaying (El Rebote)

¿Qué pasa si el atacante todavía no hackeó ninguna máquina (no puede correr Mimikatz), pero está usando la herramienta `Responder` de la nota anterior?
Si recuerdan, `Responder` roba un Hash de tipo `NetNTLMv2` (que contiene un factor dinámico de desafío/respuesta y **NO se puede usar** para hacer un Pass-The-Hash estático directo).

Aquí nace el **NTLM Relay** (El ataque del Espejo).
1. El atacante configura la red: Desactiva el robo normal de `Responder` y activa un programa adicional llamado `ntlmrelayx.py`.
2. El usuario cae en el engaño del Broadcast de `Responder` e intenta autenticarse contra el Hacker, enviándole su "Sello Dinámico de Autenticación" (NetNTLMv2).
3. El hacker (su herramienta) atrapa esa conexión al vuelo (Hombre en el medio). 
4. Pero el hacker NO guarda la conexión. Inmediatamente el Hacker se da vuelta y **rebota/reenvía (Relay)** esa mismísima autenticación dinámica súper rápida contra el Servidor Web o la Base de Datos Financiera de la empresa en milisegundos.
5. El Servidor Financiero recibe la autenticación (que es completamente legal y válida porque la generó el empleado original). El servidor lo autoriza. 
6. ¡El hacker acaba de lograr entrar al Servidor Financiero sin tener que haber crackeado nunca la contraseña, y sin haber inyectado ningún hash estático, simplemente utilizando el rebote de red (Relay) de la autenticación de una víctima ciega en el medio!

---

## 📌 Must Know (Imprescindible)
- Qué es la herramienta **Mimikatz**: El extractor de credenciales supremas. Creada por Benjamin Delpy. Lee procesos súper protegidos de Windows en RAM (como LSASS) para volcar Hashes crudos e inyectarlos. 
- Concepto de **Pass-The-Hash (PTH)**: Abusar del protocolo de Autenticación de Microsoft inyectando el Hash robado directamente para lograr iniciar sesión en redes internas (Lateral Movement), eludiendo permanentemente la necesidad de saber la contraseña en texto original.
- **NTLM Relaying:** Ocurre en vivo (Man-In-The-Middle). Es capturar una solicitud de autenticación que venía volando en la red hacia ti, para reenviarla intacta y validarla en nombre del usuario contra una máquina de altísimo valor que el hacker elija atacar.

---

## 🔄 Preguntas de repaso
1. Durante una post-explotación en una máquina local de contabilidad, utilizas el comando de recolección de Meterpreter o el binario de la herramienta francesa `Mimikatz` y extraes exitosamente el NT Hash local de un administrador del sistema. Dado el altísimo nivel de ofuscación de la contraseña en texto plano, decides "Pasar el Hash" (Pass The Hash) mediante la herramienta `CrackMapExec` contra el servidor central para ganar control. ¿En qué debilidad o regla fundamental del protocolo de Autenticación Clásico de Windows te estás apoyando para que el sistema acepte ese Hash como "Válido" para dejarte ingresar?
2. Al ejecutar la técnica NTLM Relay sobre un Director de Finanzas engañado en la red (Utilizando `Responder` junto con `ntlmrelayx`), no estás intentando adivinar ni robar su contraseña, ni estás inyectando nada crudo. Explicá cómo este concepto puramente de "Rebote/Redirección en vivo" de la firma original logra vulnerar y engañar exitosamente a un tercer Servidor Crítico (Target) al que nunca tuviste acceso original.
3. Si el Red Team extrae el NT Hash estático (Válido para Pass The Hash) de un simple "Becario de Soporte" desde su computadora. Al intentar inyectar y pasar ese hash contra el mismísimo "Controlador de Dominio Supremo" (DC) de la corporación; el ataque fallará y rebotará miserablemente arrojando acceso denegado. Explicá lógicamente por qué inyectar (Pasar) el Hash no es una receta mágica y todavía está regido obligatoriamente por el Control de Acceso nativo del Active Directory.

**➡️ Siguiente nota:** [[05 - Ataques Kerberos (Kerberoasting y AS-REP Roasting)]]
