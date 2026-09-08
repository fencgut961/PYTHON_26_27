# Unidad 3.1: Manejo de Ficheros - Lectura, Escritura y Manejo de Excepciones

En el desarrollo de software profesional, la memoria RAM es volátil; todos los datos que procesamos se pierden al finalizar la ejecución del programa. Para que la información persista más allá de la vida de un script, es indispensable trabajar con recursos externos de almacenamiento: los **ficheros** o archivos.

En esta unidad aprenderemos a interactuar con el sistema de archivos del disco de forma segura, estructurada y profesional en Python, aplicando el control de excepciones para mitigar fallos en entornos de producción.

---

## 1. Introducción a los Ficheros en Python

Un **fichero** es un conjunto de datos estructurados que se almacena de forma persistente en una unidad física de almacenamiento. En programación, distinguimos principalmente entre dos tipos:
*   **Ficheros de Texto:** Contienen caracteres legibles organizados en líneas (ej: archivos `.txt`, `.csv`, `.json`, `.log`). Se codifican tradicionalmente en formatos como UTF-8.
*   **Ficheros Binarios:** Contienen datos en formato crudo de bytes (ej: imágenes `.png`, archivos ejecutables, archivos comprimidos `.zip`, o ficheros serializados).

Para abrir y trabajar con cualquier archivo en Python, se utiliza la función integrada:
```python
open(nombre, modo, encoding)
```

### Modos de apertura esenciales
Al abrir un fichero, debemos especificar mediante una cadena de texto el propósito de la apertura. Es una excelente práctica almacenar este modo en una variable descriptiva:
*   `modo = "r"` (Lectura / Read): Abre el archivo únicamente para leer su contenido. Si el archivo no existe en la ruta especificada, Python lanzará una excepción de tipo `FileNotFoundError`.
*   `modo = "w"` (Escritura / Write): Abre el archivo para escribir datos. Si el archivo ya existe, **borra por completo su contenido** (sobrescritura) antes de escribir. Si no existe, lo crea automáticamente.
*   `modo = "a"` (Anexar / Append): Abre el archivo para añadir información al final de este, **respetando el contenido previo**. Si el archivo no existe, lo crea.
*   `modo = "rb"` o `"wb"` (Binario / Read-Write Binary): Abre el archivo en modo lectura o escritura de datos binarios (bytes puros), omitiendo la decodificación de texto.

> **Recomendación de oro:** Al trabajar con archivos de texto, siempre se debe especificar el parámetro `encoding="utf-8"`. Esto garantiza la correcta visualización de caracteres especiales, tildes y eñes independientemente del sistema operativo en el que se ejecute el programa (Windows, macOS o Linux).

---

## 2. Lectura Eficiente de Ficheros

Python proporciona varios métodos para extraer la información de un archivo abierto en modo lectura (`"r"`):

### A. Método `.read()`
Lee la totalidad del archivo y lo devuelve como una única cadena de texto (`str`).
```python
archivo = "registro_accesos.log"
modo = "r"

with open(archivo, modo, encoding="utf-8") as f:
    contenido = f.read()
    print(contenido)
```

### B. Método `.readline()`
Lee una sola línea del archivo (hasta encontrar un carácter de salto de línea `
`) y la devuelve como un `str`. Es ideal para procesar archivos línea a línea sin saturar la memoria.
```python
with open(archivo, modo, encoding="utf-8") as f:
    primera_linea = f.readline()
    print("Cabecera:", primera_linea.strip())
```

### C. Método `.readlines()`
Lee todas las líneas del archivo y las devuelve agrupadas dentro de una lista de Python (`list`), donde cada línea es un elemento de texto independiente.
```python
with open(archivo, modo, encoding="utf-8") as f:
    lista_lineas = f.readlines()
    print(f"El archivo tiene {len(lista_lineas)} registros.")
```

