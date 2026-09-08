# Guía Docente de Evaluación: Unidad 2.9 (Iterar con zip())

Esta guía proporciona el soporte completo para la evaluación interactiva de la práctica de correlación de logs en el Centro de Operaciones de Seguridad (SOC). Está diseñada para un control presencial directo que mitigue el fraude de Inteligencia Artificial mediante defensa oral y pruebas lógicas aplicadas sobre el ordenador del alumno en tiempo real.

---

## 📊 1. Esquema de Calificación Simplificado

Para agilizar el proceso de corrección y centrar la atención en la adquisición real de competencias lógicas, la evaluación se clasifica en tres estados directos de aptitud:

| Calificación | Criterio de Corte | Descripción para el Alumno |
| :--- | :--- | :--- |
| **APTO** | Cumple todos los requisitos funcionales y supera con éxito la defensa presencial. | El alumno demuestra comprender el funcionamiento físico de `zip()`, justifica las estructuras condicionales aplicadas y supera las pruebas de modificación de código en directo en el aula. |
| **APTO CON RECUPERACIÓN** | El código funciona parcialmente pero muestra fallas severas en la defensa. | El código se ejecuta de forma correcta, pero el alumno es incapaz de argumentar las decisiones lógicas tomadas, de modificar una parte simple del flujo o de explicar el comportamiento de desajuste de colecciones en `zip()`. |
| **NO APTO** | El programa tiene errores de sintaxis o no supera los mínimos. | El script no se ejecuta, genera excepciones no controladas o no cumple con las funcionalidades de correlación jerárquica y desempaquetado estrella (`zip(*...)`). |

---

## 💻 2. Solución Oficial de Referencia (PEP 8)

A continuación se detalla el código completo de la solución esperada para el script `b2_9_correlacionador_soc.py`, estructurado, comentado y documentado de forma profesional:

```python
"""
Módulo de correlación cruzada de seguridad e inteligencia de amenazas para el SOC.
Este script unifica flujos independientes de telemetría de red utilizando la función
integrada zip() y aplica lógica forense para calcular severidades.
"""

# Constantes de puertos y firmas críticas
PUERTOS_CRITICOS = (22, 3389)
FIRMAS_PELIGROSAS = ("Exploit", "Brute Force")


def correlacionar_logs(tiempos, ips, puertos, firmas):
    """
    Sincroniza múltiples listas de logs mediante zip() y genera un mapa anidado.
    
    Args:
        tiempos (list): Lista con marcas de tiempo (strings).
        ips (list): Lista con IPs sospechosas (strings).
        puertos (list): Lista con números de puertos de destino (integers).
        firmas (list): Lista con descripciones de intrusión (strings).
        
    Returns:
        dict: Mapa estructurado con incidentes e índice secuencial autogenerado.
    """
    registro_incidentes = {}
    contador = 1
    
    # Recorremos todas las listas de forma paralela
    for tiempo, ip, puerto, firma in zip(tiempos, ips, puertos, firmas):
        # Generamos la clave estructurada para el incidente
        id_incidente = f"INC-{contador:03d}"
        
        # Calculamos la severidad basándonos en las reglas de negocio
        es_puerto_critico = puerto in PUERTOS_CRITICOS
        tiene_firma_peligrosa = any(p in firma for p in FIRMAS_PELIGROSAS)
        
        if es_puerto_critico and tiene_firma_peligrosa:
            severidad = "CRÍTICA"
        elif es_puerto_critico:
            severidad = "ALTA"
        else:
            severidad = "MEDIA"
            
        # Almacenamos la estructura en el diccionario anidado
        registro_incidentes[id_incidente] = {
            "Hora": tiempo,
            "IP": ip,
            "Puerto": puerto,
            "Firma": firma,
            "Severidad": severidad
        }
        contador += 1
        
    return registro_incidentes


def mostrar_reporte_soc(registro_incidentes):
    """
    Muestra en pantalla el informe forense unificado de forma tabulada y limpia.
    
    Args:
        registro_incidentes (dict): Diccionario de incidentes generados por el correlacionador.
    """
    print("\n" + "=" * 90)
    print("📢 REPORTE UNIFICADO DE INCIDENTES - SOC SEGURIDAD")
    print("=" * 90)
    print(f"{'Nº':<4} | {'ID':<8} | {'HORA':<8} | {'IP ORIGEN':<15} | {'PUERTO':<6} | {'SEVERIDAD':<9} | {'FIRMA DETECTADA'}")
    print("-" * 90)
    
    for indice, (id_inc, datos) in enumerate(registro_incidentes.items(), start=1):
        print(f"{indice:<4} | {id_inc:<8} | {datos['Hora']:<8} | {datos['IP']:<15} | {datos['Puerto']:<6} | {datos['Severidad']:<9} | {datos['Firma']}")
        
    print("=" * 90)


def aislar_ips_amenaza(registro_incidentes):
    """
    Extrae la relación de parejas IP-Puerto de los incidentes y utiliza
    el desempaquetado estrella zip(*...) para aislar de forma segura las IPs.
    
    Args:
        registro_incidentes (dict): Diccionario de incidentes.
        
    Returns:
        tuple: IPs sospechosas para su exportación al firewall.
    """
    # Creamos una lista de tuplas con parejas (IP, Puerto)
    parejas_red = []
    for datos in registro_incidentes.values():
        parejas_red.append((datos["IP"], datos["Puerto"]))
        
    if not parejas_red:
        print("\n⚠️ No se encontraron incidentes para aislar.")
        return ()
        
    # Aplicamos el desempaquetado estrella para revertir el emparejamiento
    ips_aisladas, puertos_aislados = zip(*parejas_red)
    
    print("\n🔒 DIRECCIONES IP AISLADAS PARA BLOQUEO PREVENTIVO:")
    print(f"👉 IPs de Origen a banear en Firewall: {ips_aisladas}")
    print(f"👉 Puertos de Destino a monitorizar: {puertos_aislados}")
    
    return ips_aisladas


if __name__ == "__main__":
    # Datos crudos recolectados de forma independiente por el SOC
    timestamps = ["14:22:05", "14:23:18", "14:25:40", "14:28:11", "14:30:00"]
    source_ips = ["185.220.101.5", "192.168.1.15", "45.138.99.102", "10.0.0.8"]
    target_ports = [3389, 80, 22, 443, 8080]
    payloads = ["RDP Exploit Attempt", "Normal HTTP GET", "SSH Brute Force", "HTTPS Handshake", "WAF Blocked"]
    
    # Procesamos e informamos del desajuste controlado de colecciones
    print("⚙️ Iniciando correlación cruzada de registros...")
    print(f"ℹ️ Registros crudos: Tiempos={len(timestamps)}, IPs={len(source_ips)}, Puertos={len(target_ports)}, Firmas={len(payloads)}")
    
    incidentes = correlacionar_logs(timestamps, source_ips, target_ports, payloads)
    
    # Generamos los reportes visuales y de aislamiento
    mostrar_reporte_soc(incidentes)
    aislar_ips_amenaza(incidentes)
```

