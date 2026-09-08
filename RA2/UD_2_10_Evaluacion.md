# Guía de Evaluación Docente: Network Traceroute Threat Auditor

Esta guía proporciona el soporte lógico y metodológico para evaluar la práctica de la Unidad 2.10 de forma presencial, ágil y con un control estricto frente al uso de herramientas de inteligencia artificial.

---

## 1. Esquema de Calificación Simplificado

El alumno será evaluado directamente bajo uno de estos tres estados:

*   **APTO (A):** El alumno implementa la función de enrutamiento utilizando correctamente `enumerate(zip(...), start=1)`, realiza las comparaciones de seguridad y latencia sin errores de tipos de datos, documenta la función con Docstrings bajo el estándar PEP 257 y responde de manera fluida y solvente a la defensa oral y la prueba de modificación de código en vivo.
*   **APTO CON RECUPERACIÓN (AR):** El script es ejecutable y cumple la mayoría de los objetivos lógicos, pero presenta pequeños fallos estéticos, usa contadores manuales en lugar de `enumerate()` o el alumno titubea y requiere asistencia guiada durante la modificación en vivo o la defensa oral.
*   **NO APTO (NA):** El código presenta errores de sintaxis que impiden su ejecución, no modulariza usando funciones, altera el comportamiento lícito de la inmutabilidad de constantes o es incapaz de explicar la lógica interna de su propio programa durante la defensa oral (indicio directo de plagio o IA sin asimilación).

---

## 2. Solución Oficial (PEP 8 + Documentada)

A continuación, se detalla el código de referencia que el docente puede utilizar para corregir de forma instantánea el desempeño de los alumnos:

```python
"""
Módulo de Auditoría de Redes para el SOC.
Este script audita las trazas de ruta (traceroute) de paquetes IP buscando
saltos comprometidos o problemas de latencia física.
"""

# CONSTANTES DE SEGURIDAD (PEP 8)
IPS_SOSPECHOSAS = ("185.220.101.5", "45.142.120.9", "109.201.154.3")
LIMITE_LATENCIA = 120  # Milisegundos


def auditar_traceroute(nodos_ruta, latencias_ms):
    """
    Audita una traza de red identificando saltos maliciosos y retardos.

    Args:
        nodos_ruta (list): Lista de strings con direcciones IP de los saltos.
        latencias_ms (list): Lista de enteros con las latencias de cada nodo.

    Returns:
        tuple: (ruta_segura (bool), primer_salto_comprometido (int/None), saltos_con_lag (list))
    """
    ruta_segura = True
    primer_salto_comprometido = None
    saltos_con_lag = []

    print("\n🔍 INICIANDO AUDITORÍA FORENSE DE LA TRAZA...")
    print("-" * 65)

    # Combinamos usando zip y enumeramos empezando desde el salto #1
    for salto, (ip, latencia) in enumerate(zip(nodos_ruta, latencias_ms), start=1):
        estado_seguridad = "LÍCITO"
        estado_latencia = "NORMAL"

        # Verificación de Seguridad
        if ip in IPS_SOSPECHOSAS:
            estado_seguridad = "🔥 COMPROMETIDO"
            ruta_segura = False
            if primer_salto_comprometido is None:
                primer_salto_comprometido = salto

        # Verificación de Latencia
        if latencia > LIMITE_LATENCIA:
            estado_latencia = "⚠️ LAG ANÓMALO"
            saltos_con_lag.append((salto, ip))

        print(f"Salto {salto:02d} | IP: {ip:<15} | Latencia: {latencia:>3}ms | [{estado_seguridad}] | [{estado_latencia}]")

    print("-" * 65)
    return ruta_segura, primer_salto_comprometido, saltos_con_lag


# PROBADOR DEL SISTEMA (MAIN ENTRY POINT)
if __name__ == "__main__":
    # Caso 1: Ruta segura
    nodos_ok = ["192.168.1.1", "10.0.0.1", "172.16.0.5", "8.8.8.8"]
    latencias_ok = [5, 12, 45, 95]
    
    seguro_1, salto_1, lag_1 = auditar_traceroute(nodos_ok, latencias_ok)
    print(f"RESULTADO: Seguro={seguro_1} | Salto Alerta={salto_1} | Nodos con Lag={lag_1}")

    # Caso 2: Ruta bajo sospecha
    nodos_ataque = ["192.168.0.1", "10.100.1.1", "45.142.120.9", "109.201.154.3", "195.12.5.40"]
    latencias_ataque = [15, 30, 240, 110, 310]
    
    seguro_2, salto_2, lag_2 = auditar_traceroute(nodos_ataque, latencias_ataque)
    print(f"RESULTADO: Seguro={seguro_2} | Salto Alerta={salto_2} | Nodos con Lag={lag_2}")
```

