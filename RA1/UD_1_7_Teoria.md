# Unidad 1.7: Buenas Prácticas de Estilo, Indentación y Documentación (PEP 8 y PEP 257)

Escribir código en Python no consiste únicamente en lograr que el programa funcione y compile sin errores. En el ámbito profesional, el software pasa por constantes revisiones, mantenimiento y auditorías. Por ello, escribir código legible, homogéneo y autodocumentado es un requisito indispensable.

En lenguajes como Java o C#, las llaves `{}` delimitan físicamente las estructuras. En Python, la estética y la estructura del código están directamente fusionadas a través de la indentación obligatoria y una guía de estilo unificada conocida como **PEP 8**.

---

## 1. La Indentación Física como Estructura Obligatoria

En Python, la indentación es una regla de sintaxis. Si no se respeta, el intérprete detiene la ejecución inmediatamente lanzando un error de tipo `IndentationError`.

*   **Regla de oro:** Se deben utilizar estrictamente **4 espacios** por cada nivel de indentación.
*   **Prohibición técnica:** No se deben mezclar tabuladores (`Tab`) y espacios físicos. Aunque tu editor lo autocomplete, mezclar ambos caracteres genera errores de análisis sintáctico que a menudo son invisibles a la vista pero catastróficos para el intérprete.
*   **Configuración en IDEs:** Editores modernos como IntelliJ o PyCharm están configurados por defecto para convertir la pulsación de la tecla `Tab` en 4 espacios físicos automáticamente.

---

## 2. Guía de Estilo Oficial: PEP 8

La **PEP 8** (*Python Enhancement Proposal 8*) es el documento oficial que define las directrices y convenciones estéticas para escribir código en Python. Su fin primordial es la consistencia y la legibilidad colectiva.

### A. Nomenclatura de Identificadores (Convención de nombres)

| Elemento | Formato de Estilo | Ejemplo Correcto | Ejemplo Incorrecto |
| :--- | :--- | :--- | :--- |
| **Variables Locales** | `snake_case` (minúsculas y guiones bajos) | `intentos_fallidos` | `intentosFallidos` (camelCase) |
| **Funciones** | `snake_case` (minúsculas y guiones bajos) | `calcular_total()` | `calcularTotal()` |
| **Constantes** | `UPPERCASE_WITH_UNDERSCORES` | `MAX_CONEXIONES` | `max_conexiones` |

### B. Espacios en Blanco Alrededor de Operadores
Para facilitar la lectura rápida del flujo aritmético y de asignación, se deben aplicar espacios en blanco de forma equilibrada:

*   **Correcto (Legible):**
    ```python
    resultado = (base * altura) + 10
    total += 1
    ```
*   **Incorrecto (Apelmazado):**
    ```python
    resultado=(base*altura)+10
    total+=1
    ```

### C. Líneas Cortas (Límite de 79 Caracteres)
Las líneas de código no deben extenderse infinitamente hacia la derecha. La recomendación oficial de PEP 8 es limitar el ancho de línea a un máximo de **79 caracteres**. Esto permite abrir varias pestañas de código en paralelo en el monitor sin tener que hacer scroll horizontal.

---

## 3. Comentarios Profesionales vs. Docstrings

Python distingue de forma clara entre notas aclaratorias para programadores y la documentación formal del software accesible en tiempo de ejecución.

### A. Comentarios de Código (`#`)
Se usan exclusivamente para explicar **por qué** se ha tomado una decisión de diseño compleja o no evidente en una línea o bloque específico. No se deben usar para narrar lo obvio.

*   **Mal uso (Redundante):**
    ```python
    x = 10  # Asigna el número 10 a la variable x
    ```
*   **Buen uso (Aclaración necesaria):**
    ```python
    # Usamos un margen de 2 segundos para dar tiempo de respuesta al socket de red
    tiempo_espera = 2.000
    ```

### B. Docstrings (Cadenas de Documentación - PEP 257)
A diferencia de los comentarios, un **Docstring** es una cadena de texto encerrada entre comillas triples (`"""`) que se coloca al inicio de un script, una función o un módulo para explicar **qué hace**, qué parámetros recibe y qué devuelve.

```python
"""
Módulo de procesamiento de facturación de usuarios VIP.
Este script analiza las transacciones pendientes y genera
reportes financieros aplicando los descuentos correspondientes.
"""
```

El Docstring no es ignorado por Python en su totalidad; se asocia al objeto y puede consultarse en vivo usando la función interactiva `help(objeto)` o ser extraído por herramientas externas para generar páginas de documentación de forma 100% automatizada.

---

## 4. Comparativa: Buenas Prácticas en Acción

### Código Sucio y Antipatrón (No cumple convenciones)
```python
MAXIMOS_INTENTOS=3
intentosUser = 0
def ComprobarContrasena(passw):
#Comprobamos si el pass es correcto
  if passw=="Admin123":return True
  else: return False
```

### Código Limpio, Legible y Documentado (PEP 8 y PEP 257)
```python
"""
Módulo de seguridad para la autenticación básica de administradores.
"""

# Constante de seguridad definida según estándar PEP 8
MAX_INTENTOS = 3

def comprobar_contrasena(password):
    """
    Verifica si la contraseña ingresada coincide con la del administrador.
    
    Parámetros:
        password (str): Cadena de texto con la clave a evaluar.
        
    Devuelve:
        bool: True si coincide con la credencial correcta, False en caso contrario.
    """
    clave_correcta = "Admin123"
    return password == clave_correcta
```
