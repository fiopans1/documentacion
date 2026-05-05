# Guía completa sobre HikariCP y `HikariDataSource`

## 🧭 Índice de la guía

1. **Qué es HikariCP**
2. **Cómo funciona un pool de conexiones**
3. **Cuándo usar Hikari**
4. **Instalación y dependencias**
5. **Uso básico en Java**
6. **Uso con Spring Boot**
7. **Estructura de archivos recomendada**
8. **Cómo adaptar Hikari a tu configuración**
9. **Sobrescribir o extender `HikariDataSource`**
10. **Parámetros más importantes**
11. **Errores comunes**
12. **Buenas prácticas**

---

## 1. Qué es HikariCP

**HikariCP** es un **pool de conexiones JDBC**. Su trabajo es mantener conexiones abiertas y reutilizarlas para que tu aplicación no tenga que crear una conexión nueva cada vez que consulta la base de datos.

En lugar de esto:

```java
Connection connection = DriverManager.getConnection(url, user, password);
```

trabajas con un `DataSource` que te entrega conexiones ya preparadas:

```java
Connection connection = dataSource.getConnection();
```

HikariCP es muy usado porque es:

- Rápido
- Ligero
- Estable
- Muy común en aplicaciones Spring Boot

De hecho, **Spring Boot usa Hikari por defecto** cuando está disponible en el classpath.

---

## 2. Cómo funciona un pool de conexiones

Un pool de conexiones funciona así:

1. La aplicación arranca.
2. Hikari crea un conjunto inicial de conexiones.
3. Cuando una consulta necesita acceso a BD, pide una conexión al pool.
4. La usa.
5. En vez de destruirla, la devuelve al pool.

### Flujo simplificado

```text
Aplicación
   ↓
HikariDataSource
   ↓
Pool de conexiones activas/idle
   ↓
Base de datos
```

### Ventajas

- Menos latencia
- Menos coste de abrir/cerrar conexiones
- Mejor control de concurrencia
- Posibilidad de limitar el número máximo de conexiones

---

## 3. Cuándo usar Hikari

Usa Hikari cuando:

- Tu aplicación hace muchas consultas a base de datos
- Tienes API REST, microservicios o aplicaciones web
- Quieres controlar tiempo de espera, tamaño del pool y reciclado de conexiones
- Trabajas con Spring Boot, JPA, Hibernate, JDBC Template o MyBatis

No suele tener sentido abrir conexiones manualmente con `DriverManager` en una aplicación real si ya puedes usar un pool.

---

## 4. Instalación y dependencias

## Maven

Si es un proyecto Java puro:

```xml
<dependency>
    <groupId>com.zaxxer</groupId>
    <artifactId>HikariCP</artifactId>
    <version>5.1.0</version>
</dependency>
```

Si usas Spring Boot con starters JDBC/JPA, normalmente no hace falta declararlo aparte porque viene transitivamente.

### Ejemplo con Spring Boot

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
```

o:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

## Gradle

```gradle
implementation 'com.zaxxer:HikariCP:5.1.0'
```

---

## 5. Uso básico en Java

## Configuración programática

```java
import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

public class Main {
    public static void main(String[] args) throws Exception {
        HikariConfig config = new HikariConfig();
        config.setJdbcUrl("jdbc:postgresql://localhost:5432/appdb");
        config.setUsername("postgres");
        config.setPassword("secret");
        config.setMaximumPoolSize(10);
        config.setMinimumIdle(2);
        config.setPoolName("main-pool");

        try (HikariDataSource dataSource = new HikariDataSource(config);
             Connection connection = dataSource.getConnection();
             PreparedStatement ps = connection.prepareStatement("SELECT now()");
             ResultSet rs = ps.executeQuery()) {

            while (rs.next()) {
                System.out.println(rs.getObject(1));
            }
        }
    }
}
```

### Qué pasa aquí

- `HikariConfig` contiene la configuración
- `HikariDataSource` crea y administra el pool
- `getConnection()` toma una conexión del pool
- `close()` sobre la conexión la devuelve al pool
- `close()` sobre el `HikariDataSource` apaga el pool

---

## 6. Uso con Spring Boot

Spring Boot detecta Hikari automáticamente si:

- Tienes un starter JDBC/JPA
- Tienes una URL de base de datos configurada
- No has definido otro `DataSource` manualmente

## `application.properties`

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/appdb
spring.datasource.username=postgres
spring.datasource.password=secret
spring.datasource.driver-class-name=org.postgresql.Driver

spring.datasource.hikari.pool-name=main-pool
spring.datasource.hikari.maximum-pool-size=10
spring.datasource.hikari.minimum-idle=2
spring.datasource.hikari.connection-timeout=30000
spring.datasource.hikari.idle-timeout=600000
spring.datasource.hikari.max-lifetime=1800000
```

