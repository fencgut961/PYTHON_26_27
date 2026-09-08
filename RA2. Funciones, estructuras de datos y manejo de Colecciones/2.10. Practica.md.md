# Práctica de Ciberseguridad: Network Traceroute Threat Auditor

En los departamentos de Respuesta a Incidentes (Incident Response), los analistas forenses deben rastrear las rutas físicas que recorren las conexiones sospechosas. El enrutamiento de red consiste en saltos progresivos entre routers. Si un paquete de datos pasa por un salto que pertenece a una IP de un país bajo embargo o un servidor proxy anónimo conocido, el incidente debe catalogarse inmediatamente como una anomalía de alta severidad.

En esta práctica, programarás un analizador automatizado de trazas de red para determinar la integridad del enrutamiento y auditar la latencia de respuesta de los servidores intermediarios.

---

## El Reto: Desarrollar el Auditor de Ruta de Red

Tu objetivo es programar un script en Python (`b2_10_auditor_ruta.py`) que audite un listado de saltos de red, calcule el tiempo transcurrido, verifique la validez de los nodos utilizando un listado de IPs sospechosas y determine en qué posición exacta de la ruta se vulneró la seguridad de los datos.

### Requisitos Funcionales del Programa

1.  **Definición de Constantes de Seguridad (PEP 8):**
    *   Una tupla inmutable llamada `IPS_SOSPECHOSAS` que contenga: `"185.220.101.5"`, `"45.142.120.9"`, `"109.201.154.3"`.
    *   Una constante entera llamada `LIMITE_LATENCIA = 120` (en milisegundos).

2.  **Modularización mediante Funciones:**
    Deberás programar obligatoriamente la siguiente función con su Docstring PEP 257:
    *   `auditar_traceroute(nodos_ruta, latencias_ms)`:
        *   Recibe una lista con los nombres o IPs de los saltos de red (`nodos_ruta`) y otra lista paralela con las latencias de respuesta en milisegundos en cada nodo (`latencias_ms`).
        *   Debe recorrer de forma sincronizada ambas listas utilizando `zip()`.
        *   Para indexar los saltos físicamente empezando en el número **1**, debes envolver la estructura de iteración utilizando la función `enumerate()`.
        *   En cada iteración, debe realizar dos verificaciones lógicas:
            1.  Si la IP del nodo actual está en `IPS_SOSPECHOSAS`, debe marcar la ruta como comprometida registrando el número de salto exacto.
            2.  Si la latencia en ese salto supera el `LIMITE_LATENCIA`, debe registrar que ese salto específico ha experimentado un retraso de tráfico anómalo.
        *   Debe imprimir de forma estética y ordenada el análisis de cada salto utilizando f-strings.
        *   Debe retornar una tupla con tres resultados: un booleano indicando si la ruta es segura, el número del primer salto comprometido (o `None` si no hubo incidentes), y una nueva lista de tuplas con el formato `(salto_id, ip_nodo)` que contenga exclusivamente los saltos que sufrieron problemas de latencia lenta.

3.  **Programa Principal e Interacción:**
    *   El programa principal debe inicializar dos casos de prueba realistas para comprobar el correcto funcionamiento del algoritmo de auditoría:
        *   **Ruta Segura (Canal Limpio):**
            *   Nodos: `["192.168.1.1", "10.0.0.1", "172.16.0.5", "8.8.8.8"]`
            *   Latencias: `[5, 12, 45, 95]`
        *   **Ruta Comprometida (Ataque con desvío y retardo):**
            *   Nodos: `["192.168.0.1", "10.100.1.1", "45.142.120.9", "109.201.154.3", "195.12.5.40"]`
            *   Latencias: `[15, 30, 240, 110, 310]`
    *   Invoca a la función `auditar_traceroute()` para cada ruta, captura sus valores de retorno y muestra por consola un reporte final resumido para el administrador del SOC.

---

## Restricciones y Estilo de Programación

*   **Sin Variables Globales Sucias:** Está terminantemente prohibido utilizar variables globales para el cálculo de estados dentro de la función auditora. Todo debe ser parametrizado y devuelto con `return`.
*   **Uso obligatorio de `enumerate()`:** El reporte debe indexar los saltos lógicos empezando desde 1 haciendo uso del argumento `start=1`. Queda prohibido el uso de contadores manuales (`i = 0`, `i += 1`) o el uso indirecto de índices con `range(len())`.
*   **Docstrings:** La función debe estar documentada detallando el propósito, argumentos recibidos y tipo de dato del valor retornado.
*   **Formato de Salida:** Las IP de los saltos sospechosos o con problemas de latencia deben destacarse en la salida para facilitar la lectura del analista de guardia.
