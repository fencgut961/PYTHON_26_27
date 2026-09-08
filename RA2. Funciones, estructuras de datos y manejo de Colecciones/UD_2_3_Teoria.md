# Unidad 2.3: Funciones con Valores de Retorno (`return`)

## 🎯 Objetivos de aprendizaje
* Comprender qué es el **valor de retorno** de una función y cómo se diferencia de una salida por pantalla.
* Aprender a usar la palabra reservada `return` para enviar datos de vuelta al flujo principal del programa.
* Comprender el comportamiento del tipo especial `None` en Python.
* Diseñar funciones modulares y reutilizables especializadas en resolver un único cálculo o proceso matemático.

---

## 📚 Explicación teórica

Hasta ahora, hemos diseñado funciones que realizaban acciones directas, como imprimir menús o textos en consola usando `print()`. Sin embargo, en el desarrollo de software real, las funciones suelen actuar como **procesadores de datos**: reciben una información de entrada (parámetros), realizan cálculos y **devuelven** el resultado al flujo principal del programa para poder seguir operando con él.

Para ello, utilizamos la sentencia **`return`**.

---

### 1. La palabra reservada `return`
La directiva `return` cumple una doble función dentro de un bloque de código:
1. **Finaliza la ejecución de la función inmediatamente:** En el momento en que se procesa un `return`, el intérprete de Python sale de la función y devuelve el control al punto exacto del programa principal donde se realizó la invocación. Cualquier código situado debajo de un `return` en la misma jerarquía de ejecución nunca se ejecutará (*código muerto*).
2. **Envía el resultado de vuelta:** El valor que acompaña al `return` sustituye la llamada a la función en la expresión que la invocó.

```python
def calcular_doble(numero):
    """Calcula y devuelve el doble de un número."""
    resultado = numero * 2
    return resultado  # Sale de la función y entrega el valor de la variable

# Invocación: El valor retornado (10) se guarda en la variable 'mi_doble'
mi_doble = calcular_doble(5)
print(mi_doble)  # Imprime: 10
```

---

### 2. Diferencia clave: `print()` vs. `return`
Es un error muy común al empezar confundir mostrar un valor por pantalla con devolver un valor:

*   **`print()`** es una función de salida por consola. Solo sirve para que el usuario humano vea el dato en su pantalla. El programa no puede "leer" ni reutilizar lo que se ha impreso en la consola.
*   **`return`** es una instrucción interna de transferencia de datos. No muestra nada en la consola por sí misma, pero le entrega el dato al programa para que este pueda guardarlo en variables, pasarlo como argumento a otras funciones o usarlo en fórmulas matemáticas.

| Característica | Función con `print()` | Función con `return` |
| :--- | :--- | :--- |
| **Objetivo** | Mostrar información en la terminal. | Entregar un dato para ser operado. |
| **Reutilización** | No. El dato "muere" en la pantalla. | Sí. El dato se puede guardar y reutilizar. |
| **Resultado para el programa** | Devuelve implícitamente `None`. | Devuelve el objeto o cálculo indicado. |

---

### 3. El tipo especial `None`
Si una función no incluye una cláusula `return` explícita, o si escribe `return` a secas sin ningún valor asociado, Python añade automáticamente un **`return None`** al final de su ejecución de forma invisible.

`None` es un tipo de dato especial en Python (`NoneType`) que representa **la ausencia de valor** o un valor nulo.

```python
def mostrar_saludo(nombre):
    print(f"Hola, {nombre}!")

# Dado que la función no tiene 'return', almacena 'None' en la variable
resultado = mostrar_saludo("Marcos")
print(resultado)       # Imprime: None
print(type(resultado)) # Imprime: <class 'NoneType'>
```

---

### 4. Múltiples puntos de salida (`return` en condicionales)
Una función puede contener varios `return` dentro de su cuerpo (por ejemplo, dentro de estructuras condicionales). Sin embargo, **solo uno de ellos se ejecutará**, ya que en cuanto se procese el primero, la función finalizará de inmediato.

Esta técnica es conocida como **Early Return (Retorno Temprano)** y ayuda a limpiar el código de anidamientos complejos de `if-else`:

```python
def obtener_calificacion(nota):
    """Devuelve el estado según la nota usando retornos tempranos."""
    if not (0 <= nota <= 10):
        return "Nota inválida"  # Salida inmediata si el rango es erróneo
    
    if nota >= 5:
        return "Aprobado"
    
    return "Suspenso"  # Si no entra en los anteriores, sale por aquí
```

---

### 5. Buenas prácticas en el retorno de funciones
* **Un único propósito:** Una función debe realizar una sola tarea lógica y devolver su resultado. No mezcles cálculos matemáticos y lecturas de teclado (`input()`) dentro de la misma función.
* **Consistencia de tipos:** Procura que tu función devuelva siempre el mismo tipo de dato. Si en condiciones normales devuelve un `float`, evita que en caso de error devuelva un `str` (como `"Error"`), a menos que sea estrictamente necesario. En su lugar, es preferible lanzar una excepción o devolver un valor numérico centinela (como `-1.0` o `0.0`).
* **Documentar el retorno:** En el Docstring de la función (`"""`), detalla qué tipo de dato se devuelve y qué representa para mejorar la mantenibilidad del código.
