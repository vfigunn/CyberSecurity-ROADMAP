# 10 - Evaluación del Módulo 10 (Active Directory)

## 📝 Instrucciones
Evaluación final del módulo. Múltiples preguntas basadas en los protocolos internos, recolección pasiva, extracción de hashes y Kerberos.
Anotá tus respuestas y luego verificalas con el solucionario.

> **Importante:** Las respuestas correctas y explicaciones están en: `[[29 - Evaluaciones/Respuestas/Módulo 10 - Respuestas]]`.

---

## 🎯 Sección 1: Reconocimiento y Envenenamiento

**1. Sos un Analista Ofensivo que logró acceso estándar (sin privilegios) en una PC de la corporación. Estás a ciegas y necesitás saber qué Grupos, Usuarios y Delegaciones existen en el Directorio Activo, y cómo se enlazan para armar tu ruta de ataque (Movimiento Lateral). ¿Qué herramienta, que utiliza la Teoría de Grafos matemática, ejecutarías para mapear de manera gráfica estos "caminos de sangre" a través del dominio?**
A) Nmap (Zenmap)
B) BloodHound (Mediante el recolector SharpHound)
C) Responder
D) Mimikatz

**2. En redes corporativas Windows (Legacy o sin parchear), ¿Qué comportamiento ocurre por defecto cuando un usuario intenta contactar una máquina de la red local que NO existe (Ej. `\\Cerrvidor-Files`), y el Servidor DNS principal no logra resolverlo?**
A) Windows bloquea al usuario inmediatamente y desconecta su placa de red.
B) Windows activa los protocolos LLMNR / NBT-NS y comienza a gritar y preguntar en "Broadcast" a todos los equipos locales si alguno conoce o sabe quién es `Cerrvidor-Files`.
C) El Active Directory desactiva el servicio Kerberos por seguridad.
D) Windows abre un Ticket de Soporte Técnico automático al Firewall Cisco.

**3. Aprovechando el comportamiento mencionado en la pregunta anterior (Grito de Broadcast), encendés tu herramienta `Responder` (Hombre en el Medio). Lográs engañar a la víctima, simular ser la carpeta compartida que él buscaba, y forzarlo a intentar autenticarse en tu equipo malicioso. ¿Qué formato de Hash criptográfico (Dinámico de desafío-respuesta) obtendrás en tu terminal como resultado de esta captura exitosa en el aire?**
A) MD5 Clásico
B) Hash estático NTLM (NT Hash puro)
C) SHA-256 de Kernel
D) NetNTLMv2

---

## 🎯 Sección 2: Protocolos, Relay y Kerberoasting

**4. Obtuviste una terminal con máximos privilegios locales (`SYSTEM`) en la laptop de un Director de Sistemas vulnerada. Con el comando avanzado `hashdump` de Meterpreter (O Mimikatz), volcaste y obtuviste el Hash puramente estático (NT Hash) del Director (Ej: `e3b0c...`). ¿Qué técnica hiper-clásica de Microsoft Active Directory realizarás inyectando este Hash para iniciar sesión en los servidores críticos, saltándote el requerimiento vital de poseer o adivinar la contraseña en "Texto Plano"?**
A) SQL Injection de Identidad
B) Pass-The-Hash (PTH)
C) Golden Ticket Injection
D) AS-REP Roasting Offline

**5. Microsoft intentó modernizar y blindar la red, designando a `Kerberos` como su protocolo de autenticación maestro. A diferencia del antiguo NTLM, Kerberos elimina los Hashes voladores en vivo y centraliza la confianza en el Centro de Distribución de Llaves (KDC). ¿Qué estructura lógica emplea Kerberos para conceder e identificar accesos válidos a los usuarios a lo largo de la red?**
A) Llaves USB asimétricas (FIDO2)
B) El intercambio de Boletos o Tickets TGT y TGS encriptados.
C) Sesiones JWT (JSON Web Tokens)
D) Certificados SSL X.509 únicamente.

**6. Si poseés una cuenta normal y estándar (bajo privilegio), le podés pedir "Legalmente" al Controlador de Dominio (Kerberos) que te expida y te entregue un Boleto para entrar al Servidor de Base de Datos SQL Corporativo. El DC te entrega ese ticket de servicio (TGS) el cual viaja hacia ti **encriptado/firmado** matemáticamente utilizando el mismísimo Hash Maestro de la Cuenta que gobierna ese servidor SQL (Service Principal Name - SPN). ¿Cómo se llama la técnica donde te llevás este Ticket válido a tu casa para meterlo en fuerza bruta (Hashcat / GPU) intentando quebrar su encriptación y robarle el Hash al servidor?**
A) NTLM Relaying
B) Kerberoasting
C) DCSync (Sincronización de Réplica)
D) Pass-The-Ticket (PTT)

---

## 🎯 Sección 3: Extracción Total y Persistencia Absoluta

**7. Un equipo del Red Team finaliza exitosamente un ataque pasivo de réplica sobre el Controlador de Dominio (Ataque de Sincronización DCSync) suplantando falsamente la identidad de un controlador secundario. ¿Qué archivo relacional crudo (la base de datos maestra), que contiene absolutamente todas las cuentas, grupos, y hashes NTLM del ecosistema corporativo completo de Windows, lograron volcar (dumpear) exitosamente por la red?**
A) `shadow` (Linux)
B) `SAM.hive` (Base local)
C) `NTDS.dit` (Active Directory DB)
D) `winlogon.exe`

**8. Debido a que es imposible saber cuándo el Blue Team (Defensa) se dará cuenta del ataque y forzará un "Reseteo Masivo y Cambio de Contraseñas de Todos los Empleados", el Red Team aplica una táctica de Persistencia Absoluta a perpetuidad, conocida como el *Ataque de Golden Ticket* (Boleto Dorado). ¿Para forjar/imprimir billetes TGT válidos e infinitos, cuál es la cuenta invisible (la impresora oficial) de cuyo Hash el atacante debió apoderarse obligatoriamente de antemano?**
A) `Administrator` (UID 500)
B) `krbtgt` (Key Distribution Center Service Account)
C) `Guest` (Invitado)
D) `SYSTEM`

**9. Al desplegar y aplicar el conocimiento de Persistencia (Golden y Silver Tickets), analizá el sigilo y la evasión técnica. ¿Por qué razón arquitectónica el método del "Silver Ticket" (Falsificación de un Boleto de acceso dirigido exclusivamente al Servidor Financiero, sin control total global) suele resultar abismalmente más indetectable (evade los Logs) para los monitores de Defensa Centralizados, en contraposición al ataque del "Golden Ticket"?**
A) Porque el Silver Ticket borra físicamente los discos duros de la empresa cada 5 minutos.
B) Porque el Silver Ticket (Al falsificar un TGS directo a un servidor específico) saltea, ignora, y **NUNCA entra en contacto directo con el Controlador de Dominio (DC)** (El cual es el punto de vigilancia supremo donde el Blue Team audita todos los pases), mientras que el Golden Ticket es global y muy ruidoso.
C) Porque el Silver Ticket funciona utilizando Linux y no Windows.
D) Porque el Blue Team ignora deliberadamente a la cuenta de Servicio krbtgt.

---

> **¿Terminaste?**
> Buscá el archivo en la carpeta de evaluaciones para comparar tus resultados (Módulo 10 - Respuestas).

**➡️ Siguiente nota:** [[11 - Resumen]]