### D. Iteración Directa (La forma más eficiente)
Podemos recorrer un archivo directamente utilizando un bucle `for`. Esta es la práctica recomendada por la industria ya que Python no carga todo el archivo en la memoria RAM, sino que va leyendo línea a línea bajo demanda de forma extremadamente eficiente.
```python
with open(archivo, modo, encoding="utf-8") as f:
    for linea in f:
        # Usamos .strip() para eliminar el salto de línea al final de cada registro
        print("Registro procesado:", linea.strip())
```

---

## 3. Escritura y Anexado Seguro

Para escribir información en el disco utilizaremos el método `.write(texto)`. A diferencia de la función `print()`, el método `.write()` **no añade automáticamente un salto de línea** al final, por lo que debemos incluir explícitamente el carácter `
` cuando sea necesario separar registros.

### Ejemplo de Escritura (`"w"`):
```python
archivo_salida = "alertas_criticas.txt"
modo_escritura = "w"

with open(archivo_salida, modo_escritura, encoding="utf-8") as f:
    f.write("ALERTA: Acceso no autorizado detectado en servidor DB
")
    f.write("Nivel: CRÍTICO
")
```

### Ejemplo de Anexado (`"a"`):
```python
archivo_historico = "historico_alertas.txt"
modo_anexar = "a"

with open(archivo_historico, modo_anexar, encoding="utf-8") as f:
    f.write("05-09-2026 10:45:32 - IP 192.168.1.150 bloqueada provisionalmente
")
```

---

## 4. Gestión de Recursos con la Sentencia `with`

En versiones antiguas de Python, abrir un archivo requería cerrarlo explícitamente usando el método `.close()`:
```python
f = open("datos.txt", "r")
# ... procesar datos ...
f.close()  # Obligatorio para liberar el archivo en el sistema operativo
```
Si el programa fallaba o se interrumpía antes de llegar a la línea `.close()`, el archivo quedaba bloqueado en memoria, pudiendo provocar corrupción de datos o fugas de recursos del sistema.

Para solucionar esto, Python introdujo el **Administrador de Contexto** mediante la palabra clave **`with`**.
```python
with open("datos.txt", "r", encoding="utf-8") as f:
    contenido = f.read()
# En este punto, fuera del bloque indentado, el archivo se ha cerrado automáticamente
print("¿Archivo cerrado?:", f.closed)  # Retorna True
```
El uso de `with` garantiza que el archivo **se cerrará de forma segura y automática** tan pronto como el flujo del programa salga del bloque indentado, incluso si ocurre una excepción grave dentro de él.

---

## 5. Robustez en Producción: Manejo de Excepciones

La interacción con recursos externos al programa siempre es propensa a fallos físicos e imprevistos fuera de nuestro control directo (el archivo no existe, no tenemos permisos, el disco está lleno, etc.). Si no controlamos estos eventos, el programa sufrirá un cierre inesperado (*crash*).

Utilizaremos bloques **`try-except`** para capturar y mitigar de forma limpia los errores más habituales:
*   `FileNotFoundError`: Lanzado cuando intentamos abrir en modo lectura (`"r"`) un archivo que no existe en el disco o cuya ruta está mal escrita.
*   `PermissionError`: Lanzado cuando el sistema operativo deniega el acceso al archivo (por ejemplo, al intentar escribir en un directorio protegido o abrir un archivo exclusivo del sistema).

```python
archivo_seguridad = "config/firewall.rules"
modo_lectura = "r"

try:
    with open(archivo_seguridad, modo_lectura, encoding="utf-8") as f:
        configuracion = f.read()
        print("Reglas del cortafuegos cargadas con éxito.")
except FileNotFoundError:
    print("❌ Error crítico: No se encontró el archivo 'firewall.rules'. Cargando configuración por defecto...")
except PermissionError:
    print("❌ Error de seguridad: Permisos insuficientes para acceder a las reglas del cortafuegos.")
except Exception as e:
    print(f"❌ Error inesperado al procesar el archivo: {e}")
```

Al implementar estas estructuras condicionales de error, aseguramos que nuestra aplicación continúe funcionando de manera robusta y controlada ante cualquier anomalía externa.
