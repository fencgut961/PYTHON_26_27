# Unidad 2.1 – SteamArcade Launcher: RPG Retro-Console Menu (Práctica)

## 🎯 Objetivo de la Actividad
Aplicar la modularización de código en Python creando un lanzador de consola interactivo para un sistema de juegos arcade retro. El alumno deberá estructurar el programa utilizando funciones independientes para organizar el flujo del menú, la interfaz estética de bienvenida y la visualización de créditos, evitando repetir instrucciones en el programa principal.

---

## 🕹️ Contexto del Proyecto
Estás desarrollando la interfaz de control de **SteamArcade**, una consola física de salón de juegos diseñada para lanzar cartuchos clásicos de RPG. El sistema requiere un script de comandos que muestre una marquesina retro, un menú de navegación de opciones interactivo y una pantalla de créditos de desarrollo, todo organizado mediante funciones bien documentadas.

---

## 🧱 Requisitos del Programa

Debes crear un único archivo llamado `b2_1_steamarcade_menu.py` que cumpla con los siguientes requisitos estructurales y funcionales:

### 1. Requisitos de Modularización (Funciones Obligatorias)
Debes definir exactamente las siguientes tres funciones, asegurando que **ninguna de ellas reciba parámetros ni devuelva ningún valor**:

*   `mostrar_intro_retro()`:
    *   Debe imprimir en pantalla una marquesina de bienvenida con diseño estético de líneas (utilizando `=`, `*` o `#`) que simule la pantalla de inicio de un mueble arcade clásico de los años 80.
    *   Debe incluir un **Docstring** de una sola línea detallando su propósito.
*   `mostrar_menu_principal()`:
    *   Debe mostrar las opciones del lanzador de forma clara:
        *   `[1] Iniciar cartucho RPG: Chrono Trigger`
        *   `[2] Comprobar estado de ranura de créditos`
        *   `[3] Ver créditos de desarrollo`
        *   `[4] Apagar consola`
    *   Debe incluir un **Docstring** de una sola línea detallando su propósito.
*   `mostrar_creditos_desarrollo()`:
    *   Debe imprimir en pantalla el listado de desarrolladores, el año del copyright y el aviso legal del sistema de archivos de SteamArcade.
    *   Debe incluir un **Docstring** de una sola línea detallando su propósito.

### 2. Estructura de Control en el Programa Principal
En el flujo principal del programa (al final del archivo, respetando el orden secuencial de Python):
*   Se debe llamar inicialmente a la función `mostrar_intro_retro()`.
*   A continuación, se debe implementar un bucle controlado de persistencia (`while True`) que realice los siguientes pasos:
    1.  Llamar a la función `mostrar_menu_principal()`.
    2.  Solicitar al usuario que elija una opción mediante la consola con `input()`.
    3.  Evaluar la opción seleccionada utilizando la estructura condicional moderna `match-case` (o en su defecto, `if-elif-else`):
        *   Si selecciona `1`: Imprimir un mensaje descriptivo que simule la carga del cartucho (ej. `"💾 Cargando ROM... Chrono Trigger cargado con éxito en el banco de memoria!"`).
        *   Si selecciona `2`: Imprimir el estado actual de la moneda simulada (ej. `"🪙 Estado de Ranura: Créditos cargados [05]. Sistema listo para jugar."`).
        *   Si selecciona `3`: Invocar de forma explícita a la función `mostrar_creditos_desarrollo()`.
        *   Si selecciona `4`: Imprimir un mensaje de despedida ordenado (ej. `"🔌 Apagando sistemas de SteamArcade... ¡Hasta la próxima!"`) y romper el bucle (`break`) para finalizar la ejecución.
        *   Cualquier otra opción: Mostrar un aviso informando que la opción introducida es inválida y volver a pintar el menú.

### ⚠️ Restricciones Estrictas de Programación
*   **Prohibición de paso de argumentos:** Ninguna función puede declarar parámetros entre sus paréntesis `()`.
*   **Prohibición de retorno de datos:** Ninguna función puede incluir la sentencia `return`. Toda la interacción e impresión se realiza directamente dentro de ellas.
*   **Normas PEP 8:** Los nombres de las funciones y variables deben usar el estilo `snake_case`. El código debe estar correctamente indentado (4 espacios) y sin código colapsado en una sola línea.
*   **Docstrings:** Cada una de las funciones debe tener obligatoriamente su docstring multilínea o unilínea con comillas triples `"""`.
