# Unidad 2.3: Actividad Práctica (Funciones con Valores de Retorno)

## 🎮 Nombre de la Actividad: **Epic Store & Steam: Coin Converter & Checkout Router**

---

## 📝 El Enunciado

En el backend de las tiendas de videojuegos modernas como **Steam** o **Epic Games Store**, los flujos de cobro, conversión de divisas físicas a monedas virtuales y la aplicación de descuentos promocionales se gestionan de forma totalmente aislada e independiente mediante funciones. 

Tu objetivo en esta práctica es desarrollar un script interactivo de consola (`b2_3_checkout_store.py`) que actúe como pasarela de facturación y enrutamiento de divisas virtuales de un usuario.

El programa debe solicitar al usuario que introduzca tres datos por consola usando `input()`:
1. El **importe base** en euros (€) que desea gastar (que debes validar y convertir a decimal `float`).
2. Un **código de descuento** promocional (debe ser una cadena de texto).
3. La **región fiscal** de compra del usuario: (`"ES"` para España, `"US"` para Estados Unidos, o `"JP"` para Japón).
4. La **moneda virtual de destino** a la que desea convertir su dinero: (`"VBUCKS"` de Fortnite, `"RP"` de League of Legends, o `"STEAM_WALLET"` de la cartera de Steam).

---

## 🛠️ Estructura de Funciones Requerida

Para garantizar un código limpio, modular y profesional, debes definir obligatoriamente las siguientes **tres funciones independientes**. Ninguna de estas funciones puede contener llamadas a `print()` o `input()` en su interior; se limitarán estrictamente a procesar parámetros de entrada y devolver resultados reales mediante `return`.

### 1. `calcular_descuento_promocional(monto_base, codigo_promo)`
*   **Propósito:** Evalúa el código promocional introducido y devuelve el **importe descontado** (no el porcentaje, sino el dinero real que se resta del precio base).
*   **Parámetros:** 
    *   `monto_base` (`float`): Dinero base de la compra.
    *   `codigo_promo` (`str`): Código ingresado por el usuario.
*   **Lógica de cupones:**
    *   Si el código es `"GAMER20"`, devuelve el **20%** del monto base.
    *   Si el código es `"PROMO10"`, devuelve el **10%** del monto base.
    *   Para cualquier otro código introducido (o vacío), devuelve **0.0** euros de descuento.
*   **Retorno:** El valor del descuento calculado (`float`).

### 2. `aplicar_impuestos_regionales(monto_con_descuento, region)`
*   **Propósito:** Calcula y devuelve el **precio final total** una vez aplicados los impuestos correspondientes según la región fiscal elegida.
*   **Parámetros:**
    *   `monto_con_descuento` (`float`): Importe tras restar el descuento obtenido.
    *   `region` (`str`): Iniciales de la región de facturación.
*   **Lógica de impuestos:**
    *   Si la región es `"ES"` (España), aplica un **21%** de IVA.
    *   Si la región es `"US"` (Estados Unidos), aplica un **10%** de tasas.
    *   Si la región es `"JP"` (Japón), aplica un **5%** de tasas de consumo.
    *   Si la región introducida es inválida o no reconocida, el sistema asumirá una tasa internacional del **15%** por defecto.
*   **Retorno:** El precio total final con impuestos incluidos (`float`).

### 3. `convertir_a_moneda_virtual(precio_total_euros, divisa_destino)`
*   **Propósito:** Convierte el importe monetario final en la cantidad correspondiente de la divisa virtual seleccionada por el usuario.
*   **Parámetros:**
    *   `precio_total_euros` (`float`): Dinero total final cobrado al usuario.
    *   `divisa_destino` (`str`): Tipo de moneda del videojuego.
*   **Tasa de conversión:**
    *   `"VBUCKS"`: 1 euro equivale a **100 V-Bucks** (Fortnite).
    *   `"RP"`: 1 euro equivale a **85 Riot Points** (Valorant / League of Legends).
    *   `"STEAM_WALLET"`: 1 euro equivale a **1.12 dólares de saldo** en la cartera.
    *   Cualquier otra divisa no reconocida devolverá **0.0** monedas (transacción cancelada).
*   **Retorno:** El número de monedas virtuales generadas en el videojuego (`float`).

---

## 📂 Flujo del Programa Principal

En tu bloque de ejecución principal (fuera de las funciones), debes estructurar el flujo interactivo de la siguiente manera:

1. **Saneamiento de datos:** El usuario puede introducir los textos con espacios iniciales/finales o mezclando mayúsculas y minúsculas (por ejemplo: `"es  "`, `"Vbucks"`, `"gamer20"`). Debes normalizar y limpiar los textos de entrada en el programa principal utilizando métodos de cadena como `.strip().upper()` antes de pasarlos como argumentos.
2. **Control de importes mínimos:** Si el usuario introduce un monto numérico base de euros menor o igual a **0.0**, el sistema debe notificar el error de inmediato y no realizar ningún cálculo.
3. **Cálculo encadenado de llamadas:** Invoca secuencialmente las funciones creadas pasándoles el resultado obtenido de la anterior:
    *   Paso A: Obtén el descuento usando `calcular_descuento_promocional`.
    *   Paso B: Resta el descuento al monto base.
    *   Paso C: Pasa el monto resultante a `aplicar_impuestos_regionales` para obtener el total a cobrar en euros.
    *   Paso D: Pasa ese precio final en euros a `convertir_a_moneda_virtual` para saber qué saldo virtual recibe el usuario.
4. **Ticket de Compra Visual:** Imprime un informe estético en la terminal con un formato profesional utilizando f-strings. El reporte debe mostrar claramente el desglose de importes (redondeados a exactamente dos decimales), los códigos aplicados y la cantidad neta final de monedas virtuales asignadas a su cuenta del juego.

---

## 📋 Requisitos Formales de Estilo

*   **PEP 8 Estricto:** Sigue las convenciones estéticas oficiales de Python (nombres de variables y funciones en `snake_case`, constantes en `UPPERCASE`, espaciado limpio alrededor de operadores y dos líneas en blanco de separación entre funciones).
*   **Docstrings Obligatorios:** Cada una de las tres funciones debe ir encabezada obligatoriamente por un Docstring descriptivo que detalle su propósito, sus parámetros y su tipo de retorno.
