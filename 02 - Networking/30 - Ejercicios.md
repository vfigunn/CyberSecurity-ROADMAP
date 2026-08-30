# 30 - Ejercicios del Módulo 02

## 📝 Instrucciones
Respondé a las siguientes preguntas de reflexión integrando los conocimientos de todo el módulo de Networking. Intentá no revisar tus notas (¡ni usar IA!) para resolverlos inicialmente.

---

## 🧠 Ejercicios de Reflexión

1. **El viaje del paquete (End-to-End):**
   - Tu computadora (`192.168.1.100`, con MAC `AA:AA:AA...`) envía un correo a un servidor SMTP de Microsoft (`20.10.15.5`, con MAC `BB:BB:BB...`). En medio del camino hay un Router de tu compañía (MAC interna `CC:CC...`, MAC externa `DD:DD...`).
   - Cuando el paquete sale de *tu computadora* hacia el Router, ¿Cuál es la IP Destino y cuál es la MAC Destino en ese primer salto?
   - Cuando el paquete sale *del Router* hacia internet, ¿Cambiaron la IP de Origen/Destino? ¿Cambió la MAC de Origen/Destino? Explicá.

2. **Diferenciación de Dispositivos:**
   - Explicá la diferencia entre un Switch y un Router usando una analogía del mundo real que no sea el sistema postal.
   - Si tuvieras que frenar a un atacante que intenta escanear (hacer *ping sweeps*) puertos de tus servidores, ¿configurarías una regla en el Switch o en el Firewall/Router? ¿Por qué?

3. **La Batalla L4 (TCP vs UDP):**
   - Has descubierto un software de control remoto malicioso (Malware de tipo Remote Access Trojan - RAT) instalado en una máquina de la empresa. El malware está diseñado para robar bases de datos (Gigas de archivos de texto) hacia un servidor en Rusia de forma segura (para que ningún dato se corrompa en el envío).
   - ¿El autor del malware probablemente programó la herramienta para usar TCP o UDP para la transferencia de la base de datos? Justificá utilizando las características del protocolo.

4. **Direccionamiento IPv4 e IPv6:**
   - Sos un ingeniero configurando los servidores web principales de tu empresa que deben ser accesibles desde internet para tus clientes. ¿Le vas a asignar a estos servidores IPs del rango `10.0.0.0/8` o `172.16.0.0/12`? ¿Por qué es un error fatal de diseño?
   - Si tu empresa decide migrar al protocolo IPv6 de manera nativa (sin usar túneles), ¿seguirás dependiendo del uso intensivo de NAT (Network Address Translation)? Explicá la razón.

5. **Resolución DNS y Puertos:**
   - Abrís tu navegador web y escribís `https://wikipedia.org`.
   - Escribí el flujo de eventos, indicando el *Protocolo* y el *Puerto (Well-Known)* que tu computadora usa primero para traducir el nombre, y qué *Protocolo/Puerto* usa en segundo lugar para descargar la página web segura.

6. **Seguridad Wi-Fi:**
   - Vas a una cafetería a trabajar. Tienen una red Wi-Fi gratuita abierta (sin contraseña requerida).
   - Desde la perspectiva del riesgo en la Capa 2 (Sniffing inalámbrico), ¿por qué es crítico que solo navegues en sitios que usen HTTPS, en lugar de sitios HTTP antiguos, en redes de este tipo? 
   - ¿Qué ataque (mencionado en el módulo) podría usar el dueño de la cafetería para robar tus contraseñas engañándote, y qué rol juega el servidor DHCP en dicho ataque?

---

## 🎯 Autoevaluación

Si pudiste responder y argumentar firmemente el Ejercicio 1 (diferencia L2/L3 en el viaje del paquete) y el Ejercicio 5 (secuencia DNS -> HTTPS + Puertos), estás en un excelente nivel de comprensión conceptual. 
Si tenés dudas con los puertos o TCP/UDP, repasá las Notas 16 y 17, ya que son el 50% de las preguntas clásicas de entrevistas técnicas en ciberseguridad.

**➡️ Siguiente nota:** [[31 - Evaluación]]
