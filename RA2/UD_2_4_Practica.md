# 🎮 Unidad 2.4 – Actividad Práctica: Spotify Premium Sync & Global Volume Controller

En esta actividad práctica, diseñarás un simulador para la gestión de sesiones de reproducción y sincronización de música en **Spotify**. Trabajarás directamente con el concepto de **ámbito de variables**, experimentando la diferencia entre variables locales y globales, los efectos de sombreado y el uso seguro de parámetros frente al uso de variables globales.

---

## 🎯 Objetivos de la Actividad
*   Aplicar la distinción práctica entre variables de ámbito global y de ámbito local.
*   Resolver problemas de modificación de estado utilizando de forma controlada la palabra clave `global`.
*   Refactorizar funciones acopladas para trabajar con paso de parámetros y sentencias de retorno (`return`), aplicando buenas prácticas de diseño.
*   Construir un script interactivo basado en menús que mantenga y actualice estados en tiempo de ejecución.

---

## 💻 El Reto: Spotify Playback Simulator (`b2_4_spotify_sync.py`)

Debes crear un programa en Python que simule la gestión de estado de un reproductor de Spotify activo. Para ello, el programa contará con **tres variables globales** que representan el estado general de la sesión:

1.  `USUARIO_PREMIUM` (Booleano): Indica si el usuario actual tiene contratado el plan Premium (`True` o `False`). Por defecto empezará en `False`.
2.  `DISPOSITIVO_ACTIVO` (Cadena de texto): Representa el dispositivo donde suena la música (por ejemplo, `"Phone"`, `"PC"`, `"SmartTV"`). Por defecto será `"PC"`.
3.  `VOLUMEN_GLOBAL` (Entero): Almacena el nivel de volumen global del reproductor (de 0 a 100). Por defecto será `50`.

---

### 🧩 Requisitos y Estructura del Script

Debes implementar **cuatro funciones específicas** en tu archivo de Python:

#### 1. `mostrar_estado_reproductor()`
*   **Comportamiento:** Debe acceder y leer de forma **directa** las tres variables globales (`USUARIO_PREMIUM`, `DISPOSITIVO_ACTIVO`, `VOLUMEN_GLOBAL`) para mostrar en pantalla un panel visual con el estado actual del reproductor de Spotify.
*   **Restricción:** No debe recibir ningún parámetro ni usar la palabra clave `global` (ya que solo necesita acceso de lectura).

#### 2. `actualizar_suscripcion_premium()`
*   **Comportamiento:** Debe cambiar el estado de la variable global `USUARIO_PREMIUM` a `True`. Mostrará un mensaje informativo confirmando que la cuenta ha sido mejorada.
*   **Restricción:** Para esta función de modificación directa, **es obligatorio** utilizar la palabra clave `global`.

#### 3. `cambiar_dispositivo_con_sombreado(nuevo_dispositivo)`
*   **Comportamiento:** Esta función recibirá una cadena con el nombre de un dispositivo. Intentará asignar directamente `DISPOSITIVO_ACTIVO = nuevo_dispositivo`. Al finalizar, imprimirá el dispositivo configurado desde dentro de la función.
*   **Propósito Didáctico:** No debes usar la palabra clave `global` en esta función. Esto provocará un efecto de "sombreado" (*variable shadowing*) que mantendrá intacta la variable global exterior. Servirá para evidenciar este fenómeno técnico en la defensa presencial.

#### 4. `ajustar_volumen_seguro(volumen_actual, incremento)`
*   **Comportamiento:** Esta función representa la **buena práctica de diseño**. No debe acceder a ninguna variable global ni usar `global`.
*   **Entrada:** Recibe por parámetros el nivel de volumen actual (entero) y el incremento o decremento deseado (entero positivo o negativo).
*   **Lógica:** Calcula el nuevo volumen. Debe asegurarse de que el resultado se mantenga en el rango de **0 a 100** (si el cálculo es menor de 0, se queda en 0; si es mayor de 100, se queda en 100).
*   **Retorno:** Devuelve el valor numérico del volumen ajustado con un `return`.

---

### 🎛️ Programa Principal (Flujo Interactivo)

El cuerpo principal del script debe normalizar la entrada del usuario y ejecutar un bucle `while True` que muestre un menú de opciones interactivo por terminal:

```text
=== SPOTIFY SYNC MANAGER ===
1. Mostrar estado del reproductor
2. Activar suscripción Premium (Prueba de variable global)
3. Cambiar dispositivo activo (Prueba de sombreado de variable)
4. Ajustar volumen (Prueba de función parametrizada con retorno seguro)
5. Salir
```

*   **Opción 1:** Llama a `mostrar_estado_reproductor()`.
*   **Opción 2:** Llama a `actualizar_suscripcion_premium()`.
*   **Opción 3:** Solicita al usuario un dispositivo por teclado (ej. `"Phone"`). Limpia los espacios sobrantes y llama a `cambiar_dispositivo_con_sombreado()`.
*   **Opción 4:** Solicita por teclado un nivel de cambio (por ejemplo, `20` para subir volumen, o `-15` para bajarlo). Convierte la entrada a entero. Llama a `ajustar_volumen_seguro()` pasándole como argumentos el valor de la variable global `VOLUMEN_GLOBAL` y el incremento introducido. **Asigna el valor retornado directamente** a la variable global `VOLUMEN_GLOBAL`.
*   **Opción 5:** Finaliza la ejecución mostrando un saludo.

---

## 📋 Requisitos Formales de Entrega
*   Escribe el código siguiendo estrictamente las convenciones estéticas de la **PEP 8** (nombres de variables en minúsculas y con guiones bajos, constantes globales en mayúsculas, espaciado adecuado alrededor de operadores y dos líneas en blanco de separación entre funciones).
*   Cada función debe comenzar con su **Docstring** explicativo correspondiente (PEP 257) encerrado entre comillas triples `"""`.
*   Añade comentarios cortos explicativos en los puntos clave de la lógica para justificar por qué usas (o no) la sentencia `global`.
