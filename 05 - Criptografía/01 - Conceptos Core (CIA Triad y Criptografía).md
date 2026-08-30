# 01 - Conceptos Core (CIA Triad y Criptografía)

## 🎯 Objetivos
- Conocer la Tríada CIA, el concepto central que gobierna absolutamente todo en la industria de la Ciberseguridad.
- Entender cómo la criptografía es la herramienta principal para defender cada uno de los tres pilares de la tríada.
- Diferenciar entre Criptografía en Tránsito, en Reposo y en Uso.

---

## 🧠 La Tríada CIA (El Santo Grial)

Toda decisión, presupuesto, firewall, o algoritmo que una empresa implementa en Ciberseguridad persigue proteger uno o más de los pilares de la **Tríada CIA (Confidentiality, Integrity, Availability)**.

### 1. Confidencialidad (Confidentiality)
Garantiza que la información solo sea accesible y legible por las personas autorizadas.
- *Ejemplo de violación:* Un atacante logra robar la base de datos de tarjetas de crédito de los clientes y las lee en su casa.
- *Herramienta criptográfica:* **Encripción (Cifrado)**. Convertimos los números de tarjeta en texto ilegible; si el hacker los roba, solo verá basura matemática.

### 2. Integridad (Integrity)
Garantiza que la información **no haya sido alterada o modificada** en el camino, ya sea de forma accidental (error de disco) o maliciosa, y que mantenga su precisión original.
- *Ejemplo de violación:* Un usuario transfiere $100 al banco, pero un atacante intercepta el paquete en el router Wi-Fi y modifica el número de $100 a $10.000.
- *Herramienta criptográfica:* **Hashing y Firmas Digitales**. Generan una "huella digital" del archivo original. Si se modifica un solo bit del archivo, la huella cambiará drásticamente, delatando la trampa.

### 3. Disponibilidad (Availability)
Garantiza que la información y los sistemas estén accesibles y funcionando cuando los usuarios legítimos los necesiten.
- *Ejemplo de violación:* Un ataque DDoS (Ataque de Denegación de Servicio) satura el servidor de Netflix impidiendo que los usuarios puedan ver películas.
- *(Nota: La Criptografía ayuda poco aquí. De hecho, a veces empeora la disponibilidad, ya que desencriptar datos en tiempo real requiere mucho poder de CPU, volviendo al servidor más lento).*

### 4. Non-Repudiation (No Repudio) - El "Cuarto" pilar
Un concepto muy cercano a la Tríada. Garantiza que una persona no pueda negar haber realizado una acción o haber enviado un mensaje.
- *Ejemplo:* Alguien firma un contrato digital millonario y luego dice "Yo no fui, alguien hackeó mi cuenta". 
- *Herramienta criptográfica:* **Criptografía Asimétrica (Llaves Privadas)**. Si el mensaje se firmó con tu llave privada secreta, y solo vos tenés acceso a esa llave, es matemáticamente imposible repudiar (negar) la acción.

---

## 🔒 Estados de los Datos

La criptografía se debe aplicar de manera distinta dependiendo de en qué estado se encuentre la información en ese momento preciso.

1. **Datos en Reposo (Data at Rest):**
   La información está almacenada (quieta) en un Disco Duro, un Pendrive o en la Nube (AWS S3).
   *Protección típica:* Cifrado de disco completo (BitLocker, LUKS) o cifrado de la base de datos (AES-256).

2. **Datos en Tránsito (Data in Transit / Motion):**
   La información está viajando por los cables de red, las antenas Wi-Fi o los routers de Internet desde la PC A hacia el Servidor B. Es el momento de mayor vulnerabilidad frente a espionaje (Sniffing / Man-in-the-Middle).
   *Protección típica:* Túneles seguros como TLS/SSL (HTTPS) o SSH. Los datos se cifran antes de salir de la PC, viajan ilegibles por Internet, y se descifran al llegar al servidor.

3. **Datos en Uso (Data in Use):**
   La información fue cargada desde el disco duro hacia la Memoria RAM y el Procesador (CPU) para poder trabajar con ella (ej. Excel está sumando sueldos). 
   Este es el talón de Aquiles actual. Para que la CPU pueda procesar los datos, **los datos deben estar desencriptados en la memoria RAM**. Si un atacante tiene un malware avanzado que lee directamente la memoria RAM, puede robar los secretos aunque el disco duro o la red estén cifrados. (Existen nuevas tecnologías emergentes como la *Computación Confidencial*, pero aún son de nicho).

---

## 📌 Must Know (Imprescindible)
- Memorizar perfectamente las siglas CIA: Confidencialidad, Integridad y Disponibilidad.
- Relacionar Confidencialidad con "Encripción", e Integridad con "Hashes/Firmas".
- Saber que los datos en RAM (Datos en Uso) están generalmente desencriptados, creando un vector de ataque.

---

## 🔄 Preguntas de repaso
1. Un hospital sufre un ataque de Ransomware. El malware cifra los expedientes de todos los pacientes, impidiendo que los doctores puedan leerlos y provocando la cancelación de las cirugías, aunque la información no fue robada ni alterada en su contenido. ¿Cuál de los tres pilares principales de la tríada CIA fue vulnerado?
2. Un empleado malicioso accede a la base de datos de nóminas en el servidor interno y cambia su sueldo de $5,000 a $50,000. ¿Qué pilar de la tríada CIA ha violado directamente?
3. Un analista defensivo utiliza la herramienta `BitLocker` en las laptops de todos los empleados de la compañía. Si un empleado pierde su laptop en un taxi, ¿a qué estado de los datos (Reposo, Tránsito o Uso) está protegiendo principalmente esa herramienta?

**➡️ Siguiente nota:** [[02 - Diferencia entre Encoding, Hashing y Encripción]]
