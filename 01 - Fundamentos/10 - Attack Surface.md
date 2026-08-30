# 10 - Superficie de Ataque (Attack Surface)

## 🎯 Objetivos
- Definir qué es la Superficie de Ataque.
- Distinguir entre las diferentes superficies de ataque (digital, física y humana).
- Entender el concepto de "Reducción de la Superficie de Ataque" (Attack Surface Management - ASM).

---

## 🧠 Concepto

La **Superficie de Ataque (Attack Surface)** es la suma de todos los puntos, debilidades, interfaces y vulnerabilidades (conocidas y desconocidas) por los que un usuario no autorizado o un atacante puede intentar introducir o extraer datos de un entorno.

En palabras simples: **Son todas las posibles puertas y ventanas por las que te pueden entrar a robar.**

### Tipos de Superficies de Ataque

1. **Superficie de Ataque Digital / Red:**
   - Todo el código, aplicaciones, puertos, servidores, sitios web, y APIs que están conectados a la red (especialmente a internet).
   - *Ejemplos:* Un servidor web desactualizado, un puerto de escritorio remoto (RDP) expuesto a internet, código mal escrito en un formulario de login, bases de datos en la nube mal configuradas.

2. **Superficie de Ataque Física:**
   - Vulnerabilidades relacionadas con los dispositivos físicos y las instalaciones.
   - *Ejemplos:* Servidores en habitaciones sin cerradura, computadoras portátiles robadas, puertos USB accesibles en computadoras de recepción, dispositivos IoT (cámaras, impresoras) sin seguridad, discos duros tirados a la basura sin ser destruidos.

3. **Superficie de Ataque Humana (Ingeniería Social):**
   - Las vulnerabilidades creadas por las personas que interactúan con los sistemas (empleados, contratistas, partners).
   - *Ejemplos:* Empleados susceptibles a phishing, contraseñas compartidas en post-its, uso de contraseñas débiles o repetidas, empleados descontentos.

---

## ❓ ¿Por qué importa?

La regla de oro de la defensa es: **"No podés proteger lo que no sabés que tenés."**

A medida que las empresas crecen, compran nuevo software, contratan más gente, migran a la nube (Cloud) y permiten el trabajo remoto, su superficie de ataque crece exponencialmente. Un atacante siempre buscará el camino de menor resistencia, que a menudo es un servidor viejo que el equipo de IT se olvidó que existía.

---

## 🛡️ Reducción de la Superficie de Ataque

El objetivo del equipo de seguridad es hacer que la superficie de ataque sea lo más pequeña posible. Esto se conoce como **Minimización de la Superficie de Ataque** o *Attack Surface Reduction (ASR)*.

Acciones comunes para reducirla:
- **Cerrar puertos innecesarios:** Si un servidor web solo necesita HTTP/HTTPS (puertos 80 y 443), no debería tener el puerto SSH (22) expuesto a internet.
- **Desinstalar software innecesario:** Si no usás un programa en un servidor, borralo. Menos software = menos código que puede tener vulnerabilidades.
- **Eliminar cuentas de usuario antiguas:** Cuando un empleado se va, sus cuentas deben desactivarse el mismo día.
- **Aplicar el Principio de [[05 - Security Fundamentals/05 - Least Privilege|Mínimo Privilegio]]:** Dar a los usuarios solo los accesos estrictamente necesarios para hacer su trabajo.

---

## 🌎 Aplicación en el mundo real

El auge del "Internet de las Cosas" (IoT - Internet of Things) fue una pesadilla para la superficie de ataque. De repente, las oficinas corporativas tenían cafeteras, termostatos y cámaras de seguridad conectados al Wi-Fi de la empresa, muchas veces con contraseñas por defecto de fábrica. Los atacantes dejaron de atacar los servidores difíciles y empezaron a hackear las cafeteras para saltar (pivotar) desde ahí a la red corporativa.

---

## ❌ Errores comunes

- **Olvidar la "Shadow IT":** Cuando los empleados usan software o servicios en la nube sin la aprobación o conocimiento del departamento de IT (ej. usar un Dropbox personal para archivos de la empresa). Esta es una superficie de ataque gigantesca e invisible para los defensores.
- **Creer que la superficie de ataque es estática:** La superficie de ataque cambia todos los días. Se sube código nuevo, se instalan servidores nuevos, se contrata gente nueva. Requiere un monitoreo continuo.

---

## 📌 Must Know (Imprescindible)
- Definición de Superficie de Ataque.
- La diferencia entre la superficie digital, física y humana.
- Cómo reducir la superficie de ataque (cerrar puertos, borrar software, principio de mínimo privilegio).

## 💡 Good to Know (Bueno saberlo)
- El **Attack Surface Management (ASM)** es hoy en día una disciplina entera dentro de la ciberseguridad, y existen herramientas automáticas (EASM - External Attack Surface Management) que escanean continuamente internet buscando activos olvidados de la empresa.

---

## 📝 Para recordar
> Cuanto mayor sea tu superficie de ataque, más esfuerzo, presupuesto y herramientas necesitarás para protegerla aplicando [[09 - Defense in Depth|Defensa en Profundidad]]. Reducir la superficie es siempre el primer y más barato paso en la seguridad.

---

## 🔄 Preguntas de repaso
1. Una empresa decide desactivar todas las cuentas de los usuarios que no han iniciado sesión en los últimos 90 días. ¿De qué manera esto afecta la superficie de ataque?
2. Da un ejemplo de cómo un atacante podría explotar la superficie de ataque física de una organización.
3. Explicá por qué el "Shadow IT" (software no autorizado usado por empleados) es uno de los mayores problemas para medir la superficie de ataque.

**➡️ Siguiente nota:** [[11 - Threat Actors]]
