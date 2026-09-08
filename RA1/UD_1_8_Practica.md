# Actividad Práctica: Plataformas de Streaming Account Debugger

## 🎯 El Reto
Estás trabajando en el equipo de desarrollo de una aplicación integradora que calcula los costes mensuales de suscripción y el consumo de ancho de banda estimado para hogares que usan cuentas compartidas en plataformas como **Netflix** y **Disney+**.

Un compañero de equipo ha escrito un script preliminar en Python para automatizar estos cálculos, pero tiene **graves errores lógicos**. El script se ejecuta, no muestra errores de sintaxis rojos en la consola, pero **calcula mal los precios, se queda atrapado en bucles infinitos y deniega accesos de red de forma errónea**.

Tu misión es descargar esta plantilla, configurarla en **IntelliJ IDEA**, utilizar el **depurador (Debugger)** con breakpoints para rastrear las variables paso a paso, encontrar los errores lógicos, corregirlos y documentar tus hallazgos.

---

## 🛠️ Instrucciones de Trabajo

1.  Crea un archivo llamado `b1_9_streaming_debugger.py` dentro de tu proyecto en IntelliJ.
2.  Copia y pega de forma exacta el siguiente código inicial (que contiene los errores a propósito):

```python
"""
Plantilla Inicial - Simulador de Cuentas de Streaming (CON ERRORES LÓGICOS)
Propósito: Calcular consumo de red y tarifas de suscripción familiar.
"""

print("--- REPORTE DE INICIO DE SESIÓN DE STREAMING ---")

# --- VARIABLES DE CONFIGURACIÓN ---
PLAN_BASE_NETFLIX = 9.99
PLAN_BASE_DISNEY = 5.99
COSTO_EXTRA_PERFIL_4K = 2.50
DESCUENTO_COMBO = 3.00       # Descuento por contratar ambas plataformas
IMPUESTO_IVA = 0.21         # 21% de IVA aplicable al total final

# --- REGISTRO DE PERFILES ACTIVOS (Bucle de Control) ---
perfiles_registrados = 0
maximo_perfiles = 3

while perfiles_registrados < maximo_perfiles:
    nombre_perfil = input(f"Introduce el nombre para el perfil #{perfiles_registrados + 1}: ")
    nombre_limpio = nombre_perfil.strip()
    print(f"-> Perfil '{nombre_limpio}' registrado con éxito.")
    # NOTA: Aquí debería avanzar el contador de perfiles registrados
    # ¿Por qué se queda el programa pidiendo perfiles infinitamente?

# --- SELECCIÓN DE CALIDAD DE STREAMING ---
print("\n--- CONFIGURACIÓN DE RESOLUCIÓN ---")
perfiles_en_4k = int(input("¿Cuántos de tus perfiles usarán transmisión en Calidad 4K? "))

# --- CÁLCULO DE COSTES (Error de Precedencia y Operaciones Matemáticas) ---
# Queremos sumar los planes base de ambas plataformas, aplicar el descuento por combo,
# sumarle el coste adicional de los perfiles en 4K, y finalmente aplicar el 21% de IVA a todo el conjunto.
subtotal_suscripcion = PLAN_BASE_NETFLIX + PLAN_BASE_DISNEY - DESCUENTO_COMBO + perfiles_en_4k * COSTO_EXTRA_PERFIL_4K
total_con_iva = subtotal_suscripcion * 1.0 + IMPUESTO_IVA  # Algo falla aquí con la multiplicación y suma del IVA

print(f"\nSubtotal calculado: {subtotal_suscripcion}€")
print(f"Total Neto con IVA (Esperado aprox. para 1 perfil 4K = 18.73€): {total_con_iva}€")

# --- ESTIMACIÓN DE ANCHO DE BANDA (Error de Evaluación Lógica) ---
# Reglas de negocio de red:
# Un perfil HD consume 5.0 Mbps de internet. Un perfil 4K consume 15.0 Mbps.
# El ancho de banda total es la suma de los consumos.
# Si el consumo total supera los 20 Mbps y la red NO es de Fibra Óptica (fibra_optica = False), 
# se debe advertir de peligro de cortes de transmisión.

perfiles_hd = maximo_perfiles - perfiles_en_4k
consumo_total_mbps = (perfiles_hd * 5.0) + (perfiles_en_4k * 15.0)

tipo_red = input("\n¿Tu tipo de conexión es 'Fibra' o 'ADSL'?: ").strip().capitalize()
es_fibra = tipo_red == "Fibra"

print(f"\nConsumo estimado de red: {consumo_total_mbps} Mbps")

# Error lógico en la condición de advertencia:
if consumo_total_mbps > 20.0 and es_fibra == True:
    print("⚠️ ADVERTENCIA: Alto consumo de red detectado. Puede experimentar microcortes.")
else:
    print("✅ Estado de conexión: Red estable para el streaming simultáneo.")
```

