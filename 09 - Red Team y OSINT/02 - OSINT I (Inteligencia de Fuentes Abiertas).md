# 02 - OSINT I (Inteligencia de Fuentes Abiertas)

## 🎯 Objetivos
- Conocer la fase inicial y primordial de cualquier ciberataque corporativo.
- Entender el concepto de OSINT y los límites legales.
- Aprender las herramientas básicas para escanear a una empresa antes de tocarla.

---

## 🧠 Concepto: La Fase Cero

Si te contratan para robar el Banco Nacional en una campaña de Red Team, no encendés Metasploit el primer día para lanzarle exploits a la puerta del banco (te van a detectar y bloquear tu IP en 5 minutos). 
El primer mes completo se trata únicamente de investigar. Esta etapa se llama **Reconocimiento**.

El Reconocimiento Pasivo o **OSINT (Open Source Intelligence / Inteligencia de Fuentes Abiertas)** es el arte de extraer montañas de información táctica confidencial de una persona o empresa, **utilizando únicamente la información pública y gratuita que está subida a Internet**. 
*(El límite legal: Si lo podés encontrar buscando en Google, en redes públicas o en registros estatales sin hackear ninguna contraseña, es OSINT puro y 100% legal).*

---

## 🏢 OSINT Corporativo (Mapeando la Empresa)

¿Qué busca un hacker de un Banco antes de lanzar el primer misil cibernético?
Busca la superficie de ataque, dominios ocultos, tecnologías usadas y fugas de datos.

### 1. Extracción de Dominios y Subdominios
La empresa dice que su web es `www.banco.com`. Pero internamente tienen cientos de portales. El hacker busca los rincones olvidados.
Herramientas (como `Sublist3r` o `Amass`) buscan pasivamente en Internet hasta encontrar:
- `mail.banco.com`
- `vpn.banco.com`
- `dev-test.banco.com` (¡Un portal de pruebas descuidado por los programadores, blanco perfecto para entrar!).

### 2. DNS y Direcciones IP
Saber quién "aloja" los servidores de la empresa. 
Con herramientas básicas de Linux como `dig banco.com`, el atacante ve si el banco usa los servidores de Amazon (AWS), Azure, o tiene infraestructura propia, para preparar el armamento correcto.

### 3. Fuga de Datos (Data Leaks)
Este es el oro del OSINT corporativo. El atacante va a páginas (como *Pastebin* o repositorios públicos de *GitHub*) y busca el término `@banco.com`. 
A menudo, programadores novatos o antiguos empleados suben (por error) pedazos de código de la empresa a Internet. Si el atacante encuentra un archivo `config.php` subido en Github que dice `password="BancoSeguro123"`, el ataque termina ahí: el atacante ya tiene la contraseña maestra de la base de datos de producción sin haber hackeado nada.

---

## 👤 OSINT de Personas (Mapeando al Empleado)

La empresa tiene un Firewall impenetrable de $500,000 dólares. El atacante sabe que no puede romper el Firewall.
Decide que va a hackear a los humanos (Ingeniería Social), pero antes, necesita saber quiénes son.

Herramientas famosas como **theHarvester** escanean LinkedIn, Twitter, y foros públicos y le entregan al atacante una lista de 500 correos electrónicos reales de la empresa (`j.perez@banco.com`).

**Perfilamiento en profundidad:**
1. El atacante elige un objetivo débil: "Carlos López, Analista Contable del Banco".
2. Va al LinkedIn de Carlos y descubre que entró a la empresa hace 1 mes (Empleado nuevo, no conoce los protocolos de seguridad).
3. Va al Facebook de Carlos (público) y descubre que Carlos es fanático de coleccionar relojes antiguos, y ayer se quejó de que su perro "Bobby" está enfermo.
4. (Pausa técnica: Si la empresa usa el perro o la fecha de cumpleaños como contraseña, un ataque de diccionario pasivo armado con esta información de OSINT destrozará la cuenta de Carlos).
5. El atacante armará un correo de *Phishing* perfecto para la próxima etapa, basado 100% en esta inteligencia.

---

## 📌 Must Know (Imprescindible)
- Qué significa **OSINT (Inteligencia de Fuentes Abiertas)**: El proceso investigativo y de recolección de información sobre un objetivo (personas o infraestructuras) mediante recursos públicos, legales y gratuitos de Internet.
- **Reconocimiento Pasivo:** Significa que el atacante (su IP) NUNCA interactúa ni "toca" directamente los servidores del objetivo. Si el objetivo revisa los registros de su propio Firewall, jamás se enterará de que está siendo investigado. (El atacante busca en Google, no en el Banco).
- El objetivo final del OSINT es **Mapear la Superficie de Ataque** (Subdominios olvidados, Correos de empleados, Bases de datos filtradas) para preparar la fase táctica.

---

## 🔄 Preguntas de repaso
1. Distinguí brevemente entre el concepto de "Reconocimiento Activo" (Ej: Escanear con `Nmap` directamente al servidor web de la víctima) versus el "Reconocimiento Pasivo / OSINT" (Ej: Buscar archivos PDF públicos de la víctima en Google). ¿En cuál de los dos, el Firewall perimetral de la corporación registrará tu Dirección IP local en sus logs de seguridad?
2. Un operador del Red Team se encuentra realizando recolección de OSINT de la empresa objetivo utilizando la herramienta automatizada `theHarvester`. Al analizar los resultados que le devuelve el programa de fuentes públicas (LinkedIn, Google), se topa con una lista de 150 nombres, apellidos y cargos de los empleados. ¿Para qué fase posterior de la campaña ofensiva (Phishing) guardará celosamente esta información el atacante?
3. Buscando en los repositorios públicos de código (GitHub), lográs encontrar un script viejo subido hace 4 años por un desarrollador del Banco que ya fue despedido. El código revela que internamente el banco utilizaba el sistema operativo Windows Server 2008 y la tecnología Java 6. ¿Por qué esta información, aunque vieja, representa un hallazgo de inteligencia de alto valor para enfocar la selección del armamento en la próxima fase de explotación?

**➡️ Siguiente nota:** [[03 - OSINT II (Google Dorks y Shodan)]]
