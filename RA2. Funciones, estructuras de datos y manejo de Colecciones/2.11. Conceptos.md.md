# Unidad 2.11: Colecciones de Datos - Iteradores e Iterables

En el desarrollo de software profesional y en sistemas de automatización en tiempo real (como la monitorización de infraestructura cloud o el análisis continuo de logs de seguridad), es común procesar flujos masivos de datos elemento por elemento. Para entender cómo funciona el bucle `for` bajo el capó y cómo gestionar flujos de datos de manera eficiente en memoria, debemos comprender dos conceptos fundamentales en Python: los **iterables** y los **iteradores**.

---

## 1. ¿Qué es un Iterable?

Un **iterable** es cualquier objeto en Python que es capaz de devolver sus elementos uno a uno. Contiene datos pero no guarda el estado de por dónde va la lectura.

### Ejemplos comunes de iterables:
*   Colecciones mutables e inmutables: `list`, `tuple`, `set`, `dict`.
*   Cadenas de texto: `str` (se recorren carácter a carácter).
*   Generadores lógicos de rangos: `range()`.

Cualquier iterable se puede recorrer directamente usando un bucle `for`:

```python
servidores = ["SRV-Web", "SRV-DB", "SRV-Mail"]

for s in servidores:
    print(s)
```

---

## 2. ¿Qué es un Iterador?

Un **iterador** es el objeto físico encargado de **realizar el recorrido** sobre un iterable, manteniendo en memoria el estado actual de la iteración (sabe exactamente qué elemento ha procesado y cuál es el siguiente).

### Cómo trabajar con iteradores de forma manual:
1.  **Crear el iterador:** Se pasa el iterable a la función integrada `iter()`.
2.  **Avanzar de elemento:** Se solicita el siguiente valor con la función integrada `next()`.
3.  **Fin de la iteración:** Cuando no quedan más elementos, `next()` lanza de forma automática la excepción **`StopIteration`**.

```python
ips_atacantes = ["10.0.0.1", "192.168.1.50"]

# 1. Creamos el iterador
lector = iter(ips_atacantes)

# 2. Solicitamos elementos secuencialmente
print(next(lector))  # Salida: 10.0.0.1
print(next(lector))  # Salida: 192.168.1.50

# 3. Al quedarse sin elementos, lanza error de parada
print(next(lector))  # ❌ Lanza excepción: StopIteration
```

---

## 3. Comparativa: Iterable vs. Iterador

| Propiedad | Iterable | Iterador |
| :--- | :--- | :--- |
| **¿Contiene los datos?** | Sí, es el contenedor original de la información. | No necesariamente; solo conoce el flujo y la posición actual. |
| **¿Mantiene el estado?** | No. Si se interrumpe, no sabe por dónde iba. | Sí. Recuerda siempre cuál es el siguiente elemento. |
| **¿Soporta `next()`?** | No. Intentar hacer `next(lista)` dará un error de tipo `TypeError`. | Sí. Es su operación fundamental para avanzar en memoria. |
| **Métodos internos** | Implementa el método especial `__iter__()`. | Implementa los métodos especiales `__iter__()` y `__next__()`. |

---

## 4. Avance Seguro: `next()` con Valor por Defecto

En entornos de producción (como un script de respuesta automatizada a incidentes), permitir que un programa se detenga con una excepción no controlada (`StopIteration`) es un grave error de diseño.

Para evitar esto, la función `next()` permite pasar un **segundo argumento opcional** que actúa como valor de seguridad o centinela. Si el iterador se agota, en lugar de lanzar un error, devolverá ese valor (habitualmente `None`):

```python
nombres = ["admin", "analista"]
it = iter(nombres)

# Consumo manual seguro
while True:
    usuario = next(it, None)
    if usuario is None:
        print("Fin de la lista de usuarios. Deteniendo auditoría.")
        break
    print(f"Auditando acceso de: {usuario}")
```

Esta técnica nos permite simular el comportamiento de un bucle `for` pero con un control absoluto sobre el momento exacto en el que solicitamos el siguiente dato al procesador.

---

## 5. Iteradores con Función Centinela (Generadores Infinitos)

Python permite utilizar `iter()` de una forma avanzada: pasando una **función** y un **valor centinela** de parada:

```python
import random

def generar_puerto():
    # Simula la lectura de puertos abiertos en un firewall
    return random.randint(20, 25)

# El iterador llamará a generar_puerto() indefinidamente hasta que devuelva el puerto 22 (SSH)
it_puertos = iter(generar_puerto, 22)

for puerto in it_puertos:
    print(f"Escaneando puerto seguro: {puerto}")

print("🚨 ¡Alerta! Detectado puerto 22 (SSH) abierto. Deteniendo escaneo.")
```

---
