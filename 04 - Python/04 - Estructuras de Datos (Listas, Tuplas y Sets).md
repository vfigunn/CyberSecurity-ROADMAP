# 04 - Estructuras de Datos (Listas, Tuplas y Sets)

## 🎯 Objetivos
- Entender cómo almacenar múltiples valores en una sola variable.
- Dominar las **Listas** (el tipo de colección más usado).
- Diferenciar el caso de uso de las **Tuplas** (inmutabilidad) y los **Sets** (unicidad).

---

## 🧠 Concepto: Colecciones

Hasta ahora, nuestras "cajas" (variables) solo guardaban un único dato (un texto, un número). ¿Qué pasa si tu script de seguridad necesita almacenar los nombres de 500 puertos abiertos? No vas a crear 500 variables (`puerto1 = 80`, `puerto2 = 443`). 
Vas a crear una única "caja gigante" (Estructura de Datos) que guarde una colección entera adentro.

---

## 1. Listas (`list` - Arreglos/Arrays)

Son, por escándalo, la colección más utilizada en Python. 
Se definen envolviendo los elementos entre **corchetes `[ ]`**.
- Pueden cambiar de tamaño (podés agregar o quitar cosas).
- Pueden contener distintos tipos de datos mezclados (texto, números).
- Mantienen el orden en el que agregaste las cosas.

```python
puertos_criticos = [21, 22, 80, 443]
nombres_usuarios = ["admin", "root", "guest"]
```

### Accediendo a los datos (Índices)
En informática, **siempre se empieza a contar desde el Cero (0)**. El primer elemento de la lista está en la posición (índice) 0.
Para ver qué hay en una posición, ponés el nombre de la lista seguido del índice entre corchetes.
```python
print(puertos_criticos[0])  # Imprimirá 21
print(puertos_criticos[2])  # Imprimirá 80
```
*Truco Pro:* Si usás números negativos, Python cuenta de atrás para adelante. `puertos_criticos[-1]` te dará siempre el último elemento de la lista (443 en este caso).

### Modificando la Lista
- **Añadir algo al final (`.append()`):** Es el método más usado.
  `puertos_criticos.append(3306)`
- **Modificar un valor:** 
  `puertos_criticos[0] = 23` (El puerto 21 pasa a ser 23)
- **Eliminar algo (`.remove()` o `.pop()`):**
  `puertos_criticos.remove(80)` (Busca el 80 y lo borra).
  
---

## 2. Tuplas (`tuple`)

Las Tuplas son las hermanas de las listas. Son casi idénticas, con una sola diferencia crítica: **Son Inmutables**.
Una vez que las creás, nunca más podés agregar, modificar, ni borrar los elementos que tienen adentro. 

Se definen utilizando **paréntesis `( )`**.
```python
ip_del_servidor_core = ("192.168.1.5", 443)
```
**¿Por qué usar tuplas en seguridad?** 
Si tenés datos que bajo ninguna circunstancia deberían cambiar accidentalmente durante la ejecución del script (por ejemplo, las coordenadas IP de los servidores críticos de tu empresa), usás tuplas. Te aseguran integridad. Consumen menos memoria RAM y son más rápidas que las listas.

---

## 3. Sets (Conjuntos Matemáticos)

Los Sets se definen usando **llaves `{ }`** (¡Cuidado, los Diccionarios de la nota siguiente también usan llaves!).
Un Set es una colección desordenada que **No admite elementos duplicados**.

En seguridad, los Sets son mágicos para limpiar datos (Deduplicación).

*Escenario de ciberseguridad:*
Un atacante bombardeó tu servidor web y el archivo de log (registro) registró 10.000 ataques de 10.000 IPs distintas. Vos pasás ese archivo de texto a una Lista gigante en Python. Sabés que de esos 10.000 ataques, muchos fueron realizados por la misma IP atacante repetida cientos de veces. Vos solo querés la lista de IPs únicas.

```python
ips_atacantes_log = ["10.0.0.1", "10.0.0.5", "10.0.0.1", "10.0.0.1", "192.168.1.1"]

# Truco ninja: Convirtiendo la lista a Set, Python borra los duplicados matemáticamente.
ips_unicas = set(ips_atacantes_log)

print(ips_unicas) 
# Resultado: {"10.0.0.1", "10.0.0.5", "192.168.1.1"}
```

---

## 📌 Must Know (Imprescindible)
- Las **Listas** usan `[ ]` y son modificables (mutables). Los índices empiezan en 0.
- Las **Tuplas** usan `( )` y son de solo lectura (inmutables).
- Los **Sets** usan `{ }` y descartan automáticamente cualquier elemento duplicado.

---

## 🔄 Preguntas de repaso
1. Tenés una lista definida como: `archivos_robados = ["passwords.txt", "config.php", "db_dump.sql"]`. Si quisieras imprimir en consola el nombre del archivo de la base de datos (db_dump), ¿qué índice numérico deberías colocar dentro de los corchetes `archivos_robados[ ]`?
2. Estás escribiendo una herramienta defensiva y declaras la lista de puertos prohibidos así: `puertos = (21, 23, 445)`. Si durante la ejecución el programa intenta hacer `puertos.append(3389)`, Python lanzará un `AttributeError`. ¿Por qué sucede esto conceptualmente con la estructura de datos que elegiste?
3. Extraes miles de nombres de usuarios de la base de datos para auditarlos, y los guardas en la lista `usuarios_db`. Sabés que hay muchos nombres de usuario duplicados. En lugar de escribir un código largo para revisar y borrar uno por uno los duplicados, ¿a qué Estructura de Datos especial de Python podrías convertir la lista para que se deduplique sola en una línea de código?

**➡️ Siguiente nota:** [[05 - Estructuras de Datos (Diccionarios)]]
