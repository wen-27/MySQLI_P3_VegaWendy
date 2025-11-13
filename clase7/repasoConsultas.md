# Repaso de Consultas SQL

## 🧱 1. Consultas Básicas — Para ver y filtrar datos

### 🔹 SELECT
Sirve para leer datos de una tabla. Es la consulta más usada de todas.

```sql
SELECT * FROM empleados;
```
➡️ Muestra todos los empleados con todos sus campos.

### 🔹 WHERE
Filtra registros que cumplan una condición.

```sql
SELECT * FROM empleados WHERE sueldo > 3000;
```
➡️ Solo muestra empleados con sueldo mayor a 3000.

### 🔹 ORDER BY
Ordena los resultados (ASC = ascendente, DESC = descendente).

```sql
SELECT nombre, sueldo FROM empleados ORDER BY sueldo DESC;
```
➡️ Ordena de mayor a menor sueldo.

### 🔹 LIMIT
Sirve para mostrar solo una cantidad de registros.

```sql
SELECT * FROM empleados LIMIT 3;
```
➡️ Muestra solo los 3 primeros empleados.

## 🧮 2. Funciones de Agregado — Para hacer cálculos
Estas funciones hacen operaciones sobre columnas numéricas o conjuntos de filas.

| Función | Qué hace | Ejemplo |
|---------|----------|---------|
| AVG() | Calcula el promedio | `SELECT AVG(sueldo) FROM empleados;` |
| SUM() | Suma todos los valores | `SELECT SUM(sueldo) FROM empleados;` |
| MAX() | Devuelve el mayor valor | `SELECT MAX(sueldo) FROM empleados;` |
| MIN() | Devuelve el menor valor | `SELECT MIN(sueldo) FROM empleados;` |
| COUNT() | Cuenta cuántas filas hay | `SELECT COUNT(*) FROM empleados;` |

➡️ Sirven para resúmenes o reportes (totales, promedios, conteos, etc.)

## 🧩 3. GROUP BY — Para agrupar resultados
Agrupa filas que tienen el mismo valor en una o más columnas. Ejemplo: agrupar empleados por cargo.

```sql
SELECT cargo, AVG(sueldo) AS promedio
FROM empleados
GROUP BY cargo;
```
➡️ Muestra un promedio de sueldos por cada tipo de cargo.

## 🧱 4. HAVING — Para filtrar grupos (no filas)
HAVING se usa después de un GROUP BY para filtrar los grupos.

```sql
SELECT cargo, AVG(sueldo) AS promedio
FROM empleados
GROUP BY cargo
HAVING AVG(sueldo) > 3000;
```
➡️ Muestra solo los cargos cuyo sueldo promedio es mayor a 3000.

## 🔗 5. JOIN — Para combinar información de varias tablas
Las bases de datos reales tienen muchas tablas relacionadas. Los JOIN sirven para cruzar datos entre ellas.

### 🔸 INNER JOIN
Solo muestra registros que existen en ambas tablas.

```sql
SELECT e.nombre, v.subtotal
FROM empleados e
INNER JOIN ventas v ON e.idEmpleado = v.idEmpleado;
```
➡️ Muestra empleados que tienen ventas.

### 🔸 LEFT JOIN
Muestra todos los empleados, aunque no tengan ventas.

```sql
SELECT e.nombre, v.subtotal
FROM empleados e
LEFT JOIN ventas v ON e.idEmpleado = v.idEmpleado;
```
➡️ Los empleados sin ventas aparecerán con NULL en subtotal.

### 🔸 RIGHT JOIN
Lo contrario: todos los registros de la derecha (ventas), aunque no haya empleado.

## 🧮 6. Subconsultas — Consultas dentro de otras
Sirven para usar el resultado de una consulta dentro de otra.

```sql
SELECT nombre, sueldo
FROM empleados
WHERE sueldo > (SELECT AVG(sueldo) FROM empleados);
```
➡️ Muestra empleados que ganan más que el promedio general.

## 👁️ 7. VIEW — Consultas guardadas como si fueran tablas
Una vista (VIEW) es como una “consulta guardada” que puedes reutilizar.

```sql
CREATE OR REPLACE VIEW vw_promedio_sueldos AS
SELECT AVG(sueldo) AS promedio FROM empleados;

SELECT * FROM vw_promedio_sueldos;
```
➡️ Puedes consultar la vista como si fuera una tabla normal.

## ✍️ 8. INSERT, UPDATE y DELETE — Para modificar datos

### 🔹 INSERT
Agrega nuevos registros a una tabla.

```sql
INSERT INTO empleados (nombre, cargo, sueldo, fechaIngreso)
VALUES ('Ana Pérez', 'Tester', 2800.00, '2024-03-01');
```

### 🔹 UPDATE
Modifica datos existentes.

```sql
UPDATE empleados
SET sueldo = sueldo * 1.10
WHERE cargo = 'Analista';
```

### 🔹 DELETE
Elimina registros.

```sql
DELETE FROM empleados WHERE idEmpleado = 3;
```

## 🧠 9. CASE — Condicionales dentro de consultas
Permite aplicar lógica tipo “si… entonces…” directamente en SQL.

```sql
SELECT nombre,
       cargo,
       CASE
           WHEN sueldo < 3000 THEN 'Bajo'
           WHEN sueldo BETWEEN 3000 AND 4000 THEN 'Medio'
           ELSE 'Alto'
       END AS NivelSalarial
FROM empleados;
```
➡️ Clasifica empleados según su sueldo.

## ⚙️ 10. DML vs DDL (importante saber la diferencia)

| Tipo | Significa | Qué hace | Ejemplos |
|------|-----------|----------|----------|
| DDL | Data Definition Language | Define estructuras de la base | CREATE, ALTER, DROP |
| DML | Data Manipulation Language | Maneja los datos | SELECT, INSERT, UPDATE, DELETE |

## 🚀 Resumen Visual (para recordar)

| Objetivo | Consulta Principal | Palabras Clave |
|----------|-------------------|----------------|
| Ver datos | SELECT | WHERE, ORDER BY, LIMIT |
| Calcular valores | AVG, SUM, COUNT, etc. | GROUP BY, HAVING |
| Combinar tablas | JOIN | ON |
| Guardar consultas | VIEW | CREATE VIEW |
| Insertar | INSERT INTO | VALUES |
| Actualizar | UPDATE | SET, WHERE |
| Eliminar | DELETE FROM | WHERE |
| Condicionales | CASE | WHEN, THEN, ELSE |
| Agrupar lógicamente | GROUP BY | HAVING |
