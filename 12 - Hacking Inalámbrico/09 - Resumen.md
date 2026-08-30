# 09 - Resumen (Cheat Sheet - Hacking Inalámbrico)

Esta nota agrupa los conceptos físicos (Radiofrecuencia), Hardware, el ciclo vital de Aircrack-ng (Ataque WPA2) y la Ingeniería Social extrema para engaño y Phishing en dispositivos móviles.

---

## 📡 Hardware y Espectro (La Radio)
- **Modo Administrado (Managed):** El modo fábrica y normal de todo dispositivo. La placa Wi-Fi descarta masivamente en hardware y software todos los paquetes del aire que no vayan destinados explícitamente hacia ella.
- **Modo Promiscuo Inalámbrico (Monitor Mode):** Vital y Obligatorio para hackeo. Exige hardware compatible (Ej. Antenas Alfa/Atheros por USB). Remueve la restricción de filtro de la placa para que engulla indiscriminadamente toda la radiofrecuencia (Paquetes Cifrados y Handshakes) de vecinos y ajenos.
- **BSSID y ESSID:** BSSID es la dirección física (MAC Inalterable) del Router. ESSID es el nombre "Texto" modificable del servicio (Ej. "Red-Oficina"). Los programas ofensivos focalizan ataques matemáticos (Radar y Deauth) 100% sobre las direcciones MAC.

---

## 🤝 Protocolo WPA2 y 4-Way Handshake
- **WPA2 y Matemática Dinámica:** Reemplazó al ridículo y deducible WEP. WPA2 no permite que la contraseña vuele plana. Envía y cifra dinámicamente un proceso de 4 vías, que el atacante pasivo no puede revertir en crudo.
- **La única Táctica viable en WPA2:** Todo consta de un ciclo de 2 Etapas y Tiempos Diferentes:
  1. Capturar obligatoria e ineludiblemente el archivo `Handshake .cap` volando en vivo en la puerta del edificio (Usualmente forzando la situación mediante el "Pateo").
  2. Acudir a tu casa/guarida offline, e ingresar dicho archivo en una Trituradora GPU alimentándola con Diccionarios masivos (Fuerza Bruta) intentando adivinar y exponer si el usuario seleccionó o poseía una clave "Común y Descifrable".

---

## 🛠️ La Suite Definitiva: Aircrack-ng
El Hacking "Clásico" en Linux posee 4 Herramientas Maestras para completar el ciclo de vida:
- **`airmon-ng` (Activación):** Mata servicios restrictivos OS locales, y transforma la placa `wlan0` genérica a su forma final depredadora y espía: `wlan0mon`.
- **`airodump-ng` (El Radar ciego):** El Sonar principal para inspeccionar espectros. Permite fijar el canal específico del objetivo, la MAC BSSID, y grabar el ansiado `.cap` (Paso 1).
- **`aireplay-ng` (El Fusil Inyector / Deauth):** El "Pateo" Inyector. Su uso masivo falsifica paquetes de finalización de conexión (Desautenticación) hacia el cliente de forma indiscriminada y no encriptada. Fuerzas la caída de un celular remoto ajeno para obligarlo a escupir su Re-conexión/Handshake automático ante nuestro radar (airodump) oculto que graba silenciosamente.
- **`aircrack-ng` (El Triturador):** La fuerza bruta offline contra la criptografía en casa (Paso 2).

---

## 😈 Evil Twin y DoS: El Fin de la Matemática
Si la clave del corporativo de tu objetivo (A ser crackeada offline) posee 64 Caracteres con símbolos y números, es **Incrackeable e Impermeable** ante Aircrack-ng y Hardware GPU masivo. Por ello:
- **Deauth en Bucle (DDoS Inalámbrico):** Falsificar masivamente miles de paquetes de desconexión sin cesar contra un Access Point específico logra aislar e impedir físicamente que cualquier empleado navegue o funcione en su empresa temporalmente (Bloqueo Total del aire).
- **El Portal Cautivo / Rogue AP (Gemelo Malvado):** Acorralar y engañar. El atacante aprovecha el DDoS/Bloqueo para "Levantar y Emitir" paralelamente su propio Router gratuito desde su notebook, con el idéntico "Nombre (ESSID)" de la empresa objetivo. Los empleados caen en la red, ingresan a una trampa, y en sus navegadores emerge un portal de Ingeniería Social oficialista que exige amablemente el reingreso manual de sus Credenciales Incrackeables de Wi-Fi para restaurar el servicio. El humano colapsa y entrega de forma voluntaria la llave incrackeable.

---

## 📱 OWASP Mobile & BYOD
- **Bring Your Own Device (BYOD):** La empresa moderna no acaba en el cableado físico. El Pentester asume la infección paralela sobre los dispositivos móviles en los bolsillos del personal (AppSec Local).
- **Insecure Data Storage (M2):** Violación crítica de diseño al asumir ciegamente que las Bases Locales Móviles (`SQLite` ocultas) dentro de Android y su sistema de Sandboxing perimetral mantendrán a salvo a los Tokens de sesión JWT de un programa externo.
- **Elevación y Superusuario Final:** La seguridad Sandbox móvil perimetral (Apple y Google) se desintegra universalmente cuando un usuario humano descuidado (o Malware inyectado) ejecuta y adquiere las facultades supremas de **Rooting** (En Android) o efectúa un **Jailbreak** (En iOS). Adquiriendo este permiso masivo "El Dios Móvil", puede visualizar libremente las carpetas /data de los Programas Bancarios vecinos robando en formato plano.

🎉 **¡Felicitaciones! Has dominado el aire. (Fase 12 Finalizada)**
¡Lograste transicionar con éxito total y dominio desde el Pentesting Web, hasta Active Directory central y rematar tu viaje dominando Radiofrecuencia! 
Actualizá tu registro en el archivo de [[Progreso]]. Ahora sólo nos queda 1 módulo final (Proyecto/Cierre) antes de graduarnos.
¿Estás listo para ingresar en nuestra nota de despedida: [[13 - Cierre y Certificaciones/00 - Overview|Módulo 13 - Camino al Profesionalismo y Certificaciones]]?
