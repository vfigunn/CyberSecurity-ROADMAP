# 07 - Persistencia Absoluta (Golden y Silver Tickets)

## 🎯 Objetivos
- Entender el problema de perder el acceso al Controlador de Dominio.
- Conocer la cuenta maestra de la Criptografía (krbtgt).
- Aprender cómo un Golden Ticket le permite al Hacker entrar a la red eternamente, sin importar si los administradores cambian sus contraseñas.

---

## 🧠 Concepto: La Fábrica de Billetes

En las notas pasadas, el Hacker robó el `NTDS.dit`. Pero si el Blue Team de la empresa descubre el robo de contraseñas, al día siguiente obligarán a todos los empleados y administradores a **cambiar sus contraseñas**.
Cuando las contraseñas cambian, los Hashes robados por el Hacker ya no sirven. El Hacker pierde su Imperio.

Para evitar esto, el Hacker aplica la **Persistencia de Dominio Absoluta**.

Volvamos a la metáfora del Cine (Kerberos). El Controlador de Dominio (DC) es el "Boletero" que te entrega el Ticket de Entrada.
¿Cómo hace el Controlador de Dominio para sellar matemáticamente esos Tickets para que nadie los falsifique?
Utiliza la contraseña de una **Cuenta Especial Invisible** que vive por defecto en todos los Active Directory del mundo: **La cuenta `krbtgt`** (Key Distribution Center Service Account).

La cuenta `krbtgt` es la máquina de imprimir billetes. Nadie puede iniciar sesión con ella (está deshabilitada), pero su "Hash de Contraseña" es la tinta criptográfica que el DC usa para validar todos los Tickets TGT de la empresa.

---

## 🎫 El Golden Ticket (Boleto Dorado)

Cuando el Hacker hizo el ataque `DCSync` y se robó la base de datos `NTDS.dit`, el Hacker **se robó el Hash de la cuenta `krbtgt`**.
Es decir, el Hacker se robó la Máquina Impresora de Billetes Oficial de la empresa y se la llevó a su casa.

**El Flujo del Ataque (Meses después):**
1. La empresa cambia todas sus contraseñas. El Blue Team cree que está a salvo.
2. El Hacker (en su casa), abre la herramienta `Mimikatz`.
3. El Hacker introduce en Mimikatz el Hash de la cuenta `krbtgt` que se robó hace meses, y le dice: *"Fabricame un Boleto Maestro (TGT) a nombre de Juan, que dure 10 años, y decile al boleto que Juan es el CEO Administrador Supremo"*.
4. `Mimikatz` genera el Golden Ticket. Es 100% perfecto matemáticamente, porque fue sellado con la "Tinta oficial" de la cuenta `krbtgt`.
5. El Hacker inyecta este Boleto Dorado en su memoria (Pass-The-Ticket) y entra a la red del banco de forma directa.
6. El Active Directory lee el Boleto Dorado, revisa el sello matemático, comprueba que es perfecto, y le da al Hacker acceso total a la empresa, sin jamás haberle pedido una contraseña, y sin importar si Juan cambió su clave 50 veces.

*(La única forma que tiene el Blue Team de mitigar esto y matar el Boleto Dorado, es forzar un Doble Reseteo manual de la contraseña invisible de la propia cuenta `krbtgt` en el servidor, invalidando así la tinta matemática antigua que tiene el Hacker en su casa).*

---

## 🥈 El Silver Ticket (Boleto Plateado)

Es el hermano menor del Golden Ticket.
- El **Golden Ticket** se falsifica con la clave del `krbtgt` y te da acceso a **TODA la empresa** (Falsificás el TGT Maestro).
- El **Silver Ticket** se falsifica con el Hash de Contraseña de **Un Solo Servidor** (Ej. Te robaste la contraseña del Servidor de Base de Datos Financiero). 
Con un Boleto Plateado (Falsificación del TGS), no hackeás la empresa entera, pero te fabricás pases VIP infinitos para entrar específicamente a ese Servidor Financiero.
La ventaja para el Atacante es que el Silver Ticket es aún más indetectable, porque como es un pase directo a una sola sala de cine, el Hacker nunca necesita hablar con el Boletero (DC). El Controlador de Dominio nunca registrará ningún log en su Firewall de que el hacker entró al sistema.

---

## 📌 Must Know (Imprescindible)
- La cuenta **`krbtgt`**: Es el componente más crítico de Active Directory. Su Hash (contraseña) se usa para firmar/encriptar matemáticamente todos los TGTs.
- **Golden Ticket:** Es el ataque definitivo de *Persistencia de Dominio*. Se realiza luego de haber comprometido al DC y haber robado el hash de `krbtgt`. Permite al atacante falsificar TGTs válidos para siempre, sobreviviendo al reseteo de contraseñas de los usuarios.
- **Silver Ticket:** Es la falsificación de un TGS (Ticket de un solo servicio). Permite persistencia sobre un único servidor específico sin generar ruido ni registros en los logs del Controlador de Dominio principal.

---

## 🔄 Preguntas de repaso
1. Tras comprometer exitosamente a la corporación, un grupo atacante genera un Ticket Kerberos (TGT) forjado con una vida útil de 10 años (Pass-The-Ticket), garantizando su re-ingreso perpetuo a todos los sistemas incluso si los empleados cambian sus credenciales diarias. ¿Cuál es el nombre de esta táctica y cómo se llama la cuenta especial "oculta" del Controlador de Dominio de Microsoft cuyo Hash debieron robar previamente para poder firmar y legitimar matemáticamente esa falsificación?
2. El equipo defensivo (SOC) acaba de descubrir la intrusión y se da cuenta de que el atacante (Red Team) extrajo la Base de Datos `NTDS.dit`. Inmediatamente, la directiva del Blue Team ordena forzar el reseteo de absolutamente TODAS las contraseñas de los 15.000 empleados humanos y administradores del banco. ¿Por qué esta colosal medida de seguridad masiva fracasará en detener el re-ingreso del atacante si olvidan específicamente resetear la cuenta `krbtgt` dos veces?
3. Un atacante se roba únicamente el Hash (Contraseña) de un Servidor de Carpetas Compartidas (`Servidor-Archivos-01`). Procede a forjar un Ticket específico (TGS) firmado con ese Hash y lo inyecta en su memoria, permitiéndole entrar a las carpetas infinitamente sin interactuar ni pedirle nunca permisos al Controlador de Dominio central. Sabiendo que el alcance de este ticket falsificado es minúsculo (está limitado a una sola máquina destino), ¿qué nombre recibe esta variante de persistencia y por qué es extremadamente difícil de detectar en los logs de auditoría general (El registro del DC)?

**➡️ Siguiente nota:** [[08 - Laboratorio Teórico - Destruyendo el Bosque]]
