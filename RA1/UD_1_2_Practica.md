# Actividad Práctica 1.2: Video Game Character Parser

## 📖 Contexto del Proyecto
En el desarrollo de videojuegos de rol (RPG), los servidores de red suelen enviar los datos de los personajes de manera compacta en cadenas de texto de ancho fijo para ahorrar ancho de banda. 

Tu tarea consiste en diseñar un script en Python que reciba una trama de texto crudo de red, extraiga la información del héroe utilizando técnicas de corte de cadenas (slicing) y realice los cálculos de combate necesarios sin utilizar estructuras condicionales (`if`) ni bucles (`for`/`while`), dado que son herramientas que aún no se han explicado en clase.

---

## 🎯 El Reto
Debes procesar la siguiente trama de red que representa el estado de un personaje:
```python
trama_red = "ARAGORN   -0850-1.20-1"
```

La trama de red mide exactamente **22 caracteres** y se divide de la siguiente manera:
1. **Nombre del Personaje:** Caracteres del índice `0` al `10` (ancho fijo, relleno de espacios en blanco).
2. **Separador:** El carácter en el índice `10` es un guion `-`.
3. **Ataque Base:** Caracteres del índice `11` al `15` (un número entero representado con 4 dígitos).
4. **Separador:** El carácter en el índice `15` es un guion `-`.
5. **Multiplicador Crítico:** Caracteres del índice `16` al `20` (un número decimal con formato `X.XX`).
6. **Separador:** El carácter en el índice `20` es un guion `-`.
7. **Potenciador Activo:** El carácter en el índice `21` (un indicador binario `"1"` si tiene un buff de fuerza, o `"0"` si no lo tiene).

---

## 🛠️ Requisitos del Programa

Escribe un script en Python llamado `b1_2_character_parser.py` que realice paso a paso las siguientes acciones:

1. **Definir la trama:** Declara la variable `trama_red` con el valor indicado arriba.
2. **Aplicar Slicing:** Extrae de forma manual cada una de las cuatro secciones de datos (nombre, ataque, crítico y potenciador) utilizando rangos de índices exactos.
3. **Limpieza de textos:** Utiliza el método `.strip()` sobre el nombre extraído para eliminar todos los espacios en blanco innecesarios de los lados.
4. **Conversión de tipos (Casting):**
   * Convierte la subcadena del ataque base a tipo entero (`int`).
   * Convierte la subcadena del multiplicador crítico a tipo decimal (`float`).
   * Convierte el carácter del potenciador activo a booleano (`bool`).
     * *Pista de seguridad:* Si haces `bool("0")`, el resultado será `True` porque `"0"` es un texto no vacío. Para convertir correctamente el carácter `"1"` o `"0"` a un booleano real, convierte primero ese carácter a un entero (`int`) y luego ese entero a booleano (`bool`).
5. **Cálculo del daño de combate:**
   * Calcula el **Daño Crítico** multiplicando el ataque base por el multiplicador crítico.
   * Supongamos que si el potenciador está activo, el daño de combate se incrementa. Para esta práctica, calcularemos el daño básico total simplemente multiplicando el ataque base por el multiplicador crítico. (No apliques condicionales para el potenciador; resolveremos cómo integrarlo en la defensa presencial).
6. **Mostrar Reporte:** Imprime un informe de personaje en consola utilizando **f-strings** con un formato limpio y elegante de tabla:

```
========================================
       REPORTE DE PERSONAJE RPG         
========================================
Nombre del Héroe:  [Nombre limpio]
Ataque Base:       [Ataque] pts
Multiplicador Crí: [Crítico]x
Potenciador Activ: [True / False]
----------------------------------------
DAÑO CRÍTICO MAX:  [Daño calculado] pts
========================================
```

---

## 🚫 Restricciones Estrictas
* Está **totalmente prohibido** usar la sentencia `if`, `elif` o `else`.
* Está **totalmente prohibido** usar bucles `for` o `while`.
* Está **totalmente prohibido** usar expresiones regulares (`re`) o el método `.split()` de las cadenas. El objetivo es resolver la extracción utilizando exclusivamente la técnica de **Slicing**.