---

## 🗣️ 3. Banco de 10 Preguntas de Defensa Oral

Utiliza estas preguntas breves e individuales para interrogar a tus alumnos en directo sobre su propio código:

1.  **¿Por qué en tu reporte final solo aparecen 4 incidentes si las listas originales de marcas de tiempo y firmas tenían 5 elementos?**
    *   *Respuesta esperada:* Porque la lista de IPs sospechosas (`source_ips`) solo tenía 4 elementos. La función `zip()` se detiene automáticamente en la colección más corta, descartando los elementos sobrantes para garantizar que no se emparejen datos incompletos.
2.  **¿Qué tipo de dato devuelve la invocación directa de `zip(timestamps, source_ips)` en Python? ¿Es una lista?**
    *   *Respuesta esperada:* No es una lista, es un objeto de tipo iterador (un generador de tuplas). Para transformarlo en una estructura legible en bloque debemos convertirlo explícitamente a través de `list()` o `tuple()`, o recorrerlo con un bucle `for`.
3.  **¿Qué estructura sintáctica se utiliza para desempaquetar simultáneamente los elementos de un zip dentro del bucle?**
    *   *Respuesta esperada:* Se utiliza la asignación múltiple de variables separadas por comas en la cabecera del bucle `for tiempo, ip, puerto, firma in zip(...)`.
4.  **En la función `aislar_ips_amenaza`, ¿qué papel físico cumple el operador asterisco (`*`) junto a `zip(*parejas_red)`?**
    *   *Respuesta esperada:* Actúa como operador de desempaquetado de argumentos. Pasa cada una de las tuplas internas de la lista de forma independiente a la función `zip()`, la cual reagrupa de nuevo los primeros elementos por un lado (IPs) y los segundos por otro (puertos).
5.  **¿Qué ocurriría en memoria si en lugar de usar `zip(*...)` intentas hacer un bucle manual para rellenar las tuplas de IPs?**
    *   *Respuesta esperada:* Funcionaría igual lógicamente, pero requeriría inicializar listas vacías y realizar adiciones repetitivas, haciendo el código más largo, menos legible y menos eficiente energéticamente en sistemas de Big Data.
6.  **Si una de las listas pasadas a `zip()` contiene un elemento nulo (`None`), ¿se detiene la iteración en ese punto?**
    *   *Respuesta esperada:* No, la iteración continúa. `None` es un valor de tipo objeto válido en Python. `zip()` mide la longitud de la colección (cantidad de elementos), no la validez semántica de los mismos.
7.  **¿Por qué las colecciones resultantes de deshacer el zip mediante `zip(*...)` son tuplas y no listas?**
    *   *Respuesta esperada:* Porque la función `zip()` genera tuplas nativas de forma interna por eficiencia y consistencia de datos, garantizando que el emparejamiento de salida sea inmutable.
8.  **¿Se pueden emparejar colecciones heterogéneas, por ejemplo, una lista de strings con una tupla de enteros y un conjunto (set)?**
    *   *Respuesta esperada:* Sí. `zip()` admite cualquier objeto de tipo iterable, independientemente de su estructura física, clase interna o mutabilidad.
