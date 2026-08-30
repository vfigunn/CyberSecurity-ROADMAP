# 24 - Wireless Networking (Redes Wi-Fi) y Seguridad

## 🎯 Objetivos
- Conocer los estándares 802.11 (Wi-Fi).
- Entender por qué el medio inalámbrico es inherentemente inseguro.
- Analizar la evolución de la seguridad Wi-Fi: WEP, WPA, WPA2 y WPA3.
- Comprender los ataques clásicos (Rogue AP, Evil Twin).

---

## 🧠 Concepto: El aire es de todos

En una red de cables de cobre, si un atacante quiere robar datos, tiene que entrar físicamente al edificio, esquivar al guardia, encontrar un puerto de red y enchufarse.
En una red Wi-Fi (estándar IEEE **802.11**), los datos viajan mediante ondas de radio. Estas ondas no respetan las paredes del edificio; salen a la calle y llegan al estacionamiento.

Cualquier persona sentada en un auto a 100 metros del edificio, con una antena direccional barata y una computadora corriendo Kali Linux (en modo monitor), **puede capturar ("sniffear") todo el tráfico de la red sin necesidad de conectarse a ella**. 
Por esta razón, la Capa 1 y Capa 2 en Wi-Fi requieren medidas criptográficas fortísimas para asegurar la Confidencialidad y la Autenticación. Si el aire no se puede proteger, tenemos que proteger los datos que vuelan por él cifrándolos.

---

## 🛡️ Evolución de la Seguridad Wi-Fi

Para cifrar los datos que viajan por el aire, se usan protocolos de seguridad. Han evolucionado mucho debido a que los anteriores fueron completamente hackeados.

### 1. WEP (Wired Equivalent Privacy)
- Fue el primer intento (1997). Se suponía que daba la "misma privacidad que un cable". 
- **Estado:** Completamente roto.
- Utilizaba algoritmos de cifrado muy débiles y fallos en cómo se generaban las claves (Vector de Inicialización). Hoy en día, un Script Kiddy puede hackear (descubrir la contraseña de) una red WEP en menos de 5 minutos, sin importar qué tan larga sea la contraseña. **Nunca debe utilizarse.**

### 2. WPA (Wi-Fi Protected Access)
- Fue un parche rápido de la industria cuando se descubrió que WEP era inservible. Introdujo TKIP (Temporal Key Integrity Protocol) que cambiaba la clave constantemente para evitar ataques.
- **Estado:** Roto/Obsoleto. Ya no se usa.

### 3. WPA2
- Ha sido el estándar mundial durante 15 años. Abandonó TKIP y adoptó el cifrado militar **AES (Advanced Encryption Standard)** (lo veremos en [[06 - Cryptography/00 - Overview|Módulo 06]]).
- **WPA2-Personal (PSK):** Usa la clásica "Pre-Shared Key" (contraseña) que todos sabemos.
  - *Vulnerabilidad:* Es vulnerable a ataques de Diccionario y Fuerza Bruta. El atacante captura el "4-Way Handshake" (el saludo inicial cuando te conectas) y luego se lleva esa captura a su casa para intentar adivinar la contraseña con supercomputadoras sin estar cerca de la red.
- **WPA2-Enterprise (802.1x):** En lugar de una contraseña para todos, usa un servidor central (RADIUS/Active Directory). Cada empleado se conecta al Wi-Fi usando su propio usuario y contraseña corporativa. Si el empleado es despedido, se borra su usuario y listo, sin tener que cambiarle la contraseña del Wi-Fi a toda la empresa.

### 4. WPA3
- El estándar moderno actual.
- Bloquea los ataques de fuerza bruta offline cambiando la matemática de cómo se acuerda la contraseña inicial (usando el protocolo SAE - Simultaneous Authentication of Equals). Con WPA3, un atacante ya no puede capturar el handshake y adivinar contraseñas fuera de línea.
- *Problema:* Adopción lenta; muchos dispositivos (IoT, Smart TVs viejas, impresoras) no lo soportan.

---

## ☠️ Ataques Inalámbricos Clásicos

Dado que falsificar identificadores en Wi-Fi es fácil, existen otros ataques más allá de romper la contraseña:

### Rogue Access Point (Punto de Acceso Falso)
Un empleado, cansado de que el Wi-Fi de la empresa no llegue bien a su oficina, trae un router Wi-Fi barato de su casa y lo enchufa en la pared de la oficina corporativa. No tiene mala intención, pero ese router (que viene sin contraseña de fábrica) acaba de abrir un puente directo desde el aire de la calle hacia la red LAN interna y segura de la empresa, evadiendo todos los Firewalls.

### Evil Twin (Gemelo Malvado)
Es un ataque activo. El atacante (sentado en la cafetería) configura su laptop para transmitir una red Wi-Fi exactamente con el mismo nombre (SSID) que la red legítima de la cafetería ("Cafe_Free_WiFi"), pero emitiendo una señal de radio mucho más potente.
Los teléfonos y laptops de las víctimas, que están programados para conectarse automáticamente a la señal más fuerte con ese nombre, se desconectan de la cafetería y se conectan a la laptop del atacante. El atacante ahora es un Man-in-the-Middle y puede inspeccionar todo el tráfico o presentar páginas falsas (Phishing) pidiendo credenciales.

---

## 📌 Must Know (Imprescindible)
- Por qué Wi-Fi es la red más vulnerable en L1/L2.
- Conocer la evolución: WEP (Roto), WPA, WPA2 (Estándar, AES), WPA3 (Moderno).
- La diferencia entre WPA2-Personal (misma contraseña para todos) y WPA2-Enterprise (credenciales por usuario / 802.1x).
- Concepto de ataque Evil Twin.

---

## 🔄 Preguntas de repaso
1. Un cliente te dice que su empresa todavía usa seguridad WEP en sus terminales de almacén porque "es compatible con dispositivos antiguos". Como profesional de seguridad, ¿cuál es tu recomendación e inquietud principal?
2. ¿Por qué WPA2-Enterprise es mucho más recomendable para una corporación de 500 empleados que WPA2-Personal (PSK)?
3. Describí brevemente cómo funciona el ataque de "Evil Twin" para robar credenciales.

**➡️ Siguiente nota:** [[25 - Dispositivos de Seguridad en Red]]
