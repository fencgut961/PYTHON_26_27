# Unidad 1.4: Estructuras Condicionales (`if...elif...else` y `match-case`)

En programación, los algoritmos rara vez son líneas secuenciales que se ejecutan una detrás de otra de principio a fin. Para crear aplicaciones útiles y dinámicas, los programas deben tomar decisiones basadas en datos de entrada.

En esta unidad aprenderás a controlar el flujo de tu aplicación en Python utilizando las estructuras condicionales tradicionales y la sentencia moderna de coincidencia de patrones.

---

## 1. La Indentación como Estructura de Control

A diferencia de lenguajes como Java, C++ o C#, que delimitan los bloques de código utilizando llaves `{ }`, **Python utiliza la indentación física (espacios en blanco) obligatoria**. 

*   Cada bloque de código condicional debe estar desplazado exactamente **4 espacios a la derecha** con respecto a la línea del condicional.
*   La cabecera de la estructura condicional siempre finaliza con el carácter de dos puntos (`:`).
*   Si la indentación no es consistente o se mezclan tabuladores y espacios, el intérprete lanzará un error de sintaxis (`IndentationError`) o provocará fallos de lógica invisibles.

```python
# Ejemplo correcto ✅
edad = 18
if edad >= 18:
    print("Acceso permitido")
    print("Bienvenido al sistema")  # Ambos prints pertenecen al mismo bloque

# Ejemplo incorrecto ❌
if edad >= 18:
print("Acceso permitido")  # Error de indentación (IndentationError)
```

---

## 2. Bifurcaciones Condicionales: `if...elif...else`

La estructura `if` evalúa una expresión booleana. Si es verdadera (`True`), ejecuta el bloque de código indentado. Si es falsa (`False`), el intérprete ignora el bloque.

### 2.1 Estructura Simple (`if`)
```python
temperatura = 26
if temperatura > 25:
    print("Activar sistema de refrigeración.")
```

### 2.2 Estructura Doble (`if...else`)
Permite definir un camino alternativo en caso de que la condición no se cumpla.
```python
temperatura = 18
if temperatura > 25:
    print("Activar refrigeración.")
else:
    print("Temperatura en rango normal.")
```

### 2.3 Estructura Múltiple (`if...elif...else`)
Cuando existen más de dos escenarios posibles, se encadenan condiciones usando `elif` (contracción de *else if*). Python evalúa las condiciones de arriba a abajo y ejecuta **solo el primer bloque cuya condición resulte verdadera**.

```python
nota = 78

if nota >= 90:
    print("Sobresaliente")
elif nota >= 70:
    print("Notable")
elif nota >= 50:
    print("Aprobado")
else:
    print("Suspenso")
```

### 💡 Simplificación de Rangos en Python
En lenguajes como Java, para comprobar si un valor está dentro de un rango debes usar operadores lógicos: `if (nota >= 70 && nota < 90)`. Python permite simplificar esta sintaxis escribiendo una **comparación matemática encadenada**, lo que hace el código mucho más legible:

```python
if 70 <= nota < 90:
    print("El alumno tiene un notable.")
```

---

## 3. Coincidencia de Patrones Moderna: `match-case`

Introducida en Python 3.10, la estructura `match-case` sustituye al tradicional `switch-case` de otros lenguajes. Es ideal para evaluar una variable contra múltiples valores exactos.

### 3.1 Ventajas frente al `switch` tradicional:
*   **Sin caída accidental (*no fall-through*):** No requiere escribir la palabra `break` al final de cada caso. Python ejecuta únicamente el bloque que coincide y sale automáticamente de la estructura.
*   **Agrupamiento sencillo:** Se pueden agrupar varios valores en un mismo caso utilizando el operador de tubería o barra vertical (`|`).
*   **Caso por defecto implícito:** El caso por defecto (equivalente al `default` de Java) se define utilizando el guion bajo (`_`), que actúa como un comodín para capturar cualquier valor no especificado previamente.

```python
dia_semana = "Sábado"

match dia_semana:
    defecto = "Lunes"
    case "Lunes" | "Martes" | "Miércoles" | "Jueves" | "Viernes":
        print("Día laborable. Toca programar.")
    case "Sábado" | "Domingo":
        print("Fin de semana. Hora de descansar.")
    case _:
        print("Día no válido.")
```

---

## 4. Criterios de Selección: `if` frente a `match-case`

Para mantener tu código limpio y estructurado, aplica las siguientes pautas de diseño:

| Criterio | `if...elif...else` | `match-case` |
| :--- | :--- | :--- |
| **Tipo de condiciones** | Expresiones lógicas complejas, desigualdades (`>`, `<`, `>=`), o rangos dinámicos. | Coincidencias de valores exactos o patrones de texto predefinidos. |
| **Versatilidad** | Permite evaluar distintas variables en una misma estructura. | Se centra en evaluar el estado o valor de una única variable. |
| **Legibilidad** | Puede volverse confuso si se anidan demasiados niveles o se acumulan decenas de condiciones. | Altamente ordenado y fácil de mantener para listas de opciones cerradas. |

---

## 5. Limpieza y Normalización de Entradas de Texto

Cuando comparamos cadenas de texto que el usuario introduce por consola, es muy probable que surjan problemas de validación por diferencias de mayúsculas, minúsculas o espacios accidentales. Antes de evaluar una variable de texto en un condicional, aplica estos métodos básicos de normalización:

*   `.strip()`: Elimina espacios en blanco sobrantes al inicio y al final de la cadena.
*   `.lower()`: Convierte toda la cadena a minúsculas.
*   `.upper()`: Convierte toda la cadena a mayúsculas.
*   `.capitalize()`: Convierte la primera letra en mayúscula y el resto en minúsculas.

```python
entrada = "  nEtFlIx  "
normalizado = entrada.strip().lower()  # "netflix"
```
