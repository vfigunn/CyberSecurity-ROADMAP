# 06 - Post-Explotación (Pivoting y Persistencia)

## 🎯 Objetivos
- Entender que conseguir acceso a una computadora es solo el 10% del trabajo.
- Aprender cómo saltar de una máquina a otra usando Enrutamiento (Pivoting).
- Conocer cómo evitar perder el acceso ganado (Persistencia).

---

## 🧠 Concepto: La Post-Explotación

El hacker novato celebra cuando obtiene una sesión Meterpreter. El hacker profesional sabe que ese es recién el paso 1.

La fase de **Post-Explotación** define todo lo que hacés *después* de vulnerar la máquina.
El objetivo final de cualquier red corporativa nunca está expuesto al público. El servidor de Base de Datos de Finanzas vive en una sub-red privada sin acceso a Internet. Para hackearlo, primero tenés que hackear el débil Servidor Web público de la empresa.

Una vez que sos dueño del Servidor Web, usás la técnica de Pivoting.

---

## 🔀 Pivoting (Saltando entre Redes)

**Pivoting** significa usar una máquina hackeada (comprometerla) como un puente o un enrutador malicioso para lanzar ataques hacia otras máquinas que están más profundo en la red y que vos no podrías alcanzar desde tu casa.

**El Flujo del Pivoting en Metasploit:**
1. Lográs inyectar `Meterpreter` en el Servidor Web (Máquina 1).
2. Estando adentro de la Máquina 1, mirás sus tarjetas de red (`ipconfig`).
3. Descubrís que la Máquina 1 está conectada a Internet, pero además tiene una segunda tarjeta de red conectada a la `Subred Secreta 10.0.0.X`. (Desde tu casa, es imposible que hagas Ping a la 10.0.0.X porque está detrás del Firewall de la empresa).
4. Usando Meterpreter, usás el comando mágico: `run autoroute -s 10.0.0.0/24`.
5. Acabás de convertir a Meterpreter en un Router. 
6. Te salís a la consola principal de Metasploit. Configurás un nuevo escáner (o un nuevo Exploit). Y en la variable objetivo `RHOST` ponés la IP del Servidor de Finanzas: `set RHOST 10.0.0.50`.
7. Metasploit, al disparar el misil, se da cuenta mágicamente de que no puede llegar ahí por Internet. Entonces envía el ataque de forma invisible *a través del túnel* de tu primer Meterpreter. La Máquina 1 recibe el misil y se lo dispara en la cara a la Máquina 2 (Finanzas).

> **En Hacking real, el atacante puede encadenar (Pivotear) a través de 5 o 6 computadoras distintas** como si fueran las mamushkas rusas, hasta llegar al Controlador de Dominio de Active Directory.

---

## 🧟 Persistencia (Asegurando el Acceso)

La debilidad gigante de los exploits (y de Meterpreter en la Memoria RAM) es que si el empleado se cansa de trabajar y apaga la computadora el viernes a la tarde, tu conexión mágica muere y perdiste todo.

Para sobrevivir al reinicio, necesitás configurar **Persistencia**.

Metasploit tiene módulos específicos de post-explotación para esto (ej: `exploit/windows/local/persistence`).
- El módulo agarrará una copia miniatura del Payload y la guardará físicamente en el Disco Duro de la víctima, escondida en un rincón oscuro (`C:\Windows\Temp\virus.exe`).
- Luego, modificará el [[06 - Windows/02 - El Registro de Windows (Regedit)|Registro de Windows (Clave Run)]] para que, cada vez que la PC se encienda, Windows ejecute silenciosamente ese Payload.
- El atacante en su casa simplemente deja su consola de Metasploit en modo "Escucha". Cuando el empleado llegue el lunes y prenda la PC, el virus arrancará solo y le enviará la Reverse Shell nueva al hacker sin necesidad de disparar el exploit de nuevo.

---

## 📌 Must Know (Imprescindible)
- Qué es la **Post-Explotación**: Todo lo que ocurre luego de obtener la Shell (Extraer contraseñas, Mapear la red, Buscar archivos secretos).
- El concepto de **Pivoting**: Usar una máquina vulnerada como "Puente/Router" para redirigir ataques de Metasploit hacia subredes internas inalcanzables.
- Qué es la **Persistencia**: Alterar permanentemente el Sistema Operativo (Registro, Servicios, Tareas Programadas) para asegurar que el malware vuelva a ejecutarse y se conecte al atacante incluso tras un reinicio físico del equipo.

---

## 🔄 Preguntas de repaso
1. Estás auditando desde el exterior una empresa que cuenta con un Servidor Web (DMZ Expuesto) y un Servidor de Base de Datos Privado que almacena tarjetas de crédito. Al intentar escanear el puerto del Servidor Privado desde tu computadora, todas las peticiones mueren en el Firewall. Explicá cómo, utilizando Metasploit y la técnica de "Pivoting", podrías lanzar un Exploit contra el Servidor Privado una vez que hayas hackeado el Servidor Web.
2. Un atacante en etapa de post-explotación corre un módulo de Metasploit que le permite generar una llave (clave) maliciosa en `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run`, la cual apunta hacia un archivo `.exe` escondido en la carpeta temporal. ¿Qué concepto ofensivo acaba de establecer el atacante y contra qué acción común y legítima del empleado se está protegiendo?
3. En tus propias palabras, y tomando en cuenta la volatilidad (temporalidad) de vivir en la Memoria RAM, contrastá por qué un analista ofensivo no puede confiar en que la sesión mágica de `Meterpreter` durará viva por meses, obligándolo a tener que aplicar tácticas del paso 2 (modificar el registro de Windows/Servicios).

**➡️ Siguiente nota:** [[07 - Laboratorio Teórico - Explotando EternalBlue (MS17-010)]]
