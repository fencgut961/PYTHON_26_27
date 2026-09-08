# Unidad 2.7: Colecciones de Datos - Los Diccionarios en Python

En el desarrollo de software, y especialmente en la gestión de sistemas e incidentes, las listas y las tuplas presentan una limitación: para acceder a un dato, necesitamos conocer su índice numérico exacto. En escenarios reales de producción, esto es ineficiente. Necesitamos estructuras asociativas, conocidas como **diccionarios** o mapas (equivalentes a `HashMap` en Java o `Dictionary` en C#).

---

## 1. ¿Qué es un Diccionario?
Un **diccionario** en Python es una colección ordenada (desde Python 3.7), mutable y asociativa que almacena elementos en forma de parejas **clave-valor** (key-value).

* **Asociativa**: No accedemos a los elementos por un índice numérico (0, 1, 2...), sino mediante una **clave** única descriptiva (por ejemplo, asociamos el ID de un ticket a sus detalles).
* **Mutable**: Podemos añadir, modificar o eliminar parejas clave-valor en tiempo de ejecución.
* **Claves Inmutables**: Para que Python localice un valor de forma instantánea en memoria (búsqueda O(1)), las claves deben ser objetos inmutables y "hasheables" (cadenas, números enteros o tuplas). No se pueden usar listas como claves.
* **Valores Flexibles**: El valor asociado a una clave puede ser de cualquier tipo: enteros, flotantes, cadenas, booleanos, listas, tuplas o incluso otros diccionarios anidados.

Se definen utilizando llaves `{}` y separando las claves de los valores por dos puntos `:`:

```python
# Diccionario vacío
incidentes = {}

# Diccionario simple
ticket = {
    "id": "INC-1024",
    "ip_origen": "192.168.1.105",
    "puerto": 443,
    "activo": True
}
```

---

## 2. Acceso Seguro a los Datos

Existen dos formas de recuperar el valor de una clave en Python:

### A. Acceso Directo por Corchetes `[]`
Es la vía clásica. Sin embargo, tiene un riesgo importante: si la clave no existe en el diccionario, Python detendrá el programa lanzando una excepción de tipo `KeyError`.

```python
servidor = {"host": "SRV-WEB-01", "ip": "10.0.0.1"}

print(servidor["host"])  # Salida: SRV-WEB-01
# print(servidor["puerto"])  # ❌ Lanza KeyError (detiene el programa)
```

### B. Acceso Seguro con el Método `.get()`
Es la práctica recomendada en entornos profesionales. Permite buscar una clave y, si no existe, devuelve `None` o un valor por defecto que hayamos especificado, evitando de forma absoluta que el programa se rompa.

```python
# Si no existe "puerto", devuelve None de forma segura
puerto = servidor.get("puerto") 
print(puerto)  # Salida: None

# Podemos definir un valor de retorno predeterminado personalizado
puerto_seguro = servidor.get("puerto", 80)
print(puerto_seguro)  # Salida: 80 (ya que "puerto" no existía)
```

---

## 3. Manipulación de Datos (CRUD Básico)

Los diccionarios son altamente mutables, lo que facilita actualizar sus registros de forma directa:

### Añadir o Modificar Parejas Clave-Valor
La sintaxis para añadir una clave nueva y para modificar una existente es idéntica. Si la clave ya existe, sobrescribe su valor; si no existe, la crea dinámicamente.

```python
usuario = {"username": "lvelazquez", "rol": "Analista"}

# Modificar un valor existente
usuario["rol"] = "Administrador SOC"

# Añadir una clave nueva
usuario["ultimo_login"] = "2026-09-05"

print(usuario)
# Salida: {'username': 'lvelazquez', 'rol': 'Administrador SOC', 'ultimo_login': '2026-09-05'}
```

### Eliminación de Parejas Clave-Valor
* **Instrucción `del`**: Elimina de forma directa la clave. Si la clave no existe, lanza `KeyError`.
* **Método `.pop()`**: Elimina la clave y **retorna** su valor, permitiendo guardarlo o procesarlo. Permite opcionalmente definir un valor por defecto si la clave no se encuentra para evitar excepciones.

```python
alerta = {"id": "AL-99", "severidad": "Baja", "origen": "Firewall"}

# Uso de del
del alerta["origen"]

# Uso de pop (elimina y recupera el valor de severidad)
sev = alerta.pop("severidad")
print(sev)  # Salida: Baja

# pop seguro (no rompe si la clave no existe)
status = alerta.pop("estado", "Inexistente")
print(status)  # Salida: Inexistente
```

---

## 4. Métodos Esenciales para Iteración

Para recorrer diccionarios mediante bucles `for`, la clase `dict` ofrece tres métodos fundamentales que extraen vistas dinámicas:

1. **`.keys()`**: Devuelve una colección con todas las claves del diccionario.
2. **`.values()`**: Devuelve una colección con todos los valores almacenados.
3. **`.items()`**: Devuelve pares de tuplas `(clave, valor)`, ideal para desempaquetar directamente en los bucles.

```python
config_red = {"gateway": "192.168.1.1", "dns": "8.8.8.8", "subnet": "255.255.255.0"}

# Iterar solo sobre las claves
for parametro in config_red.keys():
    print("Clave de red:", parametro)

# Iterar solo sobre los valores
for valor in config_red.values():
    print("Valor configurado:", valor)

# Iterar sobre claves y valores simultáneamente (Recomendado)
for clave, valor in config_red.items():
    print(f"La clave [{clave}] tiene el valor [{valor}]")
```

---

## 5. Diccionarios Anidados (Estructuras de Datos Complejas)

En aplicaciones reales como los gestores de bases de datos o APIS, los datos se estructuran de forma jerárquica. Un diccionario puede contener otros diccionarios en su interior como valores.

```python
# Estructura jerárquica de inventario cloud
infraestructura = {
    "instancia_01": {
        "so": "Ubuntu 22.04",
        "cpu": 4,
        "ram": "16GB",
        "estado": "Corriendo"
    },
    "instancia_02": {
        "so": "Debian 12",
        "cpu": 2,
        "ram": "8GB",
        "estado": "Detenido"
    }
}

# Acceso encadenado de arriba hacia abajo
ram_instancia_1 = infraestructura["instancia_01"]["ram"]
print(ram_instancia_1)  # Salida: 16GB
```