---

## 3. Banco de 10 Preguntas de Defensa Oral

Formula una o dos de estas preguntas en clase al alumno mientras revisas su entrega para comprobar que comprende los conceptos fundamentales de la unidad:

1.  **¿Cuál es la ventaja técnica de usar `enumerate()` en lugar de llevar un contador manual con `cont += 1`?**
    *   *Respuesta esperada:* Evita la necesidad de declarar, inicializar e incrementar una variable temporal extra, reduciendo el riesgo de errores lógicos como bucles infinitos o desajustes por no actualizar el contador.
2.  **Si hacemos `for a, b in enumerate(mi_lista)`, ¿qué representan exactamente las variables `a` y `b`?**
    *   *Respuesta esperada:* `a` representa el índice físico secuencial del elemento (empezando en 0 por defecto), y `b` representa el valor real del elemento almacenado en esa posición de la lista.
3.  **¿Qué tipo de dato devuelve la función `enumerate()` de forma nativa en Python y cómo lo convertimos a una lista de tuplas persistente?**
    *   *Respuesta esperada:* Devuelve un iterador (objeto iterable de clase `enumerate`). Lo convertimos de forma explícita pasándolo como argumento al constructor `list()`, obteniendo una lista de tuplas.
4.  **¿Cómo forzamos a que el índice de `enumerate()` empiece a contar desde un número distinto de cero, como por ejemplo el 10?**
    *   *Respuesta esperada:* Pasando el parámetro opcional `start=10` como segundo argumento de la función: `enumerate(mi_lista, start=10)`.
5.  **En la firma del bucle `for salto, (ip, latencia) in enumerate(zip(...), start=1)`, ¿por qué se colocan paréntesis alrededor de `ip, latencia`?**
    *   *Respuesta esperada:* Porque `zip()` genera tuplas de dos elementos. Los paréntesis son obligatorios para desempaquetar la tupla de datos interna mientras `enumerate()` desempaqueta simultáneamente el índice en la variable `salto`.
6.  **¿Qué ocurre si el argumento pasado a `enumerate()` está vacío (como una lista vacía `[]`)?**
    *   *Respuesta esperada:* El bucle no realiza ninguna iteración y termina de inmediato sin lanzar ningún error ni excepción, ya que el iterador está vacío.
7.  **¿Es posible usar `enumerate()` para recorrer los caracteres de una cadena de texto (string) en un análisis criptográfico?**
    *   *Respuesta esperada:* Sí. Al ser los strings iterables, `enumerate()` asignará un índice entero progresivo a cada carácter individual de la cadena empezando por el cero.
8.  **En términos de rendimiento y consumo de memoria, ¿por qué es eficiente la función `enumerate()` con grandes flujos de datos?**
    *   *Respuesta esperada:* Porque trabaja bajo demanda (*lazy evaluation*). Genera y entrega cada índice y elemento de uno en uno en tiempo de ejecución, en lugar de duplicar o precalcular la lista entera con índices en la memoria RAM.
9.  **¿Por qué utilizamos tuplas para guardar los saltos con problemas de latencia `(salto_id, ip_nodo)` en la lista de retorno en lugar de listas?**
    *   *Respuesta esperada:* Porque cada par de datos de alerta es una entidad fija e inmutable. Utilizar tuplas previene que otros módulos del programa alteren accidentalmente la relación entre el número de salto y la IP afectada.
10. **¿Cómo se comporta `enumerate()` si intentamos aplicarlo directamente sobre un diccionario de configuración de red?**
    *   *Respuesta esperada:* Por defecto, numerará únicamente las claves del diccionario. Si queremos numerar pares completos de datos, debemos invocarlo sobre `.items()` del diccionario.

