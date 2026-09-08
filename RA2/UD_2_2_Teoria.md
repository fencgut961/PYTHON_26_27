# Unidad 2.2: Funciones con Parámetros (Modularización Avanzada)

En la unidad anterior aprendimos a crear bloques de código estáticos y aislados mediante funciones sencillas. Sin embargo, en el desarrollo de software real, las funciones necesitan comunicarse con el exterior: recibir datos, procesarlos de forma personalizada y adaptarse dinámicamente según la información provista.

---

## 1. Parámetros vs. Argumentos: La Regla de la Definición y la Invocación

Para trabajar de forma profesional, es indispensable diferenciar estos dos términos que a menudo se confunden:

*   **Parámetros:** Son las variables locales que se declaran en la cabecera de la función (entre los paréntesis de la sentencia `def`). Actúan como "plantillas" o huecos reservados para los datos que llegarán.
*   **Argumentos:** Son los valores reales y físicos que se le pasan a la función en el momento exacto de su invocación (llamada).

```python
# 'nombre' y 'nivel' son PARÁMETROS (las variables plantilla)
def configurar_jugador(nombre, nivel):
    """Muestra la configuración inicial de la sesión."""
    print(f"Cargando perfil de {nombre} (Nivel {nivel})...")

# '“Alex”' y '42' son ARGUMENTOS (los valores físicos reales)
configurar_jugador("Alex", 42)
```

---

## 2. Tipos de Parámetros en Python

Python ofrece una enorme flexibilidad para pasar información a las funciones, permitiendo combinar diferentes estrategias según el diseño del programa.

### A. Parámetros Posicionales
Son los parámetros tradicionales. El intérprete de Python asocia los argumentos a los parámetros basándose estrictamente en el **orden físico** en el que se pasan durante la invocación.

```python
def registrar_personaje(clase, arma):
    """Muestra la clase y arma del personaje."""
    print(f"Clase: {clase} | Arma: {arma}")

# Invocación correcta por posición
registrar_personaje("Guerrero", "Espada Mandoble")
# Salida: Clase: Guerrero | Arma: Espada Mandoble

# Invocación incorrecta por alteración del orden lógico
registrar_personaje("Espada Mandoble", "Guerrero")
# Salida: Clase: Espada Mandoble | Arma: Guerrero
```

*Desventaja:* Si la función tiene muchos parámetros, es extremadamente fácil cometer un error de orden, lo que provoca fallos lógicos o errores de tipo en tiempo de ejecución.

### B. Parámetros con Valores por Defecto (Argumentos Opcionales)
Permiten definir un valor predeterminado para un parámetro en la cabecera de la función. Si al invocarla el programador no pasa ese argumento, la función se ejecutará utilizando el valor por defecto sin lanzar ningún error.

```python
def conectar_servidor(usuario, puerto=8080):
    """Simula una conexión a un puerto de red."""
    print(f"Conectando al usuario {usuario} a través del puerto {puerto}")

# Se omite el puerto -> Usa el valor por defecto (8080)
conectar_servidor("moderador_twitch")

# Se especifica el puerto -> Sobrescribe el valor por defecto
conectar_servidor("streamer_vip", puerto=443)
```

⚠️ **Regla de Sintaxis Obligatoria (PEP 8):** Los parámetros con valores por defecto **siempre deben colocarse al final** de la declaración de la función, después de todos los parámetros posicionales obligatorios. De lo contrario, Python lanzará un error de sintaxis (`SyntaxError: non-default argument follows default argument`).

```python
# ❌ INCORRECTO: Lanza SyntaxError
def crear_avatar(skin="Por defecto", nombre):
    pass

# ✅ CORRECTO: Los parámetros opcionales van al final
def crear_avatar(nombre, skin="Por defecto"):
    pass
```

### C. Argumentos Nombrados (Keyword Arguments)
Permiten indicar explícitamente el nombre del parámetro al que se le asocia el valor durante la invocación del método, utilizando la sintaxis `nombre_parametro = valor`.

Al utilizar argumentos nombrados:
1.  **El orden de los factores no altera el resultado:** Puedes pasar los datos en cualquier orden.
2.  **Aumenta drásticamente la legibilidad:** Se entiende perfectamente qué significa cada dato sin necesidad de consultar la definición de la función.

```python
def crear_partida(mapa, max_jugadores, baneo_activo):
    """Configura los parámetros de una sala multijugador."""
    print(f"Mapa: {mapa} | Capacidad: {max_jugadores} | Ban: {baneo_activo}")

# Invocación explícita desordenada (perfectamente válida y legible)
crear_partida(baneo_activo=True, mapa="Nuketown", max_jugadores=12)
```

---

## 3. Buenas Prácticas y Reglas de Oro (PEP 8)

1.  **Nombres autodescriptivos:** Evita usar letras únicas (`x`, `y`, `z`) para nombrar parámetros. Usa sustantivos en minúsculas y separados por guiones bajos (ej: `puntos_vida`, `nombre_servidor`).
2.  **No uses mutables como valores por defecto:** Evita usar listas vacías `[]` o diccionarios vacíos `{}` como valores por defecto de un parámetro (ej: `def añadir_item(item, inventario=[])` ❌). Python inicializa este objeto una sola vez al cargar la función, por lo que el objeto mutable se compartirá entre todas las llamadas subsiguientes, provocando bugs silenciosos. Usa en su lugar `inventario=None` y gestiona la asignación dentro.
3.  **Docstrings paramétricos:** Documenta siempre tus funciones indicando detalladamente qué datos espera recibir cada parámetro y bajo qué tipo.
