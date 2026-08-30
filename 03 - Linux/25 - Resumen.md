# 25 - Resumen (Cheat Sheet - Linux)

Esta nota agrupa los comandos, atajos y conceptos vitales de la consola de Linux, actuando como referencia rápida para tus prácticas y ejercicios.

---

## 📂 Directorios Clave (FHS)
- **`/` (Root):** El origen de todo el disco duro.
- **`/home`:** Carpetas personales de los usuarios.
- **`/root`:** Carpeta personal del Superusuario.
- **`/etc`:** Archivos de configuración (¡Solo texto, no programas!).
- **`/var/log`:** Registros de auditoría, errores y actividad del sistema.
- **`/tmp`:** Archivos temporales (Cualquiera puede escribir. Suele tener el *Sticky Bit* `t`).
- **`/bin` y `/sbin`:** Binarios ejecutables (Comandos y programas).

---

## 🛠️ Navegación y Archivos
- **`pwd`**: Mostrar dónde estás parado ahora (Current Working Directory).
- **`ls -la`**: Listar todo (incluyendo ocultos que empiezan con `.`) en formato largo.
- **`cd`**: Cambiar de directorio (`cd ~` al home, `cd ..` un nivel arriba).
- **`mkdir -p`**: Crear carpeta (y sus padres si no existen).
- **`cp -r`**: Copiar archivos (o carpetas completas recursivamente con `-r`).
- **`mv`**: Mover o Renombrar archivos.
- **`rm -rf`**: Borrar archivos o carpetas forzosamente. ¡Peligroso! Sin papelera.

---

## 📖 Visualización y Búsqueda
- **`cat`**: Escupe todo el archivo de golpe. (Solo para textos cortos).
- **`less`**: Visualizador interactivo paginado. (`/` para buscar palabras, `q` para salir).
- **`head -n X` / `tail -n X`**: Muestra las primeras/últimas X líneas. 
- **`tail -f`**: Se queda escuchando y muestra líneas nuevas en tiempo real (vital para logs).
- **`locate`**: Busca archivos por nombre (rápido, pero usa base de datos desactualizada).
- **`find`**: Busca en vivo (Ej: `find / -name "*.txt"`).
- **`grep`**: Extrae las líneas de un archivo que contengan una palabra.
  - `-i` (Ignora mayus/minus) | `-v` (Invierte: muestra las que NO contienen la palabra).

---

## 🚰 Tuberías y Redirecciones
- **`>`**: Sobrescribe y guarda el resultado en un archivo.
- **`>>`**: Añade el resultado al final de un archivo (Append).
- **`|` (Pipe)**: Inyecta el resultado del comando izquierdo como entrada del comando derecho. (Ej: `cat logs.txt | grep "Error"`).
- **`2>/dev/null`**: Envía los mensajes de error al "agujero negro" para limpiar la pantalla.

---

## 👥 Permisos (UGO) y Procesos
- **`r` (Lectura = 4) | `w` (Escritura = 2) | `x` (Ejecución = 1).**
- **`chmod 755 archivo`**: Cambia permisos. (Dueño: todo(7). Grupo y Otros: leer/ejecutar(5)).
- **SUID (`s`)**: Permite que un usuario normal ejecute un programa tomando prestados los privilegios del *dueño* del programa (Riesgo enorme de Escalada de Privilegios si el dueño es root).
- **`sudo`**: Ejecutar un comando temporalmente como `root`.
- **`ps aux`**: Lista todos los procesos de todos los usuarios corriendo en RAM.
- **`kill [PID]`**: Señal 15 (cierre educado).
- **`kill -9 [PID]`**: Señal 9 (SIGKILL: cierra el programa violentamente y a la fuerza).

---

## ⚙️ Administración: Servicios, Paquetes y Tareas
- **`systemctl status/start/stop/enable`**: Controla los demonios/servicios del sistema (Systemd). `enable` los prende automáticamente tras reiniciar.
- **`apt update`**: Descarga el catálogo más reciente de software de los repositorios.
- **`apt upgrade`**: Instala los parches y actualizaciones.
- **`tar -czvf`**: Empaqueta y Comprime una carpeta.
- **`tar -xzvf`**: Extrae y Descomprime.
- **`ssh usuario@ip`**: Conexión remota por terminal segura.
- **`scp origen destino`**: Copia archivos por red usando el túnel SSH.
- **`crontab -e`**: Edita la lista de tareas programadas (M H D M S Comando).

---
🎉 **¡Felicitaciones por dominar la terminal Linux (Fase 4)!**
Acabas de desbloquear el poder real sobre las máquinas. Actualizá tu archivo [[Progreso]] y avanzá hacia el [[04 - Python/00 - Overview|Módulo 04 - Python]].
