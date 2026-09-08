# Unidad 1.5: Bucles e Iteración Controlada (for y while)

En los lenguajes de programación tradicionales como Java o C#, repetir una tarea implica escribir estructuras rígidas con inicializadores, condiciones de parada e incrementos manuales (como `for(int i = 0; i < 10; i++)`). En Python, el enfoque de la iteración es mucho más limpio, intuitivo y legible, adaptándose de forma automática a diferentes tipos de datos.

En esta unidad aprenderás a dominar las dos estructuras de repetición fundamentales: **for** y **while**, entendiendo sus diferencias y sabiendo cuándo utilizar cada una de forma eficiente.

---

## 1. El Bucle `for` y la Iteración sobre Secuencias

A diferencia de otros lenguajes, el bucle `for` en Python funciona esencialmente como un **foreach**: no itera sobre un contador numérico de forma nativa, sino que **recorre secuencialmente los elementos de una colección o secuencia** (como cadenas de texto, rangos numéricos o colecciones de datos).

### 1.1 Sintaxis Básica
```python
for elemento in secuencia:
    # Bloque de código a repetir
```

*   **`elemento`**: Es una variable temporal que toma automáticamente el valor del elemento actual en cada vuelta del bucle.
*   **`secuencia`**: Es el objeto iterable que se va a recorrer.

### 1.2 Iterar sobre Cadenas de Texto (`str`)
Dado que una cadena de texto es una secuencia ordenada de caracteres, el bucle `for` puede recorrerla letra a letra de forma directa:

```python
for letra in "Python":
    print(f"Letra actual: {letra}")
```

---

## 2. El Objeto `range()`: Generación de Rangos Numéricos

Cuando necesitas repetir un bloque de código un número específico de veces (como el clásico bucle indexado de Java), Python utiliza la función integrada **`range()`**.

`range()` genera una secuencia inmutable de números sobre la marcha de forma extremadamente eficiente en memoria, sin necesidad de cargarlos todos simultáneamente en la RAM.

`range()` puede recibir hasta **tres parámetros**:

1.  **`range(fin)`**: Genera números desde `0` hasta `fin - 1` (el límite superior es exclusivo).
    ```python
    for i in range(5):
        print(i)  # Imprime: 0, 1, 2, 3, 4
    ```

2.  **`range(inicio, fin)`**: Genera números desde `inicio` hasta `fin - 1`.
    ```python
    for i in range(5, 10):
        print(i)  # Imprime: 5, 6, 7, 8, 9
    ```

3.  **`range(inicio, fin, paso)`**: Genera números aplicando un incremento (o decremento) constante definido por el `paso`.
    ```python
    # Números pares del 2 al 10
    for i in range(2, 11, 2):
        print(i)  # Imprime: 2, 4, 6, 8, 10
        
    # Cuenta atrás (paso negativo)
    for i in range(5, 0, -1):
        print(i)  # Imprime: 5, 4, 3, 2, 1
    ```

---

## 3. El Bucle `while` y la Repetición Condicional

El bucle `while` ejecuta un bloque de instrucciones **mientras una condición lógica sea evaluada como verdadera (`True`)**.

Se utiliza principalmente cuando **no conocemos de antemano el número exacto de iteraciones** que necesitaremos, ya que la parada depende de un factor dinámico (como una entrada del usuario o el estado de una variable).

### 3.1 Sintaxis Básica
```python
while condicion:
    # Bloque de código a repetir
```

### 3.2 Ejemplo de Contador
```python
contador = 1
while contador <= 5:
    print(f"Vuelta número: {contador}")
    contador += 1  # Incremento obligatorio para evitar bucles infinitos
```

### 3.3 El peligro del Bucle Infinito
Si la condición del `while` nunca se evalúa como `False`, el bucle se repetirá indefinidamente, consumiendo recursos de la CPU hasta bloquear el programa.

Para evitarlo, debes garantizar que:
1.  La variable que controla la condición se actualice dentro del bloque del bucle.
2.  La condición de parada sea lógicamente alcanzable.

*(Nota: Si accidentalmente ejecutas un bucle infinito en la consola, puedes detenerlo inmediatamente pulsando `Ctrl + C` en tu teclado).*

---

## 4. Buenas Prácticas de Estilo (PEP 8) en Bucles

Para mantener un código limpio y legible, la comunidad de Python establece las siguientes pautas:

*   **Identación estricta:** Al igual que en las condiciones, todo el código dentro de un bucle `for` o `while` debe estar perfectamente indentado con **4 espacios**.
*   **Nombres de variables temporales:** En los bucles `for`, utiliza variables autodescriptivas si representan elementos concretos (ej: `for letra in "Hola":`) o la letra `i` para índices numéricos genéricos generados por `range()`.
*   **Evitar variables globales innecesarias:** Siempre que puedas resolver una repetición con un rango definido (`for`), elígelo por encima de un `while` con contador manual, ya que reduce la declaración de variables auxiliares fuera de la estructura.
