# Unidad 2.8 - Evaluación: SIEM Security Log Parser & Threat Analytics

Este documento es una herramienta exclusiva para el profesorado para la corrección presencial, la realización de defensas orales y la aplicación de pruebas lógicas individuales (Anti-IA) en el aula.

---

## ⚖️ 1. Esquema de Calificación Simplificado

*   **APTO (A):**
    *   Implementa correctamente las comprensiones de listas solicitadas en las Fases 1, 2 y 3.
    *   Extrae y analiza de forma segura los puertos atacados utilizando métodos integrados de listas (`min()`, `max()`, `sorted()`).
    *   Utiliza métodos avanzados de diccionarios (`.get()` o `.setdefault()`) para el conteo seguro de incidentes de red sin excepciones lógicas en tiempo de ejecución.
    *   El código corre de principio a fin sin fallos de sintaxis, respeta las convenciones estéticas PEP 8 y define Docstrings detallados.
    *   Supera con fluidez la defensa oral y la modificación de código en directo.
*   **APTO CON RECUPERACIÓN (AR):**
    *   El programa funciona, pero presenta duplicidades de código o no utiliza comprensiones de listas, resolviendo los problemas mediante bucles tradicionales `for` e `if` convencionales.
    *   No controla de forma nativa los errores lógicos (por ejemplo, el programa lanza `KeyError` al registrar nuevas IPs si no se usan los métodos avanzados de diccionarios).
    *   Presenta carencias de estilo (PEP 8) o no ha documentado las funciones con Docstrings.
    *   Muestra dudas severas durante la defensa oral o requiere ayuda guiada constante para completar la modificación del código en vivo.
*   **NO APTO (NA):**
    *   El código presenta errores de sintaxis que impiden su ejecución.
    *   Faltan componentes esenciales de las fases funcionales solicitadas.
    *   No comprende el flujo del programa en memoria durante la defensa oral.
    *   Es incapaz de realizar modificaciones lógicas básicas en vivo delante del profesor, evidenciando una autoría no autónoma (copia directa de una IA generativa).

---

## 💻 2. Código Solución Oficial (PEP 8 + Comentarios)

