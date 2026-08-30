# 09 - Ejercicios del Módulo 09

## 📝 Instrucciones
Poné a prueba tu mentalidad ofensiva estructurada. Pasamos de los bits y bytes, a la psicología corporativa, la estrategia de OSINT, el modelo C2 y la entrega de reportes formales.

---

## 🧠 Ejercicios de Estrategia e Inteligencia (Red Team)

1. **La Paradoja de los Colores:**
   - Un cliente te contacta porque su página web sufrió 3 ataques menores esta semana. Necesitan contratarte urgente para que dediques este fin de semana a detectar (auditar) cuáles son los agujeros técnicos del sitio web, y entregarles el reporte con la lista el lunes, a un costo económico bajo.
   - Sabiendo el contexto (urgencia, escaso tiempo, ruido y búsqueda de agujeros), definí si el cliente quiere contratarte para realizar una clásica auditoría "Pentesting", o si por error de términos, en realidad busca que despliegues una costosa e indetectable Operación de "Red Teaming".

2. **La Huella Invisible (OSINT):**
   - Tenés que investigar a una empresa sin que se enteren. En lugar de escanear la empresa de forma directa con la herramienta ruidosa Nmap, decidís buscar sus servidores en `Shodan` usando el dork o filtro: `org:"EmpresaTarget" port:"3306"`.
   - A nivel de Networking y Seguridad, explicá por qué Shodan pudo entregarte la IP de la base de datos de esa empresa, y sin embargo, el Firewall de la empresa objetivo jamás registró ni verá la IP de tu casa, protegiendo tu anonimato como si fueras un fantasma.

3. **El Dork de Extracción:**
   - Escribí y estructurá mentalmente la línea de código (Dork) exacta que inyectarías en el buscador público de Google, si tu misión de reconocimiento requiriera listar "todas las páginas web y archivos en el mundo que pertenezcan EXCLUSIVAMENTE al dominio gubernamental `.gov` (limitador de sitio), y que además EXCLUSIVAMENTE tengan el formato de archivo de Excel `.xlsx` (limitador de tipo)".

4. **Anatomía del Phishing:**
   - Durante tu investigación (Fase 1: Reconocimiento), descubriste en Facebook y foros de autos, que un administrador de redes de la empresa objetivo está intentando vender su vehículo (Un Ford Fiesta de color rojo). 
   - Durante la Fase 3: Entrega, le enviás un correo directo de "Spear-Phishing" (Phishing con Arpón) que incluye tu Payload malicioso disfrazado en un PDF falso y letal, cuyo asunto dice: *"¡Me interesa comprarte urgente el Ford Fiesta rojo, te ofrezco el doble! (Ver comprobante adjunto)"*. 
   - Justificá desde la psicología de la Capa 8 (Vulnerabilidad humana), por qué este ataque focalizado de Spear-Phishing tiene una altísima probabilidad de éxito frente al clásico y genérico ataque spam ("Te ganaste un Iphone 15"). 

5. **El Faro del Ataque (Beaconing):**
   - Una "Reverse Shell" clásica de Metasploit abre un tubo directo a la máquina de la víctima y se queda viva y conectada para siempre.
   - Un Agente comercial "C2" (Ej: Cobalt Strike o Mythic) inyectado en una víctima, entra en modo hibernación y realiza el llamado *Beaconing* aleatorio con *Jitter* cada 60 minutos. 
   - Analizá la perspectiva del Blue Team que mira las pantallas y métricas del Firewall corporativo. Explicá cómo la arquitectura que duerme (Beacon) engaña y se funde con el ruido orgánico de los empleados (Navegación web HTTP) eludiendo por meses su detección.

6. **Prioridad Matemática (CVSS):**
   - En tu reporte técnico documentaste 2 vulnerabilidades separadas, pero necesitás indicar cuál debe ser reparada primero, asignándoles el CVSS de Severidad.
   - La vulnerabilidad A (Vector de Acceso Físico): Exige que el hacker robe la llave física, camine dentro de las oficinas del Banco y conecte un cable USB al router para hackearlo.
   - La vulnerabilidad B (Vector de Acceso de Red): Exige que un atacante anónimo se conecte desde China a través de Internet (Remote/Network) e inyecte caracteres en la página web pública.
   - Basándote en la métrica estándar de "Alcance y Complejidad" de acceso, deducí qué vulnerabilidad poseerá obligatoriamente un puntaje CVSS altísimo por la magnitud y anonimato del vector, siendo el mayor riesgo de negocio.

---

## 🎯 Autoevaluación
Chequeá internamente que domines de forma absoluta el Ejercicio 1 (La enorme diferencia de foco, silenciamiento y tiempo entre un Pentest vs. un Red Team) y el Ejercicio 5 (La ventaja militar de usar Beaconing silencioso vs un túnel crudo y constante). Los profesionales que superan las entrevistas laborales corporativas dominan estos conceptos conceptuales sin vacilar.

**➡️ Siguiente nota:** [[10 - Evaluación]]
