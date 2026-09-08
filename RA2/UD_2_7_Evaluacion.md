# Unidad 2.7: Guía Docente - Evaluación y Control del Aula

Esta guía docente proporciona la estructura de evaluación presencial, la solución de referencia y las herramientas prácticas necesarias para el control y la corrección de la actividad en directo.

---

## 📊 1. Esquema de Calificación Simplificado

Para agilizar el proceso de corrección y garantizar la transparencia, se aplicarán tres niveles de calificación directa basados en evidencias observables:

*   **Apto (A)**:
    *   Crea, lee, actualiza y elimina de forma correcta parejas clave-valor utilizando diccionarios.
    *   No utiliza acceso directo por corchetes `[]` en zonas críticas sin validar previamente la existencia de la clave, priorizando el uso de `.get()` o validación con `in`.
    *   El programa cuenta con un menú interactivo basado en `match-case` que funciona de forma ininterrumpida ante entradas erróneas del usuario.
    *   El código está perfectamente estructurado y comentado bajo la norma PEP 8, incluyendo Docstrings explicativos.
*   **Apto con Recuperación (AR)**:
    *   La lógica CRUD básica funciona, pero el código carece de controles preventivos contra excepciones comunes (por ejemplo, el programa se rompe al intentar consultar o borrar un ticket que no existe debido al uso de corchetes directos sin validar).
    *   Falta modularización o el script concentra toda la lógica en un solo bloque lineal sin usar funciones.
    *   Se observan nombres de variables genéricos o mala indentación física.
*   **No Apto (NA)**:
    *   El programa no es funcional, presenta errores de sintaxis insalvables o carece de la lógica básica de manipulación de diccionarios exigida en la unidad.

---

## 💻 2. Solución Oficial de Referencia (PEP 8)

A continuación, se detalla el código completo y optimizado que cumple con los estándares profesionales de diseño:

```python
"""
b2_7_gestor_incidentes.py
Propósito: Sistema de Gestión e Incidentes de Seguridad en Memoria (SOC CRUD).
Autor: Material formativo del docente para control presencial.
"""

# Diccionario global para simular la base de datos de incidentes en memoria
tickets_soc = {
    "INC-001": {
        "host": "192.168.10.50",
        "severidad": "Alto",
        "estado": "Abierto",
        "analista": "Marta Gómez"
    },
    "INC-002": {
        "host": "Servidor-AD-01",
        "severidad": "Medio",
        "estado": "Mitigando",
        "analista": "Carlos Ruiz"
    }
}


def registrar_incidente(id_ticket, host, severidad, analista):
    """Registra un nuevo incidente validando que el ID no esté duplicado."""
    id_ticket = id_ticket.strip().upper()
    
    # Validación preventiva para evitar sobrescrituras accidentales
    if id_ticket in tickets_soc:
        print(f"❌ Error: El ticket {id_ticket} ya está registrado en el SOC.")
        return False

    tickets_soc[id_ticket] = {
        "host": host.strip(),
        "severidad": severidad.strip().capitalize(),
        "estado": "Abierto",
        "analista": analista.strip()
    }
    print(f"✅ Incidente {id_ticket} registrado con éxito bajo estado 'Abierto'.")
    return True


def mostrar_incidentes():
    """Muestra el catálogo completo de incidentes registrados en el SOC."""
    if not tickets_soc:
        print("⚠️ No hay incidentes registrados actualmente en el SOC.")
        return

    print("
================ REPORTES SOC ACTUALES ================")
    for id_ticket, detalles in tickets_soc.items():
        print(f"ID: {id_ticket}")
        print(f"  🖥️ Host: {detalles['host']}")
        print(f"  ⚠️ Severidad: {detalles['severidad']}")
        print(f"  ⚙️ Estado: {detalles['estado']}")
        print(f"  🕵️ Analista: {detalles['analista']}")
        print("-" * 50)


def actualizar_estado(id_ticket, nuevo_estado):
    """Actualiza de forma segura el estado de un ticket existente."""
    id_ticket = id_ticket.strip().upper()
    estado_normalizado = nuevo_estado.strip().capitalize()

    # Validación del estado antes de insertar
    if estado_normalizado not in ["Abierto", "Mitigando", "Cerrado"]:
        print("❌ Error: Estado no válido. Use 'Abierto', 'Mitigando' o 'Cerrado'.")
        return False

    # Acceso controlado mediante in
    if id_ticket in tickets_soc:
        tickets_soc[id_ticket]["estado"] = estado_normalizado
        print(f"🔄 Ticket {id_ticket} actualizado a estado: {estado_normalizado}")
        return True
    
    print(f"❌ Error: No se encontró ningún ticket con el ID {id_ticket}.")
    return False


def eliminar_incidente(id_ticket):
    """Elimina un ticket de incidente utilizando extracción segura con pop."""
    id_ticket = id_ticket.strip().upper()

    # Extracción y eliminación controlada
    incidente_eliminado = tickets_soc.pop(id_ticket, None)

    if incidente_eliminado:
        print(f"🗑️ Incidente {id_ticket} eliminado de la base de datos.")
        print(f"  Detalle: Se liberó el host {incidente_eliminado['host']} asignado a {incidente_eliminado['analista']}.")
        return True
    
    print(f"❌ Error: No se encontró ningún ticket con el ID {id_ticket} para eliminar.")
    return False


def main():
    """Orquestador del menú interactivo de la aplicación."""
    while True:
        print("
=== MENÚ DE CONTROL DE INCIDENTES - SOC ===")
        print("1. Registrar nuevo incidente")
        print("2. Mostrar catálogo de incidentes")
        print("3. Modificar estado de un incidente")
        print("4. Eliminar incidente de base de datos")
        print("5. Salir del sistema")
        
        opcion = input("Seleccione una opción (1-5): ").strip()

        match opcion:
            case "1":
                id_t = input("Introduce ID del ticket (ej: INC-003): ")
                host = input("Dirección IP o Nombre del host afectado: ")
                sev = input("Nivel de severidad (Bajo/Medio/Alto): ")
                ana = input("Nombre del analista asignado: ")
                registrar_incidente(id_t, host, sev, ana)
            case "2":
                mostrar_incidentes()
            case "3":
                id_t = input("Introduce ID del ticket a modificar: ")
                nuevo_est = input("Nuevo estado (Abierto/Mitigando/Cerrado): ")
                actualizar_estado(id_t, nuevo_est)
            case "4":
                id_t = input("Introduce ID del ticket a eliminar: ")
                confirmar = input(f"¿Seguro que desea eliminar el ticket {id_t}? (s/n): ").strip().lower()
                if confirmar == 's':
                    eliminar_incidente(id_t)
                else:
                    print("Cancelando operación de borrado.")
            case "5":
                print("🔒 Cerrando consola de control SOC de forma segura. ¡Hasta pronto!")
                break
            case _:
                print("❌ Opción inválida. Seleccione un número entre 1 y 5.")


if __name__ == "__main__":
    main()
```

