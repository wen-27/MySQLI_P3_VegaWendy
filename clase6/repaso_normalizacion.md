# Normalización en Bases de Datos

## 🌱 ¿Qué es la normalización?

La normalización es un proceso que se aplica al diseño de bases de datos para:

- Eliminar datos duplicados (redundancia)
- Evitar inconsistencias
- Asegurar la integridad de los datos
- Optimizar el almacenamiento y las consultas

Se hace dividiendo una tabla grande en varias más pequeñas y relacionadas.

## 🧩 1. Primera Forma Normal (1FN)

### Regla:
Cada campo debe tener un solo valor atómico (no listas, no varios valores en una misma celda). Además, cada fila debe ser única (con una clave primaria).

###  Ejemplo sin normalizar:
| id_cliente | nombre | teléfonos |
|------------|--------|-----------|
| 1 | Ana | 3101111111, 3112222222 |

 Aquí el campo teléfonos tiene dos valores →  no cumple 1FN.

### ✅ Normalizado (1FN):
| id_cliente | nombre | teléfono |
|------------|--------|----------|
| 1 | Ana | 3101111111 |
| 1 | Ana | 3112222222 |

Cada campo tiene un solo valor → ✔️ cumple 1FN.

## 🧱 2. Segunda Forma Normal (2FN)

### Regla:
Debe cumplir 1FN, y todos los atributos no clave deben depender de toda la clave primaria, no solo de una parte.

⚠️ Esto aplica solo si la clave primaria es compuesta (formada por varias columnas).

### ❌ Ejemplo sin 2FN:
| id_pedido | id_producto | nombre_producto | cantidad |
|-----------|-------------|-----------------|----------|
| 1 | 10 | Teclado | 2 |

La clave primaria podría ser (id_pedido, id_producto). Pero nombre_producto solo depende de id_producto, no de toda la clave → ❌ no cumple 2FN.

### ✅ Normalizado (2FN):

**Tabla pedidos_productos**

| id_pedido | id_producto | cantidad |
|-----------|-------------|----------|
| 1 | 10 | 2 |

**Tabla productos**

| id_producto | nombre_producto |
|-------------|-----------------|
| 10 | Teclado |

Ahora cada dato depende completamente de su clave primaria → ✔️ cumple 2FN.

## 🧠 3. Tercera Forma Normal (3FN)

### Regla:
Debe cumplir 2FN, y no debe haber dependencia transitiva. 👉 Es decir, ninguna columna no clave debe depender de otra columna no clave.

### ❌ Ejemplo sin 3FN:
| id_empleado | nombre | id_departamento | nombre_departamento |
|-------------|--------|-----------------|---------------------|
| 1 | Laura | 2 | Ventas |

Aquí nombre_departamento depende de id_departamento, y id_departamento depende de id_empleado → dependencia transitiva → ❌ no cumple 3FN.

### ✅ Normalizado (3FN):

**Tabla empleados**

| id_empleado | nombre | id_departamento |
|-------------|--------|-----------------|
| 1 | Laura | 2 |

**Tabla departamentos**

| id_departamento | nombre_departamento |
|-----------------|---------------------|
| 2 | Ventas |

Ahora cada dato depende solo de su clave primaria directa → ✔️ cumple 3FN.

## 🧭 Resumen General

| Forma Normal | Qué elimina | Reglas principales |
|--------------|-------------|-------------------|
| 1FN | Valores repetidos o no atómicos | Cada campo debe tener un solo valor |
| 2FN | Dependencias parciales | Los campos dependen de toda la clave primaria |
| 3FN | Dependencias transitivas | Ningún campo no clave depende de otro no clave |
