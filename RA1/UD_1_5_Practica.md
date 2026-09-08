# Actividad Práctica 1.5: Gacha Summon Simulator & Premium Currency Manager

En los videojuegos modernos (especialmente en los géneros RPG, Gacha y móviles como *Genshin Impact*, *Brawl Stars* o *Clash Royale*), las aperturas de cofres, sobres o invocaciones ("summons") son componentes clave. Detrás de la interfaz visual hay algoritmos que controlan el gasto de las monedas premium (gemas, diamantes) y registran las estadísticas de los drops obtenidos.

En esta actividad, simularás el backend de un motor de invocaciones premium utilizando bucles de control.

---

## 🎯 Objetivos de la Actividad
*   Utilizar el bucle `for` y la función `range()` para iterar un número fijo de repeticiones de forma controlada.
*   Recorrer secuencias de texto (`str`) carácter por carácter para filtrar y procesar datos compactos.
*   Implementar bucles `while` basados en condiciones cambiantes para gestionar recursos de forma dinámica.
*   Consolidar las buenas prácticas de nomenclatura (PEP 8) e indentación obligatoria.

---

## 🧱 Requisitos del Programa (`b1_6_gacha_simulator.py`)

Debes desarrollar un script interactivo en Python que conste de **tres fases estructuradas**, ejecutadas de forma secuencial (una tras otra). 

*⚠️ Restricción Estricta: No puedes usar funciones (`def`), listas (`[]`), tuplas (`()`) ni modificadores de bucle (`break` o `continue`), ya que son temas de unidades posteriores.*

### 🪙 Fase 1: El Multi-Summon de 10 Invocaciones (Bucle `for` con `range`)
La forma clásica de invocar en estos juegos es realizar una tirada masiva de 10 intentos (un "10-pull").
1.  Utilizando un bucle `for` y la función `range()`, simula la apertura consecutiva de **10 sobres**.
2.  En cada iteración (del 1 al 10), debes mostrar en consola el número de invocación y acumular los puntos de Experiencia de Cuenta (XP) que recibe el jugador.
3.  La XP se calcula de forma matemática: cada invocación otorga una base de **100 XP**, sumándole el número de la iteración multiplicado por 10 (ej: la tirada 1 otorga `110 XP`, la tirada 2 otorga `120 XP`...).
4.  Al finalizar el bucle, muestra en consola la cantidad **total de XP acumulada** por el jugador durante la tirada masiva.

### 🔮 Fase 2: El Decodificador de Recompensas (Bucle `for` sobre Cadena)
El servidor web envía los resultados de los drops en un string compacto de ancho fijo para ahorrar datos. El string contiene códigos de recompensas: `'C'` (Común), `'R'` (Raro) y `'E'` (Épico).
1.  Define en tu código de forma fija la cadena: `trama_drops = "CRCCERCCRE"`
2.  Utilizando un único bucle `for`, recorre la cadena carácter por carácter.
3.  Debes contar cuántos elementos de cada categoría se han obtenido (inicializa contadores de tipo entero para comunes, raros y épicos).
4.  Calcula también el **Polvo Estelar** de desmantelamiento acumulado según los drops obtenidos, sabiendo que:
    *   Cada ítem Común (`'C'`) otorga **10** de Polvo Estelar.
    *   Cada ítem Raro (`'R'`) otorga **50** de Polvo Estelar.
    *   Cada ítem Épico (`'E'`) otorga **250** de Polvo Estelar.
5.  *Pista condicional:* Utiliza las estructuras `if-elif-else` que aprendiste en la unidad anterior dentro de tu bucle para clasificar cada letra.
6.  Al finalizar el bucle, imprime un **reporte de inventario** detallado con el conteo de cada categoría de recompensa y el Polvo Estelar total obtenido.

### 💎 Fase 3: El Vaciador de Gemas (Bucle `while`)
Ahora simularás el gasto continuo de la moneda premium del juego.
1.  Pide al usuario por consola que introduzca su **saldo actual de gemas** (debes hacer un casting seguro a entero `int()`).
2.  Cada tirada individual en este banner premium tiene un coste fijo de **160 gemas**.
3.  Mediante un bucle `while`, simula compras consecutivas de sobres individuales **mientras el saldo de gemas del usuario sea suficiente** (mayor o igual a 160).
4.  En cada vuelta del bucle debes:
    *   Restar las 160 gemas del saldo.
    *   Incrementar un contador de sobres comprados.
    *   Imprimir en consola el estado actual: `"Gema gastada. Sobres acumulados: X | Gemas restantes: Y"`.
5.  Al completarse el bucle, muestra el resumen final: el **total de sobres comprados** y el **pico de gemas sobrantes** que han quedado huérfanas en la cuenta del usuario.

---

## 📋 Ejemplo de Salida Esperada en Consola

```
=== FASE 1: INICIANDO MULTI-SUMMON (10-PULL) ===
Abriendo sobre 1... ¡Drop registrado! (+110 XP)
Abriendo sobre 2... ¡Drop registrado! (+120 XP)
...
Abriendo sobre 10... ¡Drop registrado! (+200 XP)
>>> Invocación completada. Total de XP ganada: 1550 XP

=== FASE 2: DECODIFICANDO TRAMA DE DROPS ===
Procesando drops del servidor: CRCCERCCRE
* Registro: Drop Común ('C') procesado (+10 Polvo Estelar)
* Registro: Drop Raro ('R') procesado (+50 Polvo Estelar)
...
>>> REPORT DE DROPS COMPLETO:
- Ítems Comunes: 5
- Ítems Raros: 3
- Ítems Épicos: 2
- Polvo Estelar Total: 700

=== FASE 3: SIMULADOR DE COMPRA AUTOMÁTICA ===
Introduce tu saldo de gemas para invocar (160 gemas/sobre): 500
[Procesando...]
Gema gastada. Sobres acumulados: 1 | Gemas restantes: 340
Gema gastada. Sobres acumulados: 2 | Gemas restantes: 180
Gema gastada. Sobres acumulados: 3 | Gemas restantes: 20

>>> COMPRA COMPLETADA:
- Sobres totales adquiridos: 3
- Saldo final sobrante: 20 gemas
```