---

## 🗣️ 3. Banco de 10 Preguntas de Defensa Oral

Utiliza estas preguntas técnicas puntuales para formular de manera individual durante la corrección presencial. Permiten medir el grado de asimilación teórica y descartar copias ciegas:

1. **¿Qué diferencia fundamental hay entre buscar datos en un diccionario usando `tickets_soc[id_ticket]` y `tickets_soc.get(id_ticket)`?**
   * *Respuesta esperada*: El acceso directo por corchetes lanza un error de tipo `KeyError` si la clave no existe, deteniendo el programa. El método `.get()` devuelve `None` (o un valor por defecto que especifiquemos) de forma segura y sin romper la ejecución.
2. **¿Por qué las listas no pueden ser utilizadas como claves en un diccionario de Python?**
   * *Respuesta esperada*: Las claves de un diccionario deben ser objetos inmutables y "hasheables" (como strings o tuplas). Como las listas son mutables y su contenido puede variar, su valor hash no es constante, impidiendo a Python localizarlas de forma rápida y única en memoria.
3. **En la función `registrar_incidente`, ¿cómo evitamos que un usuario sobrescriba un ticket que ya estaba registrado si introduce un ID repetido?**
   * *Respuesta esperada*: Evaluamos preventivamente la existencia del ID de ticket utilizando el operador de pertenencia `in` (`id_ticket in tickets_soc`). Si el resultado es verdadero, bloqueamos el registro.
4. **Al recorrer el diccionario en `mostrar_incidentes`, ¿por qué utilizamos el método `.items()` en lugar de iterar de forma directa?**
   * *Respuesta esperada*: Iterar directamente sobre un diccionario solo devuelve sus claves. El método `.items()` extrae una vista de pares de tuplas `(clave, valor)`, permitiéndonos desempaquetar el ID del ticket y su diccionario interno de detalles simultáneamente en el bucle `for`.
5. **Si utilizáramos la instrucción `del tickets_soc[id_ticket]` y el ID ingresado no existiera en la base de datos, ¿qué ocurriría en el programa?**
   * *Respuesta esperada*: Se produciría una excepción crítica de tipo `KeyError` que abortaría la ejecución. Por eso se prefiere usar `.pop(id_ticket, None)` o validar la clave previamente.
6. **¿Qué tipo de dato devuelve el método `tickets_soc.keys()` y cómo podemos convertirlo en una lista tradicional?**
   * *Respuesta esperada*: Devuelve un objeto de tipo vista de diccionario (`dict_keys`), que es dinámico y refleja cambios en tiempo real. Se puede convertir fácilmente a lista tradicional envolviendo el método en el constructor `list()`.
