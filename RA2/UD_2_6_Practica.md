# Actividad Práctica: Incident Response - Digital Forensics & IOC Tracker

En el campo de la **ciberseguridad**, cuando se detecta una intrusión o brecha de seguridad en un servidor crítico, los analistas de Respuesta a Incidentes (Incident Response) deben recolectar muestras de red y evidencias físicas llamadas **Indicadores de Compromiso (IOCs)**.

Para asegurar que estas evidencias sean admisibles en auditorías legales o reportes forenses, no pueden sufrir ninguna alteración accidental en memoria durante el procesamiento de datos. Por esta razón, las **tuplas inmutables** de Python son la estructura de datos obligatoria para almacenar la traza original de cada evento.

---

## 🎯 Objetivos de la Actividad
Al finalizar esta práctica serás capaz de:
1. Crear y estructurar datos históricos de red utilizando **tuplas inmutables**.
2. Evitar la manipulación o corrupción accidental de evidencias físicas utilizando el control de inmutabilidad de Python.
3. Extraer y procesar la información de eventos forenses mediante el **desempaquetado (unpacking)** directo de variables.
4. Convertir datos estructurados de tuplas a listas para el saneamiento de falsos positivos y volver a bloquearlos como tuplas inmutables.
5. Diseñar funciones parametrizadas y documentadas que colaboren en el flujo de análisis del cortafuegos de red.

---

## 💻 El Reto: "Forense-Tracker 2.0"

Como analista del Centro de Operaciones de Seguridad (SOC), se te ha asignado investigar una intrusión activa en los servidores centrales de una empresa de tecnología financiera. Has extraído de la base de datos de logs un evento crudo que describe una conexión sospechosa:

* **Trama de red extraída**: `"2026-09-05 10:15:32_185.220.101.5_3389_a5c3e800fd2e411_ALTO"`

### Requisitos del Programa (`b2_6_forense_tracker.py`)

Deberás construir un programa modular y limpio en Python que realice las siguientes tareas de análisis y respuesta paso a paso:

### Fase 1: Extracción y Almacenamiento Seguro (Tuplas)
1. **Definir constantes de seguridad**: Define una tupla global inmutable llamada `PUERTOS_CRITICOS` que contenga los puertos más vigilados por ataques de intrusión: `22` (SSH), `80` (HTTP), `443` (HTTPS) y `3389` (RDP).
2. **Procesar la trama**: En el bloque principal del programa, extrae los campos de la trama sospechosa utilizando el separador de guiones bajos `_` (puedes usar el método `.split("_")`).
3. **Crear la tupla de evidencia**: Guarda la evidencia en una tupla llamada `evidencia_original` con los siguientes tipos de datos correctos tras realizar las conversiones oportunas:
   * **Timestamp** (`str`): Fecha y hora del evento.
   * **IP Origen** (`str`): Dirección IP sospechosa.
   * **Puerto Destino** (`int`): El puerto de red afectado (convertido a entero).
   * **Hash Malware** (`str`): La huella digital del archivo modificado.
   * **Severidad** (`str`): Gravedad inicial del incidente.

---

### Fase 2: Análisis Automatizado (Modularidad y Unpacking)
Crea una función llamada `analizar_incidente` con las siguientes características:
* **Entrada**: Recibe como parámetro la tupla de evidencia (`evidencia_original`).
* **Documentación**: Debe incluir su **Docstring** correspondiente detallando qué realiza y qué retorna.
* **Lógica interna**:
  1. Desempaqueta la tupla de evidencia en 5 variables locales directamente (`fecha`, `ip`, `puerto`, `hash_evidencia`, `gravedad`).
  2. Verifica si el `puerto` destino está presente en la tupla global `PUERTOS_CRITICOS`.
  3. Muestra en pantalla un reporte del análisis estético utilizando f-strings. Para mayor profesionalidad, la función debe mostrar el hash de malware truncado (mostrando solo los primeros 6 caracteres, seguidos de puntos suspensivos y los últimos 6 caracteres. Ejemplo: `a5c3e8...fd2e411`).
* **Retorno**: La función debe retornar `True` si el incidente afecta a un puerto crítico (y por tanto requiere respuesta inmediata), o `False` en caso contrario.

---

### Fase 3: Mitigación e Integridad de Datos (Conversión y Saneamiento)
Para simular el flujo del SOC, si el analista presiona una tecla para confirmar que se trata de un "falso positivo" (por ejemplo, porque la IP pertenece a una auditoría autorizada de la propia empresa):
1. Diseña una función llamada `marcar_falso_positivo` que reciba la tupla de evidencia original.
2. Como las tuplas son inmutables, la función debe:
   * Convertir la tupla a una lista mutable.
   * Cambiar el campo de `Severidad` (último elemento) al string `"FALSO_POSITIVO"`.
   * Convertir de nuevo la lista resultante a una tupla inmutable.
   * Retornar la nueva tupla inmutable saneada para evitar que vuelva a alterarse.

---

### Requisitos de Calidad de Código
* El código debe estar perfectamente formateado bajo las pautas estéticas de **PEP 8** (nombres en `snake_case`, espacios apropiados, líneas menores de 79 caracteres).
* Cada función creada debe contener un **Docstring multilínea** formal (`"""`).
* El programa principal debe ejecutar de forma interactiva las tres fases y mostrar un informe final con la estructura inmutable limpia de la evidencia saneada.

---

## 📊 Ejemplo de Salida Esperada en Consola

```text
🕵️‍♂️ SOC INCIDENT RESPONSE & FORENSICS TOOL - UNIT 2.6
Cargando bases de datos de puertos críticos... OK

[FASE 1] Recolectando evidencia física inmutable...
Evidencia bloqueada en memoria con éxito. Tipo: <class 'tuple'>

[FASE 2] Iniciando análisis automatizado del incidente...
----------------------------------------------------------------------
🚨 ALERTA DE INCIDENTE DETECTADA
----------------------------------------------------------------------
📅 Fecha y Hora:  2026-09-05 10:15:32
🌐 IP Origen:     185.220.101.5
🎯 Puerto Destino: 3389 [ALERTA: PUERTO CRÍTICO DETECTADO]
💾 Hash Evidencia: a5c3e8...fd2e411
⚠️ Severidad:      ALTO
----------------------------------------------------------------------

¿Deseas mitigar este incidente como Falso Positivo? (s/n): s

[FASE 3] Saneando base de datos forense...
Convirtiendo datos temporales para su edición... OK
Re-bloqueando evidencia final del incidente... OK

[REPORTE DE AUDITORÍA FINAL]
Contenido final seguro: ('2026-09-05 10:15:32', '185.220.101.5', 3389, 'a5c3e800fd2e411', 'FALSO_POSITIVO')
Verificación de integridad física: Intento de alteración de tupla final -> Bloqueado por el intérprete de Python (Inmutable)
```
