# Diseño de Base de Datos

## 🧭 1. ¿Qué es el Diseño de Base de Datos?

El diseño de base de datos es el proceso de organizar, estructurar y relacionar los datos para que el sistema sea eficiente, coherente y fácil de mantener.

**Objetivo:**

- Evitar la redundancia de datos, garantizar su integridad, y permitir consultas rápidas y precisas.

## 🔗 1.1 Operaciones de Combinación de Tablas (JOINS)

Las operaciones JOIN permiten combinar datos de varias tablas usando una relación común (normalmente una clave foránea).

### 🔹 Tipos principales de JOIN

| Tipo de JOIN | Qué hace | Ejemplo visual | Explicación |
|--------------|----------|----------------|-------------|
| INNER JOIN | Devuelve solo las filas que coinciden en ambas tablas | 🔸 Intersección | Muestra solo empleados que tienen ventas |
| LEFT JOIN | Devuelve todas las filas de la tabla izquierda y las coincidentes de la derecha | 🔹 Todo de la izquierda | Muestra todos los empleados, tengan o no ventas |
| RIGHT JOIN | Devuelve todas las filas de la tabla derecha y las coincidentes de la izquierda | 🔹 Todo de la derecha | Muestra todas las ventas, incluso si no hay empleado (raro) |
| FULL JOIN | Devuelve todas las filas de ambas tablas | 🔸 Unión total | Muestra todos los empleados y todas las ventas |
| CROSS JOIN | Combina cada fila de una tabla con todas las filas de otra | ⚠️ Producto cartesiano | Poco usado, genera combinaciones masivas |

**Ejemplo básico:**

```sql
SELECT e.nombre, v.subtotal
FROM empleados e
INNER JOIN ventas v ON e.idEmpleado = v.idEmpleado;
```

🧠 Devuelve los empleados que tienen al menos una venta.

## 🔍 2. Consultas Anidadas (Subconsultas)

Una subconsulta es una consulta dentro de otra consulta.
Sirve para usar el resultado de una consulta como filtro, dato o tabla temporal.

### 🧩 2.1 Subconsulta en la cláusula WHERE

Usa el resultado de otra consulta como condición.

**Ejemplo:**

```sql
SELECT nombre
FROM empleados
WHERE sueldo > (SELECT AVG(sueldo) FROM empleados);
```

🧠 Muestra empleados que ganan más que el promedio.

### 🧩 2.2 Subconsulta en la cláusula FROM

Se usa como una tabla temporal.

**Ejemplo:**

```sql
SELECT avg_por_cargo.cargo, avg_por_cargo.promedio
FROM (
  SELECT cargo, AVG(sueldo) AS promedio
  FROM empleados
  GROUP BY cargo
) AS avg_por_cargo;
```

🧠 Crea una tabla temporal con promedios por cargo y luego la consulta.

### 🧩 2.3 Subconsulta en la cláusula SELECT

Devuelve un valor calculado para cada fila.

**Ejemplo:**

```sql
SELECT nombre,
       (SELECT COUNT(*) FROM ventas v WHERE v.idEmpleado = e.idEmpleado) AS total_ventas
FROM empleados e;
```

🧠 Muestra cada empleado con la cantidad de ventas que realizó.

### 🧩 2.4 Subconsulta Correlacionada

La subconsulta depende de la consulta principal.

**Ejemplo:**

```sql
SELECT nombre, sueldo
FROM empleados e
WHERE sueldo > (
    SELECT AVG(sueldo)
    FROM empleados
    WHERE cargo = e.cargo
);
```

🧠 Muestra empleados cuyo sueldo está por encima del promedio de su propio cargo.

### 🧩 2.5 Subconsulta en la cláusula HAVING

Se usa para filtrar grupos.

**Ejemplo:**

```sql
SELECT idEmpleado, SUM(subtotal) AS total_ventas
FROM ventas
GROUP BY idEmpleado
HAVING SUM(subtotal) > (SELECT AVG(subtotal) FROM ventas);
```

🧠 Muestra los empleados cuyas ventas superan el promedio general.

### 🧩 2.6 Ejemplo práctico completo