7. **¿Es posible almacenar una lista de strings dentro de un diccionario como un valor? Pon un ejemplo rápido.**
   * *Respuesta esperada*: Sí, los valores de los diccionarios no tienen restricciones de mutabilidad ni tipo. Por ejemplo: `{"INC-001": ["IP_Threat_A", "IP_Threat_B"]}`.
8. **¿Qué diferencia conceptual hay entre un diccionario y un mapa asociativo de Java (`HashMap`) o C# (`Dictionary`) respecto a su sintaxis?**
   * *Respuesta esperada*: En Java o C# se requiere declarar explícitamente los tipos genéricos de clave y valor (`HashMap<String, Object>`), y se requiere importar clases externas. En Python es una estructura nativa extremadamente ligera y dinámica, que no exige declaración previa de tipos.
9. **Si quisiéramos borrar por completo y de un solo golpe todos los incidentes del SOC, ¿qué método integrado deberíamos invocar sobre `tickets_soc`?**
   * *Respuesta esperada*: El método `.clear()`.
10. **¿Cómo funciona la asignación por defecto en el método `.pop()` si se omite el segundo argumento y la clave no se encuentra?**
    * *Respuesta esperada*: Si no se especifica un valor por defecto y la clave no se encuentra, `.pop()` lanzará un `KeyError`. Al especificar un valor por defecto (como `.pop(id, None)`), se evita el error y se retorna dicho valor.

---

## 🛠️ 4. 10 Pruebas de Modificación en Vivo (Anti-IA)

Propón estos ejercicios interactivos individuales al alumno delante de ti en clase. Deberá modificar su script y ejecutarlo con éxito en menos de 2 minutos para demostrar que comprende la lógica de su código:

1. **Evitar ID de longitud incorrecta**: Haz que la función `registrar_incidente` rechace de forma inmediata cualquier ID de ticket que no tenga exactamente 7 caracteres.
   * *Solución esperada*: Añadir un condicional: `if len(id_ticket) != 7:` al inicio de la función y retornar `False`.
2. **Forzar mayúsculas en los hosts**: Modifica el registro de incidentes para que cualquier nombre de host ingresado se guarde obligatoriamente transformado por completo a mayúsculas.
   * *Solución esperada*: Modificar la asignación: `"host": host.strip().upper()`.
3. **Filtro de severidades restringido**: Cambia la función de registro para que solo acepte severidades entre tres opciones fijas: `"Bajo"`, `"Medio"` y `"Alto"`. Si el usuario ingresa otra cosa, mostrar un error y cancelar.
   * *Solución esperada*: Validar con `if severidad.strip().capitalize() not in ["Bajo", "Medio", "Alto"]:` y abortar.
4. **Auto-asignación por defecto**: Haz que si el usuario no introduce el nombre de un analista al registrar un incidente (deja el campo en blanco), se asigne de forma automática al analista `"SISTEMA_AUTO"`.
   * *Solución esperada*: En la función o en la captura de input: `analista = "SISTEMA_AUTO" if not analista.strip() else analista`.
5. **Mostrar solo tickets activos (Abiertos o Mitigando)**: Modifica `mostrar_incidentes` para que no imprima en pantalla aquellos tickets cuyo estado sea `"Cerrado"`.
   * *Solución esperada*: Añadir un filtro dentro del bucle `for`: `if detalles["estado"] == "Cerrado": continue`.
6. **Contador rápido de incidentes**: Añade al menú una opción rápida que imprima por consola el número total de tickets registrados actualmente en el SOC sin listar sus detalles.
   * *Solución esperada*: Imprimir directamente la longitud del diccionario: `print("Total de tickets:", len(tickets_soc))`.
7. **Bloqueo de seguridad (Whitelist local)**: Modifica la función de registro para que si la IP o host contiene la cadena `"localhost"` o `"127.0.0.1"`, se deniegue inmediatamente el registro por tratarse de un host local seguro.
   * *Solución esperada*: Validar con `if "localhost" in host.lower() or "127.0.0.1" in host:` y rechazar el registro.
8. **Impresión reducida de analistas**: Cambia `mostrar_incidentes` para que solo muestre el ID del ticket y el analista asignado, omitiendo la visualización del host, severidad y estado.
   * *Solución esperada*: Editar los `print()` dentro del bucle para eliminar las líneas que no correspondan a ID o Analista.
9. **Cierre masivo de incidentes**: Implementa una nueva opción de menú o función que recorra todo el diccionario y cambie el estado de **todos** los tickets registrados a `"Cerrado"`.
   * *Solución esperada*: Un bucle simple: `for t in tickets_soc.values(): t["estado"] = "Cerrado"`.
10. **Añadir campo de notas dinámicas**: Modifica la función de registro para que añada un nuevo campo llamado `"notas"` en el diccionario de detalles, inicializado como una cadena vacía `""`, de modo que esté preparado para futuras anotaciones.
    * *Solución esperada*: Agregar en la definición del diccionario interno: `"notas": ""`.
