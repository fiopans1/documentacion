# 🗄️ Guía Avanzada de SQL - MySQL y PostgreSQL

## 📋 Índice

1. [Introducción](#1-introducción)
2. [Instalación y Configuración](#2-instalación-y-configuración)
3. [Conceptos Fundamentales](#3-conceptos-fundamentales)
4. [Tipos de Datos](#4-tipos-de-datos)
5. [Consultas Básicas Avanzadas](#5-consultas-básicas-avanzadas)
6. [Joins Avanzados](#6-joins-avanzados)
7. [Subconsultas](#7-subconsultas)
8. [Funciones de Agregación y GROUP BY](#8-funciones-de-agregación-y-group-by)
9. [Window Functions](#9-window-functions)
10. [CTEs (Common Table Expressions)](#10-ctes-common-table-expressions)
11. [Índices y Optimización](#11-índices-y-optimización)
12. [Transacciones](#12-transacciones)
13. [Procedimientos Almacenados](#13-procedimientos-almacenados)
14. [Triggers](#14-triggers)
15. [Vistas](#15-vistas)
16. [Particionamiento](#16-particionamiento)
17. [Replicación y Alta Disponibilidad](#17-replicación-y-alta-disponibilidad)
18. [Backup y Restauración](#18-backup-y-restauración)
19. [Seguridad y Permisos](#19-seguridad-y-permisos)
20. [Diferencias MySQL vs PostgreSQL](#20-diferencias-mysql-vs-postgresql)

---

## 1. Introducción

### ¿Qué es MySQL?

MySQL es un sistema de gestión de bases de datos relacional de código abierto, adquirido por Oracle. Es conocido por su velocidad, confiabilidad y facilidad de uso.

**Características:**
- Código abierto
- Alto rendimiento
- Ampliamente usado en aplicaciones web
- Compatible con múltiples plataformas

### ¿Qué es PostgreSQL?

PostgreSQL es un sistema de bases de datos objeto-relacional de código abierto con más de 35 años de desarrollo activo. Es conocido por su cumplimiento de estándares SQL y características avanzadas.

**Características:**
- Cumplimiento estricto de estándares SQL
- Soporte para tipos de datos avanzados (JSON, XML, arrays)
- Extensible y programable
- Excelente para consultas complejas

---

## 2. Instalación y Configuración

### Instalación de MySQL

#### Linux (Ubuntu/Debian):
```bash
sudo apt update
sudo apt install mysql-server
sudo mysql_secure_installation
```

#### Windows:
Descargar desde [mysql.com](https://dev.mysql.com/downloads/installer/)

#### Docker:
```bash
docker run --name mysql-server \
  -e MYSQL_ROOT_PASSWORD=mi_password \
  -p 3306:3306 \
  -d mysql:8.0
```

### Instalación de PostgreSQL

#### Linux (Ubuntu/Debian):
```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
```

#### Windows:
Descargar desde [postgresql.org](https://www.postgresql.org/download/windows/)

#### Docker:
```bash
docker run --name postgres-server \
  -e POSTGRES_PASSWORD=mi_password \
  -p 5432:5432 \
  -d postgres:15
```

### Conectarse a las bases de datos

#### MySQL:
```bash
mysql -u root -p
mysql -h localhost -u usuario -p nombre_bd
```

#### PostgreSQL:
```bash
psql -U postgres
psql -h localhost -U usuario -d nombre_bd
```

---

## 3. Conceptos Fundamentales

### Crear base de datos

```sql
-- MySQL
CREATE DATABASE tienda CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

-- PostgreSQL
CREATE DATABASE tienda ENCODING 'UTF8' LC_COLLATE='es_ES.UTF-8';
```

### Usar base de datos

```sql
-- MySQL
USE tienda;

-- PostgreSQL (en psql)
\c tienda
```

### Crear tablas

```sql
CREATE TABLE clientes (
    id INT AUTO_INCREMENT PRIMARY KEY,  -- MySQL
    -- id SERIAL PRIMARY KEY,            -- PostgreSQL
    nombre VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    fecha_registro TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    activo BOOLEAN DEFAULT TRUE
);

CREATE TABLE productos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(200) NOT NULL,
    descripcion TEXT,
    precio DECIMAL(10, 2) NOT NULL,
    stock INT DEFAULT 0,
    categoria VARCHAR(50),
    fecha_creacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE pedidos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    cliente_id INT NOT NULL,
    fecha_pedido TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    total DECIMAL(10, 2) NOT NULL,
    estado VARCHAR(20) DEFAULT 'pendiente',
    FOREIGN KEY (cliente_id) REFERENCES clientes(id) ON DELETE CASCADE
);

CREATE TABLE detalle_pedidos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    pedido_id INT NOT NULL,
    producto_id INT NOT NULL,
    cantidad INT NOT NULL,
    precio_unitario DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (pedido_id) REFERENCES pedidos(id) ON DELETE CASCADE,
    FOREIGN KEY (producto_id) REFERENCES productos(id)
);
```

---

## 4. Tipos de Datos

### Tipos de datos en MySQL

```sql
-- Numéricos
TINYINT, SMALLINT, MEDIUMINT, INT, BIGINT
DECIMAL(10,2), FLOAT, DOUBLE

-- Cadenas
CHAR(n), VARCHAR(n)
TEXT, MEDIUMTEXT, LONGTEXT
ENUM('valor1', 'valor2')

-- Fecha y Hora
DATE, TIME, DATETIME, TIMESTAMP
YEAR

-- Binarios
BLOB, MEDIUMBLOB, LONGBLOB

-- JSON (MySQL 5.7+)
JSON
```

### Tipos de datos en PostgreSQL

```sql
-- Numéricos
SMALLINT, INTEGER, BIGINT
DECIMAL, NUMERIC
REAL, DOUBLE PRECISION
SERIAL, BIGSERIAL

-- Cadenas
CHAR(n), VARCHAR(n)
TEXT

-- Fecha y Hora
DATE, TIME, TIMESTAMP
INTERVAL

-- Booleano
BOOLEAN

-- JSON
JSON, JSONB

-- Arrays
INTEGER[], TEXT[]

-- Geométricos
POINT, LINE, POLYGON

-- UUID
UUID

-- Rangos
INT4RANGE, TSRANGE
```

---

## 5. Consultas Básicas Avanzadas

### SELECT con múltiples condiciones

```sql
-- Operadores lógicos
SELECT * FROM productos 
WHERE precio > 100 
  AND stock > 0 
  AND categoria = 'Electrónica';

-- IN
SELECT * FROM clientes 
WHERE id IN (1, 5, 10, 15);

-- BETWEEN
SELECT * FROM productos 
WHERE precio BETWEEN 50 AND 200;

-- LIKE
SELECT * FROM clientes 
WHERE nombre LIKE 'Mar%';  -- Comienza con Mar
WHERE email LIKE '%@gmail.com';  -- Termina con @gmail.com
WHERE nombre LIKE '%an%';  -- Contiene 'an'

-- IS NULL / IS NOT NULL
SELECT * FROM productos 
WHERE descripcion IS NOT NULL;

-- CASE
SELECT 
    nombre,
    precio,
    CASE 
        WHEN precio < 50 THEN 'Económico'
        WHEN precio < 200 THEN 'Medio'
        ELSE 'Premium'
    END AS categoria_precio
FROM productos;
```

### ORDER BY y LIMIT

```sql
-- Ordenar
SELECT * FROM productos 
ORDER BY precio DESC, nombre ASC;

-- Limitar resultados
SELECT * FROM productos 
ORDER BY precio DESC 
LIMIT 10;

-- Paginación (MySQL)
SELECT * FROM productos 
LIMIT 10 OFFSET 20;  -- Página 3 (20-30)

-- Paginación (PostgreSQL también soporta)
SELECT * FROM productos 
LIMIT 10 OFFSET 20;
```

### DISTINCT

```sql
-- Valores únicos
SELECT DISTINCT categoria FROM productos;

-- Contar valores únicos
SELECT COUNT(DISTINCT categoria) FROM productos;
```

---

## 6. Joins Avanzados

### INNER JOIN

```sql
-- Pedidos con información del cliente
SELECT 
    p.id AS pedido_id,
    c.nombre AS cliente,
    p.fecha_pedido,
    p.total
FROM pedidos p
INNER JOIN clientes c ON p.cliente_id = c.id;
```

### LEFT JOIN

```sql
-- Todos los clientes y sus pedidos (incluso sin pedidos)
SELECT 
    c.nombre,
    c.email,
    COUNT(p.id) AS total_pedidos,
    COALESCE(SUM(p.total), 0) AS total_gastado
FROM clientes c
LEFT JOIN pedidos p ON c.cliente_id = p.id
GROUP BY c.id, c.nombre, c.email;
```

### RIGHT JOIN

```sql
-- Todos los pedidos y sus clientes
SELECT 
    p.id,
    p.total,
    c.nombre
FROM clientes c
RIGHT JOIN pedidos p ON c.id = p.cliente_id;
```

### FULL OUTER JOIN

```sql
-- PostgreSQL
SELECT 
    c.nombre AS cliente,
    p.id AS pedido_id
FROM clientes c
FULL OUTER JOIN pedidos p ON c.id = p.cliente_id;

-- MySQL (simulado con UNION)
SELECT c.nombre, p.id
FROM clientes c
LEFT JOIN pedidos p ON c.id = p.cliente_id
UNION
SELECT c.nombre, p.id
FROM clientes c
RIGHT JOIN pedidos p ON c.id = p.cliente_id;
```

### CROSS JOIN

```sql
-- Combinación de todos los registros
SELECT 
    c.nombre,
    p.nombre AS producto
FROM clientes c
CROSS JOIN productos p;
```

### SELF JOIN

```sql
-- Ejemplo: empleados y sus supervisores
CREATE TABLE empleados (
    id INT PRIMARY KEY,
    nombre VARCHAR(100),
    supervisor_id INT,
    FOREIGN KEY (supervisor_id) REFERENCES empleados(id)
);

SELECT 
    e.nombre AS empleado,
    s.nombre AS supervisor
FROM empleados e
LEFT JOIN empleados s ON e.supervisor_id = s.id;
```

### Multiple Joins

```sql
-- Pedidos con detalles completos
SELECT 
    p.id AS pedido_id,
    c.nombre AS cliente,
    pr.nombre AS producto,
    dp.cantidad,
    dp.precio_unitario,
    (dp.cantidad * dp.precio_unitario) AS subtotal
FROM pedidos p
INNER JOIN clientes c ON p.cliente_id = c.id
INNER JOIN detalle_pedidos dp ON p.id = dp.pedido_id
INNER JOIN productos pr ON dp.producto_id = pr.id
WHERE p.estado = 'completado'
ORDER BY p.id, pr.nombre;
```

---

## 7. Subconsultas

### Subconsulta en WHERE

```sql
-- Clientes que han hecho pedidos
SELECT nombre, email
FROM clientes
WHERE id IN (
    SELECT DISTINCT cliente_id 
    FROM pedidos
);

-- Productos más caros que el promedio
SELECT nombre, precio
FROM productos
WHERE precio > (
    SELECT AVG(precio) 
    FROM productos
);
```

### Subconsulta en FROM

```sql
-- Resumen de ventas por categoría
SELECT 
    categoria,
    promedio_precio,
    total_productos
FROM (
    SELECT 
        categoria,
        AVG(precio) AS promedio_precio,
        COUNT(*) AS total_productos
    FROM productos
    GROUP BY categoria
) AS resumen
WHERE total_productos > 5;
```

### Subconsulta correlacionada

```sql
-- Empleados que ganan más que el promedio de su departamento
SELECT 
    e.nombre,
    e.salario,
    e.departamento_id
FROM empleados e
WHERE e.salario > (
    SELECT AVG(e2.salario)
    FROM empleados e2
    WHERE e2.departamento_id = e.departamento_id
);
```

### EXISTS

```sql
-- Clientes con al menos un pedido
SELECT c.nombre
FROM clientes c
WHERE EXISTS (
    SELECT 1 
    FROM pedidos p 
    WHERE p.cliente_id = c.id
);
```

---

## 8. Funciones de Agregación y GROUP BY

### Funciones básicas

```sql
SELECT 
    COUNT(*) AS total_productos,
    COUNT(DISTINCT categoria) AS total_categorias,
    SUM(stock) AS stock_total,
    AVG(precio) AS precio_promedio,
    MIN(precio) AS precio_minimo,
    MAX(precio) AS precio_maximo
FROM productos;
```

### GROUP BY

```sql
-- Ventas por categoría
SELECT 
    categoria,
    COUNT(*) AS cantidad_productos,
    AVG(precio) AS precio_promedio,
    SUM(stock) AS stock_total
FROM productos
GROUP BY categoria
ORDER BY precio_promedio DESC;
```

### HAVING

```sql
-- Categorías con más de 10 productos
SELECT 
    categoria,
    COUNT(*) AS cantidad
FROM productos
GROUP BY categoria
HAVING COUNT(*) > 10;

-- Clientes con gastos superiores a 1000
SELECT 
    c.nombre,
    SUM(p.total) AS total_gastado
FROM clientes c
INNER JOIN pedidos p ON c.id = p.cliente_id
GROUP BY c.id, c.nombre
HAVING SUM(p.total) > 1000
ORDER BY total_gastado DESC;
```

### GROUP BY con ROLLUP (MySQL)

```sql
-- Subtotales y total general
SELECT 
    categoria,
    COUNT(*) AS cantidad,
    SUM(precio) AS total
FROM productos
GROUP BY categoria WITH ROLLUP;
```

### GROUPING SETS (PostgreSQL)

```sql
-- Múltiples agrupaciones en una consulta
SELECT 
    categoria,
    fecha_creacion::DATE,
    COUNT(*) AS cantidad
FROM productos
GROUP BY GROUPING SETS (
    (categoria),
    (fecha_creacion::DATE),
    ()
);
```

---

## 9. Window Functions

### ROW_NUMBER, RANK, DENSE_RANK

```sql
-- Ranking de productos por precio
SELECT 
    nombre,
    precio,
    categoria,
    ROW_NUMBER() OVER (ORDER BY precio DESC) AS row_num,
    RANK() OVER (ORDER BY precio DESC) AS rank,
    DENSE_RANK() OVER (ORDER BY precio DESC) AS dense_rank
FROM productos;

-- Ranking por categoría
SELECT 
    nombre,
    precio,
    categoria,
    ROW_NUMBER() OVER (PARTITION BY categoria ORDER BY precio DESC) AS rank_categoria
FROM productos;
```

### LAG y LEAD

```sql
-- Comparar con valores anteriores y siguientes
SELECT 
    fecha_pedido,
    total,
    LAG(total) OVER (ORDER BY fecha_pedido) AS pedido_anterior,
    LEAD(total) OVER (ORDER BY fecha_pedido) AS pedido_siguiente,
    total - LAG(total) OVER (ORDER BY fecha_pedido) AS diferencia
FROM pedidos
ORDER BY fecha_pedido;
```

### Funciones de agregación como ventana

```sql
-- Suma acumulativa
SELECT 
    fecha_pedido,
    total,
    SUM(total) OVER (ORDER BY fecha_pedido) AS total_acumulado,
    AVG(total) OVER (ORDER BY fecha_pedido ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) AS promedio_movil_7dias
FROM pedidos
ORDER BY fecha_pedido;
```

### NTILE

```sql
-- Dividir en cuartiles
SELECT 
    nombre,
    precio,
    NTILE(4) OVER (ORDER BY precio) AS cuartil
FROM productos;
```

---

## 10. CTEs (Common Table Expressions)

### CTE básico

```sql
-- PostgreSQL y MySQL 8.0+
WITH ventas_cliente AS (
    SELECT 
        cliente_id,
        COUNT(*) AS total_pedidos,
        SUM(total) AS total_gastado
    FROM pedidos
    GROUP BY cliente_id
)
SELECT 
    c.nombre,
    c.email,
    vc.total_pedidos,
    vc.total_gastado
FROM clientes c
INNER JOIN ventas_cliente vc ON c.id = vc.cliente_id
WHERE vc.total_gastado > 500
ORDER BY vc.total_gastado DESC;
```

### CTEs múltiples

```sql
WITH 
productos_populares AS (
    SELECT 
        producto_id,
        SUM(cantidad) AS total_vendido
    FROM detalle_pedidos
    GROUP BY producto_id
    HAVING SUM(cantidad) > 100
),
categorias_top AS (
    SELECT 
        p.categoria,
        COUNT(*) AS cantidad_productos
    FROM productos p
    INNER JOIN productos_populares pp ON p.id = pp.producto_id
    GROUP BY p.categoria
)
SELECT * FROM categorias_top
ORDER BY cantidad_productos DESC;
```

### CTE recursivo

```sql
-- Generar serie de números
WITH RECURSIVE numeros AS (
    SELECT 1 AS n
    UNION ALL
    SELECT n + 1 
    FROM numeros 
    WHERE n < 10
)
SELECT * FROM numeros;

-- Jerarquía de empleados
WITH RECURSIVE jerarquia AS (
    -- Caso base: empleados sin supervisor
    SELECT 
        id,
        nombre,
        supervisor_id,
        1 AS nivel,
        nombre AS ruta
    FROM empleados
    WHERE supervisor_id IS NULL
    
    UNION ALL
    
    -- Caso recursivo
    SELECT 
        e.id,
        e.nombre,
        e.supervisor_id,
        j.nivel + 1,
        j.ruta || ' -> ' || e.nombre
    FROM empleados e
    INNER JOIN jerarquia j ON e.supervisor_id = j.id
)
SELECT * FROM jerarquia
ORDER BY nivel, nombre;
```

---

## 11. Índices y Optimización

### Crear índices

```sql
-- Índice simple
CREATE INDEX idx_productos_precio ON productos(precio);

-- Índice compuesto
CREATE INDEX idx_pedidos_cliente_fecha ON pedidos(cliente_id, fecha_pedido);

-- Índice único
CREATE UNIQUE INDEX idx_clientes_email ON clientes(email);

-- Índice de texto completo (MySQL)
CREATE FULLTEXT INDEX idx_productos_descripcion ON productos(descripcion);

-- Índice GIN para JSON (PostgreSQL)
CREATE INDEX idx_datos_json ON tabla USING GIN (columna_json);
```

### Ver índices

```sql
-- MySQL
SHOW INDEXES FROM productos;

-- PostgreSQL
\d productos
SELECT * FROM pg_indexes WHERE tablename = 'productos';
```

### Eliminar índices

```sql
DROP INDEX idx_productos_precio ON productos;  -- MySQL
DROP INDEX idx_productos_precio;  -- PostgreSQL
```

### EXPLAIN para análisis de consultas

```sql
-- Ver plan de ejecución
EXPLAIN SELECT * FROM pedidos WHERE cliente_id = 5;

-- Con detalles (MySQL)
EXPLAIN ANALYZE SELECT * FROM pedidos WHERE cliente_id = 5;

-- Con detalles (PostgreSQL)
EXPLAIN (ANALYZE, BUFFERS) SELECT * FROM pedidos WHERE cliente_id = 5;
```

### Optimización de consultas

```sql
-- Evitar SELECT *
-- Mal
SELECT * FROM productos;

-- Bien
SELECT id, nombre, precio FROM productos;

-- Usar LIMIT cuando sea apropiado
SELECT * FROM productos ORDER BY precio DESC LIMIT 10;

-- Evitar funciones en WHERE
-- Mal
SELECT * FROM pedidos WHERE YEAR(fecha_pedido) = 2024;

-- Bien
SELECT * FROM pedidos 
WHERE fecha_pedido >= '2024-01-01' 
  AND fecha_pedido < '2025-01-01';
```

---

## 12. Transacciones

### Transacciones básicas

```sql
-- Iniciar transacción
START TRANSACTION;  -- o BEGIN;

-- Realizar operaciones
UPDATE productos SET stock = stock - 1 WHERE id = 5;
INSERT INTO pedidos (cliente_id, total) VALUES (1, 99.99);

-- Confirmar cambios
COMMIT;

-- O revertir cambios
ROLLBACK;
```

### Niveles de aislamiento

```sql
-- Ver nivel actual
SELECT @@transaction_isolation;  -- MySQL
SHOW transaction_isolation;  -- PostgreSQL

-- Establecer nivel de aislamiento
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

### SAVEPOINT

```sql
START TRANSACTION;

UPDATE productos SET precio = precio * 1.1 WHERE categoria = 'Electrónica';

SAVEPOINT punto1;

UPDATE productos SET precio = precio * 0.9 WHERE categoria = 'Ropa';

-- Si algo sale mal, volver al punto de guardado
ROLLBACK TO SAVEPOINT punto1;

COMMIT;
```

### Ejemplo completo de transacción

```sql
START TRANSACTION;

-- Verificar stock
SELECT stock FROM productos WHERE id = 5 FOR UPDATE;

-- Si hay stock, actualizar
UPDATE productos SET stock = stock - 1 WHERE id = 5 AND stock > 0;

-- Crear pedido
INSERT INTO pedidos (cliente_id, total, estado) 
VALUES (1, 99.99, 'procesando');

-- Obtener ID del pedido
SET @pedido_id = LAST_INSERT_ID();

-- Insertar detalle
INSERT INTO detalle_pedidos (pedido_id, producto_id, cantidad, precio_unitario)
VALUES (@pedido_id, 5, 1, 99.99);

-- Confirmar todo
COMMIT;
```

---

## 13. Procedimientos Almacenados

### MySQL

```sql
DELIMITER //

CREATE PROCEDURE crear_pedido(
    IN p_cliente_id INT,
    IN p_producto_id INT,
    IN p_cantidad INT,
    OUT p_pedido_id INT
)
BEGIN
    DECLARE v_precio DECIMAL(10,2);
    DECLARE v_total DECIMAL(10,2);
    
    -- Iniciar transacción
    START TRANSACTION;
    
    -- Obtener precio del producto
    SELECT precio INTO v_precio
    FROM productos
    WHERE id = p_producto_id;
    
    -- Calcular total
    SET v_total = v_precio * p_cantidad;
    
    -- Crear pedido
    INSERT INTO pedidos (cliente_id, total)
    VALUES (p_cliente_id, v_total);
    
    -- Obtener ID del pedido
    SET p_pedido_id = LAST_INSERT_ID();
    
    -- Insertar detalle
    INSERT INTO detalle_pedidos (pedido_id, producto_id, cantidad, precio_unitario)
    VALUES (p_pedido_id, p_producto_id, p_cantidad, v_precio);
    
    -- Actualizar stock
    UPDATE productos
    SET stock = stock - p_cantidad
    WHERE id = p_producto_id;
    
    COMMIT;
END //

DELIMITER ;

-- Llamar al procedimiento
CALL crear_pedido(1, 5, 2, @nuevo_pedido_id);
SELECT @nuevo_pedido_id;
```

### PostgreSQL

```sql
CREATE OR REPLACE FUNCTION crear_pedido(
    p_cliente_id INTEGER,
    p_producto_id INTEGER,
    p_cantidad INTEGER
) RETURNS INTEGER AS $$
DECLARE
    v_precio DECIMAL(10,2);
    v_total DECIMAL(10,2);
    v_pedido_id INTEGER;
BEGIN
    -- Obtener precio
    SELECT precio INTO v_precio
    FROM productos
    WHERE id = p_producto_id;
    
    -- Calcular total
    v_total := v_precio * p_cantidad;
    
    -- Crear pedido
    INSERT INTO pedidos (cliente_id, total)
    VALUES (p_cliente_id, v_total)
    RETURNING id INTO v_pedido_id;
    
    -- Insertar detalle
    INSERT INTO detalle_pedidos (pedido_id, producto_id, cantidad, precio_unitario)
    VALUES (v_pedido_id, p_producto_id, p_cantidad, v_precio);
    
    -- Actualizar stock
    UPDATE productos
    SET stock = stock - p_cantidad
    WHERE id = p_producto_id;
    
    RETURN v_pedido_id;
END;
$$ LANGUAGE plpgsql;

-- Llamar a la función
SELECT crear_pedido(1, 5, 2);
```

---

## 14. Triggers

### MySQL

```sql
DELIMITER //

CREATE TRIGGER actualizar_stock_despues_pedido
AFTER INSERT ON detalle_pedidos
FOR EACH ROW
BEGIN
    UPDATE productos
    SET stock = stock - NEW.cantidad
    WHERE id = NEW.producto_id;
END //

CREATE TRIGGER auditar_cambios_precio
BEFORE UPDATE ON productos
FOR EACH ROW
BEGIN
    IF NEW.precio != OLD.precio THEN
        INSERT INTO auditoria_precios (producto_id, precio_anterior, precio_nuevo, fecha)
        VALUES (OLD.id, OLD.precio, NEW.precio, NOW());
    END IF;
END //

DELIMITER ;
```

### PostgreSQL

```sql
-- Función del trigger
CREATE OR REPLACE FUNCTION actualizar_stock_funcion()
RETURNS TRIGGER AS $$
BEGIN
    UPDATE productos
    SET stock = stock - NEW.cantidad
    WHERE id = NEW.producto_id;
    
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Crear trigger
CREATE TRIGGER actualizar_stock_trigger
AFTER INSERT ON detalle_pedidos
FOR EACH ROW
EXECUTE FUNCTION actualizar_stock_funcion();

-- Trigger para auditoría
CREATE OR REPLACE FUNCTION auditar_precios_funcion()
RETURNS TRIGGER AS $$
BEGIN
    IF NEW.precio != OLD.precio THEN
        INSERT INTO auditoria_precios (producto_id, precio_anterior, precio_nuevo, fecha)
        VALUES (OLD.id, OLD.precio, NEW.precio, NOW());
    END IF;
    
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER auditar_precios_trigger
BEFORE UPDATE ON productos
FOR EACH ROW
EXECUTE FUNCTION auditar_precios_funcion();
```

---

## 15. Vistas

### Crear vistas

```sql
-- Vista simple
CREATE VIEW vista_pedidos_completos AS
SELECT 
    p.id AS pedido_id,
    c.nombre AS cliente,
    p.fecha_pedido,
    p.total,
    p.estado
FROM pedidos p
INNER JOIN clientes c ON p.cliente_id = c.id;

-- Vista con agregación
CREATE VIEW resumen_ventas_categoria AS
SELECT 
    pr.categoria,
    COUNT(DISTINCT dp.pedido_id) AS total_pedidos,
    SUM(dp.cantidad) AS unidades_vendidas,
    SUM(dp.cantidad * dp.precio_unitario) AS ingresos_totales
FROM detalle_pedidos dp
INNER JOIN productos pr ON dp.producto_id = pr.id
GROUP BY pr.categoria;

-- Usar la vista
SELECT * FROM vista_pedidos_completos WHERE estado = 'completado';
```

### Vistas materializadas (PostgreSQL)

```sql
-- Crear vista materializada
CREATE MATERIALIZED VIEW mv_resumen_ventas AS
SELECT 
    DATE_TRUNC('day', p.fecha_pedido) AS fecha,
    COUNT(*) AS total_pedidos,
    SUM(p.total) AS ingresos
FROM pedidos p
GROUP BY DATE_TRUNC('day', p.fecha_pedido);

-- Crear índice en vista materializada
CREATE INDEX idx_mv_fecha ON mv_resumen_ventas(fecha);

-- Refrescar datos
REFRESH MATERIALIZED VIEW mv_resumen_ventas;

-- Refrescar concurrentemente (sin bloquear lecturas)
REFRESH MATERIALIZED VIEW CONCURRENTLY mv_resumen_ventas;
```

---

## 16. Particionamiento

### MySQL - Particionamiento por rango

```sql
CREATE TABLE ventas (
    id INT NOT NULL AUTO_INCREMENT,
    fecha DATE NOT NULL,
    monto DECIMAL(10,2),
    PRIMARY KEY (id, fecha)
) 
PARTITION BY RANGE (YEAR(fecha)) (
    PARTITION p2022 VALUES LESS THAN (2023),
    PARTITION p2023 VALUES LESS THAN (2024),
    PARTITION p2024 VALUES LESS THAN (2025),
    PARTITION p_futuro VALUES LESS THAN MAXVALUE
);
```

### PostgreSQL - Particionamiento declarativo

```sql
-- Tabla principal
CREATE TABLE ventas (
    id SERIAL,
    fecha DATE NOT NULL,
    monto DECIMAL(10,2)
) PARTITION BY RANGE (fecha);

-- Particiones
CREATE TABLE ventas_2023 PARTITION OF ventas
    FOR VALUES FROM ('2023-01-01') TO ('2024-01-01');

CREATE TABLE ventas_2024 PARTITION OF ventas
    FOR VALUES FROM ('2024-01-01') TO ('2025-01-01');

-- Crear índices en cada partición
CREATE INDEX idx_ventas_2023_fecha ON ventas_2023(fecha);
CREATE INDEX idx_ventas_2024_fecha ON ventas_2024(fecha);
```

---

## 17. Replicación y Alta Disponibilidad

### MySQL - Replicación Maestro-Esclavo

```sql
-- En el maestro
CREATE USER 'replicador'@'%' IDENTIFIED BY 'password';
GRANT REPLICATION SLAVE ON *.* TO 'replicador'@'%';
FLUSH PRIVILEGES;

SHOW MASTER STATUS;  -- Anotar File y Position

-- En el esclavo
CHANGE MASTER TO
    MASTER_HOST='ip_maestro',
    MASTER_USER='replicador',
    MASTER_PASSWORD='password',
    MASTER_LOG_FILE='mysql-bin.000001',
    MASTER_LOG_POS=12345;

START SLAVE;
SHOW SLAVE STATUS\G
```

### PostgreSQL - Replicación Streaming

```sql
-- En el primario, editar postgresql.conf
wal_level = replica
max_wal_senders = 3
wal_keep_size = 64MB

-- Crear usuario de replicación
CREATE USER replicador WITH REPLICATION PASSWORD 'password';

-- En el secundario (shell)
pg_basebackup -h primario -D /var/lib/postgresql/data -U replicador -P -v -R -X stream -C -S replica1

-- Iniciar secundario
pg_ctl start
```

---

## 18. Backup y Restauración

### MySQL

```bash
# Backup completo
mysqldump -u root -p --all-databases > backup_completo.sql

# Backup de una base de datos
mysqldump -u root -p tienda > backup_tienda.sql

# Backup de una tabla
mysqldump -u root -p tienda productos > backup_productos.sql

# Backup comprimido
mysqldump -u root -p tienda | gzip > backup_tienda.sql.gz

# Restaurar
mysql -u root -p tienda < backup_tienda.sql

# Restaurar desde comprimido
gunzip < backup_tienda.sql.gz | mysql -u root -p tienda
```

### PostgreSQL

```bash
# Backup completo
pg_dumpall -U postgres > backup_completo.sql

# Backup de una base de datos (formato personalizado)
pg_dump -U postgres -Fc tienda > backup_tienda.dump

# Backup en formato SQL
pg_dump -U postgres tienda > backup_tienda.sql

# Restaurar desde formato personalizado
pg_restore -U postgres -d tienda backup_tienda.dump

# Restaurar desde SQL
psql -U postgres tienda < backup_tienda.sql

# Backup incremental con WAL archiving
# Configurar en postgresql.conf:
# archive_mode = on
# archive_command = 'cp %p /path/to/archive/%f'
```

---

## 19. Seguridad y Permisos

### MySQL

```sql
-- Crear usuario
CREATE USER 'usuario'@'localhost' IDENTIFIED BY 'password';

-- Otorgar permisos
GRANT SELECT, INSERT, UPDATE ON tienda.* TO 'usuario'@'localhost';
GRANT ALL PRIVILEGES ON tienda.* TO 'admin'@'localhost';

-- Ver permisos
SHOW GRANTS FOR 'usuario'@'localhost';

-- Revocar permisos
REVOKE INSERT ON tienda.* FROM 'usuario'@'localhost';

-- Eliminar usuario
DROP USER 'usuario'@'localhost';

-- Cambiar contraseña
ALTER USER 'usuario'@'localhost' IDENTIFIED BY 'nueva_password';

-- Recargar privilegios
FLUSH PRIVILEGES;
```

### PostgreSQL

```sql
-- Crear usuario
CREATE USER usuario WITH PASSWORD 'password';

-- Crear rol
CREATE ROLE readonly;

-- Otorgar permisos a rol
GRANT SELECT ON ALL TABLES IN SCHEMA public TO readonly;
GRANT USAGE ON SCHEMA public TO readonly;

-- Asignar rol a usuario
GRANT readonly TO usuario;

-- Otorgar permisos específicos
GRANT SELECT, INSERT ON productos TO usuario;

-- Ver permisos
\du  -- Listar usuarios y roles
\dp productos  -- Ver permisos de tabla

-- Revocar permisos
REVOKE INSERT ON productos FROM usuario;

-- Eliminar usuario
DROP USER usuario;
```

---

## 20. Diferencias MySQL vs PostgreSQL

### Sintaxis y Características

| Característica | MySQL | PostgreSQL |
|----------------|-------|------------|
| **Auto-incremento** | `AUTO_INCREMENT` | `SERIAL` o `IDENTITY` |
| **Límite de resultados** | `LIMIT n OFFSET m` | `LIMIT n OFFSET m` (igual) |
| **Concatenación** | `CONCAT()` | `||` operador |
| **IFNULL** | `IFNULL(col, valor)` | `COALESCE(col, valor)` |
| **String matching** | `LIKE` (case-insensitive) | `LIKE` (case-sensitive), `ILIKE` (case-insensitive) |
| **Expresiones regulares** | `REGEXP` | `~` operador |
| **JSON** | `JSON` type | `JSON` y `JSONB` |
| **Arrays** | No nativo | Soporte nativo |
| **Full text search** | `MATCH AGAINST` | `to_tsvector`, `to_tsquery` |
| **CTE recursivo** | Desde 8.0 | Soporte completo |
| **Window functions** | Desde 8.0 | Soporte completo |

### Ejemplos comparativos

```sql
-- Auto-incremento
-- MySQL
CREATE TABLE productos (
    id INT AUTO_INCREMENT PRIMARY KEY
);

-- PostgreSQL
CREATE TABLE productos (
    id SERIAL PRIMARY KEY
);

-- Concatenación
-- MySQL
SELECT CONCAT(nombre, ' ', apellido) FROM usuarios;

-- PostgreSQL
SELECT nombre || ' ' || apellido FROM usuarios;

-- IFNULL / COALESCE
-- MySQL
SELECT IFNULL(descuento, 0) FROM productos;

-- PostgreSQL
SELECT COALESCE(descuento, 0) FROM productos;

-- Búsqueda case-insensitive
-- MySQL
SELECT * FROM productos WHERE nombre LIKE '%laptop%';

-- PostgreSQL
SELECT * FROM productos WHERE nombre ILIKE '%laptop%';

-- JSON
-- MySQL
SELECT JSON_EXTRACT(datos, '$.nombre') FROM tabla;

-- PostgreSQL
SELECT datos->>'nombre' FROM tabla;
```

---

## 🎓 Mejores Prácticas

1. **Usa índices inteligentemente**: No sobre-indexar
2. **Normaliza apropiadamente**: Balance entre normalización y rendimiento
3. **Usa transacciones**: Para operaciones que requieren consistencia
4. **Realiza backups regulares**: Automatiza el proceso
5. **Monitorea el rendimiento**: Usa EXPLAIN regularmente
6. **Documenta tus esquemas**: Comentarios en tablas y columnas
7. **Usa CTEs**: Para consultas complejas más legibles
8. **Evita N+1 queries**: Usa JOINs cuando sea apropiado
9. **Parametriza consultas**: Previene SQL injection
10. **Mantén estadísticas actualizadas**: ANALYZE en PostgreSQL, ANALYZE TABLE en MySQL

---

**¡Domina SQL y tus bases de datos relacionales! 🗄️**
