# 02 - Tipos de Conexión (Bind Shell vs Reverse Shell)

## 🎯 Objetivos
- Conocer qué es una Shell en ciberseguridad (La terminal).
- Entender el bloqueo perimetral que realizan los Firewalls (Inbound vs Outbound).
- Aprender la táctica maestra del Hacking para evadir defensas: La Reverse Shell.

---

## 🧠 Concepto: Obteniendo la Terminal (Shell)

En películas de Hollywood, el hackeo termina cuando sale el cartel de "Access Granted". En la realidad, el hackeo exitoso termina cuando el atacante obtiene una **Shell**.
Una Shell no es más que la Terminal (Bash en Linux, CMD/PowerShell en Windows) de la computadora de la víctima, pero controlada desde la casa del hacker.

Cuando configuramos el Payload en Metasploit, tenemos dos formas arquitectónicas gigantes de ordenarle a la víctima cómo queremos que nos entregue su Terminal: **Bind o Reverse**.

---

## 🚪 1. Bind Shell (La Conexión Clásica - Y Obsoleta)

"Bind" significa "Atar" o "Amarrar". 
**El Flujo:**
1. El Hacker dispara el Exploit con un Payload tipo Bind Shell.
2. El Payload se ejecuta en el servidor de la víctima.
3. El Payload le dice a la computadora de la víctima: *"Ey, abrí el puerto 4444 de tu máquina en secreto, y quedate escuchando. Al primero que se conecte a ese puerto, entregale el control de tu consola Bash"*.
4. El Hacker (desde su casa), inicia una conexión **Hacia Adentro (Inbound)**, conectándose directamente a la IP de la víctima por el puerto 4444.
5. El Hacker entra y obtiene la consola.

### ¿Por qué la Bind Shell fracasa en el 99% de los casos?
**Por el Firewall Perimetral corporativo.** 
Las empresas configuran su Firewall con la regla de oro: *"Bloquear todo el tráfico entrante (Inbound) que no sea hacia los puertos públicos Web (80/443)"*.
Aunque el Exploit logre infiltrarse por la web e instalar la Bind Shell en el puerto 4444, cuando el Hacker intente conectarse desde su casa a ese puerto 4444, el Firewall corporativo le bloqueará el paso y la conexión morirá.

---

## 🔄 2. Reverse Shell (El Engaño Maestro)

Para saltarse el Firewall que bloquea la conexión "entrante", la industria del Red Team creó el estándar absoluto: **La Conexión Inversa (Reverse Shell)**.

Acá invertimos los roles. No es el Hacker el que va a golpear la puerta de la víctima. ¡Es la Víctima la que va a llamar al Hacker!

**El Flujo:**
1. El Hacker configura su propia computadora (en su casa) para quedarse escuchando en un puerto local (Ej: 4444).
2. El Hacker dispara el Exploit con el Payload Reverse Shell. Este Payload lleva programado adentro la Dirección IP pública de la casa del Hacker.
3. El Payload se ejecuta en la víctima.
4. El Payload le dice a la computadora víctima: *"¡Llamá inmediatamente a la IP del Hacker al puerto 4444, e inyectá tu consola Bash a través del tubo hacia su pantalla!"*.
5. La computadora de la víctima realiza una conexión **Hacia Afuera (Outbound)** para buscar al hacker.

### ¿Por qué la Reverse Shell evade los Firewalls?
Porque el 90% de los Firewalls corporativos son paranoicos con el tráfico Entrante (Inbound), pero son increíblemente permisivos con el tráfico Saliente (Outbound). 
Al fin y al cabo, los empleados de la empresa necesitan navegar por Internet (tráfico saliente). El Firewall de la empresa ve que un servidor interno (la víctima) está intentando conectarse hacia una IP externa, asume que es una conexión web legítima, y lo deja salir, entregándole la consola al hacker.

---

## 📌 Must Know (Imprescindible)
- **Bind Shell:** El atacante se conecta Hacia la máquina víctima. Fracasa si el Firewall bloquea puertos entrantes.
- **Reverse Shell:** La máquina víctima se conecta Hacia la máquina del atacante. Evade el Firewall explotando la confianza del tráfico saliente.
- Saber que el 99% del uso de Metasploit y Pentesting moderno se realiza a través de Payloads **Reverse**.

---

## 🔄 Preguntas de repaso
1. Estás auditando (Pentesting) el servidor web de una corporación militar que posee un Firewall ultra-rígido. Su regla es explícita: "Bloquear absolutamente todas las conexiones Inbound (entrantes) hacia cualquier puerto, excepto el puerto TCP 443". Si lográs explotar una vulnerabilidad y ejecutás un payload `Bind_TCP` escuchando en el puerto local 9999 de la víctima, ¿podrás obtener control remoto del servidor? Justificá.
2. Basándote en el escenario anterior, el Firewall corporativo permite que todos los servidores salgan a Internet (Outbound) libremente para poder descargar actualizaciones. ¿Por qué configurar un payload `Reverse_TCP` hacia la IP de tu computadora (Atacante) tendría éxito burlando la postura defensiva del Firewall perimetral?
3. En el caso de una Reverse Shell, ¿quién es el que debe tener un puerto abierto, público y en modo "escucha" (Listening) esperando pacientemente a recibir la conexión para que el ataque funcione? ¿La víctima o el atacante?

**➡️ Siguiente nota:** [[03 - Tipos de Payloads (Staged vs Stageless)]]
