# Unidad 1.2: Variables, Tipos de Datos y Operadores Básicos

Esta unidad aborda los pilares fundamentales para almacenar y manipular datos en Python. Se explica el comportamiento de la memoria, los tipos de datos principales, la conversión de tipos (casting) y las operaciones fundamentales.

---

## 1. ¿Qué es una Variable en Python?
Una **variable** es un nombre simbólico que asociamos a un valor en la memoria del ordenador. Nos permite almacenar, reutilizar y modificar información a lo largo de la ejecución de nuestro programa.

### El modelo de etiquetas frente a las cajas de Java/C#
En lenguajes de tipado estático como Java o C#, una variable es como una **caja de tamaño y tipo fijo**. Si declaras `int edad = 20;`, estás reservando un espacio físico en la memoria exclusivo para almacenar un entero.

En Python, la filosofía es diferente:
* **Las variables son "etiquetas" (referencias):** Los valores (como el número `20` o el texto `"Aragorn"`) se crean de manera independiente en la memoria (en una zona llamada *heap* o montón). La variable es simplemente una etiqueta autoadhesiva que "apunta" o hace referencia a ese objeto físico.
* **Tipado dinámico e implícito:** No declaras el tipo de una variable. El intérprete deduce automáticamente de qué tipo es el objeto según el valor asignado. Además, una misma variable puede apuntar a un tipo de dato en un momento y a otro distinto más adelante.

```python
personaje = "Aragorn"  # 'personaje' apunta a un objeto de tipo str
nivel = 15             # 'nivel' apunta a un objeto de tipo int

# Reasignación dinámica:
nivel = "Nivel Máximo" # Ahora apunta a un str. El entero 15 queda libre en memoria.
```

### Buenas prácticas al nombrar variables (PEP 8)
* Usa la convención **snake_case**: nombres en minúsculas unidos por guiones bajos (ej. `ataque_base`, `puntos_vida`).
* No comiences nombres con números (ej. `1_jugador` es inválido; `jugador_1` es válido).
* Evita usar nombres genéricos o confusos (`x`, `y`, `temp`). Los nombres deben describir claramente qué información contienen.
* Las **constantes** (valores que no deben cambiar durante el programa) se escriben completamente en **MAYÚSCULAS** (ej. `MAX_VIDA = 100`).

---

## 2. Tipos de Datos Básicos
Python cuenta con cuatro tipos de datos integrados esenciales:

1. **`str` (string / cadena de texto):** Secuencias de caracteres delimitadas por comillas simples (`'`) o dobles (`"`). Ej. `"Mago de Fuego"`.
2. **`int` (entero):** Números enteros, tanto positivos como negativos, sin decimales. Ej. `85`. A diferencia de otros lenguajes, no sufren desbordamiento físico de memoria (tienen precisión arbitraria).
3. **`float` (decimal / coma flotante):** Números con parte decimal (utilizando el punto `.` como separador). Ej. `1.25`. Siguen el estándar de precisión binaria IEEE 754.
4. **`bool` (booleano):** Solo puede tomar dos valores: `True` (Verdadero) o `False` (Falso).

> **Dato útil:** En Python, el tipo `bool` es internamente una subclase de `int`. El valor `True` equivale aritméticamente a `1` y `False` a `0`.

---

## 3. Conversión de Tipos (Casting) e Entrada de Datos
La función `input("mensaje")` detiene la ejecución del programa, muestra un texto en la consola y espera a que el usuario escriba algo. **La información capturada por `input()` se devuelve siempre como un objeto de tipo `str`**, incluso si el usuario introduce un número.

Para operar matemáticamente con estos datos, debemos realizar una conversión de tipo (casting) invocando al constructor del tipo correspondiente:

* **`int(x)`**: Convierte `x` a entero. Si `x` contiene letras o caracteres no numéricos (ej. `int("abc")`), Python lanzará un error de tipo `ValueError`.
* **`float(x)`**: Convierte `x` a decimal. Ej. `float("1.25")` resulta en `1.25`.
* **`str(x)`**: Convierte cualquier valor `x` a texto.
* **`bool(x)`**: Convierte `x` a booleano. Devuelve `False` si el objeto está vacío (una cadena vacía `""`, el número `0` o `0.0`). Cualquier otra cosa devuelve `True`.

---

## 4. El Slicing (Rebanado) de Cadenas de Texto
El **slicing** es una técnica sumamente potente en Python que permite extraer una sección (subcadena) de un texto utilizando los índices de sus caracteres. Los caracteres de un string comienzan a contarse desde el índice `0`.

### Sintaxis básica:
```python
subcadena = cadena[inicio:fin]
```
* **`inicio`**: El índice donde comienza la extracción (incluido).
* **`fin`**: El índice donde termina la extracción (**excluido**, no se incluye este último carácter).

### Ejemplo práctico:
Si tenemos la cadena `"NIVEL-095"`, los índices de sus caracteres van del `0` al `8`:
```
N  I  V  E  L  -  0  9  5
0  1  2  3  4  5  6  7  8
```
* `cadena[0:5]` extrae `"NIVEL"` (índices 0, 1, 2, 3, 4).
* `cadena[6:9]` extrae `"095"` (índices 6, 7, 8).

Si omitimos el primer parámetro (`cadena[:fin]`), Python empieza desde el principio. Si omitimos el segundo (`cadena[inicio:]`), extrae hasta el final del texto.

---

## 5. Operadores Aritméticos y de Asignación

### Operadores Aritméticos:
* `+` (Suma)
* `-` (Resta)
* `*` (Multiplicación)
* `/` (División real): Devuelve siempre un `float`, aunque el resultado sea entero (ej. `4 / 2` da `2.0`).
* `//` (División entera): Divide descartando los decimales y redondeando hacia abajo (ej. `5 // 2` da `2`).
* `%` (Módulo): Devuelve el resto de la división entera (muy usado para saber si un número es par: `numero % 2 == 0`).
* `**` (Potencia): Eleva un número al exponente (ej. `2 ** 3` da `8`).

### Operadores de Asignación Compuestos:
Permiten realizar un cálculo y asignar el resultado a la misma variable de forma abreviada:
* `x += 3` es equivalente a `x = x + 3`
* `x -= 2` es equivalente a `x = x - 2`
* `x *= 5` es equivalente a `x = x * 5`
* `x /= 2` es equivalente a `x = x / 2`
