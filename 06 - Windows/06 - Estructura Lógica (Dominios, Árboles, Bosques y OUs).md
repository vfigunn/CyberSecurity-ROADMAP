# 06 - Estructura Lógica (Dominios, Bosques y OUs)

## 🎯 Objetivos
- Entender la jerarquía estructural de Active Directory (El Árbol Genealógico corporativo).
- Diferenciar el límite de seguridad (El Dominio) del límite máximo (El Bosque).
- Conocer la Unidad Organizativa (OU) y por qué los administradores la utilizan.

---

## 🧠 Concepto: Ordenando el Caos

Active Directory (AD) no es una simple tabla de Excel con nombres de usuarios. Es una base de datos de estructura jerárquica masiva. Imagínalo como el organigrama empresarial gigante de una corporación multinacional.

Microsoft dividió la estructura lógica de AD en 3 componentes principales (De menor a mayor):

---

## 🏢 1. El Dominio (Domain)

Es la unidad fundamental de AD. Es un límite administrativo y de seguridad.
Se nombran exactamente igual que una página web, utilizando la estructura DNS (Ej: `empresa.com` o `corp.local`).

- **El Límite de Seguridad:** Un administrador de `empresa.com` tiene poder de Dios sobre todas las PCs y usuarios que están *adentro* de `empresa.com`. Pero no tiene ningún poder sobre los empleados de `otraempresa.com`.
- Todos los objetos (usuarios, computadoras, impresoras) existen dentro del dominio.

## 🌲 2. El Árbol (Tree) y El Bosque (Forest)

¿Qué pasa si `empresa.com` (una marca de autos) se expande a Europa y decide crear una filial completamente independiente en España?
Crearán un nuevo dominio "hijo" llamado `es.empresa.com`.

- **Árbol (Tree):** Un conjunto de dominios que comparten el mismo nombre raíz (ej. `empresa.com` y su hijo `es.empresa.com`). Automáticamente, AD crea un puente de "Confianza" (Trust) entre ellos para que los empleados españoles puedan usar las impresoras de la matriz si viajan.
- **Bosque (Forest):** Es el límite máximo y supremo de Active Directory. Un bosque puede contener múltiples árboles que ni siquiera comparten el mismo nombre (ej. `empresa-autos.com` y `banco-inversor.com`, si el dueño compró ambas compañías). Todo lo que está dentro de un Bosque está conectado lógicamente.

> **Importante para el Red Team:** A nivel defensivo, los de Microsoft solían decir que el Dominio era la barrera de seguridad. Pero los hackers demostraron que si hackeabas el dominio `es.empresa.com`, podías usar los puentes de confianza para vulnerar al padre (`empresa.com`). Hoy, la industria sabe que **El verdadero y único límite de seguridad real es el Bosque (Forest).** Si un dominio cae, considerá todo el bosque comprometido.

---

## 🗂️ 3. Las Unidades Organizativas (OUs)

Dentro del Dominio, necesitás organizar a los 10.000 empleados. No podés tenerlos a todos en una sola lista gigante porque sería imposible administrarlos.

Para eso existen las **OUs (Organizational Units)**. Son literalmente "Carpetas" virtuales dentro de Active Directory.
- Creás la OU: `Recursos Humanos`. Metes a los 50 empleados de RRHH adentro.
- Creás la OU: `Servidores`. Metes a los servidores adentro.

### ¿Para qué sirven las OUs en Seguridad?
1. **Delegación de Poder:** Si hay un empleado de IT Junior al que querés darle el poder de resetear contraseñas, no le das poder de administrador sobre el Dominio entero (sería un riesgo inmenso). Le das poder administrativo *únicamente* sobre la OU `Recursos Humanos`. 
2. **Aplicación de Políticas (GPO):** (Lo veremos a fondo en la Nota 11). Podés crear una regla de seguridad que diga: "Bloquear las unidades de Pendrive USB". Y aplicarla *solamente* a la OU `Recursos Humanos`, sin afectar al resto de la empresa.

---

## 📌 Must Know (Imprescindible)
- La jerarquía de menor a mayor: Unidad Organizativa (OU) -> Dominio -> Árbol -> Bosque.
- Un Dominio se nombra utilizando formato DNS (ej. `empresa.local`).
- En la arquitectura moderna, **El Bosque** es la verdadera barrera de aislamiento; si hackean un dominio hijo, el dominio padre está en riesgo crítico mediante la explotación de *Trusts* (Confianzas).
- Las **OUs** se utilizan para ordenar objetos, delegar permisos a técnicos junior, y aplicar políticas de grupo (GPOs).

---

## 🔄 Preguntas de repaso
1. Una corporación de salud (`hospital.local`) absorbe a una pequeña clínica (`clinica-sur.com`) tras una compra empresarial. Los administradores deciden conectar ambos entornos para que los médicos compartan bases de datos, utilizando Active Directory. ¿Ambos dominios pasarán a formar parte de un mismo Árbol (Tree) o formarán parte de un mismo Bosque (Forest)? Justificá según la regla del nombre (Espacio de nombres contiguo vs disjunto).
2. Si un atacante compromete por completo un entorno de Active Directory y logra obtener los privilegios máximos a nivel de Bosque ("Enterprise Admin"), y la empresa decide limpiar solamente el Dominio principal donde detectaron al intruso, ¿por qué los ingenieros de respuesta a incidentes dirían que la remediación es insuficiente?
3. El departamento de TI de una escuela quiere que el técnico de computadoras pueda apagar o reiniciar los equipos del Laboratorio A, pero sin que tenga permiso para tocar los servidores principales de la dirección. Utilizando la estructura de Active Directory, ¿cómo lograrías esto administrativamente mediante el uso de OUs?

**➡️ Siguiente nota:** [[07 - Controladores de Dominio (Domain Controllers)]]