## `application.yml`

```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/appdb
    username: postgres
    password: secret
    driver-class-name: org.postgresql.Driver
    hikari:
      pool-name: main-pool
      maximum-pool-size: 10
      minimum-idle: 2
      connection-timeout: 30000
      idle-timeout: 600000
      max-lifetime: 1800000
```

## Qué crea Spring Boot internamente

En una configuración típica:

1. Lee `spring.datasource.*`
2. Detecta Hikari en el classpath
3. Crea un bean `HikariDataSource`
4. Hibernate/JPA/JdbcTemplate usan ese bean automáticamente

---

## 7. Estructura de archivos recomendada

Si quieres usar Hikari en un proyecto Spring Boot, una estructura ordenada suele ser esta:

```text
src/
 └── main/
     ├── java/com/tuempresa/app/
     │   ├── config/
     │   │   └── DataSourceConfig.java
     │   ├── repository/
     │   │   └── UsuarioRepository.java
     │   ├── service/
     │   │   └── UsuarioService.java
     │   └── Application.java
     └── resources/
         ├── application.yml
         └── application-dev.yml
```

### Papel de cada archivo

- **`pom.xml` / `build.gradle`**: dependencias
- **`application.yml`**: configuración base del datasource y del pool
- **`application-dev.yml` / `application-prod.yml`**: ajustes por entorno
- **`config/DataSourceConfig.java`**: personalización manual del bean `DataSource`
- **`repository/`**: capa que usa la base de datos
- **`service/`**: lógica de negocio

### Si no personalizas nada

Puedes no tener `DataSourceConfig.java` y dejar que Spring Boot lo haga todo con propiedades.

---

## 8. Cómo adaptar Hikari a tu configuración

Tienes varias formas de adaptarlo.

## Opción A: solo con propiedades

Es la más simple y recomendada cuando no necesitas lógica extra.

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/appdb
    username: root
    password: secret
    hikari:
      maximum-pool-size: 20
      minimum-idle: 5
      pool-name: mysql-pool
```

Úsala cuando:

- Solo cambias parámetros
- No necesitas varios datasources
- No necesitas lógica de construcción propia

## Opción B: crear tu propio bean `HikariDataSource`

Útil cuando quieres controlar completamente la instancia.

```java
import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.sql.DataSource;

@Configuration
public class DataSourceConfig {

    @Bean
    public DataSource dataSource() {
        HikariConfig config = new HikariConfig();
        config.setJdbcUrl("jdbc:postgresql://localhost:5432/appdb");
        config.setUsername("postgres");
        config.setPassword("secret");
        config.setMaximumPoolSize(15);
        config.setMinimumIdle(3);
        config.setPoolName("custom-pool");

        return new HikariDataSource(config);
    }
}
```

Úsala cuando:

- Necesitas construir el datasource manualmente
- Quieres decidir valores en tiempo de ejecución
- Quieres varios beans `DataSource`

## Opción C: usar `@ConfigurationProperties`

Es la mejor mezcla entre configuración externa y control del bean.

```java
import com.zaxxer.hikari.HikariDataSource;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class DataSourceConfig {

    @Bean
    @ConfigurationProperties("app.datasource")
    public HikariDataSource dataSource() {
        return new HikariDataSource();
    }
}
```

Y en YAML:

```yaml
app:
  datasource:
    jdbc-url: jdbc:postgresql://localhost:5432/appdb
    username: postgres
    password: secret
    maximum-pool-size: 12
    minimum-idle: 2
    pool-name: app-custom-pool
```

Esto te permite:

- No depender de `spring.datasource.*`
- Tener un prefijo propio
- Crear varios pools con diferentes prefijos

---

## 9. Sobrescribir o extender `HikariDataSource`

Sí, **puedes extender `HikariDataSource`**, pero normalmente **no es la primera opción recomendada**.

Lo habitual es:

- configurar propiedades
- crear un bean propio
- envolver el `DataSource`

Extender la clase tiene sentido cuando quieres:

- aplicar defaults corporativos
- centralizar comportamiento compartido
- añadir inicialización común

## Ejemplo de extensión

```java
import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;

public class CustomHikariDataSource extends HikariDataSource {

