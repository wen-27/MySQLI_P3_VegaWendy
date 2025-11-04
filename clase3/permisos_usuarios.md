# Permisos y Usuarios en MySQL

## Crear Usuario
- ```sql
    CREATE USER 'nombre_usuario'@'localhost' IDENTIFIED BY 'contraseña';
    ```
- Si cambias el `'localhost'` por el símbolo `'%'`, el usuario podrá conectarse desde cualquier dirección IP.

## Cambiar Contraseña de Usuario
- ```sql
    ALTER USER 'nombre_usuario'@'host' IDENTIFIED BY 'nueva_contraseña';
    ```
## Tipos de Permisos
| Permiso          | Qué permite hacer |
|------------------|-------------------|
| ALL PRIVILEGES  | Todos los permisos posibles sobre una base de datos o tabla. |
| SELECT          | Leer datos (consultas con SELECT). |
| INSERT          | Agregar nuevos registros (INSERT INTO). |
| UPDATE          | Modificar registros existentes (UPDATE). |
| DELETE          | Eliminar registros (DELETE). |
| CREATE          | Crear nuevas tablas o bases de datos. |
| DROP            | Borrar tablas o bases de datos. |
| ALTER           | Modificar la estructura de una tabla (añadir o cambiar columnas). |
| INDEX           | Crear o eliminar índices. |
| GRANT OPTION    | Permite al usuario dar permisos a otros. (Solo se usa con usuarios de nivel administrador). |

## Dar Permisos
- ```sql
    GRANT SELECT ON libreria.* TO 'wendy'@'%';
    ````
- (no olvides el `;`)

## Revocar Permisos
- ```sql
    REVOKE SELECT ON libreria.* FROM 'wendy'@'%';
    ```
## Ver Permisos
- ```sql
    SHOW GRANTS FOR 'wendy'@'%';
    ```