```python
"""
Módulo de Procesamiento Forense de Logs de Seguridad (SIEM).
Este script analiza tramas de red para aislar incidentes graves de seguridad,
calcular estadísticas de puertos vulnerados y mapear IPs hostiles.
"""

# Configuración Global del Firewall y Auditoría (PEP 8)
GRAVEDADES_CRITICAS = ("CRITICO", "ALTO")
PUERTOS_SENSIBLES = (22, 23, 3389, 8080)


def filtrar_incidentes_graves(logs: list) -> list:
    """
    Filtra los registros de logs originales y devuelve solo los de gravedad alta o crítica.
    
    Usa comprensión de listas para analizar las líneas divididas por el carácter pipe (|).
    Sanea de forma preventiva los espacios de cada bloque con .strip().
    """
    return [
        log for log in logs
        if log.split("|")[1].strip() in GRAVEDADES_CRITICAS
    ]


def mapear_puertos_atacados(logs_graves: list) -> list:
    """
    Extrae los puertos vulnerados de una lista de registros de incidentes graves.
    
    Usa comprensión de listas convirtiendo el puerto extraído en tipo int.
    """
    return [
        int(log.split("|")[3].strip())
        for log in logs_graves
    ]


def resumir_severidad(logs: list) -> list:
    """
    Genera un listado de acciones automáticas basadas en la severidad de cada registro.
    
    Implementa una comprensión de listas con condicional ternario (if-else).
    """
    return [
        f"{log.split('|')[2].strip()} -> BLOQUEO_INMEDIATO"
        if log.split("|")[1].strip() in GRAVEDADES_CRITICAS
        else f"{log.split('|')[2].strip()} -> MONITORIZACION_ESTANDAR"
        for log in logs
    ]


def registrar_actividad_sospechosa(logs: list) -> dict:
    """
    Registra en un diccionario la cantidad de ataques graves asociados a cada IP.
    
    Utiliza el método .get() para inicializar contadores sin KeyError.
    """
    registro_hostil = {}
    
    for log in logs:
        partes = log.split("|")
        severidad = partes[1].strip()
        ip_origen = partes[2].strip()
        
        # Filtramos solo amenazas relevantes
        if severidad in GRAVEDADES_CRITICAS:
            # Inicializamos y acumulamos de forma segura
            registro_hostil[ip_origen] = registro_hostil.get(ip_origen, 0) + 1
            
    return registro_hostil


# =====================================================================
# Orquestación del Programa Principal (SOC Dashboard)
# =====================================================================
if __name__ == "__main__":
    # Base de datos de logs crudos del SIEM
    logs_servidor = [
        "2026-09-05 10:00:15 | INFO | 192.168.1.50 | 80 | Acceso web exitoso",
        "2026-09-05 10:01:22 | CRITICO | 185.220.101.5 | 22 | Intento de login SSH fallido",
        "2026-09-05 10:02:10 | ALTO | 195.235.12.3 | 3389 | Intento RDP fuerza bruta",
        "2026-09-05 10:03:05 | INFO | 192.168.1.102 | 443 | Conexión SSL establecida",
        "2026-09-05 10:04:12 | CRITICO | 185.220.101.5 | 22 | Intento de login SSH fallido",
        "2026-09-05 10:05:40 | MEDIO | 198.51.100.12 | 8080 | Petición HTTP sospechosa",
        "2026-09-05 10:06:18 | BAJO | 192.168.1.1 | 53 | Consulta DNS normal",
        "2026-09-05 10:07:50 | CRITICO | 203.0.113.50 | 23 | Intento Telnet no autorizado"
    ]

    print("🛡️ SOC SIEM THREAT ANALYTICS CORESYSTEM")
    print("=" * 60)

    # 1. Ejecución del Filtrado Forense
    incidentes_graves = filtrar_incidentes_graves(logs_servidor)
    print("\n[ALERT] Incidentes de gravedad ALTA o CRÍTICA detectados:")
    for log in incidentes_graves:
        partes = log.split("|")
        print(f"  * {partes[2].strip()} en puerto {partes[3].strip()} -> {partes[4].strip()}")

    # 2. Análisis Estadístico de Puertos
    puertos = mapear_puertos_atacados(incidentes_graves)
    if puertos:
        print("\n📊 Estadísticas Forenses de Puertos Vulnerados:")
        print(f"  * Puerto mínimo auditado: {min(puertos)}")
        print(f"  * Puerto máximo auditado: {max(puertos)}")
        # Eliminamos duplicados y ordenamos para el reporte limpio
        print(f"  * Lista ordenada de puertos atacados: {sorted(list(set(puertos)))}")

    # 3. Acciones de Bloqueo Automatizadas
    politicas_bloqueo = resumir_severidad(logs_servidor)
    print("\n🚨 Reporte Automatizado de Acciones de Firewall:")
    for accion in politicas_bloqueo:
        print(f"  * {accion}")

    # 4. Inventario de Servidores Hostiles
    ips_hostiles = registrar_actividad_sospechosa(logs_servidor)
    print("\n🕵️ Base de Datos de IPs Hostiles Detectadas (Ataques Graves):")
    for ip, conteo in ips_hostiles.items():
        print(f"  * IP: {ip} -> {conteo} intentos críticos registrados.")
```

---

## 🗣️ 3. Banco de 10 Preguntas de Defensa Oral

1.  **En la función `filtrar_incidentes_graves`, ¿cómo procesa la comprensión de listas la estructura de los logs? Explicar la línea clave.**
    *   *Respuesta esperada:* Recorre secuencialmente cada string de la lista. En cada iteración, divide la cadena por el separador `"|"` mediante `.split("|")`, aislando el segundo elemento (índice `1`), que contiene la gravedad. Sanea los espacios con `.strip()` y valida si el valor coincide con los elementos de la tupla de gravedades críticas.
