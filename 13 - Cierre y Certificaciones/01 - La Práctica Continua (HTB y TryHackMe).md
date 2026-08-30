# 01 - La Práctica Continua (HTB y TryHackMe)

## 🎯 Objetivos
- Conocer los "Campos de Entrenamiento" oficiales de la industria.
- Diferenciar el enfoque de TryHackMe vs HackTheBox.

---

## 🧠 Concepto: Hackear Es Ilegal

Tenés las herramientas. Sabés usar SQLMap, Nmap, y Metasploit.
Pero no podés salir a escanear a tus vecinos ni a las páginas web de tu ciudad, porque es **un delito federal**.

¿Cómo gana experiencia práctica un Hacker si no puede practicar en el mundo real?
La respuesta son los **Laboratorios de Simulación y CTF (Capture The Flag)**. 
Existen empresas gigantescas que montan máquinas virtuales intencionalmente vulnerables en la nube. Te conectás a ellas por VPN de forma gratuita y podés destrozarlas legalmente.

---

## 🟢 TryHackMe (El Mejor Punto de Partida)

[TryHackMe (THM)](https://tryhackme.com/) es la plataforma definitiva para transicionar desde este Roadmap Teórico hacia la consola.

- **Filosofía:** THM no te tira al mar sin chaleco salvavidas. THM tiene "Salas (Rooms)". Cada sala es una computadora vulnerable con un tutorial paso a paso guiado a un costado de la pantalla.
- **Flujo:** Te hace preguntas ("¿Qué puerto está abierto?"). Corrés Nmap, ves el puerto, escribís la respuesta y ganás puntos. Luego te guía a explotarlo.
- **Recomendación:** Es tu parada número 1. Hacé los "Pathways" de *Junior Penetration Tester* y *Offensive Security*. Vas a mecanizar todos los comandos de Linux.

---

## 🟩 HackTheBox (Las Ligas Mayores)

[HackTheBox (HTB)](https://www.hackthebox.com/) es la meca del Hacking global. Las empresas miran tu perfil de HTB a la hora de contratarte.

- **Filosofía:** Cero ayudas. Cero tutoriales. Te entregan una IP: `10.10.11.200`. Te dicen *"Esa máquina es Windows. Tenés que hackearla y encontrar un archivo de texto llamado flag.txt en el escritorio del Administrador. Suerte"*.
- **Flujo (CTF):** Hacés reconocimiento desde cero. Identificás que es un servidor web, encontrás una vulnerabilidad en un plugin de WordPress, ganás tu Reverse Shell, hacés Escalada de Privilegios Local a SYSTEM, extraés el Hash y conseguís la Bandera (La contraseña).
- **Pro-Labs:** HTB tiene laboratorios masivos que simulan corporaciones enteras con Active Directory. Si lográs resolverlos, HTB te emite certificados que son extremadamente respetados en entrevistas laborales.

---

## 📌 Must Know (Imprescindible)
- **CTF (Capture The Flag):** La modalidad de juego/aprendizaje del hacking. Consiste en vulnerar un sistema para llegar al nivel más alto y leer un archivo de texto secreto (La bandera), demostrando que obtuviste control total.
- Nunca intentes auditar o escanear infraestructuras sin un documento firmado (**Reglas de Compromiso / RoE**). Las plataformas como THM y HTB son los únicos espacios legales para afilar el hacha todos los días.

**➡️ Siguiente nota:** [[02 - El Mapa de Certificaciones Mundiales]]
