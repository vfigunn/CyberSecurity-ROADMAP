# 10 - Búsqueda y Protocolos (LDAP)

## 🎯 Objetivos
- Conocer LDAP, el protocolo que funciona como "el motor de búsqueda" de la red.
- Entender cómo interactúan Kerberos y LDAP (Autenticación vs Autorización).
- Descubrir la vulnerabilidad de recolección de información que utiliza el Red Team (BloodHound).

---

## 🧠 Concepto: La Guía Telefónica de la Corporación

En las últimas dos notas hablamos puramente de Autenticación (Kerberos/NTLM). Su único trabajo era responder a la pregunta: *"¿Juan es quien dice ser?"*
Pero falta algo vital: ¿Cómo sabe la red qué cargo tiene Juan? ¿Cómo sabe el servidor quién es el jefe, cuántas computadoras hay en la empresa, o en qué Unidad Organizativa (OU) vive cada impresora?

Aquí entra **LDAP (Lightweight Directory Access Protocol)**.
LDAP es el protocolo que permite consultar, leer y modificar la gigantesca base de datos jerárquica de Active Directory. **Es, en resumen, la guía telefónica y de mapas de la red corporativa.**

---

## 🔍 ¿Cómo funciona LDAP en la realidad?

Si en Outlook querés enviarle un correo a tu compañero "Pedro", escribís "Pedro" en la barra de destino. Tu computadora usa silenciosamente el protocolo LDAP por el puerto de red (389) para preguntarle al Controlador de Dominio: *"Traeme los datos y el correo de Pedro"*. 
El Controlador de Dominio (usando la base de datos `NTDS.dit`) busca en el árbol, encuentra a Pedro, y te devuelve su apellido, correo y cargo.

**El Flujo Completo (Kerberos + LDAP):**
1. Un empleado llega a la oficina y se loguea. **Kerberos** hace la matemática (TGT/TGS) y verifica su identidad (Autenticación).
2. El empleado quiere abrir la carpeta "Finanzas" en un Servidor de Archivos. 
3. El Servidor de Archivos utiliza **LDAP** para preguntarle al Controlador de Dominio (Autorización): *"¿Este usuario pertenece al Grupo de Contadores?"*.
4. LDAP revisa el árbol de Active Directory, confirma que el usuario es contador, y el servidor le da acceso.

*LDAP es rápido, estructurado y usa una sintaxis específica (Ej: `CN=Juan, OU=Ventas, DC=empresa, DC=com`), parecida a las jerarquías de Carpetas/Archivos en un sistema.*

---

## 💥 La Recolección Ofensiva (BloodHound y Enumeración)

Como analista, esto te va a sorprender: **Por defecto, Active Directory (usando LDAP) le permite a CUALQUIER usuario autenticado de la empresa, leer la estructura entera del Directorio.**

Si sos el conserje, podés hacerle consultas LDAP a la base de datos y leer los nombres de los 10.000 empleados, ver quién es administrador de qué servidor, y qué usuarios tienen la contraseña mal configurada. Esto no es un bug (error) de Microsoft, fue diseñado así por "comodidad" para que los softwares de terceros funcionen fácil. (Obviamente no podés modificar nada, pero podés leer la estructura).

**La herramienta BloodHound (El terror del Blue Team):**
El Red Team aprovecha esta "lectura pública por defecto" con una herramienta de código abierto llamada **BloodHound** (El Sabueso).
1. El hacker compromete la cuenta del conserje.
2. Corre un recolector llamado `SharpHound` en la computadora del conserje.
3. El recolector lanza decenas de miles de peticiones LDAP silenciosas a la base de datos (Pide el listado de todos los grupos, permisos, computadoras conectadas).
4. El atacante exporta todo eso a su PC, lo carga en BloodHound (Que genera un gráfico de red visual inmenso) y hace un clic mágico en el botón: *"Encontrar el camino más corto hacia el Administrador de Dominio"*.
5. BloodHound analiza el caos de la red y le traza un mapa al atacante: *"Primero hackeá al Servidor 5 porque el Conserje tiene acceso ahí de casualidad, robale la clave al usuario Ana que inició sesión ahí ayer, y Ana resulta ser Administradora del Dominio"*.

*(Defenderse contra BloodHound es uno de los dolores de cabeza más grandes de la arquitectura empresarial actual, requiriendo endurecimiento estricto y segmentación de redes, y detectando grandes oleadas de búsquedas de LDAP en el Sistema de Monitoreo - SIEM).*

---

## 📌 Must Know (Imprescindible)
- **Kerberos/NTLM** se dedican a la *Autenticación* (Quién sos).
- **LDAP** se dedica a la consulta y extracción de atributos del directorio (Qué sos y Dónde estás en la base de datos).
- Conocer la herramienta **BloodHound** y entender cómo abusa del protocolo LDAP para trazar rutas de ataque visuales dentro de una corporación que está mal configurada internamente.

---

## 🔄 Preguntas de repaso
1. Cuando un empleado de Recursos Humanos abre su cliente de correo interno corporativo, ingresa en el buscador la letra "M", y el software mágicamente autocompleta mostrándole una lista con los correos, números de teléfono corporativo y oficinas de todos los empleados cuyos nombres empiezan con "M". ¿Qué protocolo de red se está utilizando, en segundo plano, para extraer esos atributos desde el Controlador de Dominio?
2. Distinguí brevemente entre los dos roles que cumplen el protocolo Kerberos y el protocolo LDAP en el ecosistema de Active Directory. (Autenticación vs. Autorización / Búsqueda).
3. Un atacante, que solo posee las credenciales de un empleado de la cafetería con los privilegios más bajos de la red, decide lanzar la herramienta `BloodHound`. ¿Por qué el atacante logrará leer/mapear el 100% de la arquitectura de la red completa sin arrojar errores de permisos (y por qué esto se considera un comportamiento "by design" de Active Directory)?

**➡️ Siguiente nota:** [[11 - Políticas de Grupo (GPOs - El arma de doble filo)]]
