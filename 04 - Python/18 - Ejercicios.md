# 18 - Ejercicios del Módulo 04

## 📝 Instrucciones
La programación no se aprende leyendo, se aprende rompiendo código y entendiendo por qué se rompió. 
Intentá resolver (mentalmente o en papel) los siguientes desafíos de lógica defensiva/ofensiva basados en lo que aprendiste.

---

## 🧠 Ejercicios de Lógica y Python

1. **El Bug de la Concatenación:**
   - Un analista novato escribe un script simple de escaneo que incluye esta línea de código:
     `print("Iniciando ataque al puerto: " + 8080)`
   - ¿Qué error arrojará Python al intentar correr esa línea y cómo lo corregirías utilizando F-Strings?

2. **Deduplicación rápida:**
   - Extraes un archivo de texto con todas las contraseñas probadas por un atacante. Lo pasás a una variable llamada `lista_passwords`. Tiene 5,000 elementos, pero notás que el atacante probó "admin123" unas 400 veces. 
   - Querés ver solo las contraseñas únicas probadas. ¿A qué tipo de Estructura de Datos de Python podrías convertir (Castear) temporalmente tu lista para que elimine todos los duplicados instantáneamente sin tener que escribir un bucle `for`?

3. **Cazando Errores (Scope):**
   - Lees el siguiente código escrito por un compañero:
     ```python
     def autenticar():
         token = "ABC123XYZ"
         print("Token validado internamente.")

     autenticar()
     print(f"El token capturado es: {token}")
     ```
   - Al ejecutarlo, la última línea arroja un error diciendo que "token no está definido". ¿Cuál es el error conceptual de Scope que cometió el compañero y cómo lo arreglarías usando un `return`?

4. **Prevención de Excepciones:**
   - Tenés este diccionario: `servidor = {"ip": "1.1.1.1", "os": "Linux"}`.
   - Si tu script hace `print(servidor["version_ssh"])`, el programa se detendrá por completo con un `KeyError` porque la clave no existe. 
   - ¿Qué método podrías usar sobre el diccionario (en lugar de los corchetes) para intentar leer la clave `"version_ssh"`, de forma tal que si no existe, el programa siga funcionando y te devuelva un valor nulo (`None`)?

5. **El Riesgo de Sobrescribir:**
   - Escribís un script que monitorea los logs y, cada vez que encuentra una anomalía, abre un archivo llamado `alertas.txt` y guarda el texto.
   - En tu código pusiste: `with open("alertas.txt", "w") as archivo:`
   - Después de una semana, te das cuenta de que el archivo solo contiene UNA alerta (la última que ocurrió) y se borraron todas las anteriores. ¿Qué letra deberías usar en el modo de apertura en lugar de la `w` para solucionar esto?

6. **Interacción con Linux (Subprocess):**
   - Necesitás que tu script de Python busque archivos con la palabra "secreto" en el directorio raíz del servidor Linux en el que estás corriendo. No querés programar la búsqueda desde cero en Python, querés apoyarte en el comando `find` nativo de Linux (visto en Módulo 03).
   - ¿Qué módulo/librería Core de Python deberías importar al principio de tu script para poder "escapar" y lanzar comandos reales en el sistema operativo?

---

## 🎯 Autoevaluación

Revisá bien tus respuestas, en especial el Ejercicio 3 (Scope) y el Ejercicio 5 (Manejo de archivos `w` vs `a`). Si dominás esos dos conceptos, te vas a ahorrar horas de debugging (búsqueda de errores) al escribir tus primeros scripts completos de seguridad.

**➡️ Siguiente nota:** [[19 - Evaluación]]
