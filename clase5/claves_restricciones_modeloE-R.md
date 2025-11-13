# Claves, Restricciones y Modelo Entidad-Relación

## 🔑 1. Claves en Bases de Datos

### 💡 ¿Qué son las claves?

Las claves son columnas o combinaciones de columnas que identifican de forma única los registros en una tabla o establecen relaciones entre ellas.
👉 Sin claves, no podríamos distinguir ni conectar los datos correctamente.

### 🟩 1.1 Clave Primaria (PRIMARY KEY)

Es el identificador único de cada fila de una tabla.

- No puede repetirse ni ser nula.
- Cada tabla solo puede tener una clave primaria.

**Ejemplo:**

```sql
CREATE TABLE empleados (
  idEmpleado INT PRIMARY KEY,
  nombre VARCHAR(100)
);
```

🧠 Significado: idEmpleado identifica de forma única a cada empleado.

### 🟦 1.2 Clave Foránea (FOREIGN KEY)

Establece una relación entre tablas.

- Enlaza una tabla hija con la tabla padre.
- El valor en la clave foránea debe existir en la tabla referenciada.

**Ejemplo:**

```sql
CREATE TABLE ventas (
  idVenta INT PRIMARY KEY,
  idEmpleado INT,
  FOREIGN KEY (idEmpleado) REFERENCES empleados(idEmpleado)
);
```

🧠 Significado: Cada venta pertenece a un empleado existente.

## ⚙️ 2. Restricciones (Constraints)

Las restricciones controlan qué valores pueden insertarse en una tabla.
Sirven para proteger la integridad de los datos.

### 🧱 2.1 UNIQUE

Evita valores duplicados en una columna.

- Se puede aplicar a una o más columnas.

**Ejemplo:**

```sql
CREATE TABLE usuarios (
  correo VARCHAR(100) UNIQUE
);
```

🧠 Ningún usuario puede tener el mismo correo.

### 🌐 2.2 DEFAULT

Asigna un valor predeterminado si no se proporciona uno.

**Ejemplo:**

```sql
CREATE TABLE empleados (
  cargo VARCHAR(50) DEFAULT 'Sin asignar'
);
```

🧠 Si no se indica el cargo, se guarda “Sin asignar”.

### 🧩 2.3 CHECK

Define una condición lógica que los datos deben cumplir.

**Ejemplo:**

```sql
CREATE TABLE productos (
  precio DECIMAL(10,2) CHECK (precio > 0)
);
```

🧠 No se permitirá registrar precios negativos o cero.

### 🚫 2.4 NOT NULL

Obliga a que una columna tenga siempre un valor (no puede quedar vacía).

**Ejemplo:**

```sql
CREATE TABLE clientes (
  nombre VARCHAR(100) NOT NULL
);
```

🧠 No se puede crear un cliente sin nombre.

### 🔢 2.5 AUTO_INCREMENT

Genera automáticamente un número consecutivo.

- Se usa casi siempre con la clave primaria.

**Ejemplo:**

```sql
CREATE TABLE empleados (
  idEmpleado INT AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100)
);
```

🧠 Cada nuevo registro obtiene un idEmpleado único (1, 2, 3…).

## 🧭 3. Modelo Entidad–Relación (E–R)

El modelo E–R es una forma visual de representar una base de datos antes de crearla en SQL.
Permite entender las entidades (tablas), sus atributos (columnas) y sus relaciones.

### 🧍 3.1 Principales componentes del modelo E-R

| Elemento | Significado | Ejemplo |
|----------|-------------|---------|
| Entidad | Objeto o persona del mundo real | Empleado, Cliente |
| Atributo | Característica de la entidad | nombre, edad |
| Relación | Vínculo entre entidades | Empleado realiza Venta |

### 🧾 3.2 Notación gráfica

En el diagrama E–R, se usa:

- 📦 Rectángulos: entidades
- 🔹 Elipses: atributos
- 🔺 Rombos: relaciones
- 🔗 Líneas: conectan entidades y relaciones
- 🔢 Cardinalidad: 1 a 1, 1 a muchos, muchos a muchos

**Ejemplo visual (texto):**

```
Empleado (1) —— realiza ——< (N) Venta
```

### 🔍 3.3 ¿Cómo crear un diagrama E-R?

1. Identificar las entidades principales.
2. Definir sus atributos y elegir la clave primaria.
3. Establecer las relaciones entre entidades.
4. Determinar la cardinalidad (1:1, 1:N, N:N).
5. Representarlo con un software (como Draw.io, MySQL Workbench, o dbdiagram.io).

### 🧠 3.4 Caso para análisis

Por ejemplo, para una empresa de ventas:

**Entidades:** Empleado, Cliente, Venta, Producto

**Relaciones:**

- Un Empleado realiza muchas Ventas
- Un Cliente puede tener muchas Ventas
- Una Venta incluye muchos Productos

👉 Esto luego se transforma en tablas SQL relacionadas con claves primarias y foráneas.

## 📚 Resumen General

| Tema | Qué hace | Ejemplo clave |
|------|----------|---------------|
| PRIMARY KEY | Identifica filas únicas | `idEmpleado INT PRIMARY KEY` |
| FOREIGN KEY | Conecta tablas | `REFERENCES empleados(idEmpleado)` |
| UNIQUE | Evita duplicados | `correo UNIQUE` |
| DEFAULT | Valor automático | `cargo DEFAULT 'Sin asignar'` |
| CHECK | Valida datos | `CHECK (precio > 0)` |
| NOT NULL | Evita valores vacíos | `nombre NOT NULL` |
| AUTO_INCREMENT | Genera IDs automáticos | `id AUTO_INCREMENT` |
| E–R Model | Representa el diseño conceptual | Entidades, atributos y relaciones |
