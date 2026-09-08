# Unidad 3.3: Creación de Módulos Personalizados y Organización de Proyectos

A medida que un software crece en funcionalidad, mantener todo el código en un único script (como `main.py`) se vuelve inviable. El archivo se vuelve ilegible, el mantenimiento se complica y se bloquea la posibilidad de colaborar en equipo. 

La solución profesional consiste en la **modularización**: dividir la aplicación en múltiples archivos de Python especializados e independientes que colaboren entre sí. En esta unidad aprenderemos a diseñar nuestros propios módulos, estructurarlos dentro de paquetes corporativos y organizar proyectos siguiendo los estándares oficiales de la industria.

---

## 1. Creación de un Módulo Personalizado

Cualquier archivo de Python con extensión `.py` es, por definición, un módulo que puede ser importado por otro archivo dentro de la misma ruta.

### Ejemplo de implementación práctica
Imaginemos que estamos desarrollando una herramienta de red y queremos separar la lógica matemática de la lógica de saneamiento de texto.

#### Paso 1: Creamos el módulo de utilidades numéricas (`redes_util.py`):
```python
# redes_util.py
"""Módulo corporativo para operaciones de análisis de red."""

def calcular_subredes(hosts_necesarios):
    """Calcula el número óptimo de subredes necesarias en base a los hosts."""
    if hosts_necesarios <= 0:
        return 0
    return hosts_necesarios * 2
```

#### Paso 2: Importamos y consumimos el módulo propio desde nuestro archivo principal (`main.py`):
```python
# main.py
"""Punto de entrada de la aplicación de red."""
import redes_util

hosts = 120
subredes_disponibles = redes_util.calcular_subredes(hosts)
print(f"Para {hosts} hosts, se han reservado {subredes_disponibles} subredes.")
```

### Variaciones de Importación de Módulos Propios
Al igual que con los módulos estándar, podemos flexibilizar el uso en nuestro archivo principal:
```python
# Importar con un alias corto
import redes_util as net
print(net.calcular_subredes(50))

# Importar la función directamente
from redes_util import calcular_subredes
print(calcular_subredes(30))
```

---

## 2. Paquetes en Python y el Rol del Fichero `__init__.py`

Cuando el número de módulos personalizados crece, no basta con tenerlos sueltos en el directorio raíz. Debemos agruparlos en subcarpetas lógicas especializadas (por ejemplo, una carpeta para la lógica de seguridad, otra para las conexiones a bases de datos, etc.).

Un **paquete** en Python es simplemente una carpeta del sistema de archivos que contiene uno o varios módulos y, de forma recomendada, un archivo especial llamado **`__init__.py`**.

```
mi_herramienta/
│── main.py
│── seguridad/                <-- Esto es un PAQUETE
│   ├── __init__.py           <-- Archivo de inicialización y control de imports
│   ├── firewall.py           <-- Módulo del paquete
│   └── cifrado.py            <-- Módulo del paquete
```

### El papel crucial de `__init__.py`
El archivo `__init__.py` cumple tres funciones esenciales en el desarrollo profesional:
1.  **Identificación del paquete:** Le indica formalmente al intérprete de Python que la carpeta que lo contiene es un paquete importable y no una carpeta de sistema normal.
2.  **Inicialización:** Se ejecuta de forma automática la primera vez que se importa el paquete. Puede utilizarse para inicializar configuraciones o variables globales.
3.  **Control de la API Pública:** Permite ocultar la complejidad interna del paquete, importando internamente las funciones de los módulos del paquete para que el programador que use el paquete pueda llamarlas directamente desde la carpeta raíz.

---

## 3. Carpetas Normales frente a Paquetes (La Evolución de Python)

A partir de **Python 3.3**, gracias a la propuesta **PEP 420 (Namespace Packages)**, ya no es estrictamente obligatorio incluir el archivo `__init__.py` para que una carpeta sea tratada como un paquete importable si se ejecuta en condiciones óptimas.

