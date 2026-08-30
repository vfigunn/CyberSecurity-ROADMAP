# 10 - Active Directory y Escalada

Bienvenido al Módulo 10. Ya vimos los fundamentos de Windows. Ahora, vamos a adentrarnos en cómo las grandes corporaciones unen miles de máquinas bajo un mismo ecosistema: **El Active Directory (AD)**.

Si querés trabajar como Pentester de redes internas o formar parte de un Red Team, este es el conocimiento que paga los sueldos más altos. 
Casi todas las empresas del planeta (Fortune 500) operan sobre Active Directory. Quien controla el Controlador de Dominio (Domain Controller), controla absolutamente cada byte de información, cada correo electrónico y cada PC de la corporación.

En este módulo, vamos a aprender cómo los atacantes logran pasar de ser un empleado sin privilegios en la recepción, a coronarse como los Reyes de la red.

---

## 📚 Notas del Módulo

### Reconocimiento y Conceptos
1. [[01 - La Anatomía del Dominio y Escalada de Privilegios]]
2. [[02 - Mapeo y Reconocimiento (BloodHound)]]

### Vectores de Ataque en AD
3. [[03 - Envenenamiento de Red (LLMNR y Responder)]]
4. [[04 - Ataques NTLM (Pass-The-Hash y Relay)]]
5. [[05 - Ataques Kerberos (Kerberoasting y AS-REP Roasting)]]

### El Colapso de la Red
6. [[06 - La Caída del DC (DCSync y NTDS.dit)]]
7. [[07 - Persistencia Absoluta (Golden y Silver Tickets)]]

## 🔬 Práctica y Evaluación
8. [[08 - Laboratorio Teórico - Destruyendo el Bosque]]
9. [[09 - Ejercicios]]
10. [[10 - Evaluación]]

## 📝 Resumen
11. [[11 - Resumen]]

---

## 🎯 ¿Qué vas a lograr?

Al finalizar este módulo, vas a ser capaz de:
- Diferenciar la Escalada de Privilegios Local de la Escalada de Dominio.
- Entender cómo funciona la herramienta BloodHound y por qué revolucionó el Hacking de AD (Aplicando la Teoría de Grafos).
- Conocer cómo extraer y robar Hashes (Pass-The-Hash).
- Entender las flaquezas del protocolo Kerberos y cómo solicitar Tickets (Kerberoasting) para romperlos offline.
- Saber cómo los hackers forjan "Billetes Falsos" (Golden Tickets) para mantener acceso a la empresa para siempre, aunque los administradores cambien las contraseñas.

**➡️ Siguiente nota:** [[01 - La Anatomía del Dominio y Escalada de Privilegios]]
