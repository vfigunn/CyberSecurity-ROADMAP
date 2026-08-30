# 09 - Autenticación II (Kerberos y el Sistema de Tickets)

## 🎯 Objetivos
- Conocer Kerberos, el protocolo de autenticación por defecto (y más seguro) de Active Directory.
- Entender el complejo flujo de los Tickets (TGT y TGS).
- Descubrir los dos vectores de ataque mortales sobre Kerberos: Golden Ticket y Kerberoasting.

---

## 🧠 Concepto: El Parque de Diversiones (Tickets)

Como NTLM (La nota anterior) era lento, peligroso y vulnerable a *Pass-The-Hash*, Microsoft adoptó **Kerberos** en el año 2000. Se convirtió en el estándar absoluto de Windows.

Kerberos no funciona validando la contraseña cada vez que querés abrir una puerta. Funciona exactamente igual que un **Parque de Diversiones (Ej. Disney)**.

1. **La Boletería (TGT):** Cuando llegás a Disney, vas a la boletería principal, mostrás tu DNI y tu dinero (Tu contraseña). La cajera verifica quién sos y te entrega una "Pulsera Dorada Vip" (Ticket de Concesión de Tickets - **TGT**). Esa pulsera prueba, ante todos, que estás autorizado a estar en el parque.
2. **Los Juegos (TGS):** Entrás al parque. Ahora querés subirte a la Montaña Rusa (Servidor de Archivos). No llevás tu DNI ni tu dinero (tu contraseña) a la montaña rusa. Simplemente le mostrás la "Pulsera Dorada" (TGT) al encargado. Él revisa la pulsera y te da un boleto específico para subirte al juego (Ticket de Servicio - **TGS**).

---

## ⚙️ El Flujo Real de Kerberos en Windows

En Kerberos (Cuyo servicio corre en el Controlador de Dominio y se llama **KDC - Key Distribution Center**), las contraseñas casi no viajan por la red, todo se maneja con criptografía asimétrica y tickets.

**Fase 1: Logueo inicial (Buscando la Pulsera VIP)**
1. El usuario Juan prende su PC. Tipea su clave.
2. La PC de Juan se comunica con el Controlador de Dominio (El KDC / Boletería).
3. El KDC valida a Juan. El KDC le envía de vuelta a Juan el Ticket Dorado (**TGT** - *Ticket Granting Ticket*).
   - *Detalle crítico:* El TGT está fuertemente encriptado con la contraseña hiper-secreta de una cuenta llamada `krbtgt` que solo vive en el Controlador de Dominio. Juan no puede leer ni modificar su propio ticket, solo lo guarda en su memoria RAM para usarlo luego.

**Fase 2: Solicitando acceso (Subiendo al Juego)**
4. A las 3 horas, Juan quiere abrir un archivo del Servidor-Finanzas. 
5. Juan NO le manda su contraseña al servidor. 
6. Juan le envía su Pulsera Dorada (TGT) al Controlador de Dominio y le dice: *"Soy yo, Juan. ¿Me das un pase para ir al juego Servidor-Finanzas?"*.
7. El DC lo valida, y le devuelve un boleto específico de servicio (**TGS** - *Ticket Granting Service*).
8. Juan se da vuelta, y le entrega el Ticket TGS directamente al Servidor-Finanzas. El Servidor-Finanzas lo lee y lo deja entrar. **(La contraseña jamás estuvo involucrada en el acceso, la red operó pura y exclusivamente pasando Tickets de un lado a otro).**

---

## 💥 Vulnerabilidades de Kerberos (Los peores miedos)

Kerberos es matemáticamente brillante, pero ha sido atacado de forma ingeniosa y devastadora.

### 1. El Golden Ticket (Boleto Dorado)
Dijimos que el TGT (La pulsera VIP) está encriptada con el Hash de la cuenta súper secreta `krbtgt` alojada en el Controlador de Dominio.
Si un atacante logra ganar acceso como Administrador y robarse el hash de esa cuenta `krbtgt`... ¡Se convierte en el Dios absoluto de Disney! 
El atacante puede usar su computadora personal (su propia boletería) para empezar a imprimir y falsificar Pulseras Doradas (Golden Tickets) a nombre de "Presidente de la Empresa", y usarlas para acceder a cualquier servidor que desee. Este ataque garantiza persistencia invisible a largo plazo.

### 2. Kerberoasting
¿Te acordás del Ticket de Servicio (TGS) que se usa para entrar a un servidor específico (El boleto de la Montaña Rusa)?
Por un fallo de diseño antiquísimo, cuando el Controlador de Dominio emite ese TGS y te lo entrega, **una porción de ese ticket está encriptada usando el Hash de la contraseña del servidor (o del servicio) al que intentás acceder**.

**El ataque ofensivo de principiantes (muy efectivo):**
1. Un empleado común (sin privilegios de Admin) le solicita lícitamente al Controlador de Dominio (DC) decenas de Tickets TGS para distintos servicios en la red.
2. El empleado guarda los TGS (boletos de servicio) en su computadora, los exporta, y se los lleva a su casa en un pendrive (Offline).
3. El hacker llega a su casa, usa una herramienta de Cracking de Hashes pesada (Fuerza Bruta con Tarjetas Gráficas) e intenta romper/desencriptar la porción del TGS encriptada con la contraseña del servicio.
4. Si la contraseña del servicio era débil (ej. "admin123"), en 5 segundos el hacker rompió el ticket y obtuvo la contraseña real de la cuenta de servicio de Active Directory. Y lo hizo desde su casa, 100% invisible para el equipo de seguridad de la empresa. A esto se le llama **Kerberoasting**.

---

## 📌 Must Know (Imprescindible)
- En Kerberos, no verificás contraseñas constantemente, presentás **Tickets**.
- **TGT:** El Ticket Maestro / Pulsera VIP (Encriptado con la clave maestra `krbtgt`).
- **TGS:** El Ticket de Servicio (Usado para abrir la puerta de un servidor final).
- Conocer los conceptos de ataque **Golden Ticket** y **Kerberoasting** (Robo y crackeo offline de tickets TGS).

---

## 🔄 Preguntas de repaso
1. En la fase inicial de la autenticación de Kerberos, un usuario legítimo se comunica con el Key Distribution Center (KDC) y recibe un Ticket Granting Ticket (TGT). ¿Por qué el usuario en su computadora personal no puede leer, modificar o falsificar el contenido interno de este ticket que acaba de recibir? *(Pista: Pensá con qué llave/contraseña está sellado ese ticket).*
2. En el ataque conocido como Golden Ticket, el atacante logró obtener privilegios máximos, extrajo un Hash ultra crítico de la red y comenzó a "falsificar" sus propios tickets TGT para engañar a los servidores. ¿A qué cuenta del sistema (cuál es el nombre del usuario de Active Directory) pertenece el hash que el atacante tuvo que haber robado?
3. Un atacante dentro de la red corporativa descarga a su computadora local un Ticket de Servicio (TGS) de la cuenta de "Microsoft SQL Database" y se lo lleva offline a una supercomputadora para crackearlo mediante Fuerza Bruta. Sabiendo cómo funciona el protocolo Kerberos, ¿qué es exactamente lo que el atacante va a obtener/averiguar el momento que termine de "romper" o "crackear" matemáticamente ese ticket TGS?

**➡️ Siguiente nota:** [[10 - Búsqueda y Protocolos (LDAP)]]
