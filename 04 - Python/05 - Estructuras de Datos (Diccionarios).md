# 05 - Estructuras de Datos (Diccionarios)

## 🎯 Objetivos
- Entender el concepto de pares Clave-Valor (Key-Value).
- Aprender a crear, leer y modificar Diccionarios en Python.
- Comprender por qué el Diccionario es la estructura de datos más crítica para trabajar con APIs web y logs (formato JSON).

---

## 🧠 Concepto: Relacionando Datos

En la nota anterior, vimos que en una [[04 - Estructuras de Datos (Listas, Tuplas y Sets)|Lista]] accedes a la información a través de un índice numérico secuencial (`lista[0]`, `lista[1]`). 

Pero, ¿qué pasa si querés guardar un registro complejo de un usuario, como su "Nombre", su "IP" y su "Rol"?
En un diccionario de papel tradicional, vos buscás una **Palabra (Clave)**, y el diccionario te devuelve su **Significado (Valor)**.
Los Diccionarios de Python (`dict`) hacen exactamente eso. No usan índices numéricos, usan **Claves con nombre (Keys)** para acceder a **Valores (Values)**.

Esto es fundamental porque te permite mapear y organizar información no secuencial (como los atributos de un servidor vulnerado).

---

## 🛠️ Creando un Diccionario

Se definen utilizando **llaves `{ }`** (igual que los Sets), pero adentro tienen que seguir obligatoriamente la estructura `Clave : Valor`, separados por comas.

```python
perfil_atacante = {
    "ip": "185.20.10.5",
    "pais": "Rusia",
    "puertos_escaneados": 1500,
    "es_amenaza": True
}
```
*Observá cómo el valor puede ser un String, un Integer, o un Boolean. ¡Incluso el valor puede ser otra Lista!*

---

## 📖 Interactuando con el Diccionario

### 1. Leer Datos (Get)
En lugar de pasarle un índice numérico entre corchetes, le pasas el nombre exacto de la Clave (en texto).
```python
# Quiero saber desde qué país me están atacando
print(perfil_atacante["pais"])  # Imprime: Rusia
```
*Problema:* Si le pedís una clave que no existe (ej. `perfil_atacante["navegador"]`), Python arrojará un error catastrófico (`KeyError`) y tu programa crasheará (se detendrá).
*Solución profesional:* En lugar de usar corchetes, usá el método **`.get()`**. Si la clave no existe, `.get()` no rompe el programa, simplemente te devuelve "Nada" (`None`), lo cual es seguro.
```python
print(perfil_atacante.get("navegador"))  # Imprime: None (y el script sigue viviendo)
```

### 2. Modificar o Agregar Datos
Para actualizar el diccionario, actuamos como si la clave ya existiera y le asignamos algo nuevo con el `=`.
- Si la clave ya existía, se **Sobrescribe** su valor.
- Si la clave NO existía, se **Crea nueva automáticamente**.
```python
# Sobrescribir
perfil_atacante["puertos_escaneados"] = 2000

# Agregar nueva clave que antes no existía
perfil_atacante["sistema_operativo"] = "Kali Linux"
```

### 3. Extraer todas las Claves o Valores
A veces, en análisis forense, no sabés qué campos te entregó la herramienta de extracción, y necesitás ver un listado rápido de las claves disponibles.
- `perfil_atacante.keys()` -> Devuelve una lista solo con las claves `["ip", "pais", ...]`.
- `perfil_atacante.values()` -> Devuelve una lista solo con los valores.
- `perfil_atacante.items()` -> Devuelve los pares completos (Ideal para bucles For).

---

## ❓ Diccionarios vs JSON (Visión de Seguridad)

Si hacés pentesting de aplicaciones web o Cloud Security, vas a interactuar constantemente con APIs web (Ej. consultar a la API de VirusTotal si un archivo es malicioso). 
Las APIs modernas en Internet siempre se comunican enviando información en un formato de texto llamado **JSON** (JavaScript Object Notation).

**Un archivo JSON es sintácticamente idéntico a un Diccionario de Python.**
El 50% de la ingeniería defensiva en Python (DevSecOps) consiste en conectarse a sistemas (Firewalls, AWS, Azure), descargar su estado en formato JSON, convertirlo mágicamente en un Diccionario de Python (con la librería `json`), y usar claves (`data["usuarios"]`) para extraer lo que te importa.
Quien domina los diccionarios, domina las APIs.

---

## 📌 Must Know (Imprescindible)
- La sintaxis de llaves `{ }` y los pares `"Clave" : Valor`.
- Cómo acceder a un valor (`diccionario["clave"]`).
- Por qué el método `.get("clave")` es más seguro que usar corchetes (Evita que el programa se rompa si la clave no existe).
- La equivalencia lógica entre los diccionarios de Python y el formato universal JSON.

---

## 🔄 Preguntas de repaso
1. Si tenés el diccionario: `servidor = {"nombre": "Web-Prod", "estado": "Activo"}`, ¿cuál es la instrucción exacta para agregarle un nuevo dato al diccionario cuya clave sea `"puerto_ssh"` y su valor sea el número `2222`?
2. Estás leyendo la salida en bruto de una herramienta de escaneo volcada a un diccionario de Python. Intentas leer el resultado haciendo `print(escaneo["vulnerabilidades"])`, pero el script "crashea" (se cierra abruptamente) con un `KeyError`. ¿Cuál es la razón del error, y qué método alternativo debiste usar para intentar obtener ese dato de forma segura sin romper el programa?
3. ¿Por qué el dominio de los Diccionarios en Python se considera una habilidad crítica si tu rol requiere interactuar constantemente con APIs RESTful de seguridad (como Shodan, CrowdStrike, o VirusTotal)?

**➡️ Siguiente nota:** [[06 - Condicionales (if, elif, else)]]