2.  **¿Cuál es la diferencia de rendimiento físico y sintaxis entre generar `puertos_ordenados = sorted(puertos)` y ejecutar `puertos.sort()`?**
    *   *Respuesta esperada:* `sorted()` es una función nativa que genera un nuevo objeto de tipo lista ordenado en memoria, manteniendo la lista original intacta. `.sort()` es un método de la clase `list` que ordena la lista *in situ*, modificando los punteros del objeto original de forma permanente y devolviendo `None`.
3.  **¿Qué peligro tiene omitir la función `int()` al extraer los puertos en `mapear_puertos_atacados` si luego usamos `min()` y `max()`?**
    *   *Respuesta esperada:* Si los puertos se quedan como cadenas de texto (`str`), las funciones `min()` y `max()` harán una comparación de orden lexicográfico (alfabético) en lugar de numérico. Por ejemplo, la cadena `"23"` se consideraría mayor que la cadena `"1024"` porque `"2"` es mayor que `"1"`.
4.  **En la función `registrar_actividad_sospechosa`, ¿por qué se utiliza el método `registro_hostil.get(ip_origen, 0)`? ¿Qué ocurriría si utilizáramos `registro_hostil[ip_origen] += 1` directamente?**
    *   *Respuesta esperada:* El método `.get(clave, defecto)` devuelve `0` si la IP no está registrada en el diccionario de sospechosos, permitiendo sumarle `1` sin problemas. Si usáramos la sintaxis de indexación directa `registro_hostil[ip_origen] += 1` con una IP nueva, el programa lanzaría inmediatamente un error de tipo `KeyError` y detendría su ejecución, ya que no se puede acumular sobre un elemento inexistente.
5.  **¿Qué es y cómo funciona el "operador ternario" implementado dentro de la comprensión de listas de `resumir_severidad`?**
    *   *Respuesta esperada:* Es una expresión condicional inline de estructura `[valor_si_verdadero if condicion else valor_si_falso]`. A diferencia del filtrado común, aquí no se descartan elementos, sino que se procesan todos los registros evaluando la condición para decidir qué cadena de texto generar en cada posición de la lista resultante.
6.  **En Python, ¿por qué no es recomendable utilizar listas o diccionarios mutables como valores por defecto en métodos como `.get()` o en parámetros de funciones?**
    *   *Respuesta esperada:* Las estructuras mutables se crean una sola vez en memoria en tiempo de definición. Si modificamos ese valor por defecto, los cambios se acumularán para todas las llamadas posteriores, provocando fugas de datos y comportamientos imprevistos.
7.  **Si una línea de los logs de la base de datos estuviera mal estructurada (por ejemplo: careciendo de caracteres `|`), ¿cómo reaccionaría el método `.split("|")` en tu script?**
    *   *Respuesta esperada:* El método `.split("|")` no daría error; simplemente devolvería una lista con un único elemento (el string completo). Sin embargo, al intentar acceder a los índices posteriores (`[1]`, `[2]`, `[3]`), el intérprete lanzaría un error de tipo `IndexError: list index out of range`.
8.  **¿Las comprensiones de listas crean copias independientes o modifican las colecciones originales?**
    *   *Respuesta esperada:* Crean un nuevo objeto de tipo lista totalmente independiente en el montón (*heap*) de memoria, dejando las colecciones e iterables originales completamente intactos.
9.  **¿Por qué es indispensable aplicar el método `.strip()` a las subcadenas generadas por `.split("|")` en este script?**
    *   *Respuesta esperada:* Porque los logs originales contienen espacios alrededor de las barras verticales (ej. `" INFO "`). Al dividir la cadena, las subcadenas resultantes conservan esos espacios. Comparar `"INFO"` con `" INFO "` resultaría en `False`, impidiendo que las reglas condicionales identifiquen la severidad o los puertos.
10. **¿Es posible aplicar comprensiones de listas sobre iterables que no sean listas (por ejemplo, tuplas o diccionarios)?**
    *   *Respuesta esperada:* Sí, las comprensiones de listas pueden recorrer cualquier objeto iterable en Python (tuplas, strings, conjuntos, rangos o diccionarios a través de sus claves o métodos como `.values()` e `.items()`), generando siempre una nueva lista como resultado de la expresión.