    public CustomHikariDataSource() {
        super();
        setPoolName("custom-default-pool");
        setMaximumPoolSize(20);
        setMinimumIdle(5);
    }

    public CustomHikariDataSource(HikariConfig configuration) {
        super(configuration);
        setRegisterMbeans(true);
    }
}
```

Y luego:

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class DataSourceConfig {

    @Bean
    public CustomHikariDataSource dataSource() {
        CustomHikariDataSource ds = new CustomHikariDataSource();
        ds.setJdbcUrl("jdbc:postgresql://localhost:5432/appdb");
        ds.setUsername("postgres");
        ds.setPassword("secret");
        return ds;
    }
}
```

## Cuándo conviene extenderlo

- Si quieres una clase reutilizable para varios proyectos
- Si siempre aplicas la misma configuración base
- Si vas a integrar métricas, hooks o naming homogéneo

## Cuándo no conviene

- Si solo necesitas cambiar `maximumPoolSize` o `connectionTimeout`
- Si puedes resolverlo con propiedades
- Si solo quieres una configuración específica de un entorno

## Alternativa más limpia: composición

Muchas veces es mejor encapsular la construcción:

```java
import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;

public final class DataSourceFactory {

    private DataSourceFactory() {
    }

    public static HikariDataSource createPostgresDataSource() {
        HikariConfig config = new HikariConfig();
        config.setJdbcUrl("jdbc:postgresql://localhost:5432/appdb");
        config.setUsername("postgres");
        config.setPassword("secret");
        config.setPoolName("factory-pool");
        return new HikariDataSource(config);
    }
}
```

Esta aproximación suele ser más mantenible que sobreescribir métodos de una clase de infraestructura.

---

## 10. Parámetros más importantes

Estos son los ajustes más usados:

### `maximumPoolSize`

Número máximo de conexiones en el pool.

### `minimumIdle`

Número mínimo de conexiones ociosas listas para usarse.

### `connectionTimeout`

Tiempo máximo que espera una petición para obtener una conexión.

### `idleTimeout`

Tiempo tras el cual una conexión ociosa puede cerrarse.

### `maxLifetime`

Tiempo máximo de vida de una conexión antes de ser reciclada.

### `poolName`

Nombre del pool, útil para logs, métricas y observabilidad.

### `leakDetectionThreshold`

Ayuda a detectar conexiones que no se devuelven al pool a tiempo.

---

## 11. Errores comunes

## 1. Pool demasiado grande

Poner un `maximumPoolSize` muy alto puede saturar la base de datos.

## 2. No cerrar conexiones

Aunque uses pool, debes cerrar `Connection`, `Statement` y `ResultSet`. Cerrar la conexión **la devuelve al pool**, no la destruye necesariamente.

## 3. Configurar tiempos incoherentes

Por ejemplo, un `maxLifetime` mayor que el timeout real del servidor de base de datos puede provocar reconexiones inesperadas.

## 4. Crear varios pools sin necesidad

Cada pool consume recursos. No crees uno por servicio o por repositorio.

## 5. Sobrescribir `HikariDataSource` sin una razón clara

Si solo quieres cambiar propiedades, no necesitas herencia.

---

## 12. Buenas prácticas

- Usa **Spring Boot autoconfigurado** si no necesitas lógica especial
- Personaliza con propiedades antes que con herencia
- Usa `@ConfigurationProperties` si quieres flexibilidad limpia
- Mantén el tamaño del pool acorde a la capacidad real de la BD
- Activa métricas o JMX si quieres observabilidad
- Usa `try-with-resources` cuando trabajes directamente con JDBC
- Centraliza la creación del datasource en una sola clase si haces configuración manual

---

## ✅ Resumen rápido

- **HikariCP** es un pool de conexiones JDBC
- **`HikariDataSource`** es la implementación que expone el datasource y administra el pool
- En **Spring Boot**, normalmente se configura con `spring.datasource.hikari.*`
- Si necesitas más control, puedes crear tu propio bean `HikariDataSource`
- **Sí puedes extender `HikariDataSource`**, pero normalmente es mejor hacerlo solo si necesitas una personalización reutilizable
- La estructura típica de archivos incluye dependencias, `application.yml` y opcionalmente `DataSourceConfig.java`

---

## 📌 Recomendación práctica

Empieza así:

1. Configura Hikari por propiedades
2. Si no te alcanza, crea un bean `HikariDataSource`
3. Solo extiende `HikariDataSource` si de verdad necesitas un comportamiento común reutilizable

Así mantienes tu proyecto simple, claro y fácil de mantener.
