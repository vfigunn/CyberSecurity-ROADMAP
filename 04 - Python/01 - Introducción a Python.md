# 01 - Introducción a Python

## 🎯 Objetivos
- Comprender por qué Python es el lenguaje preferido en el ecosistema de la seguridad.
- Entender la diferencia entre un lenguaje Compilado y uno Interpretado.
- Conocer las dos formas de ejecutar código Python (Modo Interactivo vs Script).

---

## 🧠 ¿Por qué Python en Ciberseguridad?

Python fue creado a principios de los 90 con una filosofía muy clara: **Legibilidad**. El código de Python se lee casi como si fuera inglés simple. No usa llaves `{ }` engorrosas ni puntos y comas `;` al final de cada línea como C++ o Java.

Se ha convertido en el estándar de la industria por tres razones:
1. **Velocidad de desarrollo:** En C++, escribir una herramienta que descargue una página web y extraiga enlaces podría llevarte 50 líneas de código y una hora de trabajo. En Python, toma 4 líneas y dos minutos. En medio de un ataque, el tiempo es crítico.
2. **"Pilas incluidas" (Batteries Included):** Python viene por defecto con una librería estándar monstruosa que ya sabe cómo lidiar con redes, protocolos, compresión de archivos y cifrado, sin necesidad de descargar nada extra.
3. **Multiplataforma:** Escribís el código una vez, y corre exactamente igual en Windows, Linux y macOS.

---

## ⚙️ Lenguaje Interpretado vs Compilado

Esta es una pregunta clásica de entrevista técnica.

- **Lenguajes Compilados (ej. C, C++, Go):** 
  Escribís el código fuente en texto plano, y luego usas un programa (Compilador) que traduce todo ese texto junto, de una sola vez, a código binario (Ceros y Unos / Lenguaje de Máquina). El resultado es un archivo inmodificable (como un `.exe`). 
  *Ventaja:* Son extremadamente rápidos. 
  *Desventaja:* Si lo compilaste en Windows, ese `.exe` no funciona en Linux; tenés que recompilarlo.

- **Lenguajes Interpretados (ej. Python, Bash, JavaScript):**
  El código nunca se traduce a un `.exe`. Cuando lo ejecutás, un programa intermediario llamado **Intérprete** lee tu código de texto plano, línea por línea en tiempo real, y lo va traduciendo y ejecutando sobre la marcha.
  *Ventaja:* El mismo archivo de texto `.py` corre en cualquier sistema operativo que tenga el intérprete instalado. (Es ideal para distribuir scripts de hacking).
  *Desventaja:* Es más lento que C, porque la traducción ocurre en el momento (aunque para tareas de seguridad, esa lentitud es imperceptible).

---

## 🛠️ Cómo usar Python

Asumimos que ya tenés instalado Python 3 en tu sistema (en Linux viene instalado por defecto).

### 1. El Modo Interactivo (La Calculadora)
Si en tu terminal de Linux simplemente escribís la palabra `python3` (o `python`) y apretás Enter, entrarás al entorno interactivo (indicado por tres flechas `>>>`).
```python
$ python3
>>> print("Hola Mundo")
Hola Mundo
>>> 5 + 5
10
>>> exit()
```
El modo interactivo es brillante para probar pequeñas lógicas matemáticas, testear si una expresión regular funciona, o decodificar rápidamente un hash en Base64 sin tener que crear un archivo.

### 2. El Modo Script (Archivos)
Para programas reales, escribimos el código en un archivo de texto con extensión **`.py`** (ej. `escaner.py`) usando cualquier editor (VSCode, nano, vim).
Luego, se lo pasamos al intérprete desde la consola para que lo corra de principio a fin:
```bash
$ python3 escaner.py
```

---

## 📌 Must Know (Imprescindible)
- Python es un lenguaje de programación *Interpretado* y de alto nivel.
- Entender que un archivo `.py` no puede ser ejecutado directamente por el procesador; necesita pasar a través del intérprete de Python.

---

## 🔄 Preguntas de repaso
1. Si un analista descubre una vulnerabilidad nueva (Zero-Day) y quiere publicar rápidamente una Prueba de Concepto (Exploit PoC) para que la comunidad de ciberseguridad en Linux y Windows la pruebe, ¿por qué elegiría escribir el código en Python en lugar de compilarlo en un archivo C++ (`.exe`)?
2. Entras a un servidor vulnerado y necesitás probar si la librería matemática de Python puede procesar un cálculo específico, pero no tenés permisos para crear un archivo en el disco duro. ¿Cómo usarías Python para hacer esta prueba sin crear un script?
3. En términos de ejecución por parte del Sistema Operativo, ¿cuál es la diferencia conceptual entre escribir `./escaner` (un binario de Go o C) y escribir `python3 escaner.py`?

**➡️ Siguiente nota:** [[02 - Sintaxis Básica y Variables]]