Sin embargo, **omitir este archivo se considera una mala práctica** en proyectos corporativos y educativos por los siguientes motivos:

| Característica | Carpeta normal sin `__init__.py` (PEP 420) | Paquete Profesional con `__init__.py` (Recomendado) |
| :--- | :--- | :--- |
| **Compatibilidad** | Puede dar problemas en algunos IDEs (como IntelliJ) o según cómo se invoque el script. | ✅ 100% predecible, portátil y compatible en cualquier entorno. |
| **Ejecución de Código** | ❌ Imposible ejecutar código al inicializar o importar la carpeta. | ✅ Permite correr scripts o cargar variables globales automáticamente. |
| **Imports Simplificados** | ❌ El programador debe conocer la estructura interna exacta de subarchivos para importar. | ✅ Permite centralizar los imports para ofrecer un punto de acceso limpio. |
| **Estándar Industrial** | Reservado para casos muy avanzados de distribución de librerías. | ✅ Estándar obligatorio en desarrollo profesional y académico. |

### Ejemplo práctico de simplificación con `__init__.py`

#### Configuración de `firewall.py` dentro de la carpeta `seguridad/`:
```python
# seguridad/firewall.py
def bloquear_ip(ip):
    return f"IP {ip} bloqueada en el cortafuegos."
```

#### Configuración de `__init__.py` dentro de la carpeta `seguridad/`:
```python
# seguridad/__init__.py
# Exponemos de forma directa la función del subarchivo
from .firewall import bloquear_ip
```

#### Llamada limpia desde el script principal (`main.py`):
```python
# main.py
import seguridad

# Invocación directa y limpia a través de la API del paquete
resultado = seguridad.bloquear_ip("195.12.33.4")
print(resultado)
```
Si no usáramos `__init__.py`, el programador principal se vería obligado a escribir `import seguridad.firewall` y llamarlo como `seguridad.firewall.bloquear_ip()`, exponiendo de forma innecesaria la estructura de archivos físicos del disco.

---

## 4. Guía de Organización de Proyectos Complejos

Cuando estructures tus aplicaciones finales en Python, debes seguir una arquitectura modular que separe responsabilidades de forma limpia y mantenible.

### Estructura recomendada para proyectos reales:
```
gestor_incidencias/
│
├── main.py                   # Punto de entrada único del programa (orquestación)
│
├── bases_datos/              # Paquete encargado de la persistencia de datos
│   ├── __init__.py
│   └── lector_logs.py        # Módulo de lectura/escritura de ficheros .log
│
├── seguridad/                # Paquete de control y filtrado de amenazas
│   ├── __init__.py
│   ├── firewall.py           # Módulo de bloqueo de IPs
│   └── cifrado.py            # Módulo para hash y passwords
│
├── utils/                    # Paquete de apoyo genérico
│   ├── __init__.py
│   └── formateadores.py      # Módulo para estéticas de menús e f-strings
│
└── README.md                 # Documentación técnica, dependencias e instalación
```

### Reglas de oro para mantener el orden:
1.  **Punto de entrada unificado (`main.py`):** El archivo principal no debe contener lógica de procesamiento compleja. Su única responsabilidad es importar los módulos correspondientes, pintar el menú interactivo principal (`match-case`) y coordinar las llamadas.
2.  **No usar `sys.path.append()`:** Intentar parchear las rutas de importación de Python añadiendo rutas dinámicas en caliente es un síntoma de mala arquitectura que genera programas frágiles e imposibles de exportar. La solución correcta consiste en estructurar el proyecto en paquetes e importar utilizando rutas relativas internas (`from .modulo import ...`).
3.  **Encapsular las dependencias:** Cada módulo debe resolver una única necesidad del sistema de forma aislada, evitando que dependan críticamente entre sí para facilitar las pruebas y la corrección de errores.
