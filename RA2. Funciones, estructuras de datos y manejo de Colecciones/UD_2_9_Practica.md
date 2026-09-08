# Práctica Evaluada: Cross-Incident Log Correlator & Threat Intelligence Synthesizer

En el ámbito de la ciberseguridad, los analistas de Respuesta a Incidentes (Incident Response) dentro de un Centro de Operaciones de Seguridad (SOC) reciben constantemente alertas y logs procedentes de múltiples sondas de red y servidores de forma aislada.

Para poder entender el alcance de una intrusión compleja, es obligatorio sincronizar cronológicamente estos flujos independientes de datos (timestamps, IPs de origen, puertos de destino y payloads detectados) en un único informe unificado.

En esta práctica, programarás un correlacionador cruzado de seguridad utilizando la función integrada `zip()`.

---

## 🎯 Objetivos de la Actividad
* Aplicar la función integrada `zip()` para recorrer y unificar múltiples colecciones paralelas de forma limpia y eficiente.
* Comprender y aplicar el control automático de desajustes de longitud en colecciones utilizando `zip()`.
* Utilizar el desempaquetado de tuplas con el operador estrella (`zip(*...)`) para extraer y aislar datos de amenazas de red.
* Organizar y persistir los incidentes en un diccionario anidado local como base de datos en memoria para el SOC.
* Modularizar el programa en funciones parametrizadas independientes y documentadas según PEP 8.

---

## 💻 El Reto: Correlacionador Cruzado de Seguridad

Tu empresa ha sufrido un intento de intrusión distribuido. Cuatro sondas independientes del SOC han exportado los siguientes logs en listas separadas durante el intervalo del incidente:

```python
timestamps = ["14:22:05", "14:23:18", "14:25:40", "14:28:11", "14:30:00"]
source_ips = ["185.220.101.5", "192.168.1.15", "45.138.99.102", "10.0.0.8"]
target_ports = [3389, 80, 22, 443, 8080]
payloads = ["RDP Exploit Attempt", "Normal HTTP GET", "SSH Brute Force", "HTTPS Handshake", "WAF Blocked"]
```

⚠️ **Atención:** Como puedes observar, la lista de marcas de tiempo (`timestamps`), puertos (`target_ports`) y descripciones (`payloads`) contienen 5 elementos, pero la lista de IPs sospechosas (`source_ips`) solo contiene 4 elementos debido a un fallo de red durante el volcado forense.

Tu script debe manejar de forma automática este desajuste utilizando las reglas de la función `zip()`.

---

## 🧱 Requisitos del Programa

Debes crear un único archivo ejecutable en Python llamado `b2_9_correlacionador_soc.py` que implemente la siguiente estructura modular:

### 1. Funciones Obligatorias a Desarrollar

*   **`correlacionar_logs(tiempos, ips, puertos, firmas)`**:
    *   Recibe las cuatro listas independientes como parámetros de entrada.
    *   Crea un diccionario vacío llamado `registro_incidentes`.
    *   Usa la función `zip()` para recorrer las cuatro listas simultáneamente en un único bucle.
    *   Para cada iteración, debe autogenerar un identificador secuencial como clave (ej: `"INC-001"`, `"INC-002"`, etc.).
    *   El valor asociado a cada identificador debe ser un diccionario interno con los datos del incidente: `"Hora"`, `"IP"`, `"Puerto"` y `"Firma"`.
    *   Debe añadir una lógica condicional interna para calcular el nivel de `"Severidad"` del incidente en base a estas reglas de negocio:
        *   **CRÍTICA:** Si el puerto es `22` (SSH) o `3389` (RDP) **y** la firma contiene el texto `"Exploit"` o `"Brute Force"`.
        *   **ALTA:** Si el puerto es `22` (SSH) o `3389` (RDP), pero la firma no cumple la condición anterior.
        *   **MEDIA:** En cualquier otro caso.
    *   Retorna el diccionario `registro_incidentes` completo.

*   **`mostrar_reporte_soc(registro_incidentes)`**:
    *   Recibe el diccionario de incidentes correlacionados.
    *   Muestra en consola un reporte estético, limpio y tabulado con los incidentes.
    *   Debe mostrar un número de orden natural para el operador humano (empezando en `1`) usando el índice secuencial.

*   **`aislar_ips_amenaza(registro_incidentes)`**:
    *   Recibe el diccionario de incidentes.
    *   Debe extraer todas las parejas de datos `(IP, Puerto)` de los incidentes registrados y guardarlas en una lista de tuplas.
    *   Debe aplicar el **desempaquetado estrella (`zip(*...)`)** sobre esta lista para separar las IPs por un lado y los puertos por otro de forma automática.
    *   Imprime y devuelve la tupla exclusiva con las IPs sospechosas aisladas para que el operador de red pueda agregarlas inmediatamente a las reglas de baneo del firewall.

### 2. Programa Principal e Interacción

En el bloque principal del script (`if __name__ == "__main__":`):
1.  Define las cuatro listas de logs iniciales proporcionadas en el enunciado.
2.  Invoque la función `correlacionar_logs` pasándole las listas como argumentos de entrada.
3.  Imprima un mensaje informativo explicando que se han detectado desajustes de red y detallando cuántos incidentes se han logrado correlacionar de forma segura en base al tamaño de la lista de IPs.
4.  Invoque la función `mostrar_reporte_soc` para renderizar el informe unificado en pantalla.
5.  Invoque la función `aislar_ips_amenaza` para generar y visualizar la tupla de IPs sospechosas para bloqueo preventivo.

---

## 📋 Criterios de Estilo y Presentación (PEP 8)

*   **Nombres de variables y funciones:** Todos los identificadores deben estar escritos de forma descriptiva en minúsculas y utilizando guiones bajos (`snake_case`).
*   **Constantes globales:** Las constantes del programa deben declararse en la zona superior en mayúsculas.
*   **Documentación de funciones:** Cada función desarrollada debe contar obligatoriamente con un **Docstring** explicativo entre comillas triples (`"""`) detallando brevemente su propósito, parámetros y valor de retorno.
*   **Líneas limpias:** Sin comentarios redundantes o líneas de código extremadamente largas.
