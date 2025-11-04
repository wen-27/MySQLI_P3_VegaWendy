# Consultas Básicas en MySQL

## Insertar Datos (INSERT)
```sql
INSERT INTO libros (titulo, autor, precio, stock) VALUES ('El Principito', 'Antoine de Saint-Exupéry', 45.50, 20);
```

## Actualizar Datos (UPDATE)
```sql
UPDATE libros SET precio = 55.00 WHERE titulo = 'Cien años de soledad';
```
**Nota:** El `WHERE` es muy importante porque indica qué registro vas a cambiar. Si no lo pones, MySQL cambiará todos los registros de la tabla.

## Leer Datos de las Tablas (SELECT)
```sql
SELECT titulo, precio FROM libros;
```
```sql
SELECT * FROM libros WHERE stock > 10;
```

## Eliminar Datos (DELETE)
```sql
DELETE FROM libros
WHERE titulo = 'Cien años de soledad';
```

## Consultas con AND
Mostrar libros del autor Gabriel García Márquez que cuesten menos de 60:
```sql
SELECT titulo, autor, precio
FROM libros
WHERE autor = 'Gabriel García Márquez' AND precio < 60;
```

## Consultas con OR
Mostrar libros que cuesten menos de 40 o tengan más de 50 unidades en stock:
```sql
SELECT titulo, precio, stock
FROM libros
WHERE precio < 40 OR stock > 50;
```

## Coincidencias con LIKE
LIKE se usa para buscar palabras o partes de texto. El carácter `%` significa "cualquier cosa antes o después".
```sql
SELECT titulo, autor
FROM libros
WHERE titulo LIKE '%soledad%';
```

## Funciones de Agregación
Estas funciones realizan cálculos sobre un conjunto de filas y devuelven un único valor. Se usan típicamente con `GROUP BY` (aunque no siempre es necesario).

| Función  | Descripción                          | Ejemplo       |
|----------|--------------------------------------|---------------|
| COUNT()  | Cuenta cuántas filas hay            | COUNT(*)      |
| SUM()    | Suma los valores de una columna numérica | SUM(precio)   |
| AVG()    | Calcula el promedio                  | AVG(precio)   |
| MAX()    | Devuelve el valor máximo             | MAX(precio)   |
| MIN()    | Devuelve el valor mínimo             | MIN(precio)   |

Ejemplo de uso:
```sql
SELECT COUNT(*) AS total_libros
FROM libros;
```
cuando son varios parametros seria asi:

```sql 
SELECT 
MAX(precio) AS precio_maximo,
MIN(precio) AS precio_minimo
FROM libros;
````
## Agrupar resultados con GROUP BY
se usa para agrupar filas que tienen valores iguales en una o más columnas.
Luego puedes aplicar una función de agregación (como COUNT(), AVG(), MAX(), MIN(), SUM()) a cada grupo.

```sql 
SELECT autor, AVG(precio) AS precio_promedio
FROM libros
GROUP BY autor;
```
