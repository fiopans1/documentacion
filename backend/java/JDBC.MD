# GUÍA SOBRE JDBC

## 🧭 Índice de la guía

1. **¿Qué es JDBC?**
2. **Configuración inicial**
3. **Conexión a la base de datos**
4. **Crear una consulta (`Statement`, `PreparedStatement`)**
5. **Usar el `ResultSet` y recorrer resultados**
6. **Cerrar conexiones correctamente**
7. **Ejemplo completo**
8. **Buenas prácticas**
9. **Errores comunes**
10. **Siguientes pasos: Transacciones, batch y CallableStatement**

Perfecto, vamos con la **Sección 1: ¿Qué es JDBC?**

---

## 1. ¿Qué es JDBC?

**JDBC** (Java Database Connectivity) es una API (Application Programming Interface) de Java que permite a las aplicaciones Java interactuar con bases de datos relacionales (como MySQL, PostgreSQL, Oracle, SQL Server, etc.).

JDBC define un conjunto de clases e interfaces que permiten:

- Conectarse a una base de datos.
- Ejecutar consultas SQL.
- Obtener y recorrer los resultados.
- Ejecutar sentencias de actualización (INSERT, UPDATE, DELETE).
- Manejar transacciones.

---

### 🔌 ¿Cómo funciona JDBC?

1. **Driver JDBC**: Es un puente entre Java y el motor de base de datos. Cada base de datos tiene su propio driver JDBC que debe añadirse al proyecto.
2. **Connection**: Abre una conexión con la base de datos.
3. **Statement / PreparedStatement**: Permiten ejecutar consultas SQL.
4. **ResultSet**: Representa los resultados de una consulta `SELECT`.
5. **Cerrar recursos**: Siempre es importante cerrar `Connection`, `Statement` y `ResultSet`.

---

### 📦 Paquetes principales de JDBC

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.Statement;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
```

---

### 🏛️ Arquitectura JDBC (simplificada)

```
Aplicación Java
      ↓
 JDBC API (java.sql)
      ↓
JDBC Driver (Proveedor específico)
      ↓
Base de Datos
```

---

## 2. Configuración inicial

Antes de empezar a usar JDBC, necesitas tener configurado tu entorno de desarrollo y tener acceso a una base de datos.

---

### ✅ Requisitos previos

- **Java instalado** (JDK 8 o superior)
- **Un entorno de desarrollo** (como IntelliJ IDEA, Eclipse o VS Code)
- **Una base de datos** instalada o accesible (por ejemplo: MySQL, PostgreSQL, H2, SQLite, etc.)
- **El driver JDBC** correspondiente a tu base de datos

---

### 📥 Añadir el driver JDBC

#### a) Si usas **Maven**

Añade la dependencia correspondiente en el `pom.xml`.

Por ejemplo, para **MySQL**:

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-j</artifactId>
    <version>8.0.33</version>
</dependency>
```

Para **PostgreSQL**:

```xml
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <version>42.7.1</version>
</dependency>
```

#### b) Si no usas Maven

Descarga el `.jar` del driver desde la web oficial y añádelo al classpath de tu proyecto manualmente.

---

### 🛠️ Crear una base de datos de prueba

Por ejemplo, en MySQL podrías ejecutar:

```sql
CREATE DATABASE testdb;

USE testdb;

CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    email VARCHAR(100)
);

INSERT INTO users (name, email) VALUES ('Ana', 'ana@email.com');
INSERT INTO users (name, email) VALUES ('Luis', 'luis@email.com');
```

---

### 🧪 Prueba de conexión rápida

Lo veremos más en detalle en la siguiente sección, pero aquí va un ejemplo básico:

```java
String url = "jdbc:mysql://localhost:3306/testdb";
String user = "root";
String password = "tu_contraseña";

try (Connection conn = DriverManager.getConnection(url, user, password)) {
    System.out.println("¡Conectado a la base de datos!");
} catch (SQLException e) {
    e.printStackTrace();
}
```

