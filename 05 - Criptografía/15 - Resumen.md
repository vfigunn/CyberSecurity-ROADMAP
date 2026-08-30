# 15 - Resumen (Cheat Sheet - Criptografía)

Esta nota agrupa los conceptos, algoritmos y reglas criptográficas fundamentales para proteger la Tríada CIA y diseñar arquitecturas de seguridad inquebrantables.

---

## 🏗️ La Trilogía de la Transformación
La regla de oro de la industria: No son sinónimos.

1. **Encoding (Codificación):**
   - *Objetivo:* Compatibilidad de transporte de datos.
   - *Atributos:* Público, sin llaves, 100% reversible.
   - *Algoritmos:* **Base64** (Reconocible por el relleno `=`), **URL Encoding** (`%20` = espacio).
2. **Hashing (Resumen Matemático):**
   - *Objetivo:* **Integridad** (Verificar manipulaciones).
   - *Atributos:* Destructivo, unidireccional (irreversible), tamaño de salida fijo, Efecto Avalancha.
   - *Algoritmos:* MD5 (Roto), SHA-1 (Roto), **SHA-256** (Estándar Seguro).
3. **Encripción (Cifrado):**
   - *Objetivo:* **Confidencialidad** (Mantener secretos).
   - *Atributos:* Protegido matemáticamente, requiere Llaves Secretas, es bidireccional (Reversible solo con la llave).

---

## 🔑 Contraseñas y Cracking
- **Colisión:** El desastre matemático donde dos archivos distintos generan el mismo Hash.
- **Rainbow Tables:** Bases de datos de atacantes con millones de hashes precalculados para romper contraseñas rápidamente.
- **Salt (Sal):** Texto aleatorio añadido a la contraseña *antes* del hash para hacerla única e inmunizarla contra Rainbow Tables.
- **Estándares modernos para BD:** Bcrypt, Argon2.

---

## 🔒 Sistemas Criptográficos (Encripción)

### 1. Criptografía Simétrica (Llave Compartida)
- *Regla:* Misma llave exacta para cerrar y abrir.
- *Ventaja:* Brutalmente **rápida** e ideal para encriptación de datos masivos (Discos, Bases de Datos, Reposo).
- *Desventaja:* El problema de distribución seguro de la llave por Internet.
- *Algoritmos:* DES (Obsoleto/Roto), **AES-256** (Estándar Inquebrantable).

### 2. Criptografía Asimétrica (Llave Pública/Privada)
- *Regla:* Lo que la llave Pública cierra (encripta), SOLO la Privada lo abre.
- *Uso clásico:* Para Confidencialidad (Te envían cosas con tu Llave Pública).
- *Ventaja:* Resuelve el envío por redes públicas inseguras.
- *Desventaja:* Terriblemente **lenta** (Consume mucha CPU).
- *Algoritmos:* **RSA** (Basado en números primos colosales), **ECC / Curvas Elípticas** (Llaves cortas pero ultra eficientes, ideal celulares/IoT).

### 3. El Sistema Híbrido y Diffie-Hellman (En Tránsito)
- Se utiliza Asimétrico 1 vez para acordar/enviar una clave temporal, y luego se pasa al Simétrico (AES) para velocidad.
- **Diffie-Hellman (DH):** Matemáticas de mezcla que permiten acordar una clave secreta a través de un canal espiado sin enviarla.
- **Forward Secrecy (PFS):** Las llaves DH temporales se destruyen tras la sesión. Si el servidor es vulnerado en el futuro, los chats del pasado siguen protegidos.

---

## 🖋️ Firmas, Identidad y PKI

- **Firma Digital (No Repudio):** Es criptografía asimétrica en reversa. Encriptas un **Hash** con tu propia *Llave Privada*. Cualquiera puede abrirlo con tu *Pública*, garantizando matemáticamente que fuiste vos (Autor).
- **PKI (Infraestructura de Llave Pública):** Ecosistema de confianza en Internet.
- **Certificado X.509:** Documento que ata una Llave Pública a la Identidad Real de una empresa.
- **Autoridad Certificadora (CA):** Entidad "Notarial" (ej. DigiCert) que utiliza su inmensa Llave Privada para colocar su Firma Digital en los certificados de otras empresas, otorgándoles validez universal y encendiendo el "Candadito Verde" (HTTPS).

---

## 🕵️ Esteganografía
- *Regla:* Seguridad por Oscuridad. No encripta el mensaje, **oculta la existencia** del mensaje.
- *Técnica reina:* Manipulación LSB (Least Significant Bit). Reemplazar los últimos bits de los píxeles de una imagen para inyectar datos (Archivos, órdenes de Malware) sin alterar la imagen ante el ojo humano.

---
🎉 **¡Felicitaciones por dominar la Criptografía (Fase 6)!**
Ahora comprendés exactamente cómo funciona la arquitectura de confianza del planeta entero. Actualizá tu archivo [[Progreso]] y preparate para ensuciarte las manos. En el [[06 - Windows/00 - Overview|Módulo 06 - Windows & Active Directory]], ingresaremos a la arquitectura interna de Microsoft y al corazón corporativo que el 90% de los atacantes (Ransomware) buscan destruir.
