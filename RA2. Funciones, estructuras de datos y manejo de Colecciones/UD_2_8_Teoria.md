# Unidad 2.8: Métodos Avanzados de Colecciones y Comprensión de Listas

En el desarrollo de herramientas profesionales, especialmente en ámbitos como la ciberseguridad o el análisis de sistemas, la velocidad para filtrar, transformar y resumir conjuntos masivos de datos es crucial. En esta unidad estudiaremos técnicas avanzadas de Python para manipular listas y diccionarios con la máxima eficiencia y con un código limpio y legible.

---

## 1. Métodos Avanzados de Listas

Python incorpora funciones y métodos nativos de alto rendimiento para analizar y reordenar elementos numéricos o de texto de forma directa:

*   **`sorted(lista)`**: Devuelve una **nueva lista** ordenada, dejando la lista original intacta.
*   **`lista.sort()`**: Ordena la lista **in situ** (modifica de forma permanente la original).
*   **`lista.sort(reverse=True)`**: Ordena la lista de forma permanente en orden descendente.
*   **`max(lista)`** y **`min(lista)`**: Obtienen instantáneamente el valor máximo y mínimo de una colección.
*   **`sum(lista)`**: Calcula la suma total de todos los elementos (requiere que sean exclusivamente numéricos).

### Ejemplo práctico: Análisis de puertos atacados
```python
puertos_detectados = [80, 443, 22, 3389, 8080]

# Encontrar los extremos de escaneo
print("Puerto más bajo:", min(puertos_detectados))  # 22
print("Puerto más alto:", max(puertos_detectados))   # 8080

# Generar un reporte ordenado sin destruir el registro cronológico original
puertos_ordenados = sorted(puertos_detectados)
print("Orden cronológico (original):", puertos_detectados) # [80, 443, 22, 3389, 8080]
print("Orden por riesgo (ordenado):", puertos_ordenados)    # [22, 80, 443, 3389, 8080]
```

---

## 2. Métodos Avanzados de Diccionarios

Al estructurar bases de datos temporales (como un inventario de incidentes o usuarios conectados), el uso de métodos avanzados nos previene de excepciones y simplifica la lógica:

*   **`diccionario.get(clave, valor_defecto)`**: Recupera el valor asociado a una clave. Si la clave no existe, en lugar de lanzar un error `KeyError`, devuelve el valor por defecto indicado.
*   **`diccionario.setdefault(clave, valor_defecto)`**: Intenta obtener la clave. Si no existe, la añade físicamente al diccionario con el valor predeterminado especificado y lo devuelve.
*   **`diccionario.update(otro_diccionario)`**: Fusiona diccionarios. Actualiza los valores de las claves existentes y añade las nuevas parejas clave-valor en una sola instrucción.

### Ejemplo práctico: Gestión de amenazas en un Host
```python
estado_host = {"ip": "192.168.1.100", "amenazas_detectadas": 3}

# 1. Recuperación segura de datos
print("Estado del firewall:", estado_host.get("firewall_activo", "DESCONOCIDO"))  # DESCONOCIDO

# 2. Inicialización automática de listas de logs de incidentes
estado_host.setdefault("historial_bloqueos", [])
print(estado_host)  # Incorpora 'historial_bloqueos': []

# 3. Actualización masiva de telemetría tras un análisis
estado_host.update({"amenazas_detectadas": 0, "analisis_completado": True})
print(estado_host)  # Modifica amenazas a 0 e inserta el booleano
```

---

## 3. Comprensión de Listas (List Comprehensions)

La **comprensión de listas** es una de las características más queridas y potentes de Python. Permite construir una nueva lista a partir de un iterable de forma compacta, expresiva y en **una sola línea de código**.

### Sintaxis General
```python
nueva_lista = [expresion for elemento in iterable if condicion]
```

### El Camino Tradicional vs. La Vía Pythónica
Imaginemos que necesitamos extraer los códigos de puertos críticos detectados para analizarlos individualmente:

**Enfoque tradicional (Bucle clásico):**
```python
puertos = [80, 22, 443, 3389, 21]
puertos_criticos = []

for p in puertos:
    if p < 1024:
        puertos_criticos.append(p)
```

**Enfoque moderno (Comprensión de listas):**
```python
puertos = [80, 22, 443, 3389, 21]
puertos_criticos = [p for p in puertos if p < 1024]
```

### Comprensiones con Condicional Ternario (Transformación de datos)
Si queremos transformar los elementos además de filtrarlos, podemos inyectar una estructura `if-else` al inicio de la comprensión:

```python
puertos = [80, 8080, 22, 9000]
# Clasificamos cada puerto en base a su rango estandarizado
tipos_puerto = ["Estándar" if p < 1024 else "Alternativo" for p in puertos]
print(tipos_puerto)  # ['Estándar', 'Alternativo', 'Estándar', 'Alternativo']
```

---

## 4. Comprensión de Diccionarios

De forma homóloga a las listas, podemos generar mapas asociativos clave-valor rápidamente mediante comprensiones de diccionarios:

```python
servicios_criticos = ["ssh", "rdp", "ftp"]
puertos_asociados = [22, 3389, 21]

# Unificamos colecciones para crear un mapa de referencias instantáneo
mapa_puertos = {servicios_criticos[i]: puertos_asociados[i] for i in range(len(servicios_criticos))}
print(mapa_puertos)  # {'ssh': 22, 'rdp': 3389, 'ftp': 21}
```
