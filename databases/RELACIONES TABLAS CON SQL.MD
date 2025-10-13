Para crear relaciones **1:N** y otras en SQL, es importante entender el concepto de claves primarias (**PRIMARY KEY**) y claves foráneas (**FOREIGN KEY**) dentro de las tablas. A continuación, te explico cómo hacerlo paso a paso para una relación 1:N y otras (1:1 y N:M).

---

## Relación 1:N en SQL

En una relación 1:N, una fila de una tabla puede estar relacionada con múltiples filas de otra tabla. Por ejemplo: una tabla `Clientes` puede estar relacionada con varias filas en una tabla `Pedidos`.

### Paso 1: Crear las tablas con PRIMARY KEY
Define las tablas, asegurándote de que una tiene la clave primaria que será referenciada como clave foránea en la otra.

```sql
CREATE TABLE Clientes (
    id_cliente INT PRIMARY KEY,
    nombre VARCHAR(100),
    email VARCHAR(100)
);

CREATE TABLE Pedidos (
    id_pedido INT PRIMARY KEY,
    id_cliente INT,
    fecha DATE,
    total DECIMAL(10, 2),
    FOREIGN KEY (id_cliente) REFERENCES Clientes(id_cliente)
);
```

En este ejemplo:
- `id_cliente` en `Clientes` es la clave primaria.
- `id_cliente` en `Pedidos` es la clave foránea, que conecta con la clave primaria en `Clientes`.

---

## Relación 1:1 en SQL

En una relación 1:1, una fila en una tabla está asociada exactamente con una fila en otra tabla. Por ejemplo: una tabla `Usuarios` y una tabla `Perfiles`.

### Paso 1: Crear las tablas con FOREIGN KEY UNIQUE
Una de las claves foráneas debe ser única para garantizar la relación 1:1.

```sql
CREATE TABLE Usuarios (
    id_usuario INT PRIMARY KEY,
    nombre VARCHAR(100)
);

CREATE TABLE Perfiles (
    id_perfil INT PRIMARY KEY,
    id_usuario INT UNIQUE,
    bio TEXT,
    FOREIGN KEY (id_usuario) REFERENCES Usuarios(id_usuario)
);
```

En este caso:
- `id_usuario` es único en `Perfiles`, asegurando que cada usuario solo puede tener un perfil.

---

## Relación N:M en SQL

En una relación N:M, muchas filas de una tabla están relacionadas con muchas filas de otra tabla. Por ejemplo: una tabla `Estudiantes` y una tabla `Cursos`.

### Paso 1: Crear una tabla intermedia
Se utiliza una tabla intermedia para conectar ambas tablas.

```sql
CREATE TABLE Estudiantes (
    id_estudiante INT PRIMARY KEY,
    nombre VARCHAR(100)
);

CREATE TABLE Cursos (
    id_curso INT PRIMARY KEY,
    titulo VARCHAR(100)
);

CREATE TABLE Estudiantes_Cursos (
    id_estudiante INT,
    id_curso INT,
    PRIMARY KEY (id_estudiante, id_curso),
    FOREIGN KEY (id_estudiante) REFERENCES Estudiantes(id_estudiante),
    FOREIGN KEY (id_curso) REFERENCES Cursos(id_curso)
);
```

En este ejemplo:
- `Estudiantes_Cursos` es la tabla intermedia que contiene claves foráneas de ambas tablas (`Estudiantes` y `Cursos`).
- La combinación de `id_estudiante` y `id_curso` forma una clave primaria compuesta.

---

## Resumen

1. **Relación 1:N**: Agrega una clave foránea en la tabla secundaria que referencia la clave primaria de la tabla principal.
2. **Relación 1:1**: Asegúrate de que la clave foránea tenga una restricción `UNIQUE`.
3. **Relación N:M**: Usa una tabla intermedia con claves foráneas a ambas tablas principales y una clave primaria compuesta.