---

## 3. Conexión a la base de datos

El primer paso práctico con JDBC es establecer una conexión entre tu aplicación Java y la base de datos.

---

### 🔄 Pasos para conectarse

1. **Definir los datos de conexión**
2. **Usar `DriverManager` para obtener la conexión**
3. **Controlar errores con `try-catch` y cerrar bien los recursos**

---

### 📌 Estructura típica

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class ConexionBD {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/testdb"; // Cambia testdb por tu base
        String user = "root"; // Tu usuario
        String password = "tu_contraseña"; // Tu contraseña

        try (Connection conn = DriverManager.getConnection(url, user, password)) {
            if (conn != null) {
                System.out.println("✅ Conexión establecida correctamente.");
            }
        } catch (SQLException e) {
            System.out.println("❌ Error al conectar con la base de datos:");
            e.printStackTrace();
        }
    }
}
```

---

### 🧠 Explicación de `DriverManager.getConnection`

Este método recibe:

- **URL de conexión**: indica el tipo de base de datos, el host, el puerto y el nombre de la base.

  - Ejemplos:
    - MySQL: `jdbc:mysql://localhost:3306/testdb`
    - PostgreSQL: `jdbc:postgresql://localhost:5432/testdb`
    - SQLite: `jdbc:sqlite:ruta/a/miarchivo.db`

- **Usuario** y **contraseña** de la base de datos

---

### 📌 Detalles importantes

- **No es necesario registrar el driver manualmente** (con `Class.forName(...)`) si usas JDBC 4.0+ y el driver está bien en el classpath. Java lo carga automáticamente.
- Siempre usa `try-with-resources` para cerrar automáticamente la conexión.
- Es importante manejar bien los errores (`SQLException`).

---

## 4. Crear una consulta con `Statement` y `PreparedStatement`

Una vez que estás conectado a la base de datos, el siguiente paso es ejecutar consultas SQL. En JDBC se pueden usar dos formas principales:

- `Statement`: para consultas simples (no recomendada si hay parámetros dinámicos).
- `PreparedStatement`: para consultas con parámetros (más segura y eficiente).

---

### 🎯 ¿Cuál es la diferencia clave?

| Tipo                | ¿Parámetros? | ¿Seguridad contra SQL Injection? | ¿Más eficiente en consultas repetidas? |
| ------------------- | ------------ | -------------------------------- | -------------------------------------- |
| `Statement`         | ❌ No        | ❌ No                            | ❌ No                                  |
| `PreparedStatement` | ✅ Sí        | ✅ Sí                            | ✅ Sí                                  |

---

### 🧪 Ejemplo con `Statement`

```java
String sql = "SELECT * FROM users";

try (Connection conn = DriverManager.getConnection(url, user, password);
     Statement stmt = conn.createStatement();
     ResultSet rs = stmt.executeQuery(sql)) {

    while (rs.next()) {
        System.out.println("ID: " + rs.getInt("id"));
        System.out.println("Nombre: " + rs.getString("name"));
    }

} catch (SQLException e) {
    e.printStackTrace();
}
```

---

### 🔐 Ejemplo con `PreparedStatement`

```java
String sql = "SELECT * FROM users WHERE name = ?";

try (Connection conn = DriverManager.getConnection(url, user, password);
     PreparedStatement pstmt = conn.prepareStatement(sql)) {

    pstmt.setString(1, "Ana"); // Primer parámetro: nombre

    try (ResultSet rs = pstmt.executeQuery()) {
        while (rs.next()) {
            System.out.println("ID: " + rs.getInt("id"));
            System.out.println("Email: " + rs.getString("email"));
        }
    }

} catch (SQLException e) {
    e.printStackTrace();
}
```

---

### 🧠 Métodos comunes de `PreparedStatement`

