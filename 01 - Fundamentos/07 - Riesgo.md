# 07 - Riesgo (Risk)

## 🎯 Objetivos
- Entender el concepto fundamental de Riesgo en ciberseguridad.
- Comprender la fórmula básica del cálculo de riesgo.
- Conocer las cuatro formas de tratar o gestionar un riesgo.

---

## 🧠 Concepto

En ciberseguridad, trabajamos constantemente manejando la incertidumbre. No tenemos presupuesto ni tiempo infinito para proteger todo al 100%. Por lo tanto, debemos priorizar. Para priorizar, usamos la Gestión de Riesgos (Risk Management).

El **Riesgo (Risk)** es la probabilidad de que ocurra un evento negativo (una amenaza explotando una vulnerabilidad) multiplicada por el impacto que ese evento tendría en la organización.

### La Fórmula del Riesgo

La forma más básica de entender el riesgo es esta fórmula matemática simple:

> **Riesgo = Probabilidad × Impacto**

* (Risk = Likelihood × Impact)

#### 1. Probabilidad (Likelihood)
¿Qué tan probable es que ocurra este evento? Esto depende de:
- ¿Existe una [[04 - Amenazas|Amenaza]] activa? (Ej. grupos de ransomware atacando a nuestra industria).
- ¿Existen [[05 - Vulnerabilidades|Vulnerabilidades]] en nuestro sistema?
- ¿Existe un [[06 - Exploits|Exploit]] fácil de usar?

#### 2. Impacto (Impact)
Si el evento ocurre, ¿qué tan malo será el resultado? Impacto en la [[03 - CIA Triad|Tríada CIA]]:
- **Financiero:** Pérdida de dinero por robo o multas.
- **Reputacional:** Pérdida de confianza de los clientes, mala prensa.
- **Operativo:** Sistemas caídos, empleados sin poder trabajar.
- **Legal/Regulatorio:** Demandas por filtración de datos privados.

---

## 🛡️ Tratamiento del Riesgo (Risk Treatment)

Una vez que identificamos y calculamos un riesgo, la gerencia (no los técnicos solos) debe decidir qué hacer con él. Existen cuatro estrategias principales:

1. **Mitigación (Risk Mitigation / Reduction):** Es lo que los profesionales de seguridad hacen la mayor parte del tiempo. Tomar medidas para reducir la *Probabilidad* o el *Impacto* a un nivel aceptable. 
   - *Ejemplo:* Instalar un Firewall para reducir la probabilidad de un ataque, o hacer copias de seguridad (Backups) para reducir el impacto si nos ataca un ransomware.

2. **Transferencia (Risk Transference / Sharing):** Mover el impacto financiero del riesgo a otra entidad.
   - *Ejemplo:* Contratar un seguro de ciberseguridad (Cyber Insurance) que pague los costos si sufrimos una brecha, o migrar servicios a un proveedor en la nube que asuma parte de la seguridad de la infraestructura.

3. **Aceptación (Risk Acceptance):** El costo de mitigar el riesgo es mayor que el daño potencial. La gerencia decide vivir con el riesgo.
   - *Ejemplo:* Un sistema heredado (legacy) tiene vulnerabilidades, pero actualizarlo cuesta $1 millón y la pérdida si es hackeado sería de $10,000. La empresa acepta el riesgo (debe documentarse).

4. **Evitación (Risk Avoidance):** Eliminar completamente la actividad que causa el riesgo porque es demasiado peligroso y no vale la pena.
   - *Ejemplo:* Una app quiere lanzar una función de pago con criptomonedas, pero la seguridad es demasiado compleja y el riesgo regulatorio muy alto. Deciden cancelar el proyecto de la función de pago.

---

## ❓ ¿Por qué importa?

La ciberseguridad no trata de "seguridad absoluta". Trata de **gestión de riesgos**. 
El trabajo de un profesional de ciberseguridad (especialmente en roles de [[22 - GRC|GRC]] o Management) es identificar riesgos técnicos y traducirlos a lenguaje de negocios para que los directivos puedan tomar decisiones informadas sobre dónde invertir el presupuesto.

---

## 🌎 Aplicación en el mundo real

Imaginá una empresa que tiene un servidor con una vulnerabilidad crítica.
- El servidor aloja solo el menú de la cafetería de la empresa. **(Probabilidad: Alta, Impacto: Muy Bajo -> Riesgo Bajo)**. Se puede posponer el parche.
- El servidor procesa transacciones de tarjetas de crédito de clientes. **(Probabilidad: Alta, Impacto: Muy Alto -> Riesgo Crítico)**. Todo el equipo debe detener lo que está haciendo y parchear el servidor inmediatamente.

La vulnerabilidad es la misma, pero el **Riesgo** cambia drásticamente según el impacto.

---

## ❌ Errores comunes

- **Tratar todos los hallazgos de seguridad con la misma urgencia:** Si corrés un escáner de vulnerabilidades y encuentra 500 fallos, no podés arreglar todos el mismo día. Priorizar sin analizar el Riesgo (Probabilidad x Impacto) lleva al agotamiento del equipo (burnout).
- **Creer que la mitigación elimina el riesgo:** Los controles de seguridad reducen el riesgo, pero siempre queda un "Riesgo Residual" que la empresa debe aceptar.

---

## 📌 Must Know (Imprescindible)
- La fórmula del Riesgo: Probabilidad × Impacto.
- Las cuatro formas de tratar un riesgo (Mitigar, Transferir, Aceptar, Evitar).

## 💡 Good to Know (Bueno saberlo)
- **Riesgo Inherente:** El riesgo inicial que existe *antes* de aplicar cualquier control de seguridad.
- **Riesgo Residual:** El riesgo que queda *después* de haber aplicado medidas de mitigación. La Aceptación de riesgo siempre se hace sobre el Riesgo Residual.

---

## 📝 Para recordar
> "No se puede asegurar todo al 100%. Si intentás asegurar todo, terminás no asegurando nada bien." El riesgo nos dice dónde debemos enfocar nuestros esfuerzos limitados.

---

## 🔄 Preguntas de repaso
1. Una empresa decide contratar a Amazon Web Services (AWS) para que aloje sus servidores, transfiriendo así la responsabilidad de la seguridad física del centro de datos. ¿Qué estrategia de tratamiento de riesgo están usando?
2. Si un equipo técnico descubre que arreglar una vulnerabilidad menor en un sistema antiguo cuesta más que los ingresos que genera dicho sistema en todo un año, ¿qué recomendación de tratamiento de riesgo deberían darle a la gerencia?
3. Explicá la diferencia entre Riesgo Inherente y Riesgo Residual.

**➡️ Siguiente nota:** [[08 - Controles de Seguridad]]
