# Actividad Práctica 2.2: Fortnite & RPG Combat Card Generator

En esta actividad práctica, aplicarás los conceptos de **modularización con paso de parámetros** para construir un generador automático de perfiles de personaje y tarjetas de estadísticas de combate en tiempo real para un videojuego multijugador.

---

## 🎮 El Reto: Generador de Fichas de Héroe

Tu tarea consiste en desarrollar un script ejecutable en Python (`b2_2_generador_combate.py`) que contenga una lógica estructurada basada en funciones parametrizadas capaces de procesar la entrada de datos de un jugador, clasificar sus atributos según su clase y nivel, y renderizar una tarjeta de estadísticas en la terminal.

---

## 🛠️ Requisitos Técnicos del Programa

El script debe cumplir rigurosamente con las siguientes pautas de diseño lógico y sintaxis:

### 1. Definición de la Función Principal
Debes implementar la función principal con la siguiente firma exacta:

```python
def crear_tarjeta_combate(nombre, clase, nivel=1, arma_equipada="Espada de Madera", es_vip=False):
    """
    Procesa las estadísticas de combate de un personaje y renderiza su tarjeta en consola.
    
    Parámetros:
    nombre (str): Nombre del personaje.
    clase (str): Clase del personaje (Guerrero, Mago, Pícaro).
    nivel (int): Nivel actual del personaje (por defecto 1).
    arma_equipada (str): Nombre del arma equipada (por defecto "Espada de Madera").
    es_vip (bool): Indica si el jugador posee pase VIP o Premium (por defecto False).
    """
```

### 2. Algoritmo de Procesamiento Interno (Dentro de la Función)
La función debe ejecutar los siguientes cálculos secuenciales basados en los parámetros recibidos:

*   **Saneamiento de cadenas:** Elimina los espacios duplicados o en los extremos del `nombre`, de la `clase` y del `arma_equipada` usando `.strip()`. Además, normaliza el texto de la `clase` para que siempre se evalúe de manera uniforme (ej. convirtiéndolo a título o mayúsculas iniciales).
*   **Cálculo de Estadísticas Base (Uso de `if-elif-else`):**
    *   Si la clase es `"Guerrero"`: Vida Base = `150 + (nivel * 15)`, Daño Base = `20 + (nivel * 3)`.
    *   Si la clase es `"Mago"`: Vida Base = `80 + (nivel * 8)`, Daño Base = `35 + (nivel * 5)`.
    *   Si la clase es `"Pícaro"`: Vida Base = `110 + (nivel * 11)`, Daño Base = `25 + (nivel * 4)`.
    *   Cualquier otra clase introducida se categorizará como `"Novato"` con Vida Base = `100 + (nivel * 10)` y Daño Base = `15 + (nivel * 2)`.
*   **Aplicación de Bonificadores (Uso de Aritmética Booleana o Condicionales):**
    *   **Bono de Arma Épica:** Si el nombre del `arma_equipada` tiene más de **15 caracteres de longitud**, se considera un objeto épico e incrementa el Daño Base final en **+10 puntos**.
    *   **Bono de Cuenta VIP:** Si el parámetro `es_vip` es `True`, la Vida final se incrementa en un **20%** y el Daño final en un **10%** (multiplicando los atributos por `1.20` y `1.10` respectivamente).
*   **Renderizado de la Tarjeta en Consola:**
    *   La función debe imprimir directamente en la terminal un recuadro estético utilizando caracteres ASCII (como `|`, `-`, `*`, `=`) y f-strings con alineaciones limpias que organicen la información del personaje de forma profesional.

### 3. Programa Principal (Ejecución del Script)
En el punto de entrada del script (fuera de la función), debes realizar de forma obligatoria estas **cuatro llamadas** consecutivas para probar la robustez de tus parámetros:

1.  **Llamada Posicional Básica:** Invoca a la función pasando únicamente los argumentos mínimos obligatorios (un nombre y la clase `"Guerrero"`). Observa cómo se aplican los valores por defecto del nivel, arma y estado VIP.
2.  **Llamada con Sobrescritura Parcial:** Invoca a la función pasando los parámetros obligatorios, un nivel avanzado (ej. `12`) y un arma personalizada.
3.  **Llamada con Argumentos Nombrados (Keyword Arguments):** Invoca a la función asignando explícitamente los valores a cada parámetro de forma desordenada (ej. pasando `es_vip` al principio, luego `clase`, luego `nombre`, etc.).
4.  **Llamada Interactiva:** Pide al usuario por teclado (usando `input()`) que introduzca su nombre, elija su clase y escriba su arma. Realiza las conversiones de tipo necesarias (casting) antes de pasarlas a la función.

---

## 📋 Requisitos de Estilo (PEP 8)

*   El código debe estar libre de warnings en tu IDE.
*   Las variables deben seguir el estilo `snake_case`.
*   La función debe contar obligatoriamente con un **Docstring explicativo** multilínea con triple comilla.
*   Usa constantes en mayúsculas (`UPPER_CASE`) si decides guardar modificadores globales de daño o vida.
