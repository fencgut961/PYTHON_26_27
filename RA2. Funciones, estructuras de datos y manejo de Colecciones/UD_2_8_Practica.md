# Unidad 2.8 - Práctica: SIEM Security Log Parser & Threat Analytics

En esta actividad práctica, asumirás el rol de un **Ingeniero de Automatización de Ciberseguridad** en un Centro de Operaciones de Seguridad (SOC). Tu misión es escribir un script en Python capaz de analizar de forma masiva los registros de tráfico de red y de seguridad (logs) capturados por un sistema de monitorización central (SIEM) para detectar intrusiones activas, ataques de fuerza bruta y escaneos de puertos maliciosos en servidores críticos.

---

## El Reto: Analizador Inteligente de Registros de Intrusión (SIEM)

El sistema recolecta ráfagas de logs en un formato de texto plano estructurado. Tu programa debe procesar esta lista para filtrar amenazas, categorizar la gravedad del incidente, calcular estadísticas de puertos vulnerados y estructurar la información en un diccionario central para el equipo forense.

### Datos de Entrada de Referencia
Tu script debe inicializar la siguiente lista de eventos de red como base de datos de trabajo:

```python
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
```

---

## Instrucciones y Fases del Desarrollo

Deberás estructurar tu script en Python (`b2_8_siem_parser.py`) completando de forma guiada las siguientes fases funcionales utilizando modularización de funciones con sus respectivos Docstrings:

### Fase 1: Extracción Segura y Saneamiento de Amenazas
Escribe una función llamada `filtrar_incidentes_graves(logs)` que:
*   Reciba la lista completa de strings de logs.
*   Utilizando **comprensión de listas**, devuelva una nueva lista únicamente con los registros cuya gravedad sea `"CRITICO"` o `"ALTO"`.
*   *Pauta:* Debes limpiar los espacios adicionales de las subcadenas resultantes de la separación (`.split('|')`) aplicando el método `.strip()` antes de evaluar la gravedad.

### Fase 2: Mapeo de Puertos e IPs Atacadas
Escribe una función llamada `mapear_puertos_atacados(logs_graves)` que:
*   Reciba la lista de logs graves obtenidos en la Fase 1.
*   Utilizando **comprensión de listas**, extraiga exclusivamente los números de puerto (convertidos a tipo de dato entero `int`) presentes en cada registro grave.
*   A partir de la lista de puertos extraída, el programa principal debe determinar y mostrar en pantalla:
    1.  El puerto más bajo atacado utilizando `min()`.
    2.  El puerto más alto atacado utilizando `max()`.
    3.  El listado único de puertos ordenados ascendentemente (usando `sorted()` o `.sort()`).

### Fase 3: Categorización Masiva de Severidad
Escribe una función llamada `resumir_severidad(logs)` que:
*   Reciba la lista completa de logs originales.
*   Utilizando **comprensión de listas con condicional ternario (if-else)**, genere una lista de cadenas de texto que re-clasifique los registros de la siguiente manera:
    *   Si la gravedad es `"CRITICO"` o `"ALTO"`, se clasifica como `"BLOQUEO_INMEDIATO"`.
    *   Si es de cualquier otra gravedad (`"MEDIO"`, `"BAJO"`, `"INFO"`), se clasifica como `"MONITORIZACION_ESTANDAR"`.

### Fase 4: Base de Datos de IPs Sospechosas (Diccionarios Avanzados)
Escribe una función llamada `registrar_actividad_sospechosa(logs)` que:
*   Reciba la lista original de logs.
*   Inicialice un diccionario vacío de recuentos de incidentes.
*   Recorra los logs. Para cada registro, extraiga la IP de origen y el nivel de severidad.
*   Si la severidad es `"CRITICO"` o `"ALTO"`, debe añadir o actualizar la IP en el diccionario de sospechosos sumando un ataque detectado.
*   *Requisito Obligatorio:* Para evitar que el programa falle al registrar una IP por primera vez, debes utilizar el método avanzado **`.setdefault(ip, 0)`** o **`.get(ip, 0)`** para inicializar el contador de ataques de forma limpia y segura.

---

## Requisitos de Estilo y Entrega

1.  **Guía PEP 8:** El código debe usar nombres descriptivos en formato `snake_case` para variables y funciones. Las constantes (como los umbrales de severidad o puertos críticos) deben estar escritas en mayúsculas (`UPPERCASE`).
2.  **Documentación PEP 257:** Todas las funciones desarrolladas deben poseer obligatoriamente un **Docstring multilínea** explicativo en su cabecera.
3.  **Salida formateada:** El programa principal debe imprimir los reportes de seguridad simulando una consola SOC real de forma clara y legible usando f-strings.

### Ejemplo de Salida Esperada en Pantalla:
```text
🛡️ SOC SIEM THREAT ANALYTICS CORESYSTEM
============================================================

[ALERT] Incidentes de gravedad ALTA o CRÍTICA detectados:
  * 185.220.101.5 en puerto 22 -> Intento de login SSH fallido
  * 195.235.12.3 en puerto 3389 -> Intento RDP fuerza bruta
  * 185.220.101.5 en puerto 22 -> Intento de login SSH fallido
  * 203.0.113.50 en puerto 23 -> Intento Telnet no autorizado

📊 Estadísticas Forenses de Puertos Vulnerados:
  * Puerto mínimo auditado: 22
  * Puerto máximo auditado: 3389
  * Lista ordenada de puertos atacados: [22, 23, 3389]

🚨 Reporte Automatizado de Acciones de Firewall:
  * 192.168.1.50 -> MONITORIZACION_ESTANDAR
  * 185.220.101.5 -> BLOQUEO_INMEDIATO
  * 195.235.12.3 -> BLOQUEO_INMEDIATO
  ...

🕵️ Base de Datos de IPs Hostiles Detectadas (Ataques Graves):
  * IP: 185.220.101.5 -> 2 intentos críticos registrados.
  * IP: 195.235.12.3 -> 1 intentos críticos registrados.
  * IP: 203.0.113.50 -> 1 intentos críticos registrados.
```
