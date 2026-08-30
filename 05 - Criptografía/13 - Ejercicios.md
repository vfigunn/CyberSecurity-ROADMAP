# 13 - Ejercicios del Módulo 05

## 📝 Instrucciones
Poné a prueba tu comprensión de la criptografía y las matemáticas aplicadas a la ciberseguridad. Recordá que la mayoría de vulnerabilidades no vienen de que el algoritmo AES se rompa, sino de que el humano implementó mal el algoritmo correcto.

---

## 🧠 Ejercicios de Lógica y Análisis

1. **La Paradoja de la Clave Compartida:**
   - Eres un Agente infiltrado en territorio enemigo. Tienes un disco rígido robado de 1 TB con planos de misiles y debes encriptarlo rápidamente antes de que te descubran y escapar.
   - ¿Por qué utilizarías un algoritmo de Criptografía **Simétrica** (como AES) para esta tarea física (en reposo), en lugar de intentar cifrar el disco con un algoritmo **Asimétrico** (como RSA)?

2. **Engaño de Base64:**
   - Un desarrollador Junior te muestra su código. Dice: *"Para proteger los correos de los usuarios en la Base de Datos, inventé un script que transforma el texto a Base64 y luego lo invierte (lo escribe al revés). Es súper seguro porque nadie sabe que está al revés."*
   - Basándote en el concepto de "Encoding vs Encripción" y en el pilar de la *Seguridad por Oscuridad*, refutá técnicamente su argumento demostrándole por qué su sistema no brinda ningún tipo de Confidencialidad real ante un atacante.

3. **Verificación de Integridad:**
   - Te bajás el software de código abierto "VLC Media Player". El autor dice que el SHA-256 del instalador es: `8b1a...99z`.
   - Cuando lo descargás a tu PC y generás el hash, te da: `5c3e...11x`. 
   - ¿Cuáles son las **dos** conclusiones lógicas y técnicas que podés sacar sobre lo que le pasó a ese archivo durante su viaje por Internet, justificándolo con la regla de oro del "Efecto Avalancha"?

4. **Autenticidad (El Candado Inverso):**
   - Recibís un correo de Recursos Humanos diciendo que estás despedido. Adjunto viene un PDF firmado digitalmente.
   - Utilizás la **Llave Pública** de la oficina de Recursos Humanos para intentar desencriptar la "Firma Digital" del documento, y el proceso es **Exitoso**.
   - ¿Qué te demuestra matemáticamente este éxito rotundo respecto a la verdadera identidad (No Repudio) de la persona que creó esa firma, y qué llave debió utilizar obligatoriamente para crearla?

5. **Ataque de Diccionario vs Sal (Salt):**
   - Un cibercriminal descarga una base de datos con 10,000 hashes MD5. Su supercomputadora tiene "Rainbow Tables" con todos los hashes precalculados de las palabras del idioma inglés.
   - Sin embargo, descubre que la Base de Datos fue programada por un experto y todos los hashes están "Salados" (Salted) de forma única. 
   - Explicá paso a paso por qué el cibercriminal ya no puede usar su gigantesca tabla precalculada y por qué esto le costará años de procesamiento.

6. **El Fallo del Impostor (HTTPS):**
   - Te conectás a un Wi-Fi de un aeropuerto. Intentás entrar a tu HomeBanking. 
   - El hacker (conectado al mismo Wi-Fi) hace un ataque de Hombre-en-el-Medio (MitM) e intercepta tu tráfico, enviándote su **propia** Llave Pública generada en el momento.
   - ¿Qué documento tecnológico (y qué firma criptográfica adentro de ese documento) buscará tu navegador web (Chrome) para decidir si confía en esa llave pública, bloqueando así el ataque del hacker y mostrándote una pantalla roja?

---

## 🎯 Autoevaluación
Asegurate de dominar el Ejercicio 2 (Diferencia de Encoding vs Encripción) y el Ejercicio 4 (Mecánica de la Firma Digital). Si cruzás esos conceptos en la vida real, los sistemas que diseñes o audites tendrán brechas mortales.

**➡️ Siguiente nota:** [[14 - Evaluación]]
