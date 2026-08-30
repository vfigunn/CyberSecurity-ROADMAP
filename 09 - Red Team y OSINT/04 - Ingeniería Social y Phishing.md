# 04 - Ingeniería Social y Phishing

## 🎯 Objetivos
- Entender por qué el Ser Humano es la vulnerabilidad crítica (Capa 8).
- Conocer los vectores de la Ingeniería Social.
- Diferenciar el Phishing tradicional, del Spear-Phishing corporativo armado con OSINT.

---

## 🧠 Concepto: La Capa 8 (El Humano)

El cortafuegos perimetral (Firewall) de la corporación cuesta millones de dólares, bloquea puertos `Inbound`, y filtra los exploits. Tratar de entrar a las patadas por la red externa puede tomarle 4 meses al Red Team y resultar en fracaso.

Pero hay algo que el Firewall no puede proteger: **La psicología de los empleados**.
¿Por qué gastar meses tratando de crackear el servidor de contraseñas de Microsoft, si podés simplemente mandarle un correo a la recepcionista pidiéndole que por favor te dé su contraseña y ella te la da gratis?

La **Ingeniería Social** es el arte de manipular psicológicamente a las personas (basándose en la urgencia, el miedo, la curiosidad o el respeto a la autoridad) para que entreguen credenciales, instalen un virus voluntariamente, o abran las puertas de la empresa.
*(En ciberseguridad, a los humanos se les llama jocosamente la "Capa 8" del Modelo OSI).*

---

## 🎣 Vectores (Formas) de Ataque Social

### 1. Phishing (Pesca con red)
El método electrónico más famoso del mundo. El envío masivo de correos electrónicos falsificados o SMS (Smishing).
- *El Clásico:* *"Soy Netflix. Tu tarjeta expiró. Si no hacés clic acá en 24 horas y ponés tu tarjeta, te cortamos la serie"*. Juega con la urgencia y el miedo.
- *El Mecanismo:* El link redirige al usuario hacia un sitio clonado exactamente igual al verdadero (Creado por el atacante). Cuando el usuario tipea su credencial, no viaja a Netflix, viaja al servidor del hacker.

### 2. Spear Phishing y Whaling (Pesca con Arpón)
El Phishing clásico es genérico y los empleados ya no caen tan fácil.
En operaciones de **Red Team (Simulaciones Avanzadas)**, se utiliza el Arpón (Spear Phishing). Aquí es donde el atacante usa todo lo que descubrió en la fase de **OSINT**.

1. El Atacante descubrió por OSINT (LinkedIn) que la empresa acaba de implementar un nuevo software de Recursos Humanos de la marca *BambooHR*.
2. El Atacante descubrió el correo de Juan (Director de la empresa).
3. El Atacante envía 1 solo correo (solo a Juan), falsificando el nombre (Spoofing) para que parezca venir del Gerente de IT de esa misma empresa.
   > *"Hola Juan. El sistema de BambooHR que implementamos ayer tiene un error grave con tu nómina salarial. Entrá en este link de la Intranet y confirmá tus datos o no vas a cobrar tu sueldo mañana"*.
4. Juan, leyendo algo que hace referencia exacta a su contexto interno y bajo presión directiva, cae sin dudar. Entrega sus llaves de Administrador, y la campaña ofensiva termina con éxito en menos de 1 semana.
*(Cuando el objetivo hiper-personalizado es un pez muy gordo, como un CEO o un Presidente, se le llama "Whaling" o Caza de Ballenas).*

### 3. Vishing (Phishing Telefónico o por Voz)
- Ocurre mediante llamadas de teléfono falsificadas (Caller ID Spoofing).
- El atacante simula ser del área de Soporte Técnico de la empresa (HelpDesk).
- *"Hola Clara, estamos teniendo un problema con el Active Directory y no podés recibir los correos del Jefe. Entrá a la página X y descargate el archivo que te pasé para reiniciar tu módulo de red"*.
- Clara obedece la voz humana con autoridad de Sistemas, instala el archivo, y su PC se contamina con una puerta trasera (Reverse Shell).

### 4. Baiting (Cebos / Pen-Drop)
- Juega con la codicia o la curiosidad y es un ataque físico.
- El atacante del Red Team se pasea por el estacionamiento físico de la empresa objetivo, y "pierde" o tira al piso 10 Pendrives USB, marcados con una cinta que dice *"Salarios 2026 y Recortes.xlsx"*.
- Un empleado lo encuentra en el piso, y para saciar su morbo corporativo (y de paso, robarse un pendrive gratis de 32GB), lo conecta a la computadora de la oficina. 
- En el microsegundo que lo conecta, un programa malicioso preinstalado en el pendrive se ejecuta silenciosamente (Ej. Ataques BadUSB) destrozando el aislamiento de la red física (Air-Gap) y dándole la victoria al atacante.

---

## 📌 Must Know (Imprescindible)
- Conocer la diferencia entre el **Phishing** tradicional (Masivo y genérico) y el **Spear-Phishing / Whaling** (Ultra dirigido, diseñado para 1 sola víctima basada en un extenso y meticuloso estudio de OSINT previo).
- Entender por qué el **Vishing (Voz)** y el **Baiting (Pendrives/Cebos)** son tácticas críticas en los ejercicios holísticos de Red Team (Simulando la entrada física del atacante o el bypass del hardware de la empresa).
- Asimilar que el verdadero Firewall de la Capa 8 de la corporación es la **Concientización / Capacitación continua de los empleados** sobre estas tácticas.

---

## 🔄 Preguntas de repaso
1. El equipo de Blue Team configuró filtros Anti-Spam que bloquean perfectamente los millones de correos basura del estilo "Gánate una lotería" que provienen de África o Rusia. Sin embargo, en el último mes, el departamento contable fue vulnerado por un Ransomware porque alguien descargó un archivo "Factura_URGENTE.xlsx" de un correo que aparentaba ser del proveedor oficial de la empresa. ¿Por qué variante avanzada de ataque social falló el filtro de correo (Spear-Phishing o Vishing) y en qué se diferencia del clásico ataque masivo?
2. Un Analista ofensivo de Red Team estaciona su auto frente al edificio de la víctima y arroja, estratégicamente en la entrada principal, tres memorias flash (USBs) con un sticker que dice "Fotos Navidad Gerencia". Sabiendo el vector de ataque humano, explicá cómo esta táctica conocida como "Baiting (Cebo)" explota específicamente una debilidad psicológica del empleado (Curiosidad) con la única finalidad de saltarse o eludir qué barrera técnica de seguridad del edificio.
3. Si la empresa de seguridad es contratada para realizar un ejercicio que involucra engañar por teléfono (llamando a la secretaria del CEO para exigirle las credenciales del portal VPN haciéndose pasar por un Administrador de Sistemas enojado que necesita arreglar un error crítico de producción urgente), ¿a qué técnica de ingeniería social específica están recurriendo y qué emoción (miedo/autoridad) están manipulando para doblegar a la Capa 8?

**➡️ Siguiente nota:** [[05 - Cyber Kill Chain y MITRE ATT&CK]]