```sql
SELECT e.nombre, SUM(v.subtotal) AS total_ventas
FROM empleados e
JOIN ventas v ON e.idEmpleado = v.idEmpleado
WHERE e.idEmpleado IN (
    SELECT idEmpleado
    FROM ventas
    WHERE subtotal > 3000
)
GROUP BY e.nombre;
```

🧠 Empleados que tienen al menos una venta mayor a 3000 y su total de ventas.

## ⚙️ 3. Índices en MySQL

### 💡 ¿Qué es un índice?

Un índice es una estructura especial que acelera las búsquedas en una columna.
Funciona como un “índice de libro”: permite encontrar datos más rápido.

**Ejemplo:**

```sql
CREATE INDEX idx_nombre ON empleados(nombre);
```

🧠 Mejora la velocidad al buscar empleados por nombre.

### 🧮 3.1 Creación de índices

```sql
CREATE INDEX idx_cargo ON empleados(cargo);
CREATE UNIQUE INDEX idx_email ON usuarios(email);
```

🔹 UNIQUE garantiza que no haya valores duplicados.
🔹 `DROP INDEX nombre;` sirve para eliminarlo.

⚠️ Ojo: demasiados índices ralentizan las inserciones y actualizaciones, así que deben usarse solo en columnas que se consultan frecuentemente.

## 👁️ 4. Vistas en MySQL

### 💡 ¿Qué es una vista?

Una vista es una consulta guardada con nombre, como si fuera una tabla virtual.
No almacena datos, sino una instrucción SQL.

**Ejemplo:**

```sql
CREATE VIEW vista_ventas_empleados AS
SELECT e.nombre, v.subtotal, v.fechaVenta
FROM empleados e
JOIN ventas v ON e.idEmpleado = v.idEmpleado;
```

### 🧮 4.1 Ventajas de las vistas

- Ocultan la complejidad de consultas grandes.
- Mejoran la seguridad (puedes mostrar solo ciertos datos).
- Facilitan la reutilización de consultas.
- Permiten mantener un modelo más limpio.

### 🧮 4.2 Casos de uso y ejemplos

**Ejemplo: ver solo los empleados que han vendido más de 5000**

```sql
CREATE VIEW mejores_vendedores AS
SELECT e.nombre, SUM(v.subtotal) AS total_ventas
FROM empleados e
JOIN ventas v ON e.idEmpleado = v.idEmpleado
GROUP BY e.nombre
HAVING total_ventas > 5000;
```

🧠 Luego puedes consultarla así:

```sql
SELECT * FROM mejores_vendedores;
```

### 🏗️ 4.3 Crear y eliminar vistas

```sql
CREATE VIEW nombre_vista AS (consulta);
DROP VIEW nombre_vista;
```

## 🧮 5. Consulta Compleja con JOINS y Vistas

**Ejemplo completo:**

```sql
CREATE VIEW resumen_ventas AS
SELECT e.nombre, e.cargo, SUM(v.subtotal) AS total_ventas, AVG(v.subtotal) AS promedio_ventas
FROM empleados e
LEFT JOIN ventas v ON e.idEmpleado = v.idEmpleado
GROUP BY e.nombre, e.cargo;

SELECT * FROM resumen_ventas WHERE total_ventas > 4000;
```

🧠 Esto combina:

- JOIN para relacionar empleados y ventas
- GROUP BY y funciones agregadas (SUM, AVG)
- VIEW para simplificar la consulta
- Filtro final para mostrar solo los mejores resultados

## 📚 Resumen General de Todo

| Tema | Qué hace | Ejemplo clave |
|------|----------|---------------|
| JOIN | Une datos de varias tablas | INNER JOIN, LEFT JOIN |
| Subconsulta (WHERE) | Filtra según otra consulta | `WHERE sueldo > (SELECT AVG(...))` |
| Subconsulta (FROM) | Crea una tabla temporal | `(SELECT ...) AS tabla` |
| Índice (INDEX) | Acelera las búsquedas | `CREATE INDEX idx_nombre ON empleados(nombre)` |
| Vista (VIEW) | Guarda una consulta como tabla virtual | `CREATE VIEW vista AS SELECT ...` |