---

## 🕵️‍♂️ Tu Misión como Depurador

Debes ejecutar este código en **modo depuración (Debug)** en IntelliJ utilizando las siguientes herramientas para dar caza a los fallos:

### Paso 1: Resolver el Bucle Infinito de Perfiles
1. Coloca un **breakpoint** en la línea del `while`.
2. Inicia el depurador (icono del escarabajo 🪲).
3. Introduce el primer nombre de perfil por consola.
4. Observa el panel de variables en la parte inferior. ¿Cuánto vale `perfiles_registrados` antes y después de ingresar el nombre? ¿Cambia su valor?
5. **Corrección:** Añade la línea de código necesaria dentro del bloque para incrementar el contador de perfiles en cada vuelta y evitar que el bucle corra infinitamente.

### Paso 2: Arreglar el Cálculo de la Factura (IVA)
1. Coloca un **breakpoint** justo en la línea del cálculo de `total_con_iva`.
2. Avanza usando **Step Over (F8)**.
3. Examina detalladamente el valor de la variable `total_con_iva`. ¿Por qué da un número decimal absurdo en lugar de aplicar el 21% de IVA sobre el subtotal?
4. **Corrección:** Reestructura la expresión matemática utilizando paréntesis para que el IVA se calcule multiplicando el subtotal por `(1 + IMPUESTO_IVA)`.

### Paso 3: Corregir la Alerta de Red Inestable
1. Lee atentamente la regla de negocio: *"Se debe advertir de peligro de cortes si el consumo supera los 20 Mbps y la conexión **NO** es de Fibra Óptica"*.
2. Ejecuta el programa introduciendo una conexión de tipo `"ADSL"` (por tanto, `es_fibra` valdrá `False`) y forzando un consumo de 35 Mbps (los 3 perfiles en 4K).
3. ¿Por qué el programa te muestra que el estado es `"Red estable"` si debería lanzar la advertencia?
4. Inspecciona la condición del `if` en el depurador.
5. **Corrección:** Corrige el operador lógico de la comparación para que se active cuando `es_fibra` sea `False` (usando `not es_fibra` o `es_fibra == False`).

---

## 📦 Entregables

Para dar por apta esta práctica, debes subir tu archivo `b1_9_streaming_debugger.py` corregido y que cumpla las siguientes condiciones:
* **Ningún bucle infinito:** Debe pedir exactamente 3 nombres de perfiles y avanzar.
* **Cálculos exactos:** El total con IVA debe estar correctamente calculado.
* **Lógica de red corregida:** La alerta debe saltar correctamente si el consumo supera los 20 Mbps en redes ADSL.
* **Buenas Prácticas (PEP 8 y PEP 257):** Todas las variables deben usar nomenclatura en minúsculas y guiones bajos (`snake_case`), las constantes en mayúsculas (`UPPERCASE`) y debes incorporar un **Docstring multilineal** al principio del archivo detallando:
  1. Cuál era el error del bucle `while` y cómo lo solucionaste.
  2. Cuál era el error en el cálculo del IVA.
  3. Cuál era el error en la evaluación condicional de la alerta de red.
