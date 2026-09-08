# Unidad 1.6: Control de Bucles (break y continue)

En la unidad anterior aprendimos a crear bucles (`for` y `while`) para repetir tareas de forma automática. Sin embargo, en el desarrollo de software real, a menudo necesitamos alterar el flujo natural de un bucle a mitad de camino:
* Detener el bucle antes de que termine de forma natural (porque ya encontramos lo que buscábamos o por una alerta del sistema).
* Saltarnos un paso concreto y continuar con el siguiente (porque un dato es inválido o queremos filtrar un elemento).

Para controlar estas situaciones, Python nos proporciona dos sentencias clave: **`break`** y **`continue`**.

---

## 1. La Sentencia `break` (Interrupción Inmediata)

La sentencia `break` se utiliza para **romper e interrumpir un bucle de manera inmediata**. 
En cuanto el intérprete de Python lee la instrucción `break`:
1. Detiene la ejecución del bloque del bucle.
2. Sale del bucle por completo (no da más vueltas, aunque queden elementos por recorrer o la condición siga siendo verdadera).
3. Continúa ejecutando el código que haya justo después del bucle.

### Ejemplo Práctico: Buscador de Contenido
Imagina que estamos buscando si una película específica está disponible en una lista de reproducción. En cuanto la encontremos, no tiene sentido seguir buscando y consumiendo recursos:

```python
playlist = ["Inception", "Interstellar", "The Dark Knight", "Tenet"]
pelicula_buscada = "The Dark Knight"

for pelicula in playlist:
    print(f"Comprobando: {pelicula}")
    if pelicula == pelicula_buscada:
        print("¡Encontrada! Deteniendo la búsqueda.")
        break  # Sale del bucle de inmediato

print("Búsqueda finalizada.")
```

**Salida en consola:**
```text
Comprobando: Inception
Comprobando: Interstellar
Comprobando: The Dark Knight
¡Encontrada! Deteniendo la búsqueda.
Búsqueda finalizada.
```
*Nota que "Tenet" nunca llega a comprobarse porque el bucle se detuvo antes.*

---

## 2. La Sentencia `continue` (Saltar de Iteración)

La sentencia `continue` se utiliza para **ignorar el resto del bloque de código en la vuelta actual y saltar directamente a la siguiente iteración**.
A diferencia de `break`, **no detiene el bucle por completo**; simplemente dice: *"Sáltate lo que queda por hacer en esta vuelta y pasa a la siguiente"*.

### Ejemplo Práctico: Filtro de Correos Corporativos
Queremos procesar una lista de correos electrónicos, pero queremos ignorar por completo las cuentas de prueba (`test` o vacías):

```python
correos = ["user1@gmail.com", "", "admin@company.com", "test@company.com", "user2@gmail.com"]

for email in correos:
    # Si el correo está vacío o es de test, lo ignoramos
    if email == "" or "test" in email:
        continue  # Salta a la siguiente iteración
        
    print(f"Enviando notificación a: {email}")
```

**Salida en consola:**
```text
Enviando notificación a: user1@gmail.com
Enviando notificación a: admin@company.com
Enviando notificación a: user2@gmail.com
```

---

## 3. Comparativa Rápida: `break` vs `continue`

| Sentencia | ¿Qué hace con el bucle? | ¿Qué ocurre con la iteración actual? | Caso de uso típico |
| :--- | :--- | :--- | :--- |
| **`break`** | **Lo rompe por completo.** Sale del bucle. | Se interrumpe y no se ejecutan las siguientes líneas. | Salidas de emergencia, menús interactivos ("Salir"), búsquedas completadas. |
| **`continue`** | **Sigue activo.** Pasa a la siguiente vuelta. | Se interrumpe y salta directo a evaluar la siguiente iteración. | Filtros de datos, saltar elementos dañados o vacíos, ignorar restricciones. |

---

## 4. El bucle `while True` (Bucle Infinito Controlado)

Una estructura muy utilizada en aplicaciones profesionales de consola es el bucle `while True`. Este bucle se ejecutará infinitamente a menos que usemos un `break` para romperlo. Es ideal para mantener un programa o menú interactivo activo hasta que el usuario decida salir.

```python
while True:
    accion = input("Escribe 'SALIR' para cerrar el programa: ")
    if accion.strip().upper() == "SALIR":
        print("Cerrando aplicación...")
        break  # Rompe el bucle infinito
        
    print(f"Procesando comando: {accion}")
```
