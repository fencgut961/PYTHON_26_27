# Soporte de Evaluación - Unidad 2.11

Este recurso de evaluación está diseñado para que el docente pueda calificar de forma ágil y presencial la entrega del alumno, garantizando el control del aprendizaje autónomo y blindando el proceso de evaluación contra el uso no autorizado de Inteligencias Artificiales.

---

## 🎯 Criterios de Calificación Directa

El proceso de corrección presencial se simplifica en tres únicos estados:

*   **Apto (A):** El alumno explica con claridad su código, el programa se ejecuta sin errores y no usa bucles `for` directos en la lectura. Supera la defensa oral y realiza la modificación en vivo propuesta por el docente en menos de 5 minutos.
*   **Apto con Recuperación (AR):** El programa funciona pero tiene fallos estéticos menores (PEP 8), la documentación (Docstrings) es incompleta o el alumno duda en la defensa oral pero logra completar la modificación en vivo con ayuda puntual del docente.
*   **No Apto (NA):** El script no se ejecuta, utiliza bucles `for` directos burlando el objetivo técnico de la unidad, el alumno no comprende el código que presenta o es incapaz de realizar la modificación básica en vivo.

---

## 💻 Código Solución Oficial (b2_11_auditor_soc.py)

Este código sirve de plantilla de referencia para la evaluación del profesor. Sigue estrictamente todas las pautas estéticas de la guía PEP 8 y las buenas prácticas del lenguaje:

```python
# -*- coding: utf-8 -*-
"""
Módulo de Auditoría de Seguridad SOC para Servidores de Producción.
Este programa procesa de forma manual y secuencial la telemetría de red
utilizando iteradores lógicos para prevenir sobrecargas de memoria y
excepciones en tiempo de ejecución.
"""

# Diccionario global de servidores con sus métricas
SERVIDORES_SOC = {
    "SRV-Web": [120, 5, 0],
    "SRV-DB": [45, 0, 0],
    "SRV-Mail": [300, 15, 2],
    "SRV-Auth": [80, 55, 12],
    "SRV-Gateway": [1500, 1, 0]
}


def calcular_score_amenaza(metricas):
    """
    Calcula el Score de Amenaza de un servidor en función de su telemetría.
    
    Parámetros:
    metricas (list): Lista con [conexiones, fallos_login, alertas_puertos].
    
    Retorna:
    float: El score ponderado de riesgo.
    """
    conexiones = metricas[0]
    fallos_login = metricas[1]
    alertas_puertos = metricas[2]
    
    # Aplicación de la fórmula ponderada de ciberseguridad
    score = (conexiones * 0.05) + (fallos_login * 2.0) + (alertas_puertos * 10.0)
    return score


def clasificar_estado_seguridad(score):
    """
    Determina la categoría de riesgo en función del score numérico.
    
    Parámetros:
    score (float): El score ponderado de amenaza.
    
    Retorna:
    str: Estado de seguridad ('SEGURO', 'SOSPECHOSO' o 'ALERTA CRITICA').
    """
    if score < 10.0:
        return "SEGURO"
    elif score <= 50.0:
        return "SOSPECHOSO"
    else:
        return "ALERTA CRITICA"


def main():
    """Punto de entrada principal del script de auditoría."""
    print("🔍 INICIANDO AUDITORÍA DETALLADA DE SERVIDORES SOC...")
    print("-" * 70)
    
    # 1. Creamos el iterador manual sobre las claves del diccionario
    lector_servidores = iter(SERVIDORES_SOC)
    
    # 2. Bucle de consumo manual seguro usando next(..., None)
    while True:
        servidor = next(lector_servidores, None)
        
        # Condición de parada segura al consumir todo el iterador
        if servidor is None:
            break
            
        # Extracción y cálculo secuencial
        metricas_servidor = SERVIDORES_SOC[servidor]
        score_riesgo = calcular_score_amenaza(metricas_servidor)
        estado_salud = clasificar_estado_seguridad(score_riesgo)
        
        # Impresión profesional formateando salidas y columnas
        print(f"Servidor: {servidor:<13} | Score: {score_riesgo:>6.2f} | Estado: [{estado_salud}]")
        
    print("-" * 70)
    print("✅ Análisis finalizado con éxito. Todos los iteradores de red consumidos limpiamente.")


if __name__ == "__main__":
    main()
```

---

## 🗣️ Banco de 10 Preguntas de Defensa Oral

Utiliza estas preguntas para interrogar al alumno individualmente y evaluar su grado de comprensión real de la materia:

1.  **¿Qué es un iterable en Python y en qué se diferencia de un iterador físico?**
    *   *Respuesta esperada:* Un iterable es el objeto que contiene los datos y se puede recorrer (como una lista o un diccionario), pero no recuerda en qué posición va. Un iterador es un objeto interno de control que mantiene el estado de la lectura y se avanza explícitamente usando la función `next()`.
2.  **Si ejecutas la función `next()` sobre una lista directamente (ej: `next(SERVIDORES_SOC)`), ¿qué ocurre?**
    *   *Respuesta esperada:* Lanza un error de tipo `TypeError` porque un diccionario o una lista son iterables, pero no son iteradores directamente. Hay que pasarlos previamente por `iter()`.
3.  **¿Qué excepción lanza de forma nativa Python cuando un iterador consume todos sus elementos y se vuelve a llamar a `next()`?**
    *   *Respuesta esperada:* Lanza la excepción `StopIteration`.
4.  **En tu código de la práctica, ¿cómo evitas que se lance y rompa el programa la excepción `StopIteration`?**
    *   *Respuesta esperada:* Pasando `None` como segundo parámetro en la llamada: `next(lector_servidores, None)`. Si se agota, devuelve `None` en lugar de lanzar el error.
