# Unidad 1.3: Operadores de Comparación y Lógicos (Nivel Operativo)

Esta unidad explica cómo tomar decisiones lógicas en Python evaluando relaciones entre datos y combinando múltiples condiciones. Aprenderás a utilizar operadores que devuelven respuestas binarias (`True` o `False`) y cómo optimiza Python estas evaluaciones en tiempo de ejecución.

---

## 1. Operadores de Comparación (Relacionales)

Los operadores de comparación analizan la relación entre dos valores u objetos. Su evaluación **siempre devuelve un tipo booleano (`True` o `False`)**.

| Operador | Significado | Ejemplo (`a = 15`, `b = 20`) | Resultado |
| :---: | :--- | :--- | :---: |
| `==` | Igual a | `a == b` | `False` |
| `!=` | Distinto de | `a != b` | `True` |
| `>` | Mayor que | `a > b` | `False` |
| `<` | Menor que | `a < b` | `True` |
| `>=` | Mayor o igual que | `a >= 15` | `True` |
| `<=` | Menor o igual que | `b <= 20` | `True` |

### 1.1 Encadenamiento de comparaciones (Sintaxis Matemática)
En lenguajes como Java o C#, para comprobar si una variable está en un rango, debes escribir dos condiciones unidas por un operador lógico:
```java
// Código Java
if (edad >= 18 && edad <= 30) { ... }
```

En Python, puedes **encadenar comparaciones** directamente imitando la notación matemática, lo que hace el código mucho más legible:
```python
edad = 22
# Comprobación de rango en una sola línea limpia
en_rango = 18 <= edad <= 30
print(en_rango)  # True
```

---

## 2. Diferencia Crítica: `==` frente a `is` (Valor vs Identidad)

Es muy común confundir estos dos operadores, pero representan conceptos completamente distintos en la memoria:

*   **El operador `==` (Equivalencia):** Compara si los **valores o contenidos** de dos variables son iguales (equivalente al método `.equals()` de Java).
*   **El operador `is` (Identidad):** Compara si dos variables apuntan **al mismo objeto físico en la memoria** (equivalente al operador `==` de Java para tipos por referencia).

### Ejemplo práctico:
```python
# Creamos dos listas con el mismo contenido
lista_a = [1, 2, 3]
lista_b = [1, 2, 3]
lista_c = lista_a  # Apunta al mismo objeto que lista_a

print(lista_a == lista_b)  # True -> Tienen los mismos elementos
print(lista_a is lista_b)  # False -> Son dos listas independientes en memoria
print(lista_a is lista_c)  # True -> Apuntan exactamente al mismo objeto físico
```

---

## 3. Operadores Lógicos

Los operadores lógicos se utilizan para conectar y evaluar múltiples expresiones condicionales.

| Operador | Significado | Comportamiento |
| :---: | :--- | :--- |
| `and` | Y lógico | Devuelve `True` únicamente si **todas** las condiciones son verdaderas. |
| `or` | O lógico | Devuelve `True` si **al menos una** de las condiciones es verdadera. |
| `not` | Negación | Invierte el valor booleano (pasa de `True` a `False` y viceversa). |

```python
tiene_entrada = True
edad = 20
esta_vetado = False

# El usuario puede entrar si es mayor de edad, tiene entrada y no está vetado
puede_entrar = (edad >= 18) and tiene_entrada and (not esta_vetado)
print(puede_entrar)  # True
```

---

## 4. El Mecanismo de Evaluación en Cortocircuito (Short-Circuit)

Python evalúa las expresiones lógicas de izquierda a derecha. Utiliza un sistema ultraeficiente llamado **evaluación en cortocircuito**: detiene la evaluación de la expresión en cuanto tiene la total certeza del resultado final, sin necesidad de leer el resto de condiciones.

*   **Cortocircuito de `and`:** Si evalúas `Condicion_A and Condicion_B` y la `Condicion_A` es `False`, el resultado final será obligatoriamente falso. Python **no leerá ni ejecutará** la `Condicion_B`.
*   **Cortocircuito de `or`:** Si evalúas `Condicion_A or Condicion_B` y la `Condicion_A` es `True`, el resultado final será inevitablemente verdadero. Python **no leerá ni ejecutará** la `Condicion_B`.

### Retorno de operandos en Python:
A diferencia de otros lenguajes, en Python los operadores `and` y `or` no devuelven necesariamente `True` o `False` de forma nativa. Devuelven **el último operando evaluado** que ha definido el resultado de la expresión:

```python
# Como el primer texto no está vacío (es Truthy), "or" se detiene y lo devuelve
nombre_activo = "Aragorn" or "Invitado"
print(nombre_activo)  # "Aragorn"

# Como el primer texto está vacío (es Falsy), "or" se ve obligado a evaluar el segundo y lo devuelve
nombre_defecto = "" or "Invitado"
print(nombre_defecto)  # "Invitado"
```
Este comportamiento es muy útil en programación para asignar valores por defecto en una sola línea de código sin requerir condicionales complejos.
