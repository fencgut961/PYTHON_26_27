# Unidad 2.1 – Funciones: Definición e Invocación Básica (Teoría)

En programación, a medida que los proyectos crecen, escribir todo el código de forma secuencial en un único bloque se vuelve inmanejable. La **modularización** es la técnica que nos permite dividir un problema grande en partes pequeñas, independientes y reutilizables. En Python, la herramienta fundamental para lograr esto es la **función**.

---

## 1. ¿Qué es una Función?

Una **función** es un bloque de código organizado y reutilizable que se diseña para realizar una única tarea específica. 

### Ventajas de utilizar funciones:
*   **Reutilización de código:** Escribes las instrucciones una sola vez y las ejecutas tantas veces como desees, evitando la duplicación de código.
*   **Organización y claridad:** Permite estructurar el programa de forma lógica. El código se lee como una serie de pasos descriptivos.
*   **Facilidad de mantenimiento:** Si hay un error en una tarea, solo debes corregir el código dentro de la función correspondiente.
*   **Modularidad:** Facilita el trabajo en equipo, ya que diferentes programadores pueden desarrollar funciones independientes que luego se integran.

---

## 2. Declaración de Funciones: La palabra reservada `def`

Para declarar una función en Python utilizamos la palabra clave `def`, seguida del nombre de la función y paréntesis.

### Sintaxis Básica:

```python
def nombre_de_la_funcion():
    """
    Docstring: Texto explicativo de lo que hace la función.
    """
    # Bloque de instrucciones indentado
    instruccion_1
    instruccion_2
```

### Reglas sintácticas obligatorias:
1.  **La palabra `def`:** Indica al intérprete de Python que estamos iniciando la definición de una función.
2.  **El Nombre (estilo `snake_case`):** Debe ser descriptivo, en minúsculas y con guiones bajos para separar palabras (ej. `calcular_total_compra`, `mostrar_alerta_error`). No puede empezar con números ni contener caracteres especiales.
3.  **Los Paréntesis `()`:** Son obligatorios en la definición, incluso si la función no recibe datos (parámetros).
4.  **Los Dos Puntos `:`:** Indican el final de la cabecera y el inicio del bloque de instrucciones.
5.  **La Indentación (4 espacios):** Todo el cuerpo de la función debe estar indentado con 4 espacios. En Python no existen las llaves `{ }` para delimitar bloques; el espaciado físico es una regla de sintaxis estricta.

---

## 3. Documentación de Funciones: Los Docstrings (PEP 257)

Un **Docstring** es un comentario especial encerrado entre comillas triples (`""" ... """`) que se coloca inmediatamente después de definir la función. Su propósito es documentar de forma clara **qué hace** la función.

```python
def mostrar_saludo_retro():
    """Muestra una marquesina de bienvenida al estilo arcade retro."""
    print("====================================")
    print("       WELCOME TO STEAMARCADE       ")
    print("====================================")
```

> **Buenas prácticas (PEP 257):** Aunque Python ignora los docstrings en la ejecución, son accesibles mediante herramientas de autocompletado en IDEs como IntelliJ y mediante la función interna `help(mostrar_saludo_retro)`. Es obligatorio documentar cada función que crees.

---

## 4. Invocación o Llamada a una Función

Definir una función solo guarda las instrucciones en memoria, **no las ejecuta**. Para que el código dentro de la función se ejecute, debemos **invocarla** (llamarla) escribiendo su nombre seguido de paréntesis desde el flujo principal del programa.

```python
# 1. Definimos la función
def saludar():
    """Muestra un saludo simple."""
    print("¡Hola, jugador!")

# 2. Invocamos la función
saludar()  # Imprime: ¡Hola, jugador!
saludar()  # Se ejecuta de nuevo: ¡Hola, jugador!
```

---

## 5. El Orden de Ejecución Secuencial: Definir antes de Invocar

Python es un lenguaje interpretado que lee el archivo de arriba abajo. Por lo tanto, **es obligatorio definir una función antes de llamarla**.

Si intentas invocar una función antes de su línea de definición, el intérprete lanzará un error de tipo `NameError`.

```python
# ❌ ESTO LANZA UN ERROR
reproducir_musica()  # NameError: name 'reproducir_musica' is not defined

def reproducir_musica():
    """Simula la reproducción de un tema de fondo."""
    print("🎶 Reproduciendo: Green Hill Zone Theme 🎶")
```

La estructura correcta de un script organizado con funciones debe ser:

1.  **Importaciones** (si las hay).
2.  **Constantes globales** (en mayúsculas).
3.  **Definiciones de todas las funciones** (con sus docstrings).
4.  **Código principal o de entrada** (donde se realizan las llamadas).

---

## 6. Variables Locales en Funciones Básicas

Cualquier variable creada dentro de una función es **local** a esa función. Esto significa que solo existe y es visible mientras la función se está ejecutando, y se destruye al terminar. El programa principal no puede acceder a ella.

```python
def configurar_partida():
    """Define la dificultad local de la partida."""
    dificultad = "DIFÍCIL"  # Variable local
    print(f"Modo de juego establecido en: {dificultad}")

configurar_partida()
# print(dificultad)  # ❌ ERROR: NameError: name 'dificultad' is not defined
```
