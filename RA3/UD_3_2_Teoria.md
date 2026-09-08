# Unidad 3.2: Módulos Estándar (os, sys, math) y Librerías Externas (pip)

Una de las mayores fortalezas de Python es su filosofía de **"baterías incluidas"**. Esto significa que al instalar Python, ya disponemos de un ecosistema gigantesco de herramientas listas para ser utilizadas sin necesidad de programar todo desde cero.

En esta unidad estudiaremos cómo ampliar las capacidades de nuestros programas importando módulos integrados del sistema, y aprenderemos a instalar y consumir software desarrollado por la comunidad internacional a través del gestor de paquetes de Python (`pip`).

---

## 1. ¿Qué es un Módulo?

Un **módulo** es un archivo de Python (con extensión `.py`) que agrupa funciones, variables, constantes y clases relacionadas bajo un mismo propósito, facilitando su reutilización.

Existen tres procedencias de módulos en Python:
1.  **Módulos Estándar:** Vienen preinstalados de forma nativa con el intérprete de Python (ej: `math`, `os`, `sys`, `random`, `json`).
2.  **Módulos Externos:** Desarrollados por terceros y publicados en el índice oficial PyPI. Deben ser instalados con la herramienta `pip`.
3.  **Módulos Personalizados:** Archivos propios que creamos para estructurar nuestro proyecto (los estudiaremos en la siguiente unidad).

### Sintaxis y Formas de Importación
Para incorporar el código de un módulo a nuestro script, disponemos de tres variantes sintácticas:

#### A. Importar el módulo completo (`import`)
Importa todo el archivo. Para acceder a sus funciones, debemos utilizar el espacio de nombres del módulo seguido de un punto:
```python
import math
print(math.sqrt(16))  # Salida: 4.0
```

#### B. Importar elementos específicos (`from ... import ...`)
Aísla únicamente la función o constante que necesitamos, permitiéndonos invocarla directamente por su nombre sin escribir el prefijo del módulo:
```python
from math import pi, sqrt
print(sqrt(25))    # Salida: 5.0
print(pi)          # Salida: 3.141592653589793
```

#### C. Asignar un alias o renombrar (`import ... as ...`)
Asigna un nombre abreviado o alternativo al módulo. Es muy utilizado para acortar llamadas en librerías complejas:
```python
import math as m
print(m.factorial(5))  # Salida: 120
```

---

## 2. Módulos Estándar Esenciales de la Industria

Analizaremos en profundidad tres de los módulos nativos más utilizados en el desarrollo backend, la administración de sistemas y la ciberseguridad industrial:

### A. Módulo `math` (Operaciones Científicas)
Proporciona acceso a funciones matemáticas de alta precisión y constantes de la naturaleza física.
*   `math.sqrt(x)`: Calcula la raíz cuadrada decimal de `x`.
*   `math.factorial(x)`: Devuelve el factorial de un número entero `x`.
*   `math.pow(x, y)`: Eleva `x` a la potencia de `y` (retorna un float).
*   `math.pi` y `math.e`: Constantes matemáticas fundamentales ($\pi pprox 3.14159$ y $e pprox 2.71828$).

```python
import math

radio = 4.5
area_sensor = math.pi * math.pow(radio, 2)
print(f"Área del círculo de cobertura: {area_sensor:.4f} m²")
```

### B. Módulo `os` (Interacción con el Sistema Operativo)
Permite que nuestro script de Python interactúe de forma nativa con el sistema de archivos del sistema operativo anfitrión.
*   `os.getcwd()`: Obtiene la ruta física del directorio de trabajo actual (Current Working Directory).
*   `os.listdir(ruta)`: Retorna una lista con los nombres de todos los archivos y carpetas contenidos en la ruta especificada. Usar `"."` representa la carpeta actual.
*   `os.mkdir(nombre)`: Crea una nueva carpeta en la ruta de ejecución. Lanza una excepción si el directorio ya existe.

```python
import os

print("Ubicación actual del script:", os.getcwd())
print("Archivos en el directorio activo:")
for elemento in os.listdir("."):
    print(f" - {elemento}")
```

### C. Módulo `sys` (Interacción con el Intérprete)
Facilita la interacción y obtención de parámetros de bajo nivel del propio entorno de ejecución de Python.
*   `sys.version`: Devuelve una cadena con la versión actual de Python en ejecución.
*   `sys.platform`: Indica el sistema operativo en el que se ejecuta el intérprete (ej: `"win32"` para Windows, `"darwin"` para macOS, o `"linux"` para sistemas basados en Linux).

```python
import sys

print("Servidor operativo bajo:", sys.platform)
print("Intérprete de Python:", sys.version)
```

---

## 3. Consumo de Librerías Externas con `pip`

Cuando los módulos estándar no son suficientes para resolver nuestro problema, recurrimos a librerías externas de código abierto.

### ¿Qué es `pip`?
`pip` (Package Installer for Python) es el gestor de paquetes estándar de la industria. Se encarga de descargar, instalar y actualizar dependencias desde el repositorio oficial **PyPI** (Python Package Index), resolviendo automáticamente las dependencias que requiera el paquete.

### Comandos esenciales en la terminal de comandos:
*   **Instalar un paquete:**
    ```bash
    pip install nombre_del_paquete
    ```
*   **Desinstalar un paquete:**
    ```bash
    pip uninstall nombre_del_paquete
    ```
*   **Ver paquetes instalados en el entorno:**
    ```bash
    pip list
    ```

---

## 4. Consumo Real de una API de Red con `requests`

Una de las librerías externas más utilizadas en el mundo profesional es **`requests`**, diseñada para realizar peticiones HTTP de forma intuitiva, limpia y eficiente.

Para poder utilizarla, debemos instalarla primero desde la terminal de nuestro sistema operativo o la consola embebida de IntelliJ:
```bash
pip install requests
```

Una vez instalada, podemos realizar consultas a servicios web externos (APIs) para obtener datos en tiempo real. 

### Ejemplo de integración robusta con control de errores:
```python
import requests

url_api = "https://api.github.com"

try:
    print(f"Iniciando petición GET segura a: {url_api}")
    # Realizamos la petición HTTP GET
    respuesta = requests.get(url_api, timeout=5)
    
    # Comprobamos el código de estado HTTP (200 representa éxito)
    print("Código de estado HTTP de respuesta:", respuesta.status_code)
    print("Tamaño del payload recibido:", len(respuesta.text), "caracteres.")
    
    # Mostramos de forma truncada el contenido de la respuesta
    print("\nCabecera de la respuesta (Primeros 150 caracteres):")
    print(respuesta.text[:150])

except requests.exceptions.ConnectionError:
    print("❌ Error de red: No se pudo establecer conexión con el servidor. Verifica tu acceso a internet.")
except requests.exceptions.Timeout:
    print("❌ Error de red: La solicitud superó el tiempo máximo de espera de respuesta (Timeout).")
except Exception as e:
    print(f"❌ Ocurrió una anomalía inesperada al procesar la petición: {e}")
```

Mediante el uso combinado de módulos nativos y de librerías de red externas, Python se convierte en una herramienta extremadamente potente para la automatización, monitorización y administración de sistemas en cualquier empresa tecnológica.