9.  **En la línea `for indice, (id_inc, datos) in enumerate(registro_incidentes.items(), start=1):` de tu reporte, ¿por qué hay paréntesis alrededor de `id_inc, datos`?**
    *   *Respuesta esperada:* Porque el método `.items()` del diccionario devuelve una tupla `(clave, valor)` en cada vuelta. La estructura anidada permite que `enumerate` nos dé el índice secuencial por un lado y desempaquete la tupla del ítem en las variables internas de forma limpia.
10. **¿Cómo se comporta el operador de pertenencia `in` al evaluar si una firma está en el conjunto de alertas críticas?**
    *   *Respuesta esperada:* Evalúa de forma booleana la subcadena de texto. Si el texto `"Exploit"` o `"Brute Force"` está embebido o contenido en la cadena descriptiva de la firma, devuelve `True`.

---

## 🛠️ 4. 10 Pruebas de Modificación de Código en Vivo (Anti-IA)

Plantea estas pruebas rápidas de edición en vivo al alumno frente a su pantalla para validar que ha desarrollado el script de forma autónoma:

1.  **Baneo de IPs locales:** Modifica el correlacionador para que si la IP detectada pertenece a la red de bucle local (`"127.0.0.1"`) o a una red de pruebas (`"10.0.0.8"`), se omita de forma automática y no se registre ningún incidente.
    *   *Solución:* Añadir un control dentro del bucle `for` de la función: `if ip in ("127.0.0.1", "10.0.0.8"): continue`.
2.  **Límite estricto de seguridad:** Haz que el script aborte la correlación de forma abrupta si el puerto de destino del ataque es el puerto `8080`, mostrando una alerta preventiva de "Ataque a puerto proxy web".
    *   *Solución:* Dentro del bucle, añadir: `if puerto == 8080: print("Alerta..."); break`.
3.  **Contador de criticidad:** Haz que la función `mostrar_reporte_soc` calcule y muestre al final del informe el número total de incidentes con severidad `"CRÍTICA"` que se han procesado.
    *   *Solución:* Inicializar un contador en la función, incrementarlo si `datos["Severidad"] == "CRÍTICA"` e imprimirlo al terminar.
4.  **Filtro estricto de exportación:** Modifica la función `aislar_ips_amenaza` para que solo se incluyan en la tupla de bloqueo aquellas IPs de origen que correspondan a incidentes de severidad `"CRÍTICA"` o `"ALTA"`.
    *   *Solución:* En el bucle de extracción, condicionar la adición: `if datos["Severidad"] in ("CRÍTICA", "ALTA"): parejas_red.append((datos["IP"], datos["Puerto"]))`.
5.  **Detección de incidentes por encima del promedio:** Añade una línea al final del programa principal para comprobar de forma interactiva si el número de puertos únicos atacados es superior a 3 utilizando colecciones nativas.
    *   *Solución:* Extraer y comprobar el tamaño del conjunto: `puertos_unicos = len(set(puertos_aislados))` y validar si `puertos_unicos > 3`.
6.  **Sustitución de IPs anónimas:** Si por algún motivo la IP unificada es nula o vacía, la clave del diccionario de incidentes debe asociarse con el valor `"DESCONOCIDO"` en lugar de la IP vacía.
    *   *Solución:* `ip_segura = ip if ip else "DESCONOCIDO"`.
7.  **Formateo dinámico de severidades:** Modifica el script para que los incidentes con severidad `"CRÍTICA"` se impriman de forma visual en el reporte SOC con tres asteriscos antes y después de su nombre (ej: `***CRÍTICA***`).
    *   *Solución:* `sev = f"***{datos['Severidad']}***" if datos['Severidad'] == "CRÍTICA" else datos['Severidad']` en el f-string de impresión.
8.  **Reajuste del orden del zip:** Modifica la firma de la llamada a `zip()` dentro del correlacionador para invertir el orden de desempaquetado de las variables, asegurando que el código siga funcionando de forma idéntica adaptando la cabecera.
    *   *Solución:* Cambiar `zip(tiempos, ips, puertos, firmas)` por `zip(ips, tiempos, firmas, puertos)` y recolocar las variables receptoras: `for ip, tiempo, firma, puerto in zip(...)`.
9.  **Aislamiento de marcas de tiempo críticas:** Crea una nueva función llamada `aislar_horas_criticas` que devuelva una tupla exclusiva con las horas en las que se registraron los incidentes calificados como `"CRÍTICA"`.
    *   *Solución:* `return tuple(datos["Hora"] for datos in registro_incidentes.values() if datos["Severidad"] == "CRÍTICA")`.
10. **Alineación forzada de red:** Si un analista de red inyecta una nueva IP sospechosa `"1.1.1.1"` al final de la lista de IPs sospechosas, modifica el programa para comprobar de forma dinámica si ahora el número de incidentes correlacionados sube automáticamente a 5.
    *   *Solución:* Añadir `"1.1.1.1"` a `source_ips` en el bloque principal y verificar que el reporte renderice dinámicamente un quinto registro `INC-005` (ya que ahora el tamaño mínimo de las colecciones se ha incrementado).