```java
pstmt.setInt(posición, valor);      // Para enteros
pstmt.setString(posición, valor);   // Para strings
pstmt.setDouble(posición, valor);   // Para decimales
pstmt.setDate(posición, valor);     // Para fechas (java.sql.Date)
```

---

## 5. Usar `ResultSet` y recorrer los resultados

Cuando ejecutas una consulta `SELECT`, el método `executeQuery()` te devuelve un objeto de tipo `ResultSet`. Este representa una tabla en memoria con los resultados obtenidos.

---

### 🧱 Estructura básica

```java
ResultSet rs = statement.executeQuery("SELECT * FROM users");

while (rs.next()) {
    int id = rs.getInt("id");
    String name = rs.getString("name");
    String email = rs.getString("email");

    System.out.println("ID: " + id + ", Nombre: " + name + ", Email: " + email);
}
```

---

### 🔄 ¿Cómo funciona `rs.next()`?

- `rs.next()` mueve el cursor a la siguiente fila del resultado.
- La primera vez que lo llamas, se mueve de antes de la primera fila → a la primera.
- Devuelve `false` si ya no hay más filas.

---

### 🔍 Métodos para obtener datos

| Método                    | Tipo de dato                        |
| ------------------------- | ----------------------------------- |
| `getInt("columna")`       | Entero                              |
| `getString("columna")`    | Cadena                              |
| `getBoolean("columna")`   | Booleano                            |
| `getDouble("columna")`    | Decimal                             |
| `getDate("columna")`      | Fecha (`java.sql.Date`)             |
| `getTimestamp("columna")` | Fecha y hora (`java.sql.Timestamp`) |

También puedes usar índices:

```java
int id = rs.getInt(1);       // primera columna
String name = rs.getString(2); // segunda columna
```

Pero usar nombres de columna es más claro y resistente a cambios.

---

### ✅ Ejemplo completo

```java
String sql = "SELECT id, name, email FROM users";

try (Connection conn = DriverManager.getConnection(url, user, password);
     Statement stmt = conn.createStatement();
     ResultSet rs = stmt.executeQuery(sql)) {

    while (rs.next()) {
        int id = rs.getInt("id");
        String name = rs.getString("name");
        String email = rs.getString("email");

        System.out.printf("ID: %d - Nombre: %s - Email: %s%n", id, name, email);
    }

} catch (SQLException e) {
    e.printStackTrace();
}
```

---

## 6. Cerrar conexiones correctamente

Uno de los errores más comunes y peligrosos al usar JDBC es **no cerrar bien los recursos**. Si dejas conexiones abiertas, puedes agotar el número máximo de conexiones permitidas por la base de datos, lo que puede tumbar tu aplicación.

---

### 🧱 Recursos que se deben cerrar

En orden de apertura:

1. `ResultSet`
2. `Statement` / `PreparedStatement`
3. `Connection`

Todos implementan la interfaz `AutoCloseable`, lo que permite usar `try-with-resources` (desde Java 7).

---

### ✅ Forma correcta: try-with-resources

```java
String sql = "SELECT * FROM users";

try (Connection conn = DriverManager.getConnection(url, user, password);
     PreparedStatement pstmt = conn.prepareStatement(sql);
     ResultSet rs = pstmt.executeQuery()) {

    while (rs.next()) {
        System.out.println(rs.getString("name"));
    }

} catch (SQLException e) {
    e.printStackTrace();
}
```

> 💡 Esta forma **cierra automáticamente** todos los recursos al salir del bloque `try`, incluso si hay una excepción.

---

### ❌ Forma incorrecta: sin cerrar o cerrando mal

