# Actividad Práctica 1.7: Social Media Post Sanitizer & Style Validator

## 📝 Contexto del Reto

En las agencias de marketing digital y desarrollo de plataformas como **X (Twitter)**, **Instagram** o **LinkedIn**, los gestores de contenido utilizan herramientas automatizadas de saneamiento de texto antes de publicar un *feed*. 

Tu tarea como desarrollador de backend es diseñar un script en Python que procese un borrador de texto crudo de una publicación. Este script debe limpiar espacios accidentales, validar la longitud límite de la red social, catalogar de forma automatizada los *hashtags* presentes e imprimir un informe visual.

Como el foco de esta unidad son las **Buenas Prácticas, el Estilo PEP 8 y la Documentación (PEP 257)**, el evaluador pondrá especial atención en la limpieza del código, los nombres de variables, el uso correcto de comentarios, constantes y la existencia de una documentación estructurada mediante **Docstrings**.

---

## 🎯 Objetivos de la Actividad
*   Aplicar de forma real el estándar de nomenclatura **PEP 8** (`snake_case` para variables y funciones, `UPPERCASE` para constantes).
*   Documentar el código fuente utilizando un **Docstring multilínea** formal (`"""`) bajo el estándar **PEP 257** al inicio de tu script.
*   Sanear datos de entrada de texto utilizando manipulación de cadenas y estructuras de bucles (`while`/`for`) aprendidos en unidades anteriores.
*   Estructurar el código de manera que sea limpio, fácil de leer y libre de redundancias.

---

## 📥 Datos de Entrada (Borrador Crudo)

Tu programa debe solicitar por teclado la cadena de texto del post a procesar. Para realizar las pruebas, puedes utilizar el siguiente borrador de ejemplo que simula un texto mal formateado por un redactor:

```text
  NUEVO LANZAMIENTO:   Ya está disponible la nueva actualización del juego del año.   ¿Estás listo para el combate?   #gaming #brawlstars #rpg   
```

---

## ⚙️ Requisitos Funcionales del Programa

El script debe realizar secuencialmente las siguientes operaciones:

1.  **Saneamiento de Extremos:** Eliminar los espacios en blanco sobrantes al principio y al final de la cadena de entrada.
2.  **Saneamiento de Dobles Espacios:** En el cuerpo del texto suele haber dobles o triples espacios accidentales (como `lanzamiento:   Ya`). Debes limpiar todos los espacios duplicados reduciéndolos a un solo espacio entre palabras.
    *   *Pista técnica:* Puedes resolver esto de forma limpia utilizando un bucle `while` que reemplace pares de espacios `"  "` por un espacio simple `" "` de forma reiterativa mientras sigan existiendo en el texto.
3.  **Validación de Longitud:** El programa debe comprobar si el post saneado excede el límite máximo permitido de **280 caracteres** (guardado obligatoriamente en una constante según PEP 8).
    *   Debe calcular y mostrar cuántos caracteres reales mide el post saneado y cuántos caracteres le quedan disponibles para llegar al límite (o por cuántos se ha pasado en caso de excederlo).
4.  **Buscador y Contador de Hashtags:** El sistema debe identificar cuántos *hashtags* (palabras que inician con `#`) se han incluido en la publicación.
    *   Para ello, recorre la cadena de texto utilizando un bucle `for` y cuenta de forma manual cuántas veces aparece el carácter `#`.
5.  **Reporte Final en Pantalla:** Imprime un reporte formateado que resuma el estado del post (Texto saneado, longitud final, estado de aprobación "APROBADO" o "EXCEDE LÍMITE DE CARACTERES", y el número total de hashtags encontrados).

---

## 📐 Requisitos Estrictos de Estilo (PEP 8 y PEP 257)

Para que tu actividad sea calificada como **Apto**, el código debe respetar escrupulosamente los siguientes criterios estéticos:

*   **Docstring del Archivo:** Al inicio del archivo `.py` debe existir un Docstring cerrado entre triples comillas (`"""`) que describa con claridad:
    *   El propósito del script.
    *   El autor del código.
    *   Las instrucciones breves de ejecución.
*   **Nomenclatura PEP 8:**
    *   Todas las variables locales deben usar `snake_case` (minúsculas unidas por guion bajo, ej. `texto_saneado`, `limite_superado`).
    *   La longitud máxima permitida (280) debe guardarse en una constante en mayúsculas (ej. `MAX_CARACTERES`).
*   **Comentarios pertinentes:** Añade comentarios con `#` en las partes más complejas del código, asegurando que aportan valor real y no repiten lo que ya hace la instrucción de forma evidente.
*   **Espaciado Consistente:** Evita apelmazar los operadores aritméticos o de comparación. Coloca espacios alrededor de `=`, `+`, `<`, `==`, etc.
*   **Línea Corta:** Asegúrate de que ninguna línea de código de tu script supere los 79 caracteres de ancho.
