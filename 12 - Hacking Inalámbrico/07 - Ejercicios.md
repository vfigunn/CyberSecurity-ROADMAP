# 07 - Ejercicios del Módulo 12

## 📝 Instrucciones
Poné a prueba tu comprensión de la física y lógica detrás del estándar Wi-Fi (802.11) y cómo el Hacking Inalámbrico engaña a los usuarios de teléfonos móviles corporativos.

---

## 🧠 Ejercicios de Hacking Inalámbrico (Wi-Fi y RF)

1. **La Antena Sorda:**
   - Eres un Pentester junior que acaba de instalar Kali Linux en una vieja notebook. Corres a un cibercafé para interceptar las contraseñas de las personas. Inicias el programa `airodump-ng wlan0` y te das cuenta de que la pantalla solo captura los paquetes web de tu propia notebook y de nadie más en el local.
   - Sabiendo el nombre de la restricción técnica de la placa de red interna que los fabricantes imponen; ¿Por qué te ocurre esto y cuál es el "Modo" vital del que carece tu hardware y que te obliga a comprar una Antena USB externa (Ej. marca Alfa) compatible con inyección y escucha pasiva?

2. **El Fantasma de WEP:**
   - La historia cuenta que la seguridad WEP fue la peor burla criptográfica de los 2000. Un hacker, sentándose pasivamente en un banco de plaza frente a un local, podía deducir la contraseña mediante algoritmos estadísticos al cabo de 10 minutos (Inyectando IVs).
   - Explicá por qué en el protocolo actual y reinante (**WPA2**), la mera matemática deducible se extinguió, obligando así al Atacante contemporáneo a tener que depender (Llevar la pelea) 100% al factor del "Diccionario de la fuerza bruta" contra el archivo `.cap` que capturó.

3. **La Paciencia del Cazador:**
   - Una corporación usa `WPA2` y encienden todas sus máquinas a las 08:00 AM para empezar la jornada laboral. 
   - Llegas al estacionamiento de la empresa a las 11:30 AM e inicias tu escucha/radar (`airodump-ng`) apuntando fijo a la MAC del Router principal y esperando pacientemente que caiga un "Handshake" volador.
   - Si no realizaras NINGÚN otro ataque inyector adicional, ¿Por qué razón lógica podrías quedarte sentado y esperando hasta las 18:00 PM y aún así tu radar nunca grabará un "Handshake" del edificio, volviendo tu ataque pasivo completamente inútil?

4. **El Cañón Desautenticador:**
   - Comprendiendo el callejón sin salida del escenario anterior; tomas la decisión agresiva de usar tu cañón inyector `aireplay-ng`. Falsificas la MAC del Router Oficial y disparas 50 paquetes masivos de "Deauth" (Desautenticación) hacia el smartphone del Gerente Financiero que estaba en la sala de juntas navegando. 
   - Explicá, desde la perspectiva del celular de la víctima (Comportamiento autónomo automático de Reconexión), cómo este breve ataque disruptivo "ayuda o beneficia indirectamente" a tu otro programa espía (radar airodump-ng) que sigue en ejecución constante grabando el aire en segundo plano, consiguiéndote finalmente el archivo `.cap` deseado.

5. **El Fin de la Matemática (Evil Twin):**
   - Te robaste el `.cap` y descubres, procesándolo offline en tu GPU (`Hashcat`), que la contraseña corporativa es aleatoria y alfanumérica de 45 caracteres (Incrackeable en un millón de años). 
   - Sabiendo esto, pasás al Plan B: Decidís usar la Ingeniería Social Wi-Fi (Gemelo Malvado / Evil Twin) con un "Portal Cautivo" falso.
   - Escribí el escenario paso a paso. ¿Cómo utilizás el ataque de "Deauth" (Denegación Infinita) en combinación con tu segundo Router/Antena gratuita para engañar a los usuarios del edificio, forzándolos a que ellos mismos te tipeen felizmente esa misma contraseña de 45 caracteres en una página web visual que vos mismo controlas?

6. **BYOD y Sandboxing:**
   - Como consultor Red Team Mobile, le explicas al CEO de una empresa que su modelo de "Bring Your Own Device" (BYOD) es un infierno corporativo. Muchos de sus empleados jóvenes utilizan `Android` para trabajar, y constantemente instalan archivos `.apk` truchos descargados de páginas rusas y foros externos utilizando "Sideloading". 
   - Detallá por qué un malware que logre ejecutarse como administrador supremo (**Root/Superusuario**) en esos celulares Android corporativos, es la peor amenaza concebible para la directiva de OWASP Mobile Top 10, y cómo esto destroza absolutamente la barrera oficial del Sandboxing interno de Android frente a los archivos y BBDD bancarias.

---

## 🎯 Autoevaluación
Chequeá internamente que domines de forma absoluta el Ejercicio 1 (La enorme diferencia de foco, Modo Monitor y Hardware Externo para poder atrapar "aire") y el Ejercicio 5 (El Ataque Psicológico del Portal Cautivo: La única forma de sortear y obtener contraseñas masivas e incrackeables por diccionarios estándar en WPA2). 

**➡️ Siguiente nota:** [[08 - Evaluación]]
