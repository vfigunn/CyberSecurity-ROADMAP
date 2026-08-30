# 05 - Introducción a Active Directory (El Corazón Corporativo)

## 🎯 Objetivos
- Entender el problema logístico masivo de las empresas ("Workgroup" vs "Domain").
- Comprender qué es exactamente Active Directory (AD).
- Identificar por qué es el objetivo final de todos los ataques avanzados (Ransomware/APTs).

---

## 🧠 El Problema: El Mundo Aislado (Workgroup)

Imaginá que tu empresa tiene 5 empleados (y 5 computadoras con Windows normal).
El usuario "Juan" se sienta en su PC. Como vimos en los módulos anteriores, la computadora verifica la cuenta "Juan" buscando en su propia base de datos local (SAM de Windows o `/etc/shadow` en Linux). Todo funciona perfecto.

Pero tu empresa crece a **10.000 empleados** y 10.000 computadoras.
- Juan, de Marketing, tiene que usar la PC de Recepción porque su computadora se rompió.
- Intenta poner su nombre de usuario en la computadora de Recepción y la computadora dice: *"Acceso Denegado. Yo no tengo ningún usuario llamado Juan en mi base de datos local"*.
- El departamento de IT tendría que ir caminando computadora por computadora (las 10.000), creando manualmente el usuario "Juan" con su misma contraseña en cada una, para que Juan pueda loguearse donde quiera. Y si Juan quiere cambiar su contraseña el mes que viene, hay que repetir el proceso.
- Además, ¿cómo hacés para instalarle la actualización del Antivirus a 10.000 computadoras? ¿Vas a enviar a 50 técnicos con un Pendrive a caminar por todo el edificio?

Este sistema descentralizado (donde cada PC se manda sola) se llama **Workgroup (Grupo de Trabajo)**. Es insostenible para más de 15 personas.

---

## 👑 La Solución: Active Directory y El Dominio

Para solucionar el caos, Microsoft diseñó el concepto del **Dominio (Domain)**.

1. Tomás un servidor extremadamente poderoso (una súper computadora) y lo nombrás "El Rey" (**El Controlador de Dominio - Domain Controller / DC**).
2. Dentro de ese servidor, instalás el software estrella: **Active Directory (AD)**.
3. AD es una base de datos maestra universal (utilizando protocolos de red como LDAP) que contiene a las 10.000 computadoras y a los 10.000 usuarios de la empresa.
4. Por último, configurás a las 10.000 computadoras "esclavas" para que **se unan al Dominio**. En el momento que lo hacen, pierden su independencia. Le ceden todo el poder de decisión al Rey.

### El Nuevo Escenario con AD (Single Sign-On y Centralización)
- Juan se sienta en la PC de Recepción y tipea "Juan / Contraseña".
- La PC de Recepción ya no busca en su base local. La PC envía un mensaje secreto por el cable de red al Rey (Controlador de Dominio) diciendo: *"Ey, Controlador, ¿es correcta esta contraseña?"*.
- El Controlador revisa la base de datos central, verifica que es correcta, y le responde a la PC: *"Sí, dejalo pasar"*.
- Juan pudo entrar a una PC que jamás había usado, utilizando sus mismas credenciales corporativas (Single Sign-On).

---

## 💥 Por qué es el Paraíso de los Atacantes

En un entorno Workgroup, si un atacante te envía un correo trampa (Phishing) y hackea tu computadora, **solo hackeó tu computadora**. Para afectar a las 9.999 restantes, tiene que ir una por una, buscando vulnerabilidades.

En el entorno del **Dominio Active Directory**:
Si el atacante logra comprometer la computadora del Director del departamento de IT, y desde ahí logra robarse la "Llave del Rey" (Las credenciales de `Domain Admin` - Administrador de Dominio)... **Se acabó el juego**.
El atacante ahora controla Active Directory. En 5 segundos, con un clic, puede ordenar desde el Controlador de Dominio que se despliegue y se instale el virus Ransomware simultáneamente en las 10.000 computadoras de la empresa, apagando multinacionales enteras y pidiendo millones en rescate.

> **Regla de Ciberseguridad Ofensiva:** Hackear la computadora inicial es solo el 10% del trabajo. El 90% restante del objetivo del Red Team se llama *Movimiento Lateral*. Es saltar de computadora en computadora dentro de la empresa, robando credenciales cada vez más altas, hasta llegar al premio mayor: Tomar control del Controlador de Dominio de Active Directory.

---

## 📌 Must Know (Imprescindible)
- La inmensa diferencia estructural entre un **Workgroup** (PCs aisladas con bases locales) y un **Dominio** (PCs controladas desde un punto central mediante red).
- Saber qué es **Active Directory (AD)**: Una gigantesca base de datos centralizada y estructurada (Protocolo LDAP) que maneja identidades y permisos.
- Entender el concepto de **Domain Controller (DC)**: El servidor físico real en el cual está instalada y funcionando la base de datos de Active Directory.

---

## 🔄 Preguntas de repaso
1. Una startup pequeña de 10 personas utiliza computadoras Windows 11 normales conectadas a un router Wi-Fi común, sin ningún servidor dedicado. Cuando el empleado Pedro cambia su contraseña, lo hace desde Configuración > Cuentas en su propia máquina. ¿Esta red opera bajo la filosofía de "Domain" o "Workgroup"?
2. Si una empresa decide instalar un entorno corporativo Active Directory, ¿qué sucede lógicamente con la autoridad de validación de contraseñas de las computadoras cliente? ¿Siguen consultando el archivo `SAM` de la memoria local de la propia computadora o delegan esa tarea por red?
3. Analizando el impacto de un incidente: Un atacante compromete por completo una red Workgroup de 500 computadoras, y simultáneamente, otro atacante obtiene las credenciales de "Domain Admin" en una red Active Directory de 500 computadoras. ¿Por qué el atacante de AD tiene una ventaja táctica y un poder destructivo inmediato infinitamente superior al del atacante del Workgroup?

**➡️ Siguiente nota:** [[06 - Estructura Lógica (Dominios, Bosques y OUs)]]
