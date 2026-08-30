# 05 - Seguridad Móvil (Conceptos Básicos)

## 🎯 Objetivos
- Entender el nuevo perímetro corporativo (BYOD - Bring Your Own Device).
- Conocer los dos ecosistemas (iOS vs Android) y el concepto de Jailbreak/Root.
- Dar un vistazo al modelo **OWASP Mobile Top 10**.

---

## 🧠 Concepto: La Computadora de Bolsillo

Históricamente, la seguridad del Pentesting Inalámbrico acababa cuando el atacante tomaba el control del Access Point (Router). Pero el vector de intrusión moderno de la Red Corporativa ha mutado.
Hoy los empleados de los bancos y empresas acceden a sus bases de datos desde sus Celulares, bajo las políticas **BYOD** (Trae tu propio dispositivo).
El atacante ya no intenta vulnerar el Firewall de la empresa. Intenta inyectar una aplicación maliciosa en el teléfono personal del Gerente, que luego se conectará a la red interna (Pivoting).

---

## 📱 Los Ecosistemas (Android vs iOS)

El Pentesting Móvil es un mundo de nicho absoluto (Diferente a la Web o Windows).

### Android (El Ecosistema Abierto)
- Basado en Linux.
- Permite "Sideloading" por defecto. El usuario puede bajarse una aplicación desde cualquier página web (un archivo `.apk` malicioso) e instalarlo sin pasar por el filtro de la Google Play Store.
- **Root (Superusuario):** Un atacante local o un malware busca hacer "Rooting", logrando el privilegio equivalente a `SYSTEM/Domain Admin` del teléfono, dándole control absoluto sobre toda la memoria en crudo de las otras aplicaciones del celular.

### iOS / Apple (El Jardín Vallado)
- Ecosistema hiper-restringido.
- Las aplicaciones corren en aislamiento total (Sandboxing extremo). Solo pueden instalarse descargándolas de la App Store oficial, la cual posee revisiones exhaustivas.
- **Jailbreak (Rompiendo la Jaula):** Es la táctica de hackeo donde, utilizando vulnerabilidades de bajo nivel o BootROM (Ej: `checkm8`), se le retiran las cadenas al sistema operativo de Apple, permitiendo que el usuario (o un atacante) adquiera nivel *Root* e instale herramientas fuera del entorno oficial y explore el sistema de archivos oculto. *(Por eso muchas aplicaciones bancarias detectan si el iPhone tiene Jailbreak y se bloquean automáticamente).*

---

## 🛠️ OWASP Mobile Top 10 (Los Peligros)

Cuando una corporación lanza su propia Aplicación Móvil de Banco en la tienda, el Red Team o Analista AppSec utiliza el estándar de **OWASP Mobile** para auditar su seguridad.

Los errores típicos de programación son similares al Web Hacking, pero aplicados al dispositivo local:

1. **Almacenamiento Inseguro (Insecure Data Storage):**
   - El programador de la app guarda la contraseña del banco del usuario o su Token de sesión JWT en una base local de SQLite del teléfono en texto plano crudo, asumiendo que "Nadie puede ver esos archivos". (Seguridad por oscuridad).
   - *El Ataque:* Si alguien le roba el teléfono y lo Rootea, extrae esa base de datos al instante y se lleva la clave plana.

2. **Criptografía Rota (Broken Cryptography):**
   - Usar protocolos de encriptación matemáticamente anticuados como `MD5` o el estándar `DES` dentro de la aplicación bancaria, en vez del obligatorio `AES-256`.

3. **Inyección de Código del Lado del Cliente (Client-Side Injection):**
   - La aplicación sufre de vulnerabilidades de SQLite Injection o Cross-Site Scripting (XSS local), inyectados a través de notificaciones push manipuladas o entradas de texto.

---

## 📌 Must Know (Imprescindible)
- Qué es la política **BYOD (Bring Your Own Device)**: La amenaza corporativa actual donde el perímetro de la empresa se fusiona con los teléfonos personales (y posiblemente infectados) de sus empleados.
- La distinción absoluta de los privilegios supremos en plataformas móviles, requiriendo **Root (En Android)** o aplicando tácticas de **Jailbreak (En iOS)** para escapar del Sandboxing/Caja de arena original y poder acceder a archivos ocultos (El equivalente a ganar Escalada de Privilegios Locales en Windows).
- La falla de la confianza local: La directiva OWASP de almacenamiento inseguro demuestra que el programador "Asume equivocadamente que el teléfono del usuario jamás caerá en manos enemigas o jamás será manipulado/rooteado", optando por almacenar llaves en texto plano en la memoria.

---

## 🔄 Preguntas de repaso
1. Diferenciá conceptualmente a nivel arquitectónico y corporativo por qué el ecosistema base de **Android** (Soportando la carga lateral "Sideloading" libre de instaladores `.apk` sin revisión técnica de fuentes terceras) sufre estadísticamente una superficie de riesgo (Ataque y Malware orgánico) mucho mayor frente al restrictivo "Sandboxing / Jardín Vallado" que Apple impone férreamente sobre los usuarios de **iOS**.
2. Estás realizando una auditoría (Pentesting Mobile) sobre la última actualización de una Billetera Virtual (App Financiera de celular). Al desensamblar e investigar el tráfico, descubrís que la App requiere conexión constante al backend de la corporación para validar transacciones. Sin embargo, al inspeccionar la memoria local cruda, descubrís que los programadores eligieron guardar el Token (Session VIP temporal) que autoriza esas transacciones escrito en una base `SQLite` en formato Texto Claro crudo en las entrañas de los archivos locales del celular, sin aplicar un cifrado propio como AES. ¿Qué directiva del ranking oficial OWASP Mobile Top 10 violaron de forma innegable e inmediata?
3. Sabiendo que los sistemas de telefonía restringen los alcances de lectura para aislar y blindar (Sandboxing) la visibilidad entre aplicaciones (Impidiendo que un simple juego pueda escanear las memorias de la app de tu Banco). Explicá y definí el concepto de Escalada Definitiva u obtención del Control Máximo requerido por los atacantes para burlar esta muralla en celulares (Mencionando obligatoriamente los nombres populares que estas prácticas reciben en los ecosistemas tanto de Apple como de Google).

**➡️ Siguiente nota:** [[06 - Laboratorio Teórico - Asaltando el Wi-Fi Corporativo]]
