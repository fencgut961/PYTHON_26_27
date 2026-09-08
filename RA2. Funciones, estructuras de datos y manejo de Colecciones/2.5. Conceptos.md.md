# Unidad 2.5: Colecciones de Datos - Las Listas en Python

En el desarrollo de software profesional, rara vez trabajamos con datos aislados. Habitualmente necesitamos gestionar conjuntos o colecciones de elementos (como una lista de usuarios conectados, registros de temperaturas o transacciones financieras). En esta unidad estudiaremos la colección más utilizada en Python: las **listas**.

---

## 1. ¿Qué es una Lista?
Una **lista** en Python es una colección de elementos ordenada y mutable. 

* **Ordenada**: Cada elemento tiene una posición física única identificada por un índice entero, empezando siempre desde el `0`.
* **Mutable**: Podemos alterar el contenido de la lista (añadir, modificar o eliminar elementos) en tiempo de ejecución sin necesidad de crear una nueva lista en memoria.
* **Heterogénea**: A diferencia de los arrays tradicionales en lenguajes como Java o C#, una sola lista en Python puede contener elementos de diferentes tipos de datos simultáneamente (enteros, cadenas, booleanos o incluso otras listas).

Se definen utilizando corchetes `[]`, separando los elementos por comas:

```python
# Lista vacía
logs_sistema = []

# Lista de strings
direcciones_ip = ["192.168.1.1", "10.0.0.5", "172.16.0.100"]

# Lista heterogénea (mezcla de tipos)
registro_auditoria = ["AUTH_FAIL", 401, True, 19.5]
```

---

## 2. Acceso y Modificación por Índice

El acceso a los elementos se realiza escribiendo el nombre de la lista seguido del índice entre corchetes:

```python
servidores = ["SRV-Web", "SRV-DB", "SRV-Mail"]

# Acceso directo
print(servidores[0])  # Salida: SRV-Web (primer elemento)
```

### Índices Negativos
Python permite el uso de **índices negativos**, lo cual es sumamente útil para acceder a elementos empezando desde el final de la colección (el índice `-1` representa siempre el último elemento):

```python
print(servidores[-1])  # Salida: SRV-Mail (último elemento)
print(servidores[-2])  # Salida: SRV-DB (penúltimo elemento)
```

### Modificación de Elementos
Al ser mutables, podemos reasignar el valor de cualquier posición directamente:

```python
servidores[1] = "SRV-SQL_Cluster"
print(servidores)  # Salida: ['SRV-Web', 'SRV-SQL_Cluster', 'SRV-Mail']
```

---

## 3. Operadores Básicos con Listas

Las listas interactúan de forma nativa con varios operadores de Python:

* **Operador de pertenencia (`in` / `not in`)**: Devuelve un booleano indicando si un elemento existe en la lista.
  ```python
  ips_bloqueadas = ["1.1.1.1", "8.8.8.8"]
  print("1.1.1.1" in ips_bloqueadas)      # True
  print("192.168.1.1" not in ips_bloqueadas) # True
  ```
* **Operador de concatenación (`+`)**: Une dos listas generando una nueva.
  ```python
  red_a = ["10.0.0.1"]
  red_b = ["192.168.0.1"]
  red_total = red_a + red_b  # ['10.0.0.1', '192.168.0.1']
  ```
* **Operador de repetición (`*`)**: Duplica los elementos de la lista el número de veces indicado.
  ```python
  patron_ping = [0] * 4  # [0, 0, 0, 0]
  ```

---

## 4. Métodos Esenciales de la Clase `list`

Python proporciona una amplia variedad de métodos integrados para manipular listas:

| Método | Propósito | Ejemplo |
| :--- | :--- | :--- |
| `append(x)` | Añade el elemento `x` al **final** de la lista. | `lista.append("IP_NUEVA")` |
| `insert(i, x)` | Inserta el elemento `x` en la **posición física `i`**, desplazando el resto. | `lista.insert(0, "IP_CRITICA")` |
| `remove(x)` | Elimina la **primera ocurrencia** del valor `x`. Lanza `ValueError` si no existe. | `lista.remove("1.1.1.1")` |
| `pop(i)` | Elimina y **retorna** el elemento en el índice `i`. Si se omite `i`, elimina el último. | `ultimo = lista.pop()` |
| `sort()` | Ordena la lista **in situ** (por defecto de forma ascendente). | `lista.sort()` |
| `sort(reverse=True)`| Ordena la lista **in situ** de forma descendente. | `lista.sort(reverse=True)` |
| `index(x)` | Devuelve el **índice de la primera posición** donde aparece `x`. | `pos = lista.index("8.8.8.8")` |
| `count(x)` | Cuenta cuántas veces se repite el elemento `x` en la lista. | `veces = lista.count("10.0.0.1")` |
| `clear()` | Vacía por completo la lista, dejándola con tamaño 0. | `lista.clear()` |

---

## 5. Trampa de Memoria: Copia vs. Referencia

Un error crítico y muy habitual al empezar con Python es confundir la **asignación de una referencia** con la **creación de una copia física**.

### Asignación de Referencia
Cuando haces `lista_b = lista_a`, **no** estás creando una nueva lista. Ambas variables apuntan exactamente al mismo objeto en memoria. Cualquier cambio en una se reflejará inmediatamente en la otra:

```python
original = [1, 2, 3]
referencia = original  # Apuntan al mismo objeto en memoria

referencia.append(99)
print(original)  # Salida: [1, 2, 3, 99] (¡Se ha modificado el original!)
```

### Copia Física Independiente
Para duplicar los datos de forma segura en una nueva zona de memoria, debemos usar el método `.copy()`:

```python
original = [1, 2, 3]
copia_segura = original.copy()  # Crea un objeto duplicado e independiente

copia_segura.append(99)
print(original)      # Salida: [1, 2, 3] (Intacto)
print(copia_segura)  # Salida: [1, 2, 3, 99]
```
