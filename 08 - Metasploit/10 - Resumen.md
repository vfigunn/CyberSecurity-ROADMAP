# 10 - Resumen (Cheat Sheet - Metasploit)

Esta nota agrupa los comandos esenciales de consola, la taxonomía de la explotación y los conceptos estratégicos abordados en el Módulo de Metasploit Framework.

---

## 🏗️ Taxonomía y Armamento
- **Exploit:** El misil. El código que descubre la brecha, rompe el límite de seguridad (aprovechando una vulnerabilidad del objetivo) y abre la puerta inicial (Pero por sí solo, no toma el control).
- **Payload:** La cabeza nuclear. El código destructivo que viaja dentro del Exploit y se ejecuta en la víctima para robar los datos o devolvernos una terminal remota (Shell).
- **Auxiliary:** Módulos de apoyo pasivos/activos que NO rompen memoria (Fuerza bruta FTP, Escáneres de red, Enumeración).
- **Encoders:** Algoritmos (ej. `shikata_ga_nai`) que alteran/ofuscan el código del Payload original buscando cambiar su huella para intentar evadir los Antivirus (Evasión estática).

---

## 🔌 Tipos de Control y Conexión (El engaño de Red)
- **Bind Shell (Conexión Directa):**
  - El Atacante llama a la Víctima.
  - Fracasa masivamente hoy en día porque los Firewalls corporativos bloquean, por defecto, toda conexión Entrante (Inbound).
- **Reverse Shell (Conexión Inversa - El Estándar ORO):**
  - La Víctima es manipulada (desde el Payload) para que sea ella quien llame por iniciativa propia a la casa del Atacante.
  - Como es una conexión de Salida (Outbound), evade exitosamente el 90% de los Firewalls que permiten tráfico web saliente.
- **Staged (En Fases) vs Stageless (Una Pieza):**
  - **Staged ( `/` ):** Ideal para espacios reducidos. En la Fase 1, inyecta un diminuto "Stager/Llamador". Éste conecta al hacker y descarga el verdadero virus pesado a la RAM. (Ej. `meterpreter/reverse_tcp`).
  - **Stageless ( `_` ):** El paquete viaja entero (Inline) de un solo golpe. (Ej. `meterpreter_reverse_tcp`).

---

## 💻 El Ecosistema `msfconsole` (Flujo de Comando)
Memorizar los 5 pasos universales:
1. `search [término]` - Encontrar el módulo.
2. `use [ruta_del_modulo]` - Cargar y equipar.
3. `show options` - Revisar qué parámetros obligatorios (IPs/Puertos) están pendientes de llenar.
4. `set RHOSTS [ip_victima]` / `set LHOST [ip_atacante]` - Puntería del misil y configuración de retorno.
5. `exploit` (o `run`) - Ejecutar el ataque sobre el objetivo.

---

## 👻 El Armamento Final: Meterpreter y Post-Explotación
- **Meterpreter:** Payload hiper-avanzado diseñado y escrito por Rapid7. No es una simple terminal de texto (Command Shell). Es un entorno de post-explotación interactivo e inteligente que vive y se procesa **exclusivamente en la Memoria RAM de la máquina (In-Memory Execution)**, logrando un alto grado de evasión y sigilo "Fileless" (No guarda exes ni rastros evidentes en el disco local).
- **Comandos Críticos Meterpreter:**
  - `sysinfo` (Muestra datos del SO del objetivo).
  - `getuid` (Revela el nivel de privilegios actuales, ej. SYSTEM).
  - `hashdump` (Secuestra y vuelca a pantalla todos los hashes locales -NTLM- robados para cracking offline).
  - `upload` / `download` (Para extracción o siembra local).
- **Post-Explotación (Conceptos Clave):**
  - **Persistencia:** Plantar llaves en el registro o crear tareas programadas silenciosas para que tu Payload y tu conexión Inversa revivan tras el reinicio natural de la máquina.
  - **Pivoting (Enrutamiento malicioso):** Usar la máquina recién hackeada (que posee acceso interno/Intranet a subredes secretas) como un "Túnel" por el cual disparar nuevos Exploits de Metasploit desde tu casa, salteando físicamente las barreras perimetrales.

---
🎉 **¡Finalizaste el Módulo de Metasploit (Fase 9)!**
Con este entrenamiento, pasaste de las bases teóricas de la web a saber cómo empuñar, apuntar y disparar un Exploit ofensivo real para ganar control absoluto, aplicar Pivoting y mantener el acceso táctico. No te olvides de marcar tu [[Progreso]]. Ahora, aplicaremos todas nuestras habilidades combinadas en el arte máximo de la simulación e inteligencia corporativa. Estamos listos para adentrarnos en el cierre del roadmap técnico: El [[09 - Red Team y OSINT/00 - Overview|Módulo 09 - Red Team (Simulación de Amenazas y Reconocimiento)]].