---

## 4. 10 Pruebas de Modificación de Código en Vivo (Anti-IA)

Propón al alumno editar su código en su ordenador en tiempo real frente a ti para verificar que realmente domina la lógica de su programa:

1.  **Filtro por ISP segura:** Modifica la función para que, si un salto de red pertenece a una IP de red local (que empiece por `"192.168."`), se ignore por completo de la auditoría y no se evalúe su latencia ni se imprima en el reporte.
    *   *Solución:* Añadir `if ip.startswith("192.168."): continue` al inicio del bucle.
2.  **Registro de latencia acumulada:** Añade una nueva variable local en la función para acumular y sumar la latencia total de toda la traza. Al finalizar el bucle, imprime la latencia total media dividiéndola entre el número total de saltos reales analizados.
    *   *Solución:* Declarar `total_ms = 0`, añadir `total_ms += latencia` y al final imprimir `total_ms / len(nodos_ruta)`.
3.  **Límite de incidentes crítico:** Haz que el bucle de auditoría se detenga inmediatamente (`break`) si se detectan más de 2 saltos con problemas de lag o retraso anómalo.
    *   *Solución:* Verificar si `len(saltos_con_lag) > 2` y aplicar un `break`.
4.  **Marcaje de saltos pares:** Modifica el programa para que solo se evalúe la latencia y seguridad en los saltos de red que ocupen una posición par (salto 2, salto 4, etc.) utilizando la variable del índice generado por `enumerate()`.
    *   *Solución:* Agregar una condición `if salto % 2 != 0: continue` tras el inicio del bucle.
5.  **Ocultación de IP sospechosa (Forense):** Modifica el reporte impreso para que, si una IP es detectada como sospechosa, se oculte en la consola imprimiendo únicamente sus primeros caracteres seguidos de asteriscos (ej: `45.142.***.***`).
    *   *Solución:* Truncar con rebanado o reemplazo de cadena: `ip_oculta = ip.split(".")[0] + "." + ip.split(".")[1] + ".***.***"` si está en la lista de sospechosas.
6.  **Inyección de Host de Confianza (Whitelist):** Crea una tupla constante de confianza (`WHITELIST`). Si la IP del salto se encuentra en esa lista de confianza, no se marcará como comprometida aunque coincida con las IPs de la lista sospechosa (regla de excepción).
    *   *Solución:* Crear `WHITELIST = ("...",)` y validar `if ip in IPS_SOSPECHOSAS and ip not in WHITELIST:`.
7.  **Indexación invertida (Orden de llegada):** Cambia el comportamiento del reporte para que los números de salto se asignen en orden decreciente, es decir, que el primer nodo de la lista tenga asignado el índice máximo y vaya bajando hasta el final.
    *   *Solución:* Calcular el índice de forma matemática en el f-string usando la longitud de la lista: `salto_desc = len(nodos_ruta) - salto + 1`.
8.  **Alertas de ráfagas de paquetes:** Añade una alerta especial que imprima `"🚨 RÁFAGA DE ALTA LATENCIA"` si se detectan dos saltos consecutivos con latencias que superen el `LIMITE_LATENCIA`.
    *   *Solución:* Mantener un indicador booleano `ultimo_con_lag` que se actualice al final de cada iteración, y comprobar si el actual y el anterior tienen lag.
9.  **Formatos dinámicos por criticidad:** Haz que, si una IP comprometida se detecta en un número de salto inferior al 3 (un salto muy cercano a nuestra red local), el reporte de seguridad se imprima con el texto `"🔥 CRÍTICO INTERNO"`, y si es mayor o igual a 3 `"⚠️ AMENAZA EXTERNA"`.
    *   *Solución:* Validar el valor de `salto`: `if salto < 3: estado = "CRÍTICO INTERNO" else: ...`.
10. **Exportación selectiva:** Modifica el valor de retorno del método para que la lista de tuplas de saltos lentos solo incluya aquellos saltos lentos que **no** sean sospechosos.
    *   *Solución:* En el bloque de verificación de lag, validar `if latencia > LIMITE_LATENCIA and ip not in IPS_SOSPECHOSAS:`.
