# Unidad 2.6: Colecciones de Datos - Las Tuplas en Python

En el desarrollo de software y en especial en áreas críticas como la ciberseguridad y la respuesta a incidentes, la integridad de los datos es fundamental. Cuando recolectamos evidencias de un sistema atacado (como registros de red o hashes de archivos maliciosos), necesitamos asegurarnos de que esta información permanezca inalterada. En esta unidad estudiaremos las **tuplas**, la estructura de datos ideal en Python para garantizar la inmutabilidad y la seguridad de la información.

---

## 1. ¿Qué es una Tupla?
Una **tupla** es una colección de elementos ordenada e **inmutable**. 

* **Ordenada**: Cada elemento tiene una posición física indexada que comienza en `0`.
* **Inmutable**: Una vez creada la tupla, **no es posible** modificar, añadir o eliminar ninguno de sus elementos. Permanece constante durante toda la ejecución del programa.
* **Heterogénea**: Al igual que las listas, una sola tupla puede almacenar diferentes tipos de datos simultáneamente (enteros, flotantes, strings, booleanos o colecciones).

Se definen utilizando paréntesis `()`, separando sus elementos por comas:

```python
# Tupla vacía
evidencia_vacia = ()

# Tupla con datos de un Indicador de Compromiso (IOC)
ioc_registro = ("192.168.1.105", "Malware_Trojan", 8080)
```

---

## 2. Diferencia Clave: Lista vs. Tupla

La elección entre una lista y una tupla se basa directamente en la naturaleza de los datos con los que trabajas:

| Aspecto | Listas (`list`) | Tuplas (`tuple`) |
| :--- | :--- | :--- |
| **Sintaxis** | Corchetes `[]` | Paréntesis `()` |
| **Mutabilidad** | **Mutables** (se pueden modificar en memoria) | **Inmutables** (no se pueden modificar) |
| **Casos de uso** | Colecciones dinámicas de datos que cambian (colas de tareas, logs dinámicos) | Datos constantes, estructurados e históricos (coordenadas, evidencias forenses, configuraciones) |
| **Rendimiento** | Ligeramente más lentas en memoria | Más rápidas y ligeras al ser de tamaño fijo |

---

## 3. Acceso por Índice y Slicing

El acceso a los elementos de una tupla es idéntico al de las listas, utilizando corchetes `[]` e índices enteros:

```python
registro = ("2026-09-05", "10.0.0.1", "Intrusión SSH")

# Acceso directo por índice positivo
print(registro[0])  # Salida: 2026-09-05 (Fecha del incidente)

# Acceso utilizando índices negativos
print(registro[-1]) # Salida: Intrusión SSH (Último elemento)
```

### Intento de Modificación (Inmutabilidad en acción)
Si intentamos alterar una posición de la tupla una vez creada, Python detendrá la ejecución del programa lanzando un error de tipo `TypeError`:

```python
registro = ("2026-09-05", "10.0.0.1", "Intrusión SSH")
# registro[1] = "192.168.1.5"  # ❌ TypeError: 'tuple' object does not support item assignment
```

---

## 4. Particularidad Sintáctica: Tupla Unitaria
Para crear una tupla que contenga **un solo elemento**, es estrictamente obligatorio incluir una **coma final** `,` antes de cerrar el paréntesis. De lo contrario, Python interpretará los paréntesis simplemente como un operador de agrupación matemática, guardando un string o un número normal:

```python
# ❌ Intento incorrecto (se guardará como un String normal, no como tupla)
falso_registro = ("192.168.1.1")
print(type(falso_registro))  # Salida: <class 'str'>

# ✅ Tupla unitaria correcta (lleva la coma al final)
registro_seguro = ("192.168.1.1",)
print(type(registro_seguro))  # Salida: <class 'tuple'>
```

---

## 5. Desempaquetado de Tuplas (Unpacking)
El **desempaquetado** permite extraer de forma directa los elementos de una tupla y asignarlos a variables individuales en una sola línea de código:

```python
incidente = ("Ransomware", 443, "En curso")

# Desempaquetado directo
tipo_ataque, puerto, estado = incidente

print(tipo_ataque) # Salida: Ransomware
print(puerto)      # Salida: 443
print(estado)      # Salida: En curso
```

> ⚠️ **Regla del Desempaquetado**: El número de variables a la izquierda del operador `=` debe coincidir exactamente con el número de elementos contenidos dentro de la tupla. Si hay un desfase, Python lanzará un error de tipo `ValueError` (*too many values to unpack* o *not enough values to unpack*).

---

## 6. Conversión entre Listas y Tuplas
Es habitual en ciberseguridad recolectar datos inmutables en una tupla, pero necesitar modificarlos puntualmente (por ejemplo, para filtrar un falso positivo). Para ello, podemos alternar libremente entre tipos de datos mediante las funciones `list()` y `tuple()`:

```python
# 1. Tupla inmutable de origen
ioc_original = ("10.0.0.2", "Malicioso")

# 2. Convertimos a lista para poder modificar el estado
ioc_lista = list(ioc_original)
ioc_lista[1] = "Falso Positivo"  # Modificación permitida en listas

# 3. Re-convertimos a tupla para proteger la integridad del reporte final
ioc_final = tuple(ioc_lista)
print(ioc_final)  # Salida: ('10.0.0.2', 'Falso Positivo')
```

---

## 7. Métodos Esenciales del Objeto `tuple`
Al ser colecciones inmutables, las tuplas carecen de métodos de alteración (como `.append()` o `.remove()`). Únicamente disponen de dos métodos de inspección:

* **`count(x)`**: Cuenta cuántas veces se repite el valor `x` dentro de la tupla.
* **`index(x)`**: Devuelve la primera posición física en la que aparece el valor `x` (lanza `ValueError` si no existe).

```python
puertos_atacados = (22, 80, 22, 443, 22)

# Contar incidentes en un puerto específico
print(puertos_atacados.count(22))  # Salida: 3

# Buscar la primera posición de un puerto en el histórico
print(puertos_atacados.index(443)) # Salida: 3
```

---

## 8. Uso Avanzado: Tuplas como Claves de Diccionarios
En Python, los objetos mutables (como las listas) no se pueden indexar como claves de un diccionario porque su contenido puede cambiar en cualquier momento, rompiendo la estructura de búsqueda del intérprete. 

Sin embargo, las tuplas, al ser inmutables, son **hashables**. Esto significa que pueden usarse de forma segura para indexar diccionarios complejos, por ejemplo, asociando coordenadas de geolocalización IP o pares de red a un tipo de incidente:

```python
# Diccionario que asocia pares de IP origen e IP destino con el estado de la comunicación
firewall_logs = {
    ("192.168.1.100", "8.8.8.8"): "Tráfico Seguro",
    ("10.0.0.5", "185.220.101.3"): "Conexión Sospechosa"
}

# Consulta rápida utilizando la tupla completa como clave
estado_conexion = firewall_logs[("10.0.0.5", "185.220.101.3")]
print(estado_conexion)  # Salida: Conexión Sospechosa
```
