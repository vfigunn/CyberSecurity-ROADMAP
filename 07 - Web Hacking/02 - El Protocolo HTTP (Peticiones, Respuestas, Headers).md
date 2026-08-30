# 02 - El Protocolo HTTP (Peticiones, Respuestas y Headers)

## 🎯 Objetivos
- Entender el funcionamiento "debajo del capó" del protocolo HTTP.
- Diferenciar entre los verbos (métodos) críticos: GET, POST, PUT y DELETE.
- Comprender el poder de los Headers (Cabeceras) y los Códigos de Estado (Status Codes).

---

## 🧠 Concepto: El Idioma de la Web

Vimos en el módulo de [[02 - Networking/23 - Puertos y Servicios (El Mapa)|Networking (Puertos)]] que la web opera en los puertos TCP 80 (HTTP) y TCP 443 (HTTPS - Cifrado). 

El protocolo HTTP (HyperText Transfer Protocol) es extremadamente simple, porque está basado en texto plano. Es una charla estricta entre un Cliente (tu navegador/Frontend) y el Servidor (Backend). Funciona siempre como un **Solicitud / Respuesta (Request / Response)**. El servidor jamás habla si el cliente no le pregunta primero.

---

## 📤 1. La Petición del Cliente (HTTP Request)

Cuando abrís tu navegador e intentás entrar a tu cuenta bancaria, detrás de escena tu Chrome fabrica un enorme bloque de texto y lo dispara hacia el servidor. Este bloque tiene 3 partes principales:

### Parte A: El Verbo y la Ruta
La primera línea de la petición dice *QUÉ* querés hacer y en *DÓNDE*.
- **GET:** *"Servidor, dame datos"*. (Ej: Quiero ver mi perfil).
- **POST:** *"Servidor, te envío datos nuevos"*. (Ej: Enviar el usuario y contraseña para loguearse, o subir una foto).
- **PUT:** *"Servidor, actualizá o sobrescribí esto"*. (Ej: Cambiar mi nombre de usuario).
- **DELETE:** *"Servidor, borrá esto"*.
> `POST /api/v1/login HTTP/1.1`

### Parte B: Los Headers (Cabeceras)
Son líneas de configuración (Metadatos). Le dicen al servidor quién sos, qué navegador usás, y en qué formato esperás que te respondan.
- `Host: www.banco.com` (El destino).
- `User-Agent: Mozilla/5.0...` (Le dice que usas Chrome en Windows).
- **`Cookie: session_id=x8f9...`** (VITAL en Hacking: Tu pulsera de VIP para mantenerte logueado).

### Parte C: El Body (Cuerpo)
*Nota: Las peticiones GET no tienen Body (porque solo estás pidiendo).*
Las peticiones POST tienen Body. Acá viajan los datos reales (usualmente en formato JSON) separados de los Headers por una línea en blanco.
```json
{
  "usuario": "juan",
  "clave": "12345"
}
```

---

## 📥 2. La Respuesta del Servidor (HTTP Response)

El servidor recibe ese texto, la API y la Base de datos procesan tu login, y te escupen un texto de vuelta:

### Parte A: El Código de Estado (Status Code)
La primera línea te da el resultado numérico:
- **1xx (Informativo)**
- **2xx (Éxito):** `200 OK` (Todo salió perfecto) o `201 Created` (Tu usuario se creó bien).
- **3xx (Redirecciones):** `301 / 302` (La página ya no está acá, te envío a otra URL automáticamente).
- **4xx (Errores del Cliente):** Vos te equivocaste.
  - `400 Bad Request`: Mandaste un JSON mal escrito.
  - `401 Unauthorized`: Contraseña incorrecta (O no enviaste la cookie).
  - `403 Forbidden`: Estás logueado, pero sos un empleado normal intentando acceder a una URL de Gerentes.
  - `404 Not Found`: Esa página no existe.
- **5xx (Errores del Servidor):** El programador de la empresa falló.
  - `500 Internal Server Error`: El servidor explotó (Falla de base de datos o de lógica). ¡A los hackers les encantan los 500 porque significa que rompieron algo lógicamente!

### Parte B y C: Headers y Body
- El servidor te responde con sus propios Headers (ej: `Content-Type: application/json` para avisarte qué te mandó).
- El Body de respuesta contendrá tus datos solicitados (ej. `{"saldo": 500}`).

---

## ❓ ¿Por qué es el pan de cada día del Hacking Web?

En Web Hacking, los atacantes NUNCA usan el navegador (Google Chrome) para probar ataques, porque el navegador te limita a usar los botones de la pantalla.

Los atacantes (y vos) usarán herramientas llamadas **Proxies de Intercepción (Como Burp Suite o OWASP ZAP)**. 
- Estas herramientas se colocan "en medio" de tu navegador y el Internet.
- Cuando le hacés clic a "Log In" en Chrome, la petición HTTP queda *congelada* en la herramienta.
- Desde ahí, el atacante lee el texto crudo, **modifica los Headers, inyecta código malicioso en el Body JSON, o cambia el verbo de GET a DELETE**.
- Luego presiona "Adelante (Forward)" enviando la petición fraudulenta al servidor para ver si la acepta o colapsa. (La manipulación manual de peticiones HTTP es el 90% de este trabajo).

---

## 📌 Must Know (Imprescindible)
- Qué hace cada Verbo HTTP Crítico: **GET** (Leer), **POST** (Crear/Enviar oculto), **PUT** (Modificar), **DELETE** (Borrar).
- Entender profundamente las familias de códigos de error: **200** (OK), **401** (No autenticado), **403** (No autorizado/Permisos), **404** (No encontrado), **500** (Error del backend).
- El concepto del **Proxy de Intercepción**: Intervenir, leer y manipular manualmente las peticiones HTTP de texto plano antes de que salgan al servidor.

---

## 🔄 Preguntas de repaso
1. Haces clic en un enlace a un artículo de un blog que lees todos los días. Detrás de escena, tu navegador hace la solicitud por vos. ¿Qué Verbo HTTP (Método) se está enviando en la primera línea de la petición hacia el servidor del blog?
2. Usando una herramienta de Hacking como `Burp Suite`, interceptás una petición HTTP POST hacia `https://tienda.com/api/usuario`. Decidís borrar tu clave de sesión (Cookie) y presionar "Adelante" para ver qué sucede. Basado en el estándar de códigos de estado, ¿qué número numérico (4xx) específico esperarías que te devuelva el servidor web al notar que eres un desconocido?
3. Un atacante intenta realizar una compra de $10,000 en una web mediante una petición POST, pero inyecta letras en lugar de números en el campo `"cantidad": "letras"` de su Body JSON. El servidor web choca, crashea por completo la aplicación Python y devuelve una pantalla de error crudo en lugar del mensaje de "Formato incorrecto". ¿Qué código de estado de la familia de los errores 5xx devuelve el servidor indicando un fallo total en el Backend?

**➡️ Siguiente nota:** [[03 - El Proyecto OWASP (El Top 10 de las Vulnerabilidades)]]
