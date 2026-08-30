# 02 - Mapeo y Reconocimiento (BloodHound)

## 🎯 Objetivos
- Entender el caos laberíntico de los permisos en una red de 5,000 computadoras.
- Descubrir "BloodHound", la herramienta que cambió para siempre el Red Teaming.
- Aprender el concepto matemático de la "Teoría de Grafos" en Ciberseguridad.

---

## 🧠 Concepto: Buscando el Camino a la Corona

Acabas de lograr una Escalada Local en la computadora de la Recepcionista.
Tu objetivo final es la computadora del Administrador (El Controlador de Dominio - DC). 
Pero estás en una red corporativa con 5,000 computadoras, 10,000 empleados, 800 grupos de seguridad y miles de reglas de Firewall.

¿Cómo sabés a qué computadora saltar a continuación? Si saltás a la computadora equivocada, vas a hacer ruido en la red (te detectará el EDR) y habrás perdido tu tiempo.
Antiguamente, los hackers debían analizar los permisos a mano en planillas de texto inmensas. Hasta que la industria revolucionó el ataque creando **BloodHound**.

---

## 🩸 BloodHound (El Perro Sabueso)

BloodHound es una herramienta open-source. Utiliza la matemática (Teoría de Grafos / Nodos y Aristas) para dibujar **literalmente un mapa visual navegable 3D de cómo hackear a la empresa**.

Funciona en dos pasos gigantes: Ingesta (Recolección) y Visualización (Análisis).

### Paso 1: SharpHound (El Recolector Silencioso)
El atacante sube un pequeño programa llamado `SharpHound` a la computadora hackeada de la recepcionista.
El Active Directory es muy chismoso por naturaleza. Como fue diseñado para ser eficiente, **cualquier usuario del dominio (incluso la recepcionista)** tiene permisos legales para preguntarle al Controlador de Dominio (DC): *"¿Me podés dar una lista con la configuración de todos los grupos y quién es el jefe de cada computadora?"*
SharpHound le hace mil millones de preguntas permitidas al DC, recolecta todo y te exporta un archivo ZIP con los datos, sin que suene ninguna alarma porque no hackeó nada, solo preguntó cosas de lectura pública del Directorio.

### Paso 2: La Visualización Matemática (Los Caminos de Sangre)
El atacante descarga ese ZIP a su propia computadora (en su casa) y lo importa en el visualizador BloodHound (basado en la base de datos gráfica Neo4j).

**El milagro matemático:**
El hacker selecciona la computadora de la recepcionista, selecciona el Controlador de Dominio y aprieta *"Find Shortest Path"* (Buscar camino más corto).
BloodHound dibujará los nodos y le dirá al hacker el camino físico y lógico que debe seguir.

*El mapa dirá algo como:*
1. Estás en la PC de **[Recepcionista]**.
2. Ese usuario casualmente tiene permiso de "Reset Password" (Arista) sobre la cuenta de **[Juan de Sistemas]**. *(Falla lógica 1 de la empresa).*
3. **[Juan de Sistemas]** pertenece al grupo **[HelpDesk]**.
4. El grupo **[HelpDesk]** tiene permisos de *"Administrador Local"* sobre el **[Servidor de Archivos]**.
5. ¡Sorpresa! El **[Domain Admin]** dejó una sesión activa e iniciada (Arista 'HasSession') en el **[Servidor de Archivos]**. *(Falla letal 2).*

**El plan de ataque perfecto está armado:**
El hacker ya no ataca a ciegas. Sabe que si fuerza un reseteo de clave a Juan, salta al Servidor de Archivos, y allí le roba la memoria al Domain Admin, gana el juego corporativo entero.

---

## 📌 Must Know (Imprescindible)
- Qué es **BloodHound**: Una herramienta de recolección y análisis que usa Teoría de Grafos para mapear y revelar las relaciones ocultas o complejas y los caminos de ataque más cortos dentro de un Active Directory.
- **El principio del Ingestor (SharpHound):** Entender que su fase de recolección (Escanear el dominio) **NO ES UN EXPLOIT**. Es una consulta legal de lectura masiva a la base de datos LDAP/RPC de Windows que cualquier usuario de baja jerarquía puede realizar sin permisos especiales.
- Revela **Misconfigurations** (Errores de configuración): BloodHound no busca vulnerabilidades de código de Windows; busca los errores lógicos que los humanos de IT configuraron mal a la hora de asignar permisos cruzados a lo largo de los años.

---

## 🔄 Preguntas de repaso
1. Ejecutaste exitosamente el ingestado (Recolector) `SharpHound.exe` desde la computadora del pasante que hackeaste y obtuviste el archivo ZIP final. ¿Por qué es virtualmente imposible para el Blue Team argumentar que *"El Firewall bloqueó la explotación o intento de Hackeo"*, teniendo en cuenta cómo funciona internamente la recolección de BloodHound contra el Active Directory?
2. Al cargar los datos en la base gráfica (Neo4J) de BloodHound en tu casa, generaste una consulta predefinida llamada: *"Find Shortest Paths to Domain Admins"* (Buscar los caminos más cortos a los Administradores de Dominio). El gráfico te devuelve un mapa. Analizando la utilidad de la Teoría de Grafos para un Red Team: ¿Por qué esta visión estructural 3D le otorga una ventaja abismal de tiempo y sigilo al atacante en comparación a su antiguo método manual de prueba y error en la red?
3. Analizá el siguiente hito (Arista o Conexión) frecuentemente revelado por BloodHound: El grupo de seguridad `[Soporte Nivel 1]` posee el permiso `GenericAll` (Control Total local) sobre 3,000 computadoras del dominio. Desde el punto de vista del atacante (Red Team) y del Movimiento Lateral (Pivoting), ¿por qué las computadoras de `[Soporte Nivel 1]` se vuelven instantáneamente el blanco de Hackeo más deseado de toda la corporación en la Fase Inicial?

**➡️ Siguiente nota:** [[03 - Envenenamiento de Red (LLMNR y Responder)]]
