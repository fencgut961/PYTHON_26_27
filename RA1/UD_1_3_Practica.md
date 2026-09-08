# Actividad Práctica 1.3: Twitch Custom Stream Alerts & Rewards Router

En las plataformas de streaming actuales como **Twitch** o **YouTube Gaming**, los bots de chat y los overlays de pantalla procesan constantemente tramas de datos del servidor para activar alertas visuales en vivo y asignar permisos especiales (como chat exclusivo, recompensas o emotes personalizados).

En esta actividad, desarrollarás un motor de evaluación lógica para un bot de Twitch que analiza el perfil de un espectador y calcula de forma automática sus recompensas, accesos VIP y alertas del canal.

---

## 🚀 El Reto: Procesador Lógico de Espectadores

El programa debe simular la recepción de una trama de red compacta enviada por Twitch que contiene la información consolidada del espectador:

```python
trama = "VIEWER-7890_TIER-2_BITS0250_WATCH045_MOD1"
```

El script debe recortar la trama, procesar los valores de forma secuencial y realizar un análisis de acceso y recompensas mediante **expresiones lógicas avanzadas**.

### ⚠️ Restricciones del Ejercicio (La barrera "Anti-IA")
*   **Prohibido el uso de estructuras condicionales:** No puedes utilizar `if`, `elif` ni `else`.
*   **Prohibido el uso de bucles:** No puedes utilizar `for` ni `while`.
*   Todo el enrutamiento lógico debe resolverse guardando los resultados de comparaciones y operaciones lógicas directamente en variables booleanas (`True` o `False`).

---

## 📋 Requisitos de la Actividad

### 1. Extracción y Limpieza de Datos (Slicing)
A partir de la cadena original, utiliza rebanados fijos de cadena (`slicing`) para aislar cada estadística:
*   **ID Espectador:** Índices del `0` al `11` (ej. `"VIEWER-7890"`).
*   **Nivel de Suscripción:** Índices del `12` al `18` (ej. `"TIER-2"`). Los valores posibles son `"TIER-0"` (no suscrito), `"TIER-1"`, `"TIER-2"` y `"TIER-3"`.
*   **Bits Enviados:** Índices del `19` al `27`. Debes recortar el prefijo `"BITS"` (obteniendo `"0250"`) y convertir la cadena restante a un tipo entero (`int`).
*   **Horas de Visualización:** Índices del `28` al `36`. Debes recortar el prefijo `"WATCH"` (obteniendo `"045"`) y convertir la cadena restante a un tipo entero (`int`).
*   **Rol de Moderador:** Índices del `37` al `41`. Debes extraer `"MOD1"` o `"MOD0"` y transformarlo en un booleano real (`True` si es `"MOD1"`, `False` si es `"MOD0"`).

*(Pista: Para convertir el rol de moderador a booleano de forma segura, recuerda que comparar un texto directamente devuelve un booleano: `rol == "MOD1"`).*

### 2. Motor de Reglas Lógicas (Sin usar `if`)
Define las siguientes variables booleanas utilizando operadores relacionales (`==`, `!=`, `>`, `<`, `>=`, `<=`) y lógicos (`and`, `or`, `not`):

1.  **`es_suscriptor_activo`:** Será verdadero si el texto extraído del nivel de suscripción es diferente de `"TIER-0"`.
2.  **`es_donador_destacado`:** Será verdadero si los bits enviados son estrictamente mayores a `200`.
3.  **`tiene_recompensa_fidelidad`:** El espectador obtiene esta recompensa si ha visto más de `40` horas del stream **y además** es un suscriptor activo.
4.  **`activar_alerta_shoutout`:** Se debe disparar un efecto visual en pantalla si el espectador es un Moderador **o** si es un donador destacado.
5.  **`tiene_acceso_chat_vip`:** El chat VIP se concede a los moderadores de forma automática, o bien a cualquier espectador que sea suscriptor activo y tenga acumuladas más de `10` horas de reproducción.

### 3. Reporte Consolidado (f-strings)
Muestra por pantalla un informe claro y formateado de los resultados empleando una sola estructura de salida. El resultado final en la consola de IntelliJ debe lucir exactamente así:

```text
==================================================
   TWITCH STREAM OVERLAY - VIEWER METRICS CARD   
==================================================
ID Espectador: VIEWER-7890
Nivel de Suscripción: TIER-2
Bits Enviados: 250 | Horas Vistas: 45
Rol del Moderador: True
--------------------------------------------------
          ESTADOS Y RECOMPENSAS CALCULADAS       
--------------------------------------------------
¿Es Suscriptor Activo?: True
¿Es Donador Destacado?: True
¿Tiene Premio por Fidelidad?: True
¿Activar Alerta en Pantalla?: True
¿Tiene Acceso al Chat VIP?: True
==================================================
```

---

## 💾 Instrucciones de Entrega
*   Crea el archivo **`b1_4_twitch_rules.py`**.
*   Asegúrate de que no haya advertencias estéticas en el IDE y de añadir un Docstring general al inicio explicando su funcionamiento.