5.  **¿Qué ventajas de consumo de memoria tiene el uso de iteradores en sistemas informáticos reales al leer grandes volúmenes de datos?**
    *   *Respuesta esperada:* Evaluación perezosa (*lazy evaluation*). No cargan todos los datos procesados en la memoria RAM de golpe, sino que extraen y procesan la información elemento por elemento bajo demanda.
6.  **En la línea `lector_servidores = iter(SERVIDORES_SOC)`, ¿sobre qué campos del diccionario está iterando Python por defecto?**
    *   *Respuesta esperada:* Iterará de forma predeterminada sobre las claves del diccionario (los nombres de los servidores).
7.  **¿Qué ocurriría en memoria si intentas usar un bucle `while True` sin actualizar el iterador dentro?**
    *   *Respuesta esperada:* El programa entraría en un bucle infinito consumiendo recursos de procesador porque la variable de control nunca alcanzaría el estado `None`.
8.  **¿Cómo cambiarías el iterador para que devuelva los valores de las métricas directamente en lugar de los nombres de los servidores?**
    *   *Respuesta esperada:* Creando el iterador sobre la vista de valores: `iter(SERVIDORES_SOC.values())`.
9.  **¿Por qué es una mala práctica mutar o cambiar de tamaño un diccionario (añadir o quitar claves) mientras se está iterando de forma activa sobre él?**
    *   *Respuesta esperada:* Python lanzará un error en tiempo de ejecución (`RuntimeError: dictionary changed size during iteration`) porque rompe la consistencia interna de los punteros lógicos del iterador.
10. **¿Cuál es la diferencia entre los comentarios de línea `#` y los Docstrings `"""` según el estándar de Python?**
    *   *Respuesta esperada:* Los comentarios son ignorados por completo y sirven para los desarrolladores. Los Docstrings documentan formalmente módulos y funciones, y son accesibles de forma dinámica en tiempo de ejecución a través del atributo `__doc__` o la función `help()`.

---

## 🛠️ 10 Pruebas de Modificación de Código en Vivo (Anti-IA)

Propón uno de estos pequeños ejercicios interactivos para que el alumno lo resuelva en su ordenador en directo frente a ti para demostrar que ha asimilado la lógica:

1.  **Aislamiento de Alertas Críticas:** Haz que el programa detenga de inmediato el análisis (rompiendo el bucle) en el primer momento en que detecte un servidor en estado `"ALERTA CRITICA"`.
    *   *Solución:* Añadir un condicional dentro del bucle `while`: `if estado_salud == "ALERTA CRITICA": break`.
2.  **Omitir Servidores Excluidos (Lista Blanca):** Modifica el bucle de auditoría para que ignore por completo el análisis y no imprima nada en pantalla si el servidor se llama `"SRV-Web"` usando la sentencia `continue`.
    *   *Solución:* Insertar un control temprano: `if servidor == "SRV-Web": continue` justo después de validar que no sea `None`.
3.  **Contador de Servidores Vulnerables:** Declara un contador antes del bucle y muestra al final del reporte cuántos servidores en total han sido clasificados como `"ALERTA CRITICA"` o `"SOSPECHOSO"`.
    *   *Solución:* Inicializar `vulnerables = 0` y sumar `vulnerables += 1` si el estado es distinto de "SEGURO".
4.  **Inyección de Emergencia:** Añade de forma manual un nuevo servidor al diccionario al inicio del programa llamado `"SRV-Sec-Backup"` con métricas `[10, 0, 0]` y comprueba que se procese automáticamente por el iterador.
    *   *Solución:* Insertar la clave en el diccionario de inicio: `SERVIDORES_SOC["SRV-Sec-Backup"] = [10, 0, 0]`.
5.  **Auditores con Nombre Personalizado:** Permite que el operario del SOC introduzca su nombre al iniciar el programa y muéstralo en la cabecera del reporte mediante un f-string.
    *   *Solución:* Solicitar `analista = input("Nombre analista: ")` e imprimirlo con `f"Analista de guardia: {analista}"`.
6.  **Formatear Nombres en Minúsculas:** Haz que los nombres de los servidores se muestren siempre en minúsculas en el reporte final, sin importar cómo estén definidos en el diccionario.
    *   *Solución:* Usar el método de cadena `.lower()` en la impresión: `servidor.lower()`.
7.  **Reducir Peso de Conexiones:** El departamento de sistemas avisa de que el tráfico de red ha subido. Modifica la ponderación para que las conexiones activas tengan un peso de `0.01` en lugar de `0.05` y comprueba cómo cambian los scores.
    *   *Solución:* Cambiar en `calcular_score_amenaza`: `(conexiones * 0.01)`.
8.  **Reporte Resumido (Sin Score):** Modifica la cadena f-string de la impresión final para que no muestre la columna numérica de los scores, imprimiendo solo el nombre del servidor y su estado lógico entre corchetes.
    *   *Solución:* Editar el print: `print(f"Servidor: {servidor:<13} | Estado: [{estado_salud}]")`.
9.  **Bloqueo de Seguridad en Base a Conexiones:** Si un servidor supera las 1000 conexiones activas, clasifícalo directamente como `"ALERTA CRITICA"` independientemente de lo que sume su score general.
    *   *Solución:* Agregar la condición en `clasificar_estado_seguridad` o directamente en el bucle principal evaluando `metricas_servidor[0] > 1000`.
10. **Alineación Invertida de Reporte:** Cambia la alineación del reporte para que los nombres de los servidores se muestren alineados a la derecha ocupando un espacio fijo de 15 caracteres.
    *   *Solución:* Modificar el marcador de formato en el f-string por: `{servidor:>15}`.
