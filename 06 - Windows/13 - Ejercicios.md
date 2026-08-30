# 13 - Ejercicios del Módulo 06

## 📝 Instrucciones
Poné a prueba tu comprensión de la arquitectura de Microsoft y la red de Active Directory. Pensá como el atacante (Red Team) para entender los vectores de entrada, y como el defensor (Blue Team) para detectar las configuraciones erróneas de los administradores.

---

## 🧠 Ejercicios de Lógica y Análisis (Windows y AD)

1. **La caza del Rootkit (Anillos):**
   - El equipo forense encuentra un proceso que está robando información. Sin embargo, no pueden matarlo ni eliminar su archivo asociado desde el usuario "Administrador", porque el proceso se autoprotege.
   - Sabiendo que los procesos en Ring 3 (User Mode) no tienen permisos directos sobre el hardware ni sobre otros procesos protegidos, ¿en qué Anillo (Ring) debe estar operando necesariamente el núcleo de este malware, y qué "formato de archivo" en Windows (ej. drivers) suele utilizarse para alcanzar ese nivel?

2. **Dilema del Permiso Compartido:**
   - Hay una carpeta corporativa llamada `Datos_Clientes` en el Servidor X.
   - Entrás a las propiedades de Compartición de Red (Share) y configuras: `Grupo Finanzas -> LECTURA`.
   - Entrás a las propiedades del disco duro (NTFS) y configuras: `Grupo Finanzas -> CONTROL TOTAL`.
   - Si un miembro de finanzas accede por la red (desde su oficina) e intenta borrar un archivo, ¿podrá hacerlo? Explicá la regla de cálculo matemático de permisos que aplica Windows en este caso.

3. **El Robo del Siglo (Archivos Sagrados):**
   - Un grupo de atacantes logra comprometer un servidor y descargan silenciosamente un archivo pesado. Al otro día, todas las contraseñas de todos los empleados de la empresa fueron descubiertas por los atacantes offline.
   - Teniendo en cuenta la arquitectura de Active Directory, ¿cuál es el nombre del archivo específico que los atacantes lograron robar, y en qué tipo de servidor corporativo (Servidor Miembro o Controlador de Dominio) estaba físicamente almacenado?

4. **El Desafío de NTLM (Pass-The-Hash):**
   - Explicá paso a paso por qué, si los administradores configuran sus sistemas para requerir contraseñas de 50 caracteres complejísimos (ej. `@a$X9!bZ...`), esto no detiene en absoluto a un atacante que logra hacer un ataque de "Pass-The-Hash" robando la memoria del proceso `lsass.exe`.

5. **El Ataque al Parque de Diversiones (Kerberos):**
   - Un atacante que ya tiene acceso inicial a la red logra generar peticiones de tickets de servicio (TGS) hacia el Controlador de Dominio de manera legítima, usando una cuenta normal sin privilegios. El atacante exporta esos tickets a un Pendrive y se los lleva a su casa.
   - ¿Cómo se llama este ataque masivo de la industria? ¿Y qué porción del ticket está intentando crackear (romper) el atacante desde su casa usando placas de video?

6. **Armamento Masivo (Políticas de Grupo):**
   - Sos un ingeniero de Blue Team. Te llega un correo alertando que todas las unidades de CD/DVD de las 5,000 computadoras del edificio acaban de ser deshabilitadas al mismo tiempo, en un lapso de 90 minutos, sin que ningún empleado toque su computadora.
   - A nivel infraestructura, ¿qué herramienta centralizada de Windows Server debió haber modificado obligatoriamente el atacante para desplegar este comportamiento simultáneo de forma nativa e invisible?

---

## 🎯 Autoevaluación
Es vital que comprendas a la perfección el Ejercicio 4 (Pass-The-Hash) y el Ejercicio 5 (Kerberoasting). Estos dos ataques son responsables del 80% del Movimiento Lateral en las corporaciones de Fortune 500.

**➡️ Siguiente nota:** [[14 - Evaluación]]