```java
Connection conn = null;
PreparedStatement pstmt = null;
ResultSet rs = null;

try {
    conn = DriverManager.getConnection(...);
    pstmt = conn.prepareStatement("SELECT ...");
    rs = pstmt.executeQuery();

    // usar rs...

} catch (SQLException e) {
    e.printStackTrace();
} finally {
    try { if (rs != null) rs.close(); } catch (SQLException e) { e.printStackTrace(); }
    try { if (pstmt != null) pstmt.close(); } catch (SQLException e) { e.printStackTrace(); }
    try { if (conn != null) conn.close(); } catch (SQLException e) { e.printStackTrace(); }
}
```

> Aunque esta forma funciona, es **más propensa a errores** y mucho más verbosa.

---

### 💡 Recomendaciones clave

- Siempre usa `try-with-resources` si puedes.
- Nunca dejes conexiones abiertas en métodos que se ejecutan frecuentemente.
- Si usas un **pool de conexiones** (por ejemplo con Spring o HikariCP), devolver la conexión al pool también se hace cerrándola.

---

## 7. Ejemplo completo: Conectar, consultar y mostrar resultados

Este ejemplo muestra cómo:

1. Conectarse a la base de datos.
2. Ejecutar una consulta usando `PreparedStatement`.
3. Recorrer un `ResultSet`.
4. Cerrar todos los recursos correctamente con `try-with-resources`.

---

### 📦 Supongamos que tenemos esta tabla en MySQL

```sql
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    email VARCHAR(100)
);

INSERT INTO users (name, email) VALUES ('Ana', 'ana@email.com');
INSERT INTO users (name, email) VALUES ('Luis', 'luis@email.com');
```

---

### ✅ Código Java completo

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class EjemploJDBC {

    public static void main(String[] args) {
        // Datos de conexión
        String url = "jdbc:mysql://localhost:3306/testdb"; // Cambia testdb por tu base
        String user = "root"; // Usuario de la base de datos
        String password = "tu_contraseña"; // Contraseña

        // Consulta SQL con parámetro
        String sql = "SELECT id, name, email FROM users WHERE name = ?";

        // Usamos try-with-resources para cerrar todo correctamente
        try (Connection conn = DriverManager.getConnection(url, user, password);
             PreparedStatement pstmt = conn.prepareStatement(sql)) {

            pstmt.setString(1, "Ana"); // Establecemos el parámetro

            try (ResultSet rs = pstmt.executeQuery()) {
                while (rs.next()) {
                    int id = rs.getInt("id");
                    String name = rs.getString("name");
                    String email = rs.getString("email");

                    System.out.printf("ID: %d - Nombre: %s - Email: %s%n", id, name, email);
                }
            }

        } catch (SQLException e) {
            System.out.println("❌ Error al ejecutar la consulta:");
            e.printStackTrace();
        }
    }
}
```

---

### 🧠 ¿Qué estamos haciendo?

- Usamos `PreparedStatement` para evitar inyecciones SQL.
- Establecemos un parámetro (`name = "Ana"`).
- Recorremos el `ResultSet` y mostramos los datos.
- Manejamos errores con `try-catch`.
- Cerramos automáticamente todo gracias a `try-with-resources`.

---

## 8. Buenas prácticas con JDBC

Estas recomendaciones te ayudarán a escribir código más limpio, seguro y eficiente al trabajar con JDBC:

---

### 🔒 Seguridad

- **Usa `PreparedStatement` siempre que haya parámetros** en la consulta para prevenir inyecciones SQL.
- Nunca construyas strings SQL concatenando directamente datos de entrada del usuario.

---

### 🧹 Gestión de recursos

- Usa **`try-with-resources`** siempre que puedas para asegurarte de cerrar conexiones, statements y result sets.
- No reutilices `Connection` o `Statement` en varios hilos sin sincronización (no son thread-safe).

---

### ⚠️ Manejo de errores

- Captura excepciones con `SQLException` y **muestra información útil** (`e.getMessage()`, `e.getErrorCode()`, `e.getSQLState()`).
- Registra los errores en un fichero de log si estás en producción.

---

### 🔁 Reutilización

- Extrae la lógica de conexión y consulta en métodos o clases reutilizables.
- Considera usar un **pool de conexiones** (como HikariCP o Apache DBCP) para mejorar el rendimiento en aplicaciones grandes.

---

## 9. Consultas de escritura: `INSERT`, `UPDATE`, `DELETE`

Con JDBC no solo puedes hacer consultas `SELECT`, también puedes modificar datos.

---

### ✏️ `INSERT` (añadir datos)

```java
String sql = "INSERT INTO users (name, email) VALUES (?, ?)";

