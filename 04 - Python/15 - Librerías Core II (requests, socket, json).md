# 15 - Librerías Core II (`requests`, `socket`, `json`)

## 🎯 Objetivos
- Dominar el manejo universal de datos con la librería `json`.
- Entender cómo hacer peticiones HTTP/Web amigables con `requests` (Librería externa - Pip).
- Comprender el contacto de bajo nivel (Red Cruda) utilizando `socket`.

---

## 🧠 Concepto: Python en la Red

En la nota anterior vimos cómo interactuar con el Sistema Operativo (Archivos y Comandos). Pero la Ciberseguridad trata casi exclusivamente sobre Redes e Internet. Necesitamos que Python hable hacia afuera.

Para ello, veremos 3 librerías críticas: una para formatear datos, otra para alto nivel (Capa 7), y otra para bajo nivel (Capa 4).

---

## 📦 1. Formateo: La librería `json`

En la [[05 - Estructuras de Datos (Diccionarios)|Nota 05]] dijimos que el formato **JSON** es el lenguaje universal que usan las APIs (sistemas) de Internet para comunicarse. Visualmente, es casi idéntico a un Diccionario de Python.
El módulo nativo `json` (viene preinstalado) sirve como "Traductor" bidireccional entre textos de Internet y Diccionarios vivos de Python.

- **`json.loads()` (Load String - Cargar de texto):** Convierte un String gigante (que bajaste de Internet) en un Diccionario real de Python para que puedas manipularlo y acceder a sus Claves.
- **`json.dumps()` (Dump String - Volcar a texto):** Hace lo opuesto. Toma un Diccionario tuyo, y lo serializa transformándolo en un bloque de Texto (String) para que puedas enviarlo por la red de forma compatible.

```python
import json

# Recibiste este texto crudo (String) desde una API de una cámara IP hackeada
texto_de_internet = '{"usuario": "admin", "clave": "12345"}'

# Traducción Mágica: De String a Diccionario
diccionario = json.loads(texto_de_internet)

print(diccionario["clave"]) # Imprime 12345
```

---

## 🌐 2. Alto Nivel (HTTP): La librería `requests`

*(Ojo: Esta librería **NO** viene instalada por defecto. Es tan buena que se considera el estándar de la industria, pero debés descargarla usando `pip install requests` en tu terminal primero).*

Es tu navegador web (Chrome) comprimido en código. En Hacking Web (Bug Bounty, Pentesting de APIs), escribirás docenas de scripts con `requests`.

Te permite hacer peticiones HTTP `GET` (Descargar) y `POST` (Enviar formularios/login).

```python
import requests

url = "http://web_de_prueba.com/api/v1/usuarios"

# Hacemos una petición GET (como si escribieras la URL en tu navegador)
respuesta = requests.get(url)

# Verificamos si la página cargó bien (HTTP 200 OK)
if respuesta.status_code == 200:
    print("¡Conexión Exitosa!")
    # Podemos extraer el texto (HTML/JSON) de la respuesta
    print(respuesta.text)
else:
    print(f"Error. El servidor devolvió: {respuesta.status_code}")
```
*Las famosas herramientas de fuerza bruta de login de internet están escritas internamente utilizando bucles `for` y `requests.post()` para enviar miles de contraseñas por segundo.*

---

## 🔌 3. Bajo Nivel (TCP/UDP): La librería `socket`

En el [[02 - Networking/16 - TCP vs UDP|Módulo 02 - Networking]] aprendimos que en la Capa 4 de Transporte existen los Sockets (La unión de una IP + Un Puerto). 
La librería nativa `socket` de Python es la forma más cruda, primitiva y poderosa de comunicarse. 
- No habla HTTP. No sabe lo que es una página web. 
- Simplemente abre un "tubo de comunicación" directo entre dos máquinas a través del protocolo TCP o UDP para enviar y recibir texto crudo o Bytes.

Es extremadamente complejo de usar correctamente, pero **es la base de la creación de Reverse Shells (Malware) y Escáneres de Puertos.**

```python
import socket

ip_objetivo = "192.168.1.1"
puerto = 80

# 1. Creamos el objeto Socket (Configurado como IPv4 y protocolo TCP)
mi_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Configurar un límite de tiempo (Si no responde en 2s, falla)
mi_socket.settimeout(2)

# 2. Intentamos conectar. (Usamos try/except para manejar fallos como vimos en la Nota 11)
try:
    mi_socket.connect((ip_objetivo, puerto))
    print(f"¡El puerto {puerto} está ABIERTO!")
    
except (socket.timeout, ConnectionRefusedError):
    print(f"El puerto {puerto} está CERRADO o FILTRADO.")

# 3. Cerramos el socket como buenos ciudadanos
finally:
    mi_socket.close()
```

---

## 📌 Must Know (Imprescindible)
- Diferencia entre `json.loads` (De Internet hacia el Diccionario Python) y `json.dumps` (Del Diccionario al Texto para Internet).
- Entender que `requests.get()` es como abrir Chrome y poner una URL. 
- Saber que para ver el estado de un servidor (ej. el error "404 Not Found") leemos la variable `respuesta.status_code`.
- Concepto de la librería `socket` como el acceso de bajo nivel (TCP/IP) para Hacking e Infraestructura.

---

## 🔄 Preguntas de repaso
1. Estás leyendo la documentación de la API del Threat Intelligence de VirusTotal. La documentación dice que, tras hacer una petición, el sistema te devolverá un enorme bloque de texto en formato `JSON` estructurado. ¿Qué función de la librería nativa de Python utilizarías sobre ese texto para transformarlo instantáneamente en un Diccionario interactivo y fácil de procesar?
2. Escribís un script con la librería `requests` para intentar leer una página confidencial (`admin_panel.php`). Al imprimir la variable `respuesta.status_code`, el script te devuelve un número `403`. Considerando tus conocimientos de los códigos de estado HTTP (Módulo Networking), ¿qué deducís que pasó con tu petición?
3. Para construir un pequeño malware de "Ransomware" que extrae los datos de la PC víctima y los sube a la base de datos de tu servidor C2 a través de Internet (Capa 7 - HTTP), ¿cuál de las dos librerías preferirías utilizar por su facilidad y alto nivel: `requests` o `socket`? ¿Y por qué?

**➡️ Siguiente nota:** [[16 - Laboratorio 1 - Creador de Hashes]]
