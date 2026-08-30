# 18 - Bash Scripting Básico (Variables y Condicionales)

## 🎯 Objetivos
- Entender qué es un Script y para qué sirve automatizar en Bash.
- Aprender la estructura básica de un archivo Bash (El Shebang `#!/bin/bash`).
- Conocer cómo usar Variables locales y sentencias Condicionales (`if`).

---

## 🧠 Concepto: Automatización en Hacking y Defensa

Escribir en la terminal comando por comando es útil, pero lento. ¿Qué pasa si tenés que crear 50 usuarios, asignarles contraseñas y darles permisos a carpetas, todos los días a las 8 AM?
Un **Script** es simplemente un archivo de texto plano donde escribís una lista de comandos de Linux, uno debajo del otro. Luego, le decís a la computadora: *"Ejecutá este archivo y hacé todos estos comandos de corrido sin que yo tenga que teclearlos"*.

**Bash** es el intérprete (Shell) que lee ese archivo. Bash Scripting es considerado "programación ligera" y es el lenguaje más universal en servidores. (En el Módulo 04 veremos Python, que es para programación más compleja).

---

## 🛠️ Estructura de un Bash Script

Creamos un archivo llamado `mi_script.sh` (la extensión `.sh` es una convención, no es obligatoria en Linux).

**Paso 1: El Shebang (`#!`)**
La primera línea de tu script siempre debe ser el *Shebang*. Le dice al sistema operativo qué programa debe usar para leer este archivo.
```bash
#!/bin/bash
echo "Iniciando script de auditoría..."
```

**Paso 2: Permisos de Ejecución**
Si intentas correr el script haciendo `./mi_script.sh`, Linux te dará "Permiso Denegado". (Ver [[10 - Permisos de Archivos (chmod, chown)|Nota 10]]). 
¡Tenés que darle el permiso de ejecución (`x`) primero!
```bash
$ chmod +x mi_script.sh
$ ./mi_script.sh
```

---

## 📦 Variables en Bash

Las variables sirven para almacenar información temporalmente en tu script. (Son variables locales de este archivo, no variables de entorno globales).

- **Definir una variable:** No puede haber espacios alrededor del signo igual (`=`).
- **Llamar a una variable:** Le antepones el símbolo de dólar (`$`).

```bash
#!/bin/bash
IP_ATACANTE="192.168.1.50"
echo "Alerta: Hemos detectado ataques desde la IP $IP_ATACANTE"

# También podés guardar el resultado de un comando en una variable usando $()
USUARIO_ACTUAL=$(whoami)
echo "El script está siendo corrido por: $USUARIO_ACTUAL"
```

---

## ⚖️ Condicionales (`if / else`)

Los condicionales le dan "inteligencia" a tu script. Le permiten tomar decisiones lógicas basándose en situaciones.

**La sintaxis básica del `if` en Bash:**
(Nota lo estrictos que son los espacios dentro de los corchetes `[ ]`).
```bash
#!/bin/bash

# Comparamos si el usuario actual es "root"
if [ "$USER" == "root" ]; then
    echo "Genial, sos Administrador. Procediendo a hackear..."
else
    echo "Error: Debés ejecutar este script como root o usando sudo."
    exit 1  # Esto termina el script inmediatamente con un código de error
fi
```

### Operadores de comparación más útiles
Bash es un poco raro. Para comparar texto (Strings) usa los clásicos `==` o `!=`. Pero para comparar **Números**, usa letras:
- `-eq` : Equal (Igual a)
- `-ne` : Not Equal (No igual a)
- `-gt` : Greater Than (Mayor que)
- `-lt` : Less Than (Menor que)

*Ejemplo numérico:*
```bash
#!/bin/bash
PUERTOS_ABIERTOS=5

if [ $PUERTOS_ABIERTOS -gt 2 ]; then
    echo "Peligro: Hay más de 2 puertos abiertos en el servidor."
fi
```

---

## ❓ ¿Por qué importa en Seguridad?

Los Scripts en Bash son el armamento de la ciberseguridad.
- **Red Team:** Escriben "Bash Scripts" rápidos para automatizar la enumeración (Escaneo). Un script puede escanear una red, encontrar puertos abiertos, y tratar de lanzar un exploit contra cada uno de ellos, todo mientras el atacante se toma un café.
- **Blue Team:** Escriben scripts para extraer información valiosa rápidamente, como buscar archivos modificados en las últimas 24 horas y enviar un reporte por mail automáticamente.

---

## 📌 Must Know (Imprescindible)
- Qué es el *Shebang* (`#!/bin/bash`).
- Cómo otorgarle permisos de ejecución al script (`chmod +x`).
- Cómo declarar y leer una variable (Sin espacios en el `=`, y con `$` para leerla).
- Estructura básica de un `if / then / else / fi`.

---

## 🔄 Preguntas de repaso
1. Creás un archivo llamado `limpieza.sh` que contiene 10 comandos `rm` para borrar archivos temporales, pero al escribir `./limpieza.sh` en tu terminal obtenés un error de permisos. ¿Cuál es el comando exacto que debés correr para solucionar esto?
2. En un script de Bash, tenés la línea: `EDAD = 18`. ¿Por qué Bash arrojará un error de sintaxis en esa línea específica?
3. Necesitás escribir un condicional `if` que verifique si una variable numérica (ej. cantidad de inicios de sesión fallidos) es *Mayor que* 10. ¿Qué operador usarías (`>`, `==` o `-gt`)?

**➡️ Siguiente nota:** [[19 - Bash Scripting (Bucles)]]