try (Connection conn = DriverManager.getConnection(url, user, password);
     PreparedStatement pstmt = conn.prepareStatement(sql)) {

    pstmt.setString(1, "María");
    pstmt.setString(2, "maria@email.com");

    int filasInsertadas = pstmt.executeUpdate();
    System.out.println("Filas insertadas: " + filasInsertadas);

} catch (SQLException e) {
    e.printStackTrace();
}
```

---

### 🔁 `UPDATE` (modificar datos)

```java
String sql = "UPDATE users SET email = ? WHERE name = ?";

try (Connection conn = DriverManager.getConnection(url, user, password);
     PreparedStatement pstmt = conn.prepareStatement(sql)) {

    pstmt.setString(1, "nuevo@email.com");
    pstmt.setString(2, "Ana");

    int filasActualizadas = pstmt.executeUpdate();
    System.out.println("Filas actualizadas: " + filasActualizadas);

} catch (SQLException e) {
    e.printStackTrace();
}
```

---

### ❌ `DELETE` (eliminar datos)

```java
String sql = "DELETE FROM users WHERE name = ?";

try (Connection conn = DriverManager.getConnection(url, user, password);
     PreparedStatement pstmt = conn.prepareStatement(sql)) {

    pstmt.setString(1, "Luis");

    int filasEliminadas = pstmt.executeUpdate();
    System.out.println("Filas eliminadas: " + filasEliminadas);

} catch (SQLException e) {
    e.printStackTrace();
}
```

---

### 🧠 Nota

- Para `INSERT`, `UPDATE` y `DELETE` se usa `executeUpdate()` en lugar de `executeQuery()`.
- Este método devuelve un `int` con el número de filas afectadas.

---

## 10. Manejo de transacciones

Una transacción permite agrupar varias operaciones SQL en una sola unidad lógica: **todo o nada**. Es fundamental cuando modificas varias tablas o haces operaciones críticas.

---

### 🛠️ Ejemplo básico de transacción

```java
try (Connection conn = DriverManager.getConnection(url, user, password)) {
    conn.setAutoCommit(false); // Desactivamos el auto-commit

    try (PreparedStatement pstmt1 = conn.prepareStatement("INSERT INTO users (name, email) VALUES (?, ?)");
         PreparedStatement pstmt2 = conn.prepareStatement("UPDATE users SET email = ? WHERE name = ?")) {

        // Operación 1
        pstmt1.setString(1, "Carlos");
        pstmt1.setString(2, "carlos@email.com");
        pstmt1.executeUpdate();

        // Operación 2
        pstmt2.setString(1, "nuevo@email.com");
        pstmt2.setString(2, "Ana");
        pstmt2.executeUpdate();

        // Confirmamos ambas operaciones
        conn.commit();
        System.out.println("✅ Transacción completada con éxito");

    } catch (SQLException e) {
        conn.rollback(); // Deshacemos todo si falla algo
        System.out.println("❌ Transacción fallida. Cambios deshechos.");
        e.printStackTrace();
    }

} catch (SQLException e) {
    e.printStackTrace();
}
```

---

### 🧠 Importante

- Usa `conn.setAutoCommit(false)` para iniciar una transacción manual.
- Llama a `conn.commit()` al final si todo fue bien.
- Usa `conn.rollback()` en caso de error para deshacer los cambios.

---
