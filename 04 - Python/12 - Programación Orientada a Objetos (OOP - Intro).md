# 12 - Programación Orientada a Objetos (OOP - Intro)

## 🎯 Objetivos
- Entender conceptualmente qué es la Programación Orientada a Objetos (OOP).
- Comprender los conceptos de Clases, Objetos, Atributos y Métodos.
- Saber por qué necesitas entender OOP para leer código de Hacking o crear scripts grandes, aunque no escribas Clases propias al principio.

---

## 🧠 Concepto: Modelando la Realidad

Hasta ahora hemos escrito código de manera "Estructurada" o "Procedural". El programa es una lista de instrucciones de arriba hacia abajo, tal vez organizadas en [[08 - Funciones (def, return, argumentos)|Funciones]]. Esto funciona perfecto para scripts de 100 líneas.

Pero, ¿qué pasa si estás creando una herramienta masiva (como Metasploit) que tiene 50.000 líneas de código? Organizar todo con variables sueltas y listas es imposible.
La OOP es un paradigma (una forma de pensar) que agrupa **Datos** y **Acciones** dentro de "Objetos", imitando al mundo real.

---

## 1. Clases (Los Planos)

Una **Clase** no es algo real, es un *Plano* o *Molde*.
Si estuvieras diseñando un videojuego, la Clase sería el concepto abstracto de "Un Enemigo". El plano dice que todo Enemigo debe tener un dato llamado "Salud", un dato llamado "Arma", y una habilidad (acción) llamada "Atacar".

```python
# Definición de la Clase (El Molde). Convención: Empiezan con Mayúscula.
class Servidor:
    
    # Este es el Constructor. Se ejecuta automáticamente cuando creas un objeto.
    # 'self' es una palabra mágica que significa "Yo mismo".
    def __init__(self, ip, sistema_operativo):
        # Atributos (Las variables o los "Datos" que guardará el objeto)
        self.ip = ip
        self.os = sistema_operativo
        self.comprometido = False
        
    # Métodos (Las funciones o "Acciones" que el objeto puede hacer)
    def hackear(self):
        print(f"Lanzando exploit contra {self.ip}...")
        self.comprometido = True
```

---

## 2. Objetos / Instancias (La Realidad)

Un **Objeto** es la materialización del Plano.
Usando el molde `Servidor` de arriba, podemos crear (Instanciar) 500 servidores reales distintos en la memoria RAM, cada uno con su propia IP, pero todos compartiendo la habilidad de ser hackeados.

```python
# Instanciando (Creando) dos objetos reales diferentes
servidor_web = Servidor("192.168.1.10", "Linux")
servidor_bd = Servidor("10.0.0.5", "Windows")

# Accediendo a los Datos (Atributos) usando el PUNTO (.)
print(servidor_web.ip)        # Imprime 192.168.1.10
print(servidor_bd.os)         # Imprime Windows

# Ejecutando Acciones (Métodos) usando el PUNTO (.)
servidor_web.hackear()
print(servidor_web.comprometido)  # Ahora imprime True
```

---

## ❓ ¿Por qué importa en Seguridad?

Como analista Junior, casi **nunca** tendrás que escribir tus propias Clases. Tus scripts de 20 líneas no las necesitan.

**Sin embargo, NECESITAS saber leerlas.**
En Python, "Todo es un Objeto". Cuando creás un texto (`texto = "Hola"`), en realidad Python está usando silenciosamente el molde `String` para crear un objeto texto. 

- ¿Por qué hacíamos `texto.lower()`? Porque `.lower()` es un **Método** (acción) que pertenece a la Clase String.
- ¿Por qué hacíamos `diccionario.keys()`? Porque `.keys()` es un Método de la Clase Diccionario.

En ciberseguridad, usarás librerías escritas por gigantes (como la librería de peticiones web `requests`). Cuando le pidas a la librería que visite una página web, **te devolverá un Objeto Respuesta**.
Si no entendés OOP, no sabrás que para ver si la página cargó bien tenés que leer su atributo de estado (`respuesta.status_code`), y para ver el HTML tenés que leer su atributo de texto (`respuesta.text`).

---

## 📌 Must Know (Imprescindible)
- La diferencia filosófica entre una **Clase** (El plano arquitectónico abstracto) y un **Objeto** (El edificio real construido a partir del plano).
- Los **Atributos** son simplemente variables (datos) que viven dentro del objeto.
- Los **Métodos** son simplemente funciones (acciones) que viven dentro del objeto y se llaman usando la notación de punto (`objeto.metodo()`).

---

## 🔄 Preguntas de repaso
1. Al interactuar con un objeto red en Python llamado `conexion_tcp`, escribís el código `conexion_tcp.cerrar_puerto()`. Basado en la sintaxis, ¿`cerrar_puerto()` es un Atributo (Dato) o un Método (Acción) de ese objeto?
2. Un equipo de Red Team está escribiendo una herramienta masiva en Python para simular una Botnet (miles de computadoras zombis atacando). Explicá conceptualmente cómo la Programación Orientada a Objetos les facilita la vida al tener que administrar 10.000 bots distintos en la memoria RAM, en comparación a usar programación tradicional con listas y variables globales.
3. Observá este código: `usuario = "admin"`. Si consideramos que en Python "Todo es un Objeto", ¿cuál fue el Molde o Clase base que el sistema utilizó de fondo para darle vida a ese texto?

**➡️ Siguiente nota:** [[13 - Módulos y Paquetes (pip, import)]]
