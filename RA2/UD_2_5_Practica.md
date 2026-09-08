# Actividad Práctica - Unidad 2.5: Firewall IP Threat Intelligence & Blacklist Manager

En el ámbito de la ciberseguridad, los firewalls y los sistemas de prevención de intrusos (IPS) gestionan de forma dinámica listas de direcciones IP sospechosas o maliciosas para bloquear conexiones no autorizadas. 

En esta actividad, desarrollarás un prototipo de consola para un gestor de inteligencia de amenazas de red. El programa permitirá administrar una lista de direcciones IP bloqueadas (blacklist), depurar falsos positivos, ordenar el registro y simular la comprobación de paquetes de red entrantes.

---

## 🎯 Objetivos de la Actividad
* Aplicar el concepto de **listas mutables** y el uso práctico de sus índices.
* Implementar y dominar los métodos esenciales de manipulación de listas (`append`, `remove`, `sort`, `copy`).
* Utilizar operadores de pertenencia (`in` / `not in`) como mecanismo de control de seguridad.
* Modularizar el diseño del programa utilizando funciones parametrizadas que colaboren entre sí.

---

## 🧱 Requisitos de la Aplicación

El script se llamará obligatoriamente **`b2_5_firewall_manager.py`**. Debe estructurarse mediante las siguientes funciones:

### 1. `inicializar_blacklist()`
* **Propósito**: Retorna una lista inicial con 5 direcciones IP simuladas de atacantes ya identificados en la red.
* **Valores iniciales requeridos**: `"192.168.1.50"`, `"10.0.0.99"`, `"8.8.4.4"`, `"172.16.25.10"`, `"195.235.12.3"`.

### 2. `registrar_amenaza(blacklist, ip)`
* **Propósito**: Añade una nueva IP sospechosa a la lista de bloqueos.
* **Comportamiento**: 
  1. Debe limpiar la dirección IP de espacios en blanco usando `.strip()`.
  2. Debe verificar si la IP ya se encuentra en la lista utilizando el operador `in`.
  3. Si la IP ya estaba bloqueada, mostrará un aviso en consola indicando: `[!] Advertencia: La IP {ip} ya está en la blacklist.`
  4. Si no estaba, la añadirá al final de la lista usando `.append()` y confirmará: `[+] IP {ip} bloqueada con éxito.`

### 3. `eliminar_falso_positivo(blacklist, ip)`
* **Propósito**: Desbloquear una IP que ha sido clasificada como amenaza por error.
* **Comportamiento**:
  1. Limpiar la IP de espacios con `.strip()`.
  2. Verificar si la IP existe en la lista.
  3. Si existe, eliminarla usando el método `.remove()` y notificar: `[-] IP {ip} retirada de la blacklist.`
  4. Si no existe, notificar: `[?] Error: La IP {ip} no se encuentra registrada.`

### 4. `comprobar_trafico(blacklist, ip_origen)`
* **Propósito**: Simular un paquete entrante al servidor y decidir si se le permite el acceso.
* **Comportamiento**:
  1. Limpiar la dirección IP.
  2. Si la IP está en la `blacklist`, mostrar el mensaje: `[ACCESO DENEGADO ⛔] Conexión bloqueada desde {ip_origen}.`
  3. Si no está en la lista, mostrar: `[ACCESO CONCEDIDO ✅] Paquete aceptado desde {ip_origen}.`

### 5. `exportar_reporte(blacklist)`
* **Propósito**: Mostrar un reporte limpio y ordenado de la blacklist actual sin alterar la lista de trabajo en memoria.
* **Comportamiento**:
  1. Crear una **copia física independiente** de la blacklist usando `.copy()`.
  2. Ordenar alfabéticamente la copia utilizando el método `.sort()`.
  3. Imprimir el listado ordenado en consola con un f-string detallando su índice.

---

## 🚀 Flujo del Programa Principal

El programa principal deberá integrar estas funciones mediante un menú interactivo continuo `while True`:

```
=== FIREWALL THREAT INTELLIGENCE ===
1. Ver Blacklist ordenada (Reporte)
2. Registrar nueva amenaza (IP)
3. Retirar falso positivo (Desbloquear)
4. Simular tráfico entrante (Auditoría)
5. Salir
```

### Restricciones Técnicas
1. **PEP 8 Estricto**: Nombres de variables y funciones en `snake_case`, constantes en `UPPERCASE` si las hubiera.
2. **Docstrings Obligatorios**: Cada función debe incluir su docstring descriptivo con comillas triples `"""`.
3. **Saneamiento**: Todas las entradas por teclado deben sanearse mediante `.strip()` para evitar fallos por espacios accidentales.

---

## 📝 Ejemplo de Salida Esperada

```
=== FIREWALL THREAT INTELLIGENCE ===
1. Ver Blacklist ordenada (Reporte)
2. Registrar nueva amenaza (IP)
3. Retirar falso positivo (Desbloquear)
4. Simular tráfico entrante (Auditoría)
5. Salir
Elige una opción: 1

--- REPORTES DE FIREWALL (IPs Bloqueadas) ---
[1] - 10.0.0.99
[2] - 172.16.25.10
[3] - 192.168.1.50
[4] - 195.235.12.3
[5] - 8.8.4.4
Total amenazas activas: 5

=== FIREWALL THREAT INTELLIGENCE ===
...
Elige una opción: 2
Introduce la IP maliciosa a bloquear:  192.168.1.100  
[+] IP 192.168.1.100 bloqueada con éxito.

=== FIREWALL THREAT INTELLIGENCE ===
...
Elige una opción: 4
Introduce la IP de origen del paquete: 10.0.0.99
[ACCESO DENEGADO ⛔] Conexión bloqueada desde 10.0.0.99.
```
