# 📘 Unidad 2.4 – Teoría: Ámbito y Scope de Variables

En cualquier lenguaje de programación, las variables no son accesibles desde cualquier rincón del código por defecto. El espacio donde una variable es visible, activa y utilizable se denomina **ámbito** o **scope**.

En esta unidad analizaremos cómo gestiona Python el ciclo de vida de sus variables y cómo interactúan las funciones con el entorno exterior, comparando el modelo de Python con el tipado estructurado que ya conoces.

---

## 1. El Ámbito de las Variables (Scope)

Cuando declaras una variable en Python, su ubicación determina su "tiempo de vida" y su visibilidad:
*   **Si se crea dentro de una función:** Su existencia se limita al interior de dicha función.
*   **Si se crea fuera de cualquier función:** Es visible en todo el script de Python.

Para entender el orden de búsqueda que realiza Python para encontrar una variable, se utiliza la regla **LEGB** (de menor a mayor nivel de ámbito):
1.  **L**ocal (dentro de la función actual).
2.  **E**nclosing (en funciones anidadas, que veremos más adelante).
3.  **G**lobal (en el nivel superior del script).
4.  **B**uilt-in (nombres predefinidos de Python como `print`, `len`, `int`).

---

## 2. Variables Locales

Una **variable local** es aquella que se define en el interior de una función.

*   **Aislamiento:** Solo existe mientras la función se está ejecutando. Una vez que la función llega a su fin (o ejecuta un `return`), la variable se destruye y la memoria se libera.
*   **Inaccesibilidad:** Si intentas acceder a una variable local desde el programa principal, Python lanzará un error de tipo `NameError`.

```python
def configurar_conexion():
    servidor_local = "192.168.1.50"  # Variable local
    print("Conectando al servidor:", servidor_local)

configurar_conexion()

# Intentar acceder desde fuera de la función
print(servidor_local)  # ❌ Lanza NameError: name 'servidor_local' is not defined
```

*Ventaja:* Esto garantiza la **encapsulación**. Diferentes funciones pueden declarar variables locales con el mismo nombre (por ejemplo, `i` o `resultado`) sin interferir entre sí.

---

## 3. Variables Globales

Una **variable global** es aquella que se declara en el cuerpo principal del script, fuera de cualquier bloque `def`.

*   **Lectura Directa:** Cualquier función dentro del mismo archivo de Python puede **leer** el valor de una variable global sin necesidad de pasarla como parámetro.

```python
empresa = "Spotify Premium"  # Variable global

def mostrar_cabecera():
    # Acceso de lectura directo a la variable global
    print(f"--- Bienvenido a {empresa} ---")

mostrar_cabecera()  # Imprime: --- Bienvenido a Spotify Premium ---
```

---

## 4. El Conflicto de Escritura y la Palabra Clave `global`

Aunque cualquier función puede *leer* una variable global, **no puede modificarla directamente** simplemente asignándole un nuevo valor. 

Si intentas asignar un valor a una variable global dentro de una función, Python asumirá que estás creando una **nueva variable local** con el mismo nombre, dejando la variable global exterior intacta. A esto se le llama **sombreado de variable** (*variable shadowing*).

```python
volumen = 50  # Variable global

def subir_volumen():
    volumen = 80  # ⚠️ Esto NO cambia la global; crea una variable local 'volumen'
    print("Volumen local dentro de la función:", volumen)

subir_volumen()  # Imprime 80
print("Volumen global fuera de la función:", volumen)  # Imprime 50 (no ha cambiado)
```

### La Sentencia `global`

Si realmente necesitas que una función modifique de forma persistente una variable global, debes indicarle explícitamente a Python que la variable que vas a usar pertenece al ámbito global mediante la palabra clave `global`.

```python
reproductor_activo = False  # Variable global

def encender_reproductor():
    global reproductor_activo  # Le decimos a Python que use la variable global
    reproductor_activo = True  # Modificación persistente
    print("Reproductor encendido.")

encender_reproductor()
print("¿Está activo fuera de la función?", reproductor_activo)  # True
```

---

## 5. Buenas Prácticas y Acoplamiento

El uso de la palabra reservada `global` se considera una **mala práctica de diseño** en el software profesional por los siguientes motivos:
1.  **Efectos Secundarios Silenciosos:** Hace que las funciones dependan del estado externo y puedan alterar variables lejanas de forma imprevista, dificultando la depuración.
2.  **Pérdida de Modularidad:** Una función que usa `global` no se puede reutilizar fácilmente en otro programa porque depende de que existan esas variables globales específicas fuera de ella.
3.  **Dificultad de Testeo:** Para probar la función, debes configurar primero el entorno de variables globales de manera manual.

### La Solución Pythónica (Alternativa Limpia)

En lugar de usar `global`, la regla de oro es: **pasa los datos necesarios como parámetros y devuelve los estados modificados con `return`**, asignando el resultado en el flujo principal.

```python
# ❌ Enfoque acoplado con 'global':
saldo = 10.0

def recargar_saldo_malo(cantidad):
    global saldo
    saldo += cantidad

# ✅ Enfoque limpio (Sin 'global'):
def recargar_saldo_bueno(saldo_actual, cantidad):
    """Devuelve el nuevo saldo calculado de forma aislada."""
    return saldo_actual + cantidad

# El programa principal se encarga de reasignar el estado de forma explícita
saldo = 10.0
saldo = recargar_saldo_bueno(saldo, 15.0)  # saldo ahora vale 25.0
```
