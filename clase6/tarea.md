# hacer este reto de consultas de la base de datos de ambientalistas 

## nivel 1
-- 1 id y nombre de los 10 primeros parques por orden alfabético
```sql
SELECT id, nombre 
FROM parque_natural 
ORDER BY nombre ASC 
LIMIT 10;
```
-- 2 id, region y departamentos de la tabla departamento
```sql
SELECT id, region, departamentos 
FROM departamento;
```

-- 3 nvehículos cuyo tipo sea exactamente '4x4'
```sql
SELECT * 
FROM vehiculo 
WHERE tipo = '4x4';
```

-- 4 alojamientos con capacidad >= 4, ordenados por precio_x_noche descendente
```sql
SELECT * 
FROM alojamiento 
WHERE capacidad >= 4 
ORDER BY precio_x_noche DESC;
```

-- 5 parques declarados a partir del 2015-01-01
```sql
SELECT id, nombre, dia_declarado 
FROM parque_natural 
WHERE dia_declarado >= '2015-01-01';
```

-- 6 los 5 visitantes con cedula más alta
```sql
SELECT id, nombre, cedula 
FROM visitante 
ORDER BY cedula DESC 
LIMIT 5;
```

-- 7 áreas con extensión entre 15 y 25
```sql
SELECT id, nombre, extension 
FROM area2 
WHERE extension BETWEEN 15 AND 25;
```
-- 8 personal con sueldo entre 1’800.000 y 2’200.000
```sql
SELECT id, nombre, sueldo 
FROM personal_parque 
WHERE sueldo BETWEEN 1800000 AND 2200000;
```
-- 9 visitantes cuya profesión sea 'Ingeniero' o 'Docente'
```sql
SELECT * 
FROM visitante 
WHERE profesion IN ('Ingeniero', 'Docente');
```

-- 10 alojamiento con el mayor precio por noche
```sql
SELECT id, categoria, precio_x_noche 
FROM alojamiento 
ORDER BY precio_x_noche DESC 
LIMIT 1;
```


## nivel2

-- 1 id y nombre de los 10 primeros parques por orden alfabético
```sql
SELECT id, nombre 
FROM parque_natural 
ORDER BY nombre ASC 
LIMIT 10;
```

-- 2 id, region y departamentos de la tabla departamento
```sql
SELECT id, region, departamentos 
FROM departamento;
```

-- 3 vehículos cuyo tipo sea exactamente '4x4'
```sql
SELECT * 
FROM vehiculo 
WHERE tipo = '4x4';
```

-- 4 alojamientos con capacidad >= 4, ordenados por precio_x_noche descendente
```sql
SELECT * 
FROM alojamiento 
WHERE capacidad >= 4 
ORDER BY precio_x_noche DESC;
```

-- 5 parques declarados a partir del 2015-01-01
```sql
SELECT id, nombre, dia_declarado 
FROM parque_natural 
WHERE dia_declarado >= '2015-01-01';
```

-- 6 los 5 visitantes con cedula más alta
```sql
SELECT id, nombre, cedula 
FROM visitante 
ORDER BY cedula DESC 
LIMIT 5;
```

-- 7 áreas con extensión entre 15 y 25
```sql
SELECT id, nombre, extension 
FROM area2 
WHERE extension BETWEEN 15 AND 25;
```

-- 8 personal con sueldo entre 1’800.000 y 2’200.000
```sql
SELECT id, nombre, sueldo 
FROM personal_parque 
WHERE sueldo BETWEEN 1800000 AND 2200000;
```

-- 9 visitantes cuya profesión sea 'Ingeniero' o 'Docente'
```sql
SELECT * 
FROM visitante 
WHERE profesion IN ('Ingeniero', 'Docente');
```

-- 10 alojamiento con el mayor precio por noche
```sql
SELECT id, categoria, precio_x_noche 
FROM alojamiento 
ORDER BY precio_x_noche DESC 
LIMIT 1;
```

## nivel 3
-- 1 Visitantes con categoría de alojamiento y nombre del parque
```sql
SELECT v.nombre AS visitante, 
       a.categoria AS categoria_alojamiento, 
       p.nombre AS parque
FROM visitante v
INNER JOIN alojamiento a ON v.alojamiento_id = a.id
INNER JOIN parque_natural p ON a.parque_natural_id = p.id;
```

-- 2 Entradas con el nombre del parque
```sql
SELECT e.nombre AS entrada, e.ubicacion, p.nombre AS parque
FROM entrada e
INNER JOIN parque_natural p ON e.id_parque = p.id;
```

-- 3 Personal de vigilancia con empleado, área y vehículo
```sql
SELECT pp.nombre AS empleado, 
       a.nombre AS area, 
       v.marca AS vehiculo
FROM personal_de_vigilancia pv
INNER JOIN personal_parque pp ON pv.personal_parque_id = pp.id
INNER JOIN area2 a ON pv.area2_id = a.id
INNER JOIN vehiculo v ON pv.vehiculo_id = v.id;
```

-- 4 Especies con su investigador (funciones) y parque
```sql
SELECT e.denominacion_cientifica AS especie, 
       pi.funciones AS investigador, 
       p.nombre AS parque
FROM especie e
INNER JOIN personal_investigador pi ON e.personal_investigador_id = pi.id
INNER JOIN area2 a ON e.area2_id = a.id
INNER JOIN parque_natural p ON a.parque_id = p.id;
```

-- 5 Entidades y sus departamentos (LEFT JOIN)
```sql
SELECT en.nombre AS entidad, 
       d.departamentos AS departamento
FROM entidad en
LEFT JOIN departamento d ON en.departamento_id = d.id;
```

-- 6 Personal investigador con título y fecha
```sql
SELECT pi.id AS id_investigador, 
       pi.funciones, 
       t.titulo, 
       t.fecha
FROM personal_investigador pi
INNER JOIN titulacion t ON pi.titulacion_id = t.id;
```