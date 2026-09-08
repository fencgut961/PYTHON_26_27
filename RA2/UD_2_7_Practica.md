# Unidad 2.7: Actividad Práctica - SOC Incident Ticket Manager (CRUD)

En los centros de operaciones de ciberseguridad (SOC), la rapidez para categorizar, registrar, actualizar y mitigar incidentes es crucial para la supervivencia de la infraestructura corporativa. 

En esta actividad práctica, asumirás el rol de desarrollador de herramientas internas para un equipo de Respuesta a Incidentes (Incident Response). Tu misión es programar una base de datos local en memoria que funcione como un sistema interactivo de **Gestión de Tickets de Incidentes de Ciberseguridad (CRUD)** utilizando diccionarios estructurados y condicionales modernos.

---

## 🎯 Objetivos de Aprendizaje
* Aplicar el concepto de **diccionarios asociativos** clave-valor como bases de datos en memoria.
* Implementar operaciones CRUD completas (Crear, Leer, Actualizar y Eliminar) sobre diccionarios anidados.
* Utilizar el método seguro `.get()` para prevenir interrupciones accidentales de ejecución.
* Modularizar el diseño del programa separando responsabilidades en funciones limpias parametrizadas.
* Diseñar un menú de consola intuitivo y robusto utilizando la sentencia `match-case`.

---

## 🧱 Estructura de Datos Base

El programa trabajará sobre un diccionario global llamado `tickets_soc`.
* La **clave** principal será un identificador de ticket con formato de texto (ej. `"INC-001"`, `"INC-002"`).
* El **valor** asociado a cada clave será otro diccionario (anidado) que almacenará de manera estructurada los siguientes datos técnicos:
  * `"host"` (Dirección IP o nombre de la máquina afectada, string).
  * `"severidad"` (Nivel de riesgo: `"Bajo"`, `"Medio"`, `"Alto"`, string).
  * `"estado"` (Fase de mitigación: `"Abierto"`, `"Mitigando"`, `"Cerrado"`, string).
  * `"analista"` (Nombre del profesional asignado al incidente, string).

Estructura de inicialización recomendada para pruebas iniciales:
```python
tickets_soc = {
    "INC-001": {
        "host": "192.168.10.50",
        "severidad": "Alto",
        "estado": "Abierto",
        "analista": "Marta Gómez"
    },
    "INC-002": {
        "host": "Servidor-AD-01",
        "severidad": "Medio",
        "estado": "Mitigando",
        "analista": "Carlos Ruiz"
    }
}
```

---

## 🛠️ Requisitos Funcionales (CRUD)

Debes programar un script en Python llamado `b2_7_gestor_incidentes.py` que implemente las siguientes funciones con sus correspondientes Docstrings explicativos:

### 1. Registrar Incidente (`Create`)
* **Función**: `registrar_incidente(id_ticket, host, severidad, analista)`
* **Comportamiento**: 
  * Debe verificar de forma previa si el `id_ticket` ya existe en el diccionario.
  * Si existe, mostrar un aviso informativo en consola (`"❌ Error: El ticket ya existe."`) y cancelar el registro para evitar la sobrescritura accidental de evidencias.
  * Si no existe, crearlo con el estado inicial predeterminado en `"Abierto"`. Saneará el host eliminando los espacios innecesarios antes de insertarlo.

### 2. Mostrar Incidentes (`Read`)
* **Función**: `mostrar_incidentes()`
* **Comportamiento**:
  * Si el diccionario global está vacío, mostrar en pantalla: `"⚠️ No hay incidentes registrados en el SOC."`.
  * Si contiene datos, recorrer el diccionario con un bucle utilizando `.items()` para desempaquetar la clave y sus detalles, imprimiendo un reporte limpio y formateado con f-strings.

### 3. Actualizar Estado (`Update`)
* **Función**: `actualizar_estado(id_ticket, nuevo_estado)`
* **Comportamiento**:
  * Debe comprobar de manera segura si el ticket existe.
  * Si existe, actualizará el valor de la clave `"estado"` al valor indicado (normalizando a `"Abierto"`, `"Mitigando"` o `"Cerrado"`).
  * Si no existe, imprimirá un mensaje controlado sin romper la ejecución.

### 4. Eliminar Incidente (`Delete`)
* **Función**: `eliminar_incidente(id_ticket)`
* **Comportamiento**:
  * Solicitará la confirmación de eliminación de un ticket.
  * Si el ticket existe, utilizará el método `.pop()` para extraer y eliminar el incidente de la base de datos, mostrando en consola un mensaje de confirmación que indique qué máquina y qué analista se han desvinculado del sistema de alertas.
  * Si no existe, informará del fallo de manera amigable.

---

## 🖥️ Menú del Sistema e Interacción
El programa principal debe ejecutar un bucle interactivo continuo `while True` que limpie las opciones por pantalla y permita la navegación mediante una estructura `match-case`:

```
=== MENÚ DE CONTROL DE INCIDENTES - SOC ===
1. Registrar nuevo incidente
2. Mostrar catálogo de incidentes
3. Modificar estado de un incidente
4. Eliminar incidente de base de datos
5. Salir del sistema
```

---

## 📖 Normas de Estilo (PEP 8)
* El código debe estructurarse de manera limpia, sin amontonamiento de líneas.
* Se requiere un **Docstring explicativo multilínea** al inicio del script que describa el propósito global del programa y docstrings unilíneos o multilíneos en cada función.
* No utilizar variables globales directamente dentro de las funciones de forma incontrolada, pasar las estructuras como parámetros si es necesario, o documentar adecuadamente su acceso.
* Toda salida en pantalla y entrada de teclado debe estar saneada con métodos de limpieza de texto (`.strip()`, `.capitalize()`).
