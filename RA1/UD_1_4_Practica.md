# Práctica Unidad 1.4: Netflix Smart Profile & Catalog Router

## 🎮 Contexto del Reto

Trabajas en el equipo de ingeniería del Backend de **Netflix**. El sistema de distribución de streaming necesita procesar la información de perfil de un usuario que acaba de iniciar sesión para determinar de forma segura:
1.  Si su perfil tiene restricciones de acceso por edad.
2.  La calidad de reproducción máxima autorizada según su nivel de suscripción.
3.  Una sugerencia personalizada del catálogo para mostrar en su pantalla de inicio.

Tu tarea es diseñar un script en Python que capture los datos del usuario, aplique reglas condicionales lógicas y devuelva un informe estructurado de configuración del perfil.

---

## 🛠️ Requisitos del Script (`b1_5_netflix_router.py`)

El programa debe solicitar tres datos obligatorios al usuario a través de la consola mediante `input()`:
1.  Su **edad** (que debe convertirse a entero `int`).
2.  Su **plan de suscripción** (el usuario puede escribir libremente; por ejemplo, con espacios o letras variadas).
3.  Su **género favorito** de entretenimiento (puede ser: `Acción`, `Comedia`, `Sci-Fi`, `Anime`).

El script debe estructurarse estrictamente bajo las siguientes especificaciones:

### Paso 1: Normalización de Datos
Para evitar fallos de coincidencia causados por espacios adicionales o discrepancias entre mayúsculas y minúsculas:
*   El **plan de suscripción** debe ser limpiado de espacios extremos y convertido completamente a **minúsculas** (ej: `"  Premium  "` debe transformarse en `"premium"`).
*   El **género favorito** debe ser limpiado de espacios extremos y convertido de manera que **la primera letra sea mayúscula** (ej: `"  sci-fi  "` debe transformarse en `"Sci-Fi"`, `"acción"` en `"Acción"`, etc. *Pista: usa `.strip().capitalize()` o manipulación de cadenas*).

### Paso 2: Validación de Acceso y Resolución de Pantalla (Uso de `if...elif...else`)
Debes implementar una estructura condicional que evalúe la edad y el plan del usuario bajo estas directrices:
*   **Restricción de Edad:** Si el usuario tiene **menos de 16 años** y su género favorito normalizado es **"Acción"**, el acceso a este género debe ser catalogado como **BLOQUEADO** por control parental. Se le informará con un mensaje advirtiendo de la restricción y se modificará automáticamente su sugerencia a **"Comedia"** de forma preventiva. En cualquier otro caso, el estado del control parental será **PERMITIDO**.
*   **Calidad de Streaming:** Determina la calidad máxima del flujo de video según el plan del usuario:
    *   Si el plan normalizado es `"premium"`, la calidad autorizada es **"Ultra HD (4K)"**.
    *   Si el plan normalizado es `"estándar"`, la calidad autorizada es **"Full HD (1080p)"**.
    *   Si el plan normalizado es `"básico"`, la calidad autorizada es **"HD (720p)"**.
    *   Si escribe cualquier otro texto en el plan, se le asignará por defecto la calidad de visualización **"SD (480p)"** y se mostrará un mensaje de advertencia indicando que el plan es "Desconocido/Invitado".

### Paso 3: Enrutador de Catálogo Personalizado (Uso obligatorio de `match-case`)
Utilizando de forma obligatoria la sentencia moderna `match-case` sobre el género favorito (que ya ha sido validado y potencialmente reasignado por el control de edad en el Paso 2), asigna una sugerencia de serie o película destacada del catálogo real de Netflix:
*   Si el género es `"Sci-Fi"` o `"Ciencia ficción"`: Sugerir **"Stranger Things"**.
*   Si el género es `"Anime"` o `"Manga"`: Sugerir **"One Piece"**.
*   Si el género es `"Comedia"` o `"Humor"`: Sugerir **"Wednesday"** (Miércoles).
*   Si el género es `"Acción"`: Sugerir **"Cobra Kai"**.
*   Para cualquier otra entrada, el caso comodín/defecto (`_`) debe sugerir un éxito global de la plataforma: **"La Casa de Papel"**.

---

## 🖥️ Ejemplo de Salida Esperada en Consola

### Caso A: Acceso Exitoso con Plan Premium
```
=== BIENVENIDO AL CONFIGURADOR DE PERFIL DE NETFLIX ===
Introduce tu edad: 20
Introduce tu plan (Básico / Estándar / Premium):   pReMiUm  
Introduce tu género favorito (Acción / Comedia / Sci-Fi / Anime): acción

[CONFIGURACIÓN DE PERFIL EXITOSA]
- Control Parental: ACCESO PERMITIDO (Apto para mayores de 16)
- Calidad de Video Autorizada: Ultra HD (4K)
- Sugerencia Personalizada de Portada: "Cobra Kai" (Género: Acción)
```

### Caso B: Activación de Control Parental por Edad
```
=== BIENVENIDO AL CONFIGURADOR DE PERFIL DE NETFLIX ===
Introduce tu edad: 14
Introduce tu plan (Básico / Estándar / Premium): ESTANDAR
Introduce tu género favorito (Acción / Comedia / Sci-Fi / Anime): acción

[ADVERTENCIA] Acceso al género Acción denegado para menores de 16 años. 
Se ha reajustado automáticamente tu recomendación de catálogo.

[CONFIGURACIÓN DE PERFIL EXITOSA]
- Control Parental: RESTRICCIÓN ACTIVA (Acceso redirigido a Comedia)
- Calidad de Video Autorizada: Full HD (1080p)
- Sugerencia Personalizada de Portada: "Wednesday" (Género: Comedia)
```

---

## 📐 Requisitos de Entrega
1.  El código debe guardarse en un único archivo llamado exactamente `b1_5_netflix_router.py`.
2.  Debe cumplir estrictamente con las normas de estilo **PEP 8** (nombres de variables en `snake_case`, constantes en `UPPERCASE` si las hubiera, uso correcto de espacios alrededor de operadores).
3.  El código debe incluir un **Docstring de triple comilla** al inicio del script que describa el propósito del programa, el autor y la fecha, además de comentarios limpios que expliquen los pasos principales.
