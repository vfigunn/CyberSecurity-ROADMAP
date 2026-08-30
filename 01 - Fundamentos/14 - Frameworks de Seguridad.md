# 14 - Frameworks de Seguridad

## 🎯 Objetivos
- Entender qué es un Framework (marco de trabajo) en ciberseguridad.
- Conocer los tres frameworks más influyentes (NIST CSF, CIS Controls, ISO 27001).
- Comprender la diferencia entre un framework de control, uno programático y uno de riesgo.

---

## 🧠 Concepto

La ciberseguridad es demasiado compleja para que una organización la implemente adivinando o recordando qué cosas proteger. 

Un **Framework de Seguridad** es una estructura teórica, un conjunto de buenas prácticas, normas y directrices que ayudan a las organizaciones a gestionar su [[07 - Riesgo|Riesgo]] de ciberseguridad. En palabras simples: **Es un "mapa" o "receta" escrita por expertos que te dice paso a paso qué cosas deberías estar haciendo para protegerte.**

En lugar de inventar la rueda (y olvidarte de asegurar un servidor), usás un framework como guía y lista de verificación.

---

## 🏛️ Los Tres Grandes Frameworks

Aunque hay decenas de frameworks (y los estudiaremos a fondo en el [[22 - GRC|Módulo 22 (GRC)]]), para los fundamentos debes conocer los tres más populares:

### 1. NIST Cybersecurity Framework (NIST CSF)
Creado por el Instituto Nacional de Estándares y Tecnología de EE. UU. (NIST). Es probablemente el framework más popular y fácil de entender a nivel ejecutivo y técnico.
El NIST CSF no te dice *cómo* instalar un antivirus, te dice *qué* funciones clave debe tener tu programa de seguridad. Se divide en cinco (ahora seis en su versión 2.0) funciones principales (Core Functions):

1. **Govern (Gobernar):** Las políticas de alto nivel, la estrategia y el compromiso de la gerencia con la seguridad.
2. **Identify (Identificar):** Saber qué tenés. Inventario de equipos, software, datos críticos y riesgos. (No podés proteger lo que no sabés que tenés).
3. **Protect (Proteger):** Implementar [[08 - Controles de Seguridad|Controles]] preventivos. (Ej. Firewalls, capacitaciones, contraseñas).
4. **Detect (Detectar):** Saber cuándo estás siendo atacado. (Ej. Monitoreo, SOC, alarmas).
5. **Respond (Responder):** Qué hacer cuando el ataque es detectado para contenerlo. (Plan de Respuesta a Incidentes).
6. **Recover (Recuperar):** Cómo volver a la normalidad después del ataque. (Backups, comunicación pública).

### 2. CIS Controls (Center for Internet Security)
A diferencia del NIST CSF (que es estratégico), el CIS es altamente táctico y técnico. 
Es una lista priorizada de 18 controles (antes eran 20) que te dicen exactamente qué hacer en los sistemas. 

Se dividen en Implementation Groups (IG1, IG2, IG3) según el tamaño de la empresa. 
- *Ejemplo:* El Control CIS 1 es "Inventario de activos de hardware". El Control CIS 4 es "Configuración segura de activos y software".
- Si una empresa pequeña solo aplica los controles básicos del CIS (IG1), elimina el 80% de los ciberataques más comunes (como el ransomware no dirigido).

### 3. ISO/IEC 27001
Es una norma internacional certificable. A diferencia de NIST y CIS, una empresa puede contratar a un auditor externo para que verifique que cumplen con ISO 27001 y obtener un certificado oficial (muy valioso para conseguir grandes clientes o contratos gubernamentales).
- Se enfoca fuertemente en establecer y mantener un **SGSI (Sistema de Gestión de Seguridad de la Información)**.
- Es muy pesado en documentación, políticas y auditorías.

---

## ❓ ¿Por qué importa?

La seguridad no es solo apretar botones en una terminal. Si sos un consultor o trabajás en defensa, vas a usar frameworks constantemente para medir la madurez (maturity) de la empresa. 
- "Nuestra detección (Detect) es fuerte porque compramos un EDR nuevo, pero nuestra recuperación (Recover) es débil porque los backups fallaron la última vez." Esto es lenguaje de NIST CSF.

---

## 🌎 Uso conjunto

Los frameworks no compiten, se complementan. Una empresa madura suele usar:
- **ISO 27001** para estructurar su programa y obtener el certificado.
- **NIST CSF** para comunicarse con la junta directiva mostrando en qué funciones están fuertes o débiles.
- **CIS Controls** para darle tareas técnicas y específicas a los ingenieros de sistemas.

---

## ❌ Errores comunes

- **Tratar el framework como un fin en sí mismo:** A esto se le llama "seguridad basada en compliance (cumplimiento)". La empresa se enfoca solo en tildar las casillas del framework para pasar una auditoría, pero los sistemas reales siguen siendo vulnerables. El cumplimiento no garantiza la seguridad.

---

## 📌 Must Know (Imprescindible)
- Qué es un Framework de Seguridad.
- Las 6 funciones centrales del NIST CSF v2.0 (Govern, Identify, Protect, Detect, Respond, Recover).

## 💡 Good to Know (Bueno saberlo)
- El gobierno suele imponer frameworks obligatorios para ciertas industrias mediante leyes. Por ejemplo, PCI-DSS (Payment Card Industry Data Security Standard) es obligatorio en todo el mundo para cualquier empresa que procese tarjetas de crédito.

---

## 📝 Para recordar
> "La seguridad es un proceso, no un producto". Los frameworks son los manuales de instrucciones que te ayudan a diseñar, implementar y mantener ese proceso a lo largo del tiempo.

---

## 🔄 Preguntas de repaso
1. Una empresa compra un nuevo servidor de copias de seguridad (Backups) para poder restaurar datos en caso de un ataque de ransomware. ¿En qué función del NIST CSF encaja esta compra?
2. Si un equipo técnico necesita una lista concreta, técnica y priorizada de cosas para configurar en los servidores, ¿qué framework (de los tres mencionados) sería el más adecuado para empezar?
3. ¿Por qué una empresa podría elegir implementar ISO 27001 en lugar de (o además de) NIST CSF?

**➡️ Siguiente nota:** [[15 - Laboratorio]]
