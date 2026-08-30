# 10 - Manejo de Archivos I/O (`open`, `read`, `write`)

## 🎯 Objetivos
- Aprender a leer archivos físicos del disco duro (como Logs o diccionarios de passwords) para procesarlos en Python.
- Aprender a crear y escribir archivos (como reportes o evidencias).
- Entender la importancia del bloque seguro `with open()`.

---

## 🧠 Concepto: Input / Output (I/O)

La memoria RAM es volátil. Toda variable, diccionario o lista que creamos en los capítulos anteriores existe solo mientras el script se está ejecutando. Cuando termina, los datos se evaporan.
Para lograr **Persistencia**, o para analizar grandes volúmenes de datos históricos (como los registros de Apache de un servidor hackeado), nuestro script necesita la capacidad de conectarse con el Disco Duro.

---

## 🛠️ El bloque mágico: `with open()`

La forma tradicional de abrir un archivo en Python requería un paso para "abrirlo" y un paso obligatorio para "cerrarlo" (`archivo.close()`). Si olvidabas cerrarlo (o el script fallaba a la mitad), el archivo físico quedaba bloqueado en el sistema operativo y generaba problemas.

La forma moderna y obligatoria de hacerlo en ciberseguridad es usando el Gestor de Contexto (Context Manager): **`with open(...) as variable:`**. 
Este bloque especial garantiza que **el archivo se cerrará automáticamente de forma segura tan pronto como termine la indentación**, pase lo que pase.

### Sintaxis y Modos de Apertura
`with open("ruta/del/archivo.txt", "MODO") as archivo:`

Los **Modos** (igual que en las [[08 - Redirecciones y Pipes|redirecciones de Linux]]) son:
- **`r` (Read):** Solo lectura (por defecto). Si no existe, da error.
- **`w` (Write):** Escritura. **Borra/sobrescribe** todo el archivo si ya existía (¡Peligro!).
- **`a` (Append):** Añade texto al final de lo que ya exista (Ideal para Logs continuos).

---

## 📖 1. Leer Archivos (Modo `r`)

Supongamos que sos un analista forense y querés analizar un log de inicio de sesión (`auth.log`).

```python
# Modo lectura ("r"). El archivo se carga en la variable interna "archivo_log"
with open("auth.log", "r") as archivo_log:
    # Opción A: Leer TODO el contenido de golpe en un solo String gigante. (Peligroso si pesa 5 GB).
    texto_completo = archivo_log.read()
    
    # Opción B (La profesional): Iterar línea por línea usando un bucle FOR.
    # No consume toda la memoria RAM, sin importar el tamaño del archivo.
    for linea in archivo_log:
        # Usamos .strip() para limpiar los "Enter" invisibles al final de cada línea
        linea_limpia = linea.strip()
        
        if "Failed password" in linea_limpia:
            print(f"Intento fallido detectado: {linea_limpia}")
            
# Al salir de la indentación (aquí), Python cerró automáticamente el archivo físico.
```

---

## 📝 2. Escribir Archivos (Modo `w` y `a`)

Escribir reportes, guardar hashes capturados o exportar listas de IPs vulnerables.

```python
# Lista generada por nuestro escáner en RAM
ips_vulnerables = ["192.168.1.10", "10.0.5.40"]

# Abrimos (o creamos) en modo Escritura ("w"). ¡Sobrescribirá si ya existe!
with open("reporte_vulnerables.txt", "w") as archivo_salida:
    
    # Escribimos una cabecera
    archivo_salida.write("--- INFORME DE IPs COMPROMETIDAS ---\n")
    
    # Iteramos la lista
    for ip in ips_vulnerables:
        # Importante: .write() NO pone un Enter al final automáticamente como hace print()
        # Debemos agregárselo manualmente usando el salto de línea: \n
        archivo_salida.write(f"IP: {ip} \n")
```

---

## ❓ ¿Por qué importa en Seguridad?

El manejo de archivos está en el corazón del scripting.
- **Red Team (Fuerza Bruta):** Todo ataque de diccionario (ej. con Hydra o un script propio) depende de abrir un archivo `.txt` con 14 millones de contraseñas filtradas (como el famoso archivo `rockyou.txt`), leerlo línea por línea con un `for`, y enviar cada palabra a una web.
- **Blue Team (SIEM casero):** Podés escribir un script en Python (y programarlo con `cron` en Linux) que abra el Log del Firewall (modo `r`), verifique si hay conexiones raras, y escriba los resultados en un archivo nuevo de Excel (CSV) para el equipo directivo (modo `w`).

---

## 📌 Must Know (Imprescindible)
- La superioridad del bloque `with open() as x:` para evitar dejar archivos "abiertos" o colgados en la memoria (Fugas de memoria / Memory Leaks).
- La diferencia crítica entre modo `w` (Sobrescribe y destruye) y `a` (Añade al final).
- Cómo leer grandes archivos sin llenar la memoria RAM usando un `for linea in archivo:`.
- Que el método `.write()` requiere que pongas los saltos de línea `\n` manualmente (a diferencia de `print()`).

---

## 🔄 Preguntas de repaso
1. Estás escribiendo un Keylogger (Malware) en Python que debe ejecutarse en la máquina de la víctima y registrar en un archivo oculto cada tecla presionada durante meses. ¿Qué Modo de apertura (`r`, `w` o `a`) debés usar en la función `open()` para el archivo de registro y por qué?
2. Un compañero escribe un script para buscar correos en una Base de Datos filtrada en texto plano que pesa 12 Gigabytes. Su código usa `contenido = archivo.read()` y luego busca dentro de la variable `contenido`. Al correrlo en su laptop (que tiene 8 GB de RAM), la computadora se congela completamente. ¿Por qué ocurrió esto y qué técnica de lectura de archivos debió emplear?
3. Sabiendo que los archivos contienen saltos de línea invisibles (el "Enter") al final de cada oración, cuando leemos un archivo línea por línea en Python con un `for`, ¿qué método de los *Strings* (visto en la Nota 03) solemos usar en la variable `linea` para limpiar esos espacios/saltos de línea y poder procesar el texto limpio?

**➡️ Siguiente nota:** [[11 - Manejo de Excepciones (try, except)]]
