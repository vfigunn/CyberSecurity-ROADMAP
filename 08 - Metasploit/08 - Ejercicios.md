# 08 - Ejercicios del Módulo 08

## 📝 Instrucciones
Poné a prueba tu comprensión técnica sobre la taxonomía del Hacking, las direcciones del tráfico de red, y la post-explotación con Metasploit. Muchos de estos escenarios son el día a día de un Pentester u Operador de Red Team.

---

## 🧠 Ejercicios de Lógica y Análisis (Explotación)

1. **La Taxonomía del Misil:**
   - Supongamos que descubrís un fallo (Bug) crítico en el software de la cámara web de una computadora, el cual te permite inyectar 10 líneas de código arbitrario (Buffer Overflow). Decidís crear un código minúsculo que ejecute el comando para borrar el disco duro (`del C:\*`).
   - Diferenciá en dos oraciones claras: ¿Qué pedazo exacto de todo este escenario es el "Exploit", y qué pedazo exacto es el "Payload"?

2. **Dilema del Tráfico (Evasión de Firewalls):**
   - El Servidor Web `Apache` de un Banco tiene un Firewall brutal que descarta y destruye cualquier conexión Inbound (Entrante) desde Internet hacia el servidor si no va dirigida estrictamente a los puertos 80/443. 
   - Sin embargo, el atacante logra explotar la página (ya que su misil entra por el puerto 80) e inyecta un Payload. Explicá cómo y por qué utilizar un Payload `Reverse Shell` hacia su puerto `4444` le otorga la conexión remota, eludiendo mágicamente el bloqueo perimetral del banco.

3. **Restricción de Equipaje (Staged vs Stageless):**
   - Estás auditando una vulnerabilidad que solo te deja subir (inyectar) 200 bytes de información en la memoria de la máquina, y falla si se pasa del límite. 
   - Sabiendo que el poderoso Troyano Meterpreter ocupa más de 1 Megabyte de peso. ¿Cómo logra Metasploit entregarte una consola Meterpreter completa en esa máquina? (Explicá el funcionamiento del Stager o Payload en "Fases" utilizando el símbolo `/`).

4. **Variables de Puntería:**
   - Has abierto `msfconsole` y cargaste tu exploit y payload con el comando `use`. Te disponés a configurar las direcciones de red para lanzar el ataque.
   - En Metasploit, si la IP de tu computadora (Atacante) es `192.168.1.5` y la IP de la Servidor a hackear (Víctima) es `10.0.0.50`, ¿qué valores exactos tenés que asignarle, usando la consola, a la variable global `RHOSTS` y a la variable local `LHOST` antes de disparar?

5. **El Fantasma en la Memoria:**
   - Un Antivirus tradicional (Windows Defender) basa gran parte de su seguridad en revisar el disco duro local cada vez que se escribe, modifica, o descarga un nuevo archivo `.exe`.
   - Tras explotar a la víctima con Metasploit, se despliega una consola remota de `Meterpreter`. Explicá detalladamente por qué el Antivirus queda totalmente ciego y no levanta ninguna alerta sobre este troyano sumamente peligroso, basándote en la forma nativa de inyección ("In-Memory Execution").

6. **El Túnel Interno (Pivoting):**
   - Acabas de obtener una shell (conexión) en el Servidor Web expuesto en la DMZ de la empresa. Corrés el comando de red y descubrís que ese servidor web tiene "dos caras" (dos tarjetas de red): Una conectada a Internet, y otra conectada físicamente a la Bóveda Secreta `10.50.0.X` a la que nadie puede acceder desde afuera.
   - Explicá cómo usar la técnica de Post-Explotación llamada "Pivoting" para lograr comprometer la bóveda, y por qué se dice que estás usando al Servidor Web como un "Enrutador Malicioso".

---

## 🎯 Autoevaluación
Es vital que comprendas el Ejercicio 2 (Por qué domina la Reverse Shell) y el Ejercicio 3 (El uso del Stager para evadir restricciones de tamaño). Dominar esto es comprender la lógica universal del despliegue de malware moderno a través de redes restringidas.

**➡️ Siguiente nota:** [[09 - Evaluación]]
