# 09 - Defensa en Profundidad (Defense in Depth)

## 🎯 Objetivos
- Comprender el concepto de Defensa en Profundidad y su analogía con un castillo medieval.
- Entender por qué un solo control de seguridad nunca es suficiente.
- Aplicar el concepto mediante el uso de múltiples capas de controles.

---

## 🧠 Concepto

La **Defensa en Profundidad (Defense in Depth - DiD)** es un enfoque estratégico en el que se implementan múltiples capas de [[08 - Controles de Seguridad|Controles de Seguridad]] sucesivas para proteger la información.

El principio central es simple: **Asumimos que cualquier control individual, por muy bueno que sea, eventualmente fallará o será evadido**. Si un atacante supera una capa de defensa, debe encontrarse inmediatamente con otra capa diferente.

### La Analogía del Castillo Medieval
Es la analogía clásica para explicar la DiD. Un rey no protegía su oro solo poniendo una puerta fuerte. Utilizaba:
1. Un foso con cocodrilos (Seguridad Perimetral).
2. Un puente levadizo (Control de Acceso).
3. Murallas altas (Firewalls).
4. Guardias patrullando (Sistemas Detectivos / IDS).
5. Puertas internas cerradas con llave (Segmentación de red).
6. La bóveda final (Cifrado de datos).

Si el atacante cruza el foso, todavía tiene que trepar la muralla. Si trepa la muralla, tiene que evitar a los guardias. Ningún control por sí solo detiene a todos, pero en conjunto hacen que el ataque sea extremadamente difícil, costoso y lento, dándole tiempo a los defensores para reaccionar.

---

## 🏗️ Las Capas de la Defensa en Profundidad

La DiD moderna se suele visualizar como capas concéntricas (como una cebolla), donde el dato crítico está en el centro:

1. **Datos (Data):** La capa más profunda. Controles: [[06 - Cryptography/01 - Conceptos Básicos|Cifrado]] en reposo, Hashing, control de acceso a archivos.
2. **Aplicación (Application):** Controles: Web Application Firewall (WAF), validación de entradas, prácticas de código seguro (DevSecOps).
3. **Host / Endpoint:** Las computadoras y servidores. Controles: Antivirus, EDR, parches del sistema operativo, deshabilitar servicios innecesarios.
4. **Red Interna (Internal Network):** Controles: Segmentación de red (VLANs), IDS interno, NAC (Network Access Control).
5. **Perímetro de Red (Perimeter):** La frontera con internet. Controles: Firewalls externos, VPNs, sistemas anti-DDoS.
6. **Seguridad Física (Physical):** Controles: Guardias, cámaras, cerraduras de centros de datos.
7. **Políticas, Procedimientos y Concientización (Policies & Procedures):** La capa que envuelve a todas. Entrenamiento a empleados (para que no caigan en phishing) y políticas de respuesta a incidentes.

---

## ❓ ¿Por qué importa?

En la ciberseguridad, el atacante tiene una ventaja asimétrica: solo necesita encontrar **una** vulnerabilidad, **un** error, **una** contraseña débil para entrar. El defensor debe proteger **todo** todo el tiempo.

La Defensa en Profundidad es la forma de equilibrar la balanza. Al forzar al atacante a superar múltiples obstáculos diferentes (técnicos, administrativos y físicos), se aumenta la probabilidad de que cometa un error, haga ruido y sea detectado.

---

## 🌎 Aplicación en el mundo real

Imaginá un ataque de Ransomware que entra por un correo de Phishing:

1. **Capa Administrativa:** El empleado fue entrenado pero cae igual (el control falla).
2. **Capa Perimetral (Filtro de Email):** El filtro antispam no reconoce el malware y deja pasar el correo (el control falla).
3. **Capa Host (Antivirus):** El empleado descarga el archivo. El Antivirus tradicional no lo detecta porque es un Zero-Day (el control falla).
4. **Capa de Aplicación/Host (Permisos):** El malware intenta instalarse, pero el empleado no tiene permisos de Administrador en su PC. El malware *no puede* ejecutarse. **(¡El control funciona y detiene el ataque!)**

Gracias a la Defensa en Profundidad, el fallo de los tres primeros controles no resultó en un desastre, porque el cuarto control actuó como red de seguridad.

---

## ❌ Errores comunes

- **"El modelo de caramelo":** Es el opuesto a DiD. Se refiere a tener un perímetro duro (un Firewall súper caro) pero un interior blando y pegajoso (una vez que el atacante pasa el firewall, no hay seguridad interna, las contraseñas son débiles y todo es accesible).
- **Usar los mismos controles en cada capa:** Si usas el mismo software antivirus en el filtro de correo y en los endpoints, un malware que burle uno, burlará el otro. La DiD requiere controles **diversos**.

---

## 📌 Must Know (Imprescindible)
- Concepto de Defensa en Profundidad (asumir que los controles fallarán y tener respaldos).
- Conocer algunas de las capas principales (Perímetro, Host, Datos, Físico).

## 💡 Good to Know (Bueno saberlo)
- La Defensa en Profundidad no solo frena ataques, también compra tiempo. Cuantas más capas tenga que superar el atacante, más tiempo tienen los controles detectivos (como un [[14 - SOC & SIEM/01 - Qué es un SOC|SOC]]) para descubrirlo y expulsarlo antes de que robe los datos.

---

## 📝 Para recordar
> "La seguridad no es un producto que compras, es un proceso. Y ese proceso requiere redundancia. Si tu seguridad depende de que un solo sistema o una sola persona no se equivoque nunca, estás en peligro."

---

## 🔄 Preguntas de repaso
1. Explicá cómo el concepto del "castillo medieval" se aplica a la seguridad de una base de datos corporativa.
2. ¿Por qué el modelo de seguridad de "perímetro duro, interior blando" es peligroso en la actualidad?
3. En el ejemplo del ataque de ransomware de esta nota, ¿qué capa de la Defensa en Profundidad salvó finalmente a la empresa?

**➡️ Siguiente nota:** [[10 - Attack Surface]]
