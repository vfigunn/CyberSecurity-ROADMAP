# 04 - Ataques de Denegación (Deauth) y Evil Twin

## 🎯 Objetivos
- Comprender que no todo se trata de romper la clave oficial.
- Aprender cómo funciona el "Bloqueo Ciego" Wi-Fi (DoS por Deauth).
- Conocer la peor amenaza para usuarios móviles (El Portal Cautivo / Gemelo Malvado).

---

## 🧠 Concepto: Cuando la contraseña es invulnerable

Si la contraseña del Wi-Fi Corporativo es de 40 caracteres con símbolos (Imposible de crackear con diccionarios y Aircrack-ng), el atacante cambia su estrategia. 
En lugar de atacar la matemática del Router, **ataca la psicología y confianza del Usuario.**

---

## 🚫 Denegación de Servicio Wi-Fi (Deauth DoS)

En la nota anterior, usamos un Paquete de Deauth (Desautenticación) solo 1 segundo para forzar un reinicio y capturar un Handshake.
Pero una falla gravísima del estándar 802.11 es que los paquetes Deauth (La orden que el Router da para desconectar a una máquina) viajan **Sin Encriptar** en el aire. Cualquiera puede falsificarlos masivamente.

Si el Hacker de afuera agarra su antena y dispara *miles* de paquetes de Deauth por segundo a todas las direcciones MAC de la empresa, ocurre un **Denial of Service (DoS) Masivo**.
Ningún empleado va a poder conectarse al Wi-Fi de la empresa. Literalmente, el hacker bloquea el aire. La empresa entra en caos y desesperación porque el Wi-Fi se corta a cada segundo.
*(Este ataque se usa frecuentemente como preparación psicológica para el verdadero golpe).*

---

## 😈 Evil Twin (El Gemelo Malvado)

El Evil Twin es la trampa perfecta. El hacker aprovecha la desesperación del usuario.

**El Flujo del Ataque (Ingeniería Social Inalámbrica):**
1. La empresa tiene una red llamada *"Oficina-Segura"*.
2. El Hacker arranca un DoS masivo de Deauth (Como vimos arriba). Ningún empleado de la oficina tiene Internet.
3. El Hacker enciende una Segunda Antena propia en su computadora (o usando la suite automatizada `WifionIce` o herramientas como `Wifiphisher`). 
4. El Hacker crea un **Punto de Acceso Falso (Fake AP)** y le pone de nombre público: *"Oficina-Segura"*. Como la antena del Hacker está más cerca o tiene más potencia, los celulares de la oficina intentan conectarse a esta nueva red.
5. El Hacker no le puso clave (Es abierta). El celular del empleado entra gratis, pero **NO tiene Internet**.
6. En cambio, cuando el empleado abre el navegador Chrome o Safari, el Hacker lo redirige (Redirección DNS local) a una página falsa gigante e hiperrealista con el Logo de la Corporación.
7. La página dice: *"Estimado empleado, el Router Central tuvo un fallo. Por favor, introduzca su Contraseña Oficial del Wi-Fi a continuación para reiniciar el firmware y devolverle su conexión a Internet"*. (Esto se conoce como Portal Cautivo o Captive Portal).
8. El empleado, apurado por seguir trabajando, teclea su contraseña de Wi-Fi hiper-compleja en la página del hacker. ¡El Hacker la recibe en texto plano en su terminal de Linux y se retira con éxito!

*(El Evil Twin no solo roba claves Wi-Fi, puede configurarse para mostrar la página de logueo de Office365 o Gmail, y robar las credenciales globales del empleado).*

---

## 📌 Must Know (Imprescindible)
- **Ataque de Deauth (Denegación):** Abuso pasmosamente fácil y destructivo. Se falsifican paquetes de "Desconexión" en bucle para aislar permanentemente y derribar el tráfico de una red completa de la cual el atacante ni siquiera tiene la contraseña. (Solo las redes WPA3 o el estándar protegido 802.11w previenen esto, pero son raros aún).
- **Evil Twin (Gemelo Malvado):** La combinación táctica de Redes e Ingeniería Social. Clonar el nombre de la red legítima, aislar a los clientes, forzarlos a conectar al equipo atacante y desplegar un "Portal Cautivo" falso en sus navegadores (Phishing local) exigiendo sus credenciales.

---

## 🔄 Preguntas de repaso
1. Analizando la estructura del protocolo estándar Wi-Fi (802.11 b/g/n sin protecciones extra), explicá la gravísima vulnerabilidad estructural (falta de encriptación) en los "Paquetes de Gestión / Management Frames" que permite a un Analista ofensivo disparar un Ataque DDoS de Desautenticación Masivo, expulsando a todos los empleados del corporativo aunque el hacker no posea la clave WPA2 original para iniciar sesión.
2. Basándote en el comportamiento nativo y "Tonto" de tu Teléfono Celular, el cual intenta reconectarse por costumbre a las redes conocidas (Aquellas memorizadas bajo nombres/SSIDs como "Starbucks_Guest" o "Aeropuerto"): Si un Red Teamer levanta un Punto de Acceso malicioso gratuito con exactamente el mismo nombre de la red del aeropuerto pero con una señal de Antena 50% más fuerte, ¿Qué comportamiento automático realizará el celular del pasajero y cómo este fenómeno sustenta la base del ataque "Gemelo Malvado" (Evil Twin)?
3. A pesar de lograr aislar y forzar a que la víctima de Ingeniería Social se conecte a tu antena gratuita o "Rogue AP / Gemelo Malvado", te encontrás con un bloqueo lógico: La víctima aún no ha entregado ninguna información secreta. Describe cuál es el paso final indispensable (Relacionado al "Portal Cautivo" / Engaño web de Phishing) que debe configurar el atacante a nivel de servidor local DNS para completar el robo de la contraseña original de WPA2 del corporativo.

**➡️ Siguiente nota:** [[05 - Seguridad Móvil (Conceptos Básicos)]]
