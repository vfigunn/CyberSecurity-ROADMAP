# 09 - Ejercicios del Módulo 10

## 📝 Instrucciones
Este es el examen práctico de lógica más valorado en Entrevistas de Red Team. Demostrá que entendés la diferencia entre una escalada local y una escalada global, y cómo abusar de los protocolos internos de Windows.

---

## 🧠 Ejercicios de Estrategia (Active Directory)

1. **La Meta Suprema:**
   - Lograste by-passear el Antivirus y obtuviste acceso remoto (`Reverse Shell`) en la laptop de un Analista Financiero. Un compañero tuyo Novato te dice: *"¡Excelente! Ahora instalemos un Ransomware y destruyamos esta laptop"*.
   - A nivel estratégico de Red Team, ¿por qué destruir la laptop de un solo empleado es un fracaso táctico gigantesco y cuál debería ser en realidad la Meta Suprema (El Servidor/Rol) y el archivo final (BBDD) que debes cazar para poseer el reino entero?

2. **La Trampa de los Permisos (BloodHound):**
   - Una corporación cuenta con 10,000 empleados. Es imposible saber a ojo quién tiene permiso sobre quién. Al exportar los datos usando el recolector `SharpHound.exe` y abrirlos en el mapa 3D interactivo en tu casa, detectás lo siguiente: 
   - El *Grupo "Mantenimiento IT"* tiene permiso para reiniciar la clave de la *Cuenta "Juan_Soporte"*. Y la cuenta *"Juan_Soporte"* tiene el privilegio de Administrador Local sobre el *Servidor Financiero*.
   - Explicá por qué este hallazgo no es un Exploit técnico de "Código Roto" de Windows, sino que representa la esencia real de la metodología de ataque estructural llamada *Misconfiguration* (Mala configuración de la delegación humana).

3. **Gritos Ciegos (LLMNR / Responder):**
   - Alguien escribe `\\Servidor-Viejo-Contable`. El Servidor DNS no existe y el DNS Oficial rebota la petición. Windows, en su comportamiento Legacy, empieza a gritar en *Broadcast* por la red: *"¿Quién carajo es Servidor-Viejo-Contable?"*.
   - Conectás tu herramienta `Responder`. Explicá cómo procedés para Envenenar y Mentirle a ese grito para efectuar el ataque de Man-In-The-Middle, y detallá específicamente qué objeto criptográfico de valor te entregará inocentemente la máquina víctima como resultado de tu engaño.

4. **El Límite del Pase (Pass-The-Hash vs Relay):**
   - Extraes un Hash crudo (Formato NT Hash estático) de la Memoria RAM de una computadora víctima usando Mimikatz: `[NTLM: e19ccf...329b]`.
   - Podés inyectar ese Hash crudo en la red (usando `CrackMapExec`) para iniciar sesión remotamente en otra máquina, haciendo un Pass-The-Hash directo sin saber la clave.
   - Contrastá esto con el Hash capturado volando en vivo por la red a través de un envenenamiento con `Responder` (El formato dinámico *NetNTLMv2* que vimos en el ejercicio 3). ¿Por qué no podés agarrar ese NetNTLMv2 y usarlo estáticamente como un Pass-The-Hash el día de mañana, obligándote forzosamente a tener que rebotarlo instantáneamente en pleno vuelo (NTLM Relaying)?

5. **Ataque Legítimo (Kerberoasting):**
   - Entendiendo que la filosofía fundamental de `Kerberos` (El Perro de 3 Cabezas de Microsoft) se basa en la utilización de *Boletos (Tickets)* y no en el envío de contraseñas.
   - Sos un usuario raso. Le pedís al Controlador de Dominio (DC) que te genere un Ticket TGS para ir a visitar un poderoso Servidor Microsoft SQL corporativo (SPN).
   - El Controlador de Dominio obedece alegremente y te da un Ticket TGS sellado/encriptado criptográficamente con la clave secreta (Hash) del administrador de ese Servidor SQL. Explicá cómo procede (fuera de la red de la corporación) el ataque llamado *Kerberoasting* para destruir esa encriptación, y cómo convierte a una simple petición de Tickets legal en una sustracción masiva de contraseñas.

6. **Inmortalidad en el AD (Golden Tickets):**
   - El Blue Team, sospechando que hackearon a sus empleados, obliga por Directiva a que los 5,000 miembros del banco cambien su contraseña de Windows y activen el Autenticador (MFA 2-Pasos) en sus celulares, aislando la red.
   - Vos (Atacante de Red Team) te reís en tu casa. Porque hace un mes robaste la base de datos `NTDS.dit` del Controlador de Dominio, y extrajiste el sagrado Hash de la cuenta mágica e invisible llamada `krbtgt`.
   - Explicá por qué ser dueño del Hash de `krbtgt` te permite inyectar "Billetes Falsos Dorados" (Golden Tickets) a perpetuidad, ignorando absolutamente los recientes cambios de contraseñas de los 5,000 empleados y el MFA que impuso el Blue Team. 

---

## 🎯 Autoevaluación
Es crítico y mandatorio dominar el Ejercicio 3 (Cómo el error de un empleado de escribir mal una IP desata el ataque Broadcast del hacker con `Responder`) y el Ejercicio 5 (Kerberoasting: Cómo se abusa de que un Servidor te entregue legalmente Tickets encriptados que podés descifrar en tu casa con placas de video).
**Si un técnico afirma que para hackear necesita la contraseña en texto plano, es un técnico desactualizado.** En Windows y AD, los Hashes y los Tickets *son* el nuevo texto plano. 

**➡️ Siguiente nota:** [[10 - Evaluación]]
