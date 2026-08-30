# 03 - OSINT II (Google Dorks y Shodan)

## 🎯 Objetivos
- Aprender a hackear con Google (Google Dorking).
- Conocer a "Shodan", el Google oscuro del Internet de las Cosas.
- Entender el concepto de "Dork" (Filtro avanzado).

---

## 🧠 Concepto: Los Motores de Búsqueda no mienten

Todo el mundo sabe buscar en Google. Pero Google es literalmente un robot que escanea trillones de páginas web y guarda en su memoria todo el texto, PDFs, y contraseñas expuestas que encontró.
Para el analista de inteligencia (OSINT) y el Pentester, Google es la herramienta de ciberseguridad más grande del planeta.

---

## 🔍 Google Dorking (Hacking con Buscadores)

Un **Dork** es un Operador de Búsqueda Avanzado. En lugar de escribir *"archivos de excel del gobierno"* en la barra de Google, le das a Google filtros matemáticos precisos.

### Los Operadores de Oro:
- `site:` (Limita la búsqueda a un dominio específico). Ej: `site:empresa.com`
- `filetype:` (Busca una extensión de archivo particular). Ej: `filetype:pdf`
- `intitle:` (Busca una palabra exacta en el Título o pestaña de la página).
- `inurl:` (Busca una palabra dentro del link de la página).

### Ejemplos Letales de Google Dorking:
Un Red Teamer no busca la página principal. Busca errores de los administradores (Directorios expuestos, listados de contraseñas, cámaras web abiertas).

- **El Dork de documentos clasificados:**
  > `site:gobierno.com filetype:pdf "confidencial"`
  *(Google le entregará instantáneamente al atacante todos los PDFs indexados que digan confidencial del gobierno, sin siquiera visitar la página gubernamental).*

- **El Dork de Listado de Directorios (Falla común):**
  > `intitle:"index of" "passwords.txt"`
  *(Este dork caza servidores web donde el administrador olvidó poner una página de inicio, y el servidor muestra el listado de archivos en crudo como si fuera el disco `C:\`. Aquí el atacante acaba de encontrar archivos de contraseñas expuestos a nivel mundial).*

- **El Dork de Cámaras de Seguridad:**
  > `inurl:/view.shtml`
  *(Busca en el mundo las interfaces web específicas de las cámaras de seguridad sin contraseña).*

---

## 👁️ Shodan (El Buscador de Dispositivos - IoT)

Google escanea y guarda páginas web (Texto). 
**Shodan** (`www.shodan.io`) es el motor de búsqueda más aterrador de Internet. En lugar de escanear páginas web, **Shodan escanea Servidores, Puertos Abiertos y Dispositivos de Internet de las Cosas (IoT)**.

Shodan está 24 horas al día, 7 días a la semana, probando tocar la puerta de CADA dirección IP del planeta Tierra.
Cuando toca un puerto y le responden, Shodan guarda la respuesta (El "Banner").

### ¿Qué encuentra Shodan?
- Centrales eléctricas, Plantas potabilizadoras de agua, Semáforos de ciudades.
- Resonadores magnéticos de hospitales, Cámaras de bebés, Heladeras inteligentes.
- **Y todos los servidores de Bases de Datos (MongoDB / SQL) que las empresas dejaron accidentalmente expuestos a Internet.**

### Shodan Dorking:
Al igual que en Google, los atacantes y defensores usan Dorks en Shodan para cazar presas en la red profunda.
- **Cazando servidores anticuados de Windows (SMB/EternalBlue):**
  > `port:445 os:"Windows 7"`
  *(Shodan te arroja instantáneamente 10,000 IPs reales del mundo que tienen el puerto peligroso abierto).*
- **Cazando fallas industriales de infraestructura sin clave:**
  > `port:502 has_screenshot:true`
  *(Busca protocolos de plantas industriales donde Shodan logró conectarse, y además te muestra una foto tomada por la cámara o panel del sistema, totalmente desprotegido).*

> **Nota de Seguridad del Blue Team:** Tu primera tarea siempre es buscar la IP pública (o dominios) de TU propia empresa en Shodan. Si Shodan encontró tu servidor de bases de datos, significa que los atacantes ya lo encontraron también.

---

## 📌 Must Know (Imprescindible)
- **Google Dorking:** Técnicas que utilizan operadores lógicos avanzados (`site:`, `filetype:`, `intitle:`) para manipular el motor de Google y descubrir información confidencial (OSINT), vulnerabilidades y archivos expuestos en un objetivo específico.
- **Shodan:** A diferencia de Google (que indexa texto de páginas web), Shodan indexa los "Banners" de los puertos de red de dispositivos físicos reales (IoT, Routers, Servidores, SCADA) conectados a Internet en todo el planeta.
- Ambos son herramientas de **Reconocimiento Pasivo** extremo. Quien hace el escaneo ruidoso y peligroso por vos es Google o Shodan, ocultando tu propia identidad y dirección IP frente a los logs de la víctima.

---

## 🔄 Preguntas de repaso
1. En un ejercicio de OSINT para una auditoría a la empresa `hospital.com`, decidís utilizar el motor de Google para intentar encontrar historiales médicos o planillas expuestas que hayan sido indexadas por error. Construí la línea exacta del "Google Dork" que utilizarías para forzar a Google a devolverte exclusivamente archivos de Excel (xls o xlsx) que provengan únicamente del dominio del hospital.
2. Un periodista tecnológico lee una noticia alarmante: "Existen 5,000 cámaras de seguridad caseras en el mundo que están transmitiendo en vivo a Internet sin contraseña". Sabiendo qué tecnología se utiliza para indexar, escanear banners e interactuar con el Internet de las Cosas (IoT), ¿qué motor de búsqueda especializado habrán utilizado los investigadores para obtener ese reporte numérico masivo de dispositivos y puertos expuestos?
3. Desde la óptica del anonimato del atacante (Red Team), analizá lo siguiente: Si el objetivo de la campaña ofensiva posee un sofisticado Centro de Operaciones de Seguridad (SOC / Blue Team) que monitorea cada paquete que toca su servidor web. ¿Por qué utilizar el Dork `site:empresa.com intitle:"Index of"` para encontrar carpetas vulnerables en Google resulta indetectable para los defensores, en comparación a utilizar herramientas activas como el escáner `Dirb` o `Nmap` sobre el servidor de la empresa?

**➡️ Siguiente nota:** [[04 - Ingeniería Social y Phishing]]
