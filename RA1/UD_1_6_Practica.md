# Actividad Práctica 1.6: Spotify Smart Playlist & Queue Manager

En plataformas de streaming de música y podcast como **Spotify**, la reproducción secuencial de pistas requiere algoritmos inteligentes que puedan tomar decisiones en tiempo real sobre el flujo de reproducción. 

En esta actividad, diseñarás un **gestor de cola de reproducción inteligente** para Spotify que simule un sistema capaz de reproducir pistas de audio, saltarse anuncios publicitarios (Ad Skipping), ignorar archivos corruptos y realizar un apagado de emergencia si se detecta un estado crítico del dispositivo.

---

## 💻 El Reto: Algoritmo de Control de Reproducción

Dispones de una cola de reproducción representada por una secuencia de pistas en texto crudo dentro de tu programa:

```python
cola_reproduccion = [
    "SONG: Blinding Lights - The Weeknd",
    "AD: Coca-Cola Summer Splash",
    "SONG: Starboy - The Weeknd",
    "ERROR: Audio File Corrupt (0x302)",
    "SONG: Die With A Smile - Bruno Mars",
    "AD: Spotify Premium Promo",
    "SONG: One Dance - Drake",
    "CRITICAL: Battery Low 2%",
    "SONG: Stay - Justin Bieber"
]
```

Debes escribir un script en Python que recorra de forma secuencial esta cola de reproducción y tome decisiones dinámicas de flujo basándose en las etiquetas de cada elemento.

### 📋 Requisitos de la Aplicación

1. **Recorrido de la Cola:**
   Utiliza un bucle `for` para iterar sobre la lista `cola_reproduccion`. En cada vuelta del bucle, debes analizar qué tipo de elemento estás procesando.

2. **Filtro de Anuncios (`AD:`):**
   * Si la pista actual contiene la etiqueta `"AD:"` en su texto, el programa debe mostrar un mensaje en consola: `"[Saltando Anuncio] -> [Nombre del anuncio] "`.
   * Debes utilizar de forma obligatoria la sentencia **`continue`** para saltar la reproducción y pasar directamente a la siguiente canción sin incrementar el contador de música.

3. **Filtro de Errores de Audio (`ERROR:`):**
   * Si la pista contiene la etiqueta `"ERROR:"`, el programa debe reportar un aviso en consola: `"[Fallo de Conexión] -> Omitiendo archivo dañado: [Detalle del error]"`.
   * Al igual que con los anuncios, debes utilizar **`continue`** para ignorar el archivo corrupto y seguir con la lista.

4. **Apagado de Emergencia por Batería (`CRITICAL:`):**
   * Si la pista contiene la etiqueta `"CRITICAL:"`, significa que el sistema del teléfono móvil se va a apagar. El script debe mostrar un mensaje alarmante: `"[ALERTA DEL SISTEMA] -> [Detalle del aviso]. Apagando reproductor..."`.
   * Debes utilizar de forma obligatoria la sentencia **`break`** para detener la reproducción de inmediato, interrumpiendo todo el bucle.

5. **Reproducción de Canciones (`SONG:`):**
   * Si es una canción válida (`"SONG:"`), el programa debe "reproducirla", mostrando en pantalla: `"🔊 Reproduciendo: [Nombre de la canción]... [OK]"`.
   * Debes llevar la cuenta (usando un acumulador numérico entero) de cuántas canciones reales han sido escuchadas con éxito.

6. **Estadísticas Finales:**
   * Al salir del bucle (sea de forma natural o por el apagado de emergencia), el programa debe imprimir un reporte resumen con el total de canciones reproducidas.

---

## ⚠️ Restricciones Estrictas

* **Prohibido** modificar la lista original `cola_reproduccion` antes de iniciar el bucle.
* **Prohibido** crear funciones personalizadas o clases (estructuras POO).
* Tu código debe seguir estrictamente las normas estéticas **PEP 8**:
  * Nombres de variables descriptivos y en formato `snake_case`.
  * Comentarios explicativos con el carácter `#` en bloques clave.
  * Correcta indentación obligatoria de 4 espacios.
  * Añadir un **Docstring** explicativo con comillas triples (utilizando comillas triples `"""`) al inicio del archivo describiendo el propósito del script.
