# 01 - Arquitectura de Windows (Procesos y Memoria)

## 🎯 Objetivos
- Conocer la separación estructural entre User Mode (Capa 3) y Kernel Mode (Capa 0).
- Entender qué es un Proceso y un Hilo (Thread) y cómo conviven en la Memoria RAM.
- Identificar los procesos críticos "intocables" del sistema operativo.

---

## 🧠 Concepto: La Torre de Privilegios (Anillos)

A diferencia de Linux, donde "Todo es un archivo", Windows es un sistema operativo altamente estructurado y guiado por eventos, APIs y Objetos.
Para evitar que un virus (o un juego mal programado) destruya tu disco duro, la arquitectura de los procesadores Intel/AMD y de Windows se divide en **Anillos de Privilegios (Rings)**.

1. **Ring 3 - User Mode (Modo Usuario):**
   - Aquí viven TODAS las aplicaciones que abres: Google Chrome, Word, tu videojuego, e incluso tu script de Python. 
   - **Limitación extrema:** Un programa en User Mode *no tiene permiso* para hablar directamente con el Hardware (Disco, Tarjeta de red, RAM de otros programas). 
   - Si tu script de Python quiere abrir un archivo del disco, debe pedirle "por favor" al Kernel usando funciones oficiales (API de Windows). Si el Kernel acepta, el Kernel abre el archivo y se lo devuelve al script.

2. **Ring 0 - Kernel Mode (El Núcleo):**
   - Aquí vive el cerebro de Windows (Ntoskrnl.exe) y los Controladores (Drivers) de tu placa de video, teclado, etc.
   - Tienen control **absoluto y total** sobre el hardware. 
   - Si un proceso en Ring 0 falla, genera la famosa Pantalla Azul de la Muerte (BSOD), porque el sistema entero colapsa.
   - *Nota de Seguridad:* Los Antivirus más avanzados (EDRs) operan en Ring 0 para poder vigilar desde lo más alto a los programas que corren en Ring 3.

---

## ⚙️ Procesos e Hilos (Threads)

Cuando hacés doble clic en `calculadora.exe`, Windows carga ese archivo en la Memoria RAM. En ese momento, nace un **Proceso**.

- **El Proceso:** Es el "contenedor" o la casa. A cada proceso, Windows le asigna un número de identificación único temporal llamado **PID (Process ID)** y le otorga una porción de Memoria RAM privada. *Regla de oro: Un proceso no puede leer la memoria RAM de otro proceso (a menos que se usen técnicas avanzadas de hacking como el Process Injection).*
- **Los Hilos (Threads):** Un proceso en sí mismo es inerte, no hace nada. Los que hacen el trabajo son los Hilos. Son las "manos" del proceso que ejecutan el código matemático en el Procesador (CPU). Un solo proceso (ej. Chrome) puede tener cientos de hilos trabajando al mismo tiempo (uno reproduciendo el video de YouTube, otro escuchando tu teclado).

---

## 🛡️ Los 4 Procesos Críticos de Windows

Como analista deSOC / Blue Team, debes conocer de memoria los procesos madre de Windows. Si ves estos procesos corriendo desde lugares extraños (ej. desde tu carpeta "Descargas"), sabes que estás infectado por un Malware que intenta disfrazarse.

1. **`System` (PID 4 siempre):**
   - Es el Kernel en sí mismo (Ring 0) reflejado en el administrador de tareas.
2. **`smss.exe` (Session Manager Subsystem):**
   - Es el primer proceso "User Mode" que arranca al prender la PC. Se encarga de iniciar el resto de las cosas vitales y la pantalla de inicio de sesión.
3. **`lsass.exe` (Local Security Authority Subsystem Service):**
   - **El Santo Grial de los Hackers.** Es el guardia de seguridad de tu PC. Guarda en su Memoria RAM tus contraseñas y tus tokens temporales de inicio de sesión. 
   - El 99% de las herramientas de robo de credenciales ofensivas (como la legendaria *Mimikatz*) atacan directamente a la memoria de `lsass.exe` para extraer contraseñas en texto claro.
4. **`svchost.exe` (Service Host):**
   - Verás docenas de estos corriendo al mismo tiempo. Es un proceso "contenedor genérico". Windows tiene cientos de servicios invisibles (Windows Update, Audio, Red), y en lugar de tener 100 procesos `.exe` distintos, Windows los agrupa y los corre por debajo de múltiples `svchost.exe`.
   - *Táctica Malware:* Muchos virus se nombran a sí mismos `svchost.exe` para pasar desapercibidos en tu lista de procesos. (La forma de atraparlos es viendo desde qué carpeta se están ejecutando).

---

## 📌 Must Know (Imprescindible)
- La diferencia de poder entre **User Mode (Ring 3)** (Aplicaciones limitadas) y **Kernel Mode (Ring 0)** (Control Total y absoluto del hardware).
- Un Proceso es un contenedor con memoria aislada, un Hilo es la unidad de ejecución en la CPU.
- Memorizar **`lsass.exe`** como el objetivo número 1 del robo de credenciales en Windows.

---

## 🔄 Preguntas de repaso
1. Estás desarrollando un Malware avanzado de tipo *Rootkit*. Para que tu malware sea totalmente invisible a los ojos de cualquier programa del usuario (incluyendo al Administrador de Tareas y Antivirus básicos), ¿en qué anillo de privilegios (Ring 0 o Ring 3) intentarías inyectar el código de tu Rootkit instalándolo como un "Driver falso" del sistema?
2. Un empleado abre el Administrador de Tareas y ve un proceso llamado `lsass.exe` corriendo desde la ruta `C:\Users\Juan\Desktop\`. Basado en tu conocimiento sobre los procesos críticos del sistema (que deberían estar alojados en System32), ¿qué conclusión instantánea sacás sobre ese proceso?
3. Sabiendo que, por diseño arquitectónico, el Proceso A no tiene permiso para leer los datos alojados en la Memoria RAM privada del Proceso B, ¿por qué los creadores de Antivirus necesitan que sus motores funcionen en conjunto con el Kernel del Sistema Operativo para poder analizar comportamientos maliciosos?

**➡️ Siguiente nota:** [[02 - El Registro de Windows (Regedit)]]