---

## 🛠️ 4. Las 10 Pruebas de Modificación en Vivo (Anti-IA)

El profesor elegirá una o más opciones para que el alumno edite su código frente a él en clase y verifique su capacidad de modificación lógica autónoma:

1.  **Filtro de exclusión de red local (Whitelisting):** Modifica la comprensión de listas de `filtrar_incidentes_graves` para que descarte automáticamente cualquier ataque si la IP de origen empieza por `"192.168.1."`, simulando que el tráfico interno de confianza está exento de bloqueos del firewall.
    *   *Solución esperada:* Añadir la condición `and not log.split("|")[2].strip().startswith("192.168.1.")` al final del list comprehension.
2.  **Reporte de longitud de detalles de logs (Transformación):** Añade una nueva comprensión de listas en el programa principal que genere una lista con el número de caracteres (longitud) de la columna de descripción (columna de índice `4`) de cada uno de los registros graves.
    *   *Solución esperada:* `longitudes_detalles = [len(log.split("|")[4].strip()) for log in incidentes_graves]`.
3.  **Contador sumatorio de puertos vulnerados (Agregación matemática):** Usando funciones integradas de Python, calcula y muestra la suma total de los valores de puerto de todos los incidentes graves registrados en la Fase 2.
    *   *Solución esperada:* `suma_puertos = sum(puertos)`.
4.  **Añadir un umbral numérico de puerto para bloqueo (Filtro numérico):** Modifica la función `filtrar_incidentes_graves` para que, además de la severidad crítica, solo filtre el incidente si el puerto atacado es inferior a `1024` (puertos bien conocidos y sensibles).
    *   *Solución esperada:* Convertir el puerto en la condición: `and int(log.split("|")[3].strip()) < 1024`.
5.  **Detección de IPs sospechosas bajo alias:** Modifica la comprensión de diccionarios de la teoría para crear un mapa rápido que asocie las IPs únicas hostiles encontradas en `ips_hostiles` con la cadena `"IP_BLOQUEADA"`.
    *   *Solución esperada:* `registro_alias = {ip: "IP_BLOQUEADA" for ip in ips_hostiles.keys()}`.
6.  **Saneamiento de protocolo unificado en mayúsculas:** Supongamos que la descripción de las alertas está en minúsculas. Diseña una comprensión de listas que tome la descripción de los incidentes graves y los devuelva en mayúsculas con el método `.upper()`.
    *   *Solución esperada:* `descripciones_mayus = [log.split("|")[4].strip().upper() for log in incidentes_graves]`.
7.  **Inversión estricta de orden en reporte:** Cambia el orden de la visualización de los puertos atacados mapeados en la Fase 2 de modo que se muestre una lista ordenada de forma descendente (de mayor a menor).
    *   *Solución esperada:* Usar `sorted(list(set(puertos)), reverse=True)` o ejecutar `.sort(reverse=True)`.
8.  **Control estricto de nulos en IPs:** Modifica la función `registrar_actividad_sospechosa` de tal manera que si la IP de origen es una cadena vacía o `"0.0.0.0"`, ignore el registro y continúe con el bucle.
    *   *Solución esperada:* Añadir un control condicional: `if ip_origen in ("", "0.0.0.0"): continue` dentro del bucle.
9.  **Filtro avanzado por comodines de ataque:** Crea una comprensión de listas que filtre y devuelva los logs del SIEM que contengan explícitamente la palabra `"SSH"` en su descripción (columna de índice 4), independientemente de la severidad.
    *   *Solución esperada:* `logs_ssh = [log for log in logs_servidor if "SSH" in log.split("|")[4]]`.
10. **Aislamiento de marcas de tiempo (Timestamps):** Crea un list comprehension que asocie a cada registro del SIEM únicamente su marca de tiempo (los primeros 19 caracteres del log, la fecha y la hora), eliminando todo el resto de la cadena.
    *   *Solución esperada:* `timestamps = [log.split("|")[0].strip() for log in logs_servidor]`.
