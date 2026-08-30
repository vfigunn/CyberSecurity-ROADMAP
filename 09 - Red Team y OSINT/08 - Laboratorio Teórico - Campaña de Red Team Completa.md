# Lab 09.1 - Operación Fantasma (Campaña de Red Team)

## 🎯 Objetivo
Unir los 7 pasos de la **Cyber Kill Chain** (OSINT, Phishing, Pivoting y C2) en un escenario narrativo hiperrealista para asimilar cómo el Hacking se convierte en Inteligencia Militar.

---

## 📋 La Misión: "Operación Farmacéutica"

Fuiste contratado por la Junta Directiva de `BioPharma Inc.` (Una empresa de desarrollo de vacunas).
Te dieron permiso legal y 3 meses de tiempo.
Tu objetivo final (La Bandera): **Extraer el PDF con la fórmula de la nueva vacuna secreta, simulando ser un Hacker de Estado.** El Director de Seguridad (Blue Team) no sabe que vos fuiste contratado. El juego empieza hoy.

---

### Fase 1: Reconocimiento (OSINT y Shodan)
**Semana 1:** No tocás ni de cerca los servidores de BioPharma (Su Firewall de PaloAlto es impenetrable).
- Usás **Google Dorks** (`site:biopharma.com filetype:pdf`) y leés manuales corporativos públicos para entender cómo hablan.
- Vas a **LinkedIn** y extraés pasivamente 150 nombres de empleados usando la herramienta `theHarvester`. Buscás el eslabón más débil: Encontrás a *"Sofía, Secretaria del Sector I+D"*.
- Con **Shodan**, descubrís que la empresa usa el sistema de correos Office365. Estás listo.

### Fase 2 y 3: Armamento y Entrega (Phishing)
**Semana 3:** Comprás un dominio falso llamado `biopharma-IT-support.com` (Phishing avanzado).
- Configurás un Servidor de **Command & Control (C2)** en la nube (Usando `Cobalt Strike` o `Mythic`). Creas un "Agente Malicioso (Beacon)" disfrazado.
- Enganchás tu virus adentro de una falsa *"Actualización de Políticas de Vacaciones 2026.docx"*. 
- Le mandás un correo de **Spear-Phishing** directamente a Sofía, falsificando el nombre de Recursos Humanos, usando términos corporativos que aprendiste en tu fase de Reconocimiento.

### Fase 4 y 5: Explotación e Instalación (El Engaño)
**Semana 4 (Día D):**
- Sofía (La Capa 8), que no está entrenada en ciberseguridad, recibe el correo.
- Lo abre. El documento de Word (que contiene un Macro letal inyectado o Exploit de Zero-Day) se ejecuta en su máquina silenciosamente.
- El virus no abre una ventana. El virus altera subrepticiamente el Registro de Windows de la PC de Sofía (**Persistencia**) asegurándose que revivirá mañana, y se prepara para contactar al atacante.

### Fase 6: C2 (El Beaconing Silencioso)
**Semana 5 (Supervivencia):**
- ¿Por qué el SOC (Blue Team) de BioPharma no te detectó? Porque tu Agente en la PC de Sofía no usa "Meterpreter ruidoso". Usa **Beaconing**.
- El virus duerme 4 horas. Despierta, manda un ping HTTP de 1 byte a tu dominio falso de IT `biopharma-IT-support.com` (Camuflado como si Sofía estuviera navegando una web normal), y vuelve a dormir, usando *Jitter* (tiempo aleatorio) para romper patrones matemáticos del Firewall.
- Recién a la semana 6, cuando Sofía se va a dormir, le das la orden al Agente C2 desde tu tablero para que expanda su poder, realizando un Volcado de Hashes (`hashdump`) y robando su usuario.

### Fase 7: Acciones Finales (Pivoting hacia el Tesoro)
**Semana 8:**
- Con el usuario de Sofía hackeado, tenés el problema de que su computadora no tiene la vacuna. Solo la tiene el Servidor Central (Base de Datos). 
- Utilizás la PC de Sofía como "Enrutador Puente" (**Pivoting / Movimiento Lateral**). 
- Escaneás la red interna, y lográs hackear el Servidor Central desde la propia computadora de Sofía (Desde adentro, saltando el Firewall Perimetral).
- Localizás el archivo: `Vacuna_Secreta_V2.pdf`.
- Extraes el documento gota a gota, camuflándolo dentro de peticiones engañosas al protocolo DNS (La técnica técnica T1048 de MITRE ATT&CK) para no disparar las alarmas de volumen del Blue Team.

---

## 📝 Conclusión del Laboratorio (Cierre de Campaña)
Semana 12. Reunís al CEO en una sala y le entregás tu Reporte Ejecutivo.
El Blue Team de BioPharma se desploma en la silla al ver que estuvieron vulnerados por 2 meses y medio sin enterarse.
Frente a ellos realizas el **Purple Teaming**: Les explicás amistosamente que su tecnología (Firewalls / EDR) era impecable, pero que su vulnerabilidad real estaba en el factor humano (Sofía cayendo en el Spear-Phishing) y en no monitorear el tráfico saliente anómalo de DNS. Redactan juntos las nuevas normas y blindan a la empresa para el futuro. Misión Cumplida.

**➡️ Siguiente nota:** [[09 - Ejercicios]]
