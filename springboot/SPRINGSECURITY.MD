## **1. Introducción a Spring Security**

Spring Security es un potente framework que proporciona autenticación, autorización y varias funcionalidades relacionadas con la seguridad para aplicaciones basadas en Spring. Es ampliamente utilizado en aplicaciones modernas debido a su flexibilidad, personalización y facilidad de integración.

---

### **1.1. ¿Qué es Spring Security?**
Spring Security es un módulo de Spring Framework diseñado para manejar:
- **Autenticación**: Verificar quién eres (por ejemplo, inicio de sesión con usuario y contraseña).
- **Autorización**: Determinar qué puedes hacer (por ejemplo, acceso basado en roles o permisos).

Además, protege aplicaciones de:
- Ataques comunes, como **CSRF**, **XSS** y **fuerza bruta**.
- Problemas relacionados con la seguridad en APIs REST.

**Características clave:**
- Soporte para autenticación basada en formularios, tokens JWT, OAuth2 y más.
- Fácil integración con bases de datos, sistemas de directorio (LDAP) y proveedores externos (Google, GitHub, etc.).
- Extensible y personalizable para manejar requisitos de seguridad avanzados.

**Ventaja:** Proporciona una capa de seguridad robusta y configurable sin necesidad de escribir todo desde cero.

---

### **1.2. Principales características de Spring Security**
Spring Security ofrece una amplia gama de funcionalidades. Estas son algunas de las más destacadas:

1. **Autenticación**:
   - Autenticación en memoria, en bases de datos o con servicios externos.
   - Compatible con estándares modernos como OAuth2 y JWT.
   - Soporte para múltiples esquemas de autenticación.

2. **Autorización**:
   - Control de acceso basado en roles y permisos.
   - Anotaciones como `@PreAuthorize`, `@Secured` para proteger métodos y endpoints.

3. **Protección contra amenazas comunes**:
   - **CSRF**: Protección contra ataques de Cross-Site Request Forgery.
   - **XSS**: Protección contra inyección de scripts maliciosos.
   - **Clickjacking**: Configuración de cabeceras como `X-Frame-Options`.

4. **Integración con aplicaciones modernas**:
   - Compatible con APIs REST y arquitecturas de microservicios.
   - Integración con proveedores de identidad externos (OAuth2, OpenID Connect).

5. **Extensibilidad**:
   - Uso de filtros personalizados para manejar autenticación y autorización complejas.
   - Personalización de flujos de seguridad según las necesidades del proyecto.

---

### **1.3. Arquitectura de Spring Security**

La arquitectura de Spring Security se basa en una serie de componentes que trabajan juntos para implementar autenticación y autorización:

#### **Componentes principales:**
1. **SecurityFilterChain**:
   - Es una cadena de filtros que intercepta todas las solicitudes HTTP.
   - Define qué reglas de seguridad se aplican a qué rutas o recursos.
   - Por ejemplo: permitir acceso público a `/login` y proteger `/admin`.

2. **AuthenticationManager**:
   - Es el componente principal para gestionar la **autenticación**.
   - Procesa las credenciales del usuario y devuelve un objeto `Authentication` si son válidas.
   - Puede delegar la autenticación a diferentes proveedores, como bases de datos o LDAP.

3. **AuthenticationProvider**:
   - Un proveedor de autenticación específico.
   - Por ejemplo, `DaoAuthenticationProvider` para autenticar usuarios almacenados en una base de datos.

4. **AccessDecisionManager**:
   - Decide si un usuario autenticado tiene permiso para acceder a un recurso específico.
   - Basado en roles, permisos u otras reglas definidas.

5. **UserDetailsService**:
   - Interfaz para cargar datos del usuario (por ejemplo, desde una base de datos).
   - Devuelve un objeto `UserDetails` con información como nombre, contraseña y roles.

---

### **Flujo básico de Spring Security**

1. **Solicitud HTTP**: Un cliente envía una solicitud al servidor.
2. **Intercepción por `SecurityFilterChain`**: 
   - La solicitud pasa por la cadena de filtros.
   - Se verifica si requiere autenticación o autorización.
3. **Autenticación**:
   - El sistema autentica al usuario utilizando credenciales (como usuario/contraseña).
   - Si la autenticación falla, se devuelve un error (`401 Unauthorized`).
4. **Autorización**:
   - Si el usuario está autenticado, se verifica si tiene permisos para acceder al recurso solicitado.
   - Si no tiene permisos, se devuelve un error (`403 Forbidden`).
5. **Acceso al recurso**:
   - Si pasa la autenticación y autorización, el recurso es servido.

---

### **Ejemplo práctico de introducción**

Aquí tienes un ejemplo básico de cómo funciona Spring Security con un usuario en memoria:

#### **Dependencias Maven:**
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

#### **Configuración inicial:**
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
public class SecurityConfig {

    // Configuración de usuarios en memoria
    @Bean
    public UserDetailsService userDetailsService() {
        UserDetails user = User.withDefaultPasswordEncoder()
                .username("admin")
                .password("password")
                .roles("ADMIN")
                .build();
        return new InMemoryUserDetailsManager(user);
    }

    // Configuración de seguridad HTTP
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .anyRequest().authenticated() // Requiere autenticación para todas las rutas
            .and()
            .httpBasic(); // Usa autenticación básica (usuario/contraseña en el encabezado HTTP)
        return http.build();
    }
}
```

---

### **Prueba del ejemplo**
1. Ejecuta la aplicación y accede a cualquier ruta (`http://localhost:8080`).
2. Se abrirá un cuadro de diálogo solicitando usuario y contraseña.
3. Usa las credenciales:
   - **Usuario:** `admin`
   - **Contraseña:** `password`

---

### **Conclusión**

Spring Security proporciona una base sólida para manejar seguridad en aplicaciones Spring. La introducción cubre los conceptos básicos, y su arquitectura modular lo hace flexible para adaptarse a cualquier necesidad de seguridad, desde sistemas simples hasta configuraciones avanzadas con JWT, OAuth2 o integraciones externas.



## **2. Configuración básica de Spring Security**

En esta sección aprenderás cómo configurar Spring Security desde cero para proteger tu aplicación. Esto incluye las dependencias necesarias, configuraciones iniciales y una introducción al componente principal: **`SecurityFilterChain`**.

---

### **2.1. Dependencias necesarias**

Spring Security es un módulo de Spring Boot, y su configuración requiere únicamente agregar las dependencias correspondientes en tu proyecto.

#### **Maven**
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

#### **Gradle**
```groovy
implementation 'org.springframework.boot:spring-boot-starter-security'
implementation 'org.springframework.boot:spring-boot-starter-web'
```

**Nota:** Al incluir `spring-boot-starter-security`, Spring Boot aplica automáticamente una configuración básica que:
- Restringe el acceso a todas las rutas.
- Usa autenticación básica HTTP.
- Genera una contraseña por defecto que puedes ver en los logs al iniciar la aplicación.

---

### **2.2. Configuración básica con Java-based**

Spring Security permite configurar la seguridad mediante clases Java (sin necesidad de XML). La configuración se define en una clase con la anotación `@Configuration` y componentes específicos como **`SecurityFilterChain`**.

#### **Ejemplo básico**
En este ejemplo, configuraremos la seguridad para permitir el acceso público a algunas rutas y requerir autenticación para otras.

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/public/**").permitAll()  // Permite acceso público a rutas que inicien con /public
                .anyRequest().authenticated()          // Requiere autenticación para cualquier otra ruta
            .and()
            .formLogin()                               // Habilita un formulario de inicio de sesión predeterminado
            .and()
            .logout().permitAll();                    // Permite cerrar sesión
        return http.build();
    }
}
```

#### **Explicación del ejemplo:**
1. **`authorizeRequests`**:
   - Configura las reglas de acceso.
   - `antMatchers("/public/**").permitAll()`: Permite acceso público a rutas que comiencen con `/public`.
   - `anyRequest().authenticated()`: Requiere que todas las demás rutas estén protegidas por autenticación.

2. **`formLogin()`**:
   - Activa un formulario de inicio de sesión predeterminado.

3. **`logout().permitAll()`**:
   - Permite a cualquier usuario cerrar sesión.

---

### **2.3. Clase `SecurityFilterChain` y su papel**

`SecurityFilterChain` es la clase clave en Spring Security que define cómo se manejan las solicitudes HTTP. Contiene todas las reglas de seguridad que serán aplicadas a la aplicación.

#### **Cómo funciona:**
- Cada solicitud HTTP pasa por la cadena de filtros (Filter Chain).
- Los filtros aplican las reglas de seguridad (autenticación, autorización, etc.).
- Si un filtro bloquea la solicitud (por ejemplo, por falta de autenticación), el flujo se detiene y se devuelve un error HTTP.

---

### **2.4. Configuración de usuarios en memoria**

Para autenticar usuarios, puedes usar un almacén de usuarios en memoria. Esto es útil para pruebas o aplicaciones pequeñas.

#### **Ejemplo:**
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;

@Configuration
public class SecurityConfig {

    @Bean
    public UserDetailsService userDetailsService() {
        UserDetails user = User.withDefaultPasswordEncoder()
                .username("user")
                .password("password")
                .roles("USER")
                .build();

        UserDetails admin = User.withDefaultPasswordEncoder()
                .username("admin")
                .password("admin")
                .roles("ADMIN")
                .build();

        return new InMemoryUserDetailsManager(user, admin);
    }

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/admin/**").hasRole("ADMIN")  // Solo usuarios con rol ADMIN pueden acceder
                .antMatchers("/user/**").hasRole("USER")   // Solo usuarios con rol USER pueden acceder
                .antMatchers("/public/**").permitAll()     // Acceso público
                .anyRequest().authenticated()              // Cualquier otra solicitud requiere autenticación
            .and()
            .httpBasic();                                  // Autenticación básica
        return http.build();
    }
}
```

#### **Prueba del ejemplo:**
1. Usa las siguientes credenciales:
   - Usuario: `user`, Contraseña: `password` → Accede a `/user/**`.
   - Usuario: `admin`, Contraseña: `admin` → Accede a `/admin/**`.
2. Intenta acceder a `/public/**` (sin credenciales) → Debería ser accesible.

---

### **2.5. Deshabilitar seguridad por defecto (opcional)**

Si deseas desactivar todas las reglas de seguridad (por ejemplo, para desarrollo), puedes configurar Spring Security como sigue:

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http.csrf().disable()  // Desactiva CSRF
            .authorizeRequests().anyRequest().permitAll(); // Permite todas las solicitudes sin autenticación
        return http.build();
    }
}
```

**Nota:** Esto no es recomendable en entornos de producción.

---

### **2.6. Prácticas recomendadas en configuración básica**

1. **Usa roles y permisos**:
   - Define roles claros (`USER`, `ADMIN`) y asocia permisos a las rutas.

2. **Habilita HTTPS**:
   - Asegúrate de que las aplicaciones en producción usen HTTPS para proteger las credenciales y la información sensible.

3. **Usa un encoder seguro para contraseñas**:
   - No uses contraseñas en texto plano. Usa codificadores como `BCrypt`.

   ```java
   @Bean
   public PasswordEncoder passwordEncoder() {
       return new BCryptPasswordEncoder();
   }
   ```

---

### **Resumen**

En esta sección, aprendiste cómo:
1. Configurar Spring Security en tu proyecto.
2. Usar `SecurityFilterChain` para definir reglas de seguridad.
3. Autenticar usuarios en memoria y proteger rutas con roles.

Con esta base, puedes personalizar las reglas y usuarios según las necesidades de tu aplicación.

## **3. Autenticación en Spring Security**

La autenticación es el proceso de verificar la identidad de un usuario. En Spring Security, se realiza mediante diversos mecanismos, como usuarios en memoria, bases de datos, LDAP o JWT. Esta sección profundiza en los conceptos clave, cómo configurar la autenticación y las herramientas que proporciona el framework.

---

### **3.1. Concepto de autenticación en Spring Security**

La autenticación responde a la pregunta: **¿Quién eres?**

En Spring Security:
- **El usuario** envía credenciales (usuario y contraseña).
- **El sistema** valida las credenciales.
- Si la autenticación es exitosa, se genera un objeto **`Authentication`**, que contiene la identidad y roles del usuario autenticado.

Componentes principales en la autenticación:
1. **`AuthenticationManager`**: Orquesta el proceso de autenticación.
2. **`AuthenticationProvider`**: Proveedor que valida las credenciales.
3. **`UserDetailsService`**: Carga información del usuario desde una base de datos o cualquier fuente de datos.
4. **Password Encoder**: Se encarga de codificar y verificar contraseñas.

---

### **3.2. Implementación de usuarios en memoria (`InMemoryUserDetailsManager`)**

Este es el método más simple para autenticar usuarios. Es útil para pruebas o aplicaciones pequeñas.

#### **Ejemplo básico de configuración:**
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;
import org.springframework.security.crypto.password.NoOpPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;

@Configuration
public class SecurityConfig {

    @Bean
    public UserDetailsService userDetailsService() {
        UserDetails user = User.withUsername("user")
                .password("password")
                .roles("USER")
                .build();

        UserDetails admin = User.withUsername("admin")
                .password("admin")
                .roles("ADMIN")
                .build();

        return new InMemoryUserDetailsManager(user, admin);
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return NoOpPasswordEncoder.getInstance(); // Contraseñas en texto plano (solo para pruebas)
    }
}
```

#### **Prueba:**
1. Accede a cualquier ruta protegida.
2. Inicia sesión con las credenciales:
   - **Usuario:** `user`, Contraseña: `password`
   - **Usuario:** `admin`, Contraseña: `admin`

---

### **3.3. Uso de bases de datos para autenticar usuarios**

En aplicaciones reales, los usuarios suelen almacenarse en bases de datos. Spring Security permite cargar usuarios desde una base de datos mediante **`JdbcUserDetailsManager`** o implementando **`UserDetailsService`**.

#### **Ejemplo con `JdbcUserDetailsManager`:**
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.provisioning.JdbcUserDetailsManager;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;

import javax.sql.DataSource;

@Configuration
public class SecurityConfig {

    @Bean
    public UserDetailsService userDetailsService(DataSource dataSource) {
        return new JdbcUserDetailsManager(dataSource); // Carga usuarios desde la base de datos
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder(); // Codificación de contraseñas segura
    }
}
```

#### **Estructura esperada de la base de datos:**
Spring Security usa las tablas `users` y `authorities` por defecto.

```sql
CREATE TABLE users (
    username VARCHAR(50) NOT NULL PRIMARY KEY,
    password VARCHAR(100) NOT NULL,
    enabled BOOLEAN NOT NULL
);

CREATE TABLE authorities (
    username VARCHAR(50) NOT NULL,
    authority VARCHAR(50) NOT NULL,
    FOREIGN KEY (username) REFERENCES users(username)
);
```

#### **Insertar usuarios de ejemplo:**
```sql
INSERT INTO users (username, password, enabled) VALUES 
('user', '$2a$10$QWERTYUIOPASDFGHJKLZXCVBNM1234567890', true), -- Contraseña codificada
('admin', '$2a$10$1234567890ZXCVBNMASDFGHJKLQWERTYUIOP', true);

INSERT INTO authorities (username, authority) VALUES 
('user', 'ROLE_USER'),
('admin', 'ROLE_ADMIN');
```

---

### **3.4. Autenticación con `UserDetails` y `UserDetailsService`**

Para personalizar la autenticación, puedes implementar tu propio **`UserDetailsService`** y cargar los usuarios desde cualquier fuente (por ejemplo, una API externa).

#### **Implementación personalizada:**
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.security.crypto.password.PasswordEncoder;

@Configuration
public class SecurityConfig {

    @Bean
    public UserDetailsService userDetailsService() {
        return username -> {
            // Simulación de búsqueda en base de datos
            if ("user".equals(username)) {
                return org.springframework.security.core.userdetails.User
                        .withUsername("user")
                        .password("$2a$10$encryptedPassword123")
                        .roles("USER")
                        .build();
            } else if ("admin".equals(username)) {
                return org.springframework.security.core.userdetails.User
                        .withUsername("admin")
                        .password("$2a$10$encryptedPassword456")
                        .roles("ADMIN")
                        .build();
            } else {
                throw new UsernameNotFoundException("Usuario no encontrado");
            }
        };
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

---

### **3.5. Codificación de contraseñas (Password Encoding)**

Nunca se deben almacenar contraseñas en texto plano. Spring Security incluye soporte para codificadores seguros como **BCrypt**, que es el estándar recomendado.

#### **Configurar un codificador de contraseñas:**
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;

@Configuration
public class SecurityConfig {

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

#### **Ejemplo de codificación:**
```java
public static void main(String[] args) {
    PasswordEncoder encoder = new BCryptPasswordEncoder();
    String rawPassword = "password123";
    String encodedPassword = encoder.encode(rawPassword);
    System.out.println("Contraseña codificada: " + encodedPassword);
}
```

---

### **Flujo de Autenticación**

1. **El cliente envía credenciales**: Usuario y contraseña.
2. **Spring Security delega la validación** al `AuthenticationManager`.
3. **Cargar información del usuario**:
   - Se consulta al `UserDetailsService` o base de datos.
   - Se valida la contraseña usando el `PasswordEncoder`.
4. **Generar el objeto `Authentication`**:
   - Contiene información del usuario autenticado, como roles.
5. **Acceso permitido o denegado**:
   - Si es exitoso, se devuelve el recurso solicitado.
   - Si falla, se responde con un error (`401 Unauthorized`).

---

### **Resumen**

- **Usuarios en memoria**: Rápido y fácil para pruebas.
- **Autenticación con bases de datos**: Usa `JdbcUserDetailsManager` o implementa `UserDetailsService`.
- **Codificación de contraseñas**: Es crucial para proteger credenciales.


## **4. Autorización en Spring Security**

La autorización es el proceso de verificar si un usuario autenticado tiene los permisos necesarios para acceder a un recurso específico. Mientras la autenticación responde a la pregunta **"¿Quién eres?"**, la autorización responde a **"¿Qué puedes hacer?"**.

En esta sección, exploraremos los conceptos clave de la autorización en Spring Security y cómo configurarla usando roles, permisos y expresiones avanzadas.

---

### **4.1. Conceptos clave de autorización**

1. **Roles vs. Autoridades:**
   - **Roles**: Representan un conjunto de permisos o privilegios agrupados. Ejemplo: `ROLE_USER`, `ROLE_ADMIN`.
   - **Autoridades (Authorities)**: Representan permisos más específicos. Ejemplo: `READ_PRIVILEGE`, `WRITE_PRIVILEGE`.

2. **Control basado en rutas:**
   - Protege las rutas de tu aplicación definiendo quién puede acceder a cada una.

3. **Expresiones de seguridad:**
   - Puedes usar expresiones como `hasRole()`, `hasAuthority()`, o combinaciones lógicas (`and`, `or`) para definir reglas avanzadas.

---

### **4.2. Configuración básica de autorización**

#### **Definir roles y permisos en las rutas**
Usando la API fluida de Spring Security, puedes configurar permisos para rutas específicas.

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/admin/**").hasRole("ADMIN")  // Solo usuarios con el rol ADMIN
                .antMatchers("/user/**").hasAnyRole("USER", "ADMIN")  // Usuarios con los roles USER o ADMIN
                .antMatchers("/public/**").permitAll()  // Acceso público
                .anyRequest().authenticated()  // Todas las demás rutas requieren autenticación
            .and()
            .formLogin()  // Habilita un formulario de inicio de sesión predeterminado
            .and()
            .logout().permitAll();  // Permite cerrar sesión
        return http.build();
    }
}
```

#### **Diferencias clave en roles y autoridades:**
- **`hasRole("ROLE_ADMIN")`:** Verifica si el usuario tiene el rol `ROLE_ADMIN`.
- **`hasAuthority("WRITE_PRIVILEGE")`:** Verifica si el usuario tiene un permiso específico.

---

### **4.3. Configuración de roles y permisos personalizados**

Si deseas definir permisos más específicos (no solo roles), puedes usar `GrantedAuthority`. Esto es útil para aplicaciones que requieren control detallado.

#### **Definición de roles y permisos:**
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;

@Configuration
public class SecurityConfig {

    @Bean
    public UserDetailsService userDetailsService() {
        UserDetails user = User.withUsername("user")
                .password(passwordEncoder().encode("password"))
                .authorities("READ_PRIVILEGE")  // Permiso específico
                .build();

        UserDetails admin = User.withUsername("admin")
                .password(passwordEncoder().encode("admin"))
                .authorities("READ_PRIVILEGE", "WRITE_PRIVILEGE", "ROLE_ADMIN") // Permisos + Rol
                .build();

        return new InMemoryUserDetailsManager(user, admin);
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

#### **Configurar acceso basado en permisos:**
```java
http
    .authorizeRequests()
        .antMatchers("/write/**").hasAuthority("WRITE_PRIVILEGE")  // Permiso específico
        .antMatchers("/read/**").hasAuthority("READ_PRIVILEGE")
        .anyRequest().authenticated();
```

---

### **4.4. Expresiones avanzadas para autorización**

Spring Security ofrece expresiones personalizadas para crear reglas avanzadas en las rutas.

#### **Ejemplo de expresiones comunes:**
- **`hasRole("ADMIN")`**: Verifica si el usuario tiene el rol `ROLE_ADMIN`.
- **`hasAuthority("WRITE_PRIVILEGE")`**: Verifica si el usuario tiene el permiso `WRITE_PRIVILEGE`.
- **`hasAnyRole("USER", "ADMIN")`**: Permite acceso si el usuario tiene cualquiera de los roles.
- **`principal.username.equals("admin")`**: Permite acceso solo al usuario `admin`.
- **`isAuthenticated()`**: Verifica que el usuario haya iniciado sesión.
- **`isAnonymous()`**: Permite acceso a usuarios no autenticados.

#### **Ejemplo práctico con expresiones:**
```java
http
    .authorizeRequests()
        .antMatchers("/profile/**").access("hasRole('USER') and principal.username.equals('john_doe')")  // Solo el usuario 'john_doe'
        .antMatchers("/admin/**").access("hasRole('ADMIN') and isAuthenticated()")  // Solo admins autenticados
        .anyRequest().authenticated();
```

---

### **4.5. Autorización a nivel de método**

Además de proteger rutas, puedes definir reglas de autorización a nivel de métodos en tus controladores o servicios. Esto se logra con las anotaciones **`@PreAuthorize`** y **`@PostAuthorize`**.

#### **Habilitar anotaciones de seguridad:**
Primero, habilita el uso de anotaciones en tu configuración:

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.method.configuration.EnableMethodSecurity;

@Configuration
@EnableMethodSecurity  // Habilita anotaciones @PreAuthorize y @PostAuthorize
public class MethodSecurityConfig {
}
```

#### **Usar anotaciones:**
- **`@PreAuthorize`**: Verifica condiciones antes de ejecutar el método.
- **`@PostAuthorize`**: Verifica condiciones después de ejecutar el método.

#### **Ejemplo en un controlador:**
```java
import org.springframework.security.access.prepost.PreAuthorize;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MyController {

    @GetMapping("/admin")
    @PreAuthorize("hasRole('ADMIN')")  // Solo usuarios con rol ADMIN
    public String adminAccess() {
        return "Acceso permitido solo a administradores";
    }

    @GetMapping("/user")
    @PreAuthorize("hasAuthority('READ_PRIVILEGE')")  // Usuarios con permiso específico
    public String userAccess() {
        return "Acceso permitido a usuarios con privilegios de lectura";
    }
}
```

---

### **4.6. Manejo de excepciones de autorización**

Si un usuario no tiene permisos para acceder a un recurso, Spring Security genera automáticamente una excepción de tipo **`AccessDeniedException`**, que se traduce en un error HTTP 403 (Forbidden).

#### **Personalizar el manejo de excepciones:**
Puedes personalizar la respuesta para casos de acceso denegado.

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.security.access.AccessDeniedException;
import org.springframework.security.web.access.AccessDeniedHandler;
import org.springframework.security.web.access.AccessDeniedHandlerImpl;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@Configuration
public class CustomAccessDeniedHandler implements AccessDeniedHandler {

    @Override
    public void handle(HttpServletRequest request, HttpServletResponse response, AccessDeniedException ex) throws IOException {
        response.setStatus(HttpServletResponse.SC_FORBIDDEN);
        response.getWriter().write("Acceso denegado: No tienes los permisos necesarios.");
    }
}
```

---

### **4.7. Buenas prácticas en autorización**

1. **Definir roles claros**:
   - Usa roles bien definidos como `ROLE_USER` y `ROLE_ADMIN` para agrupar permisos.

2. **Usar permisos específicos cuando sea necesario**:
   - Para un control más granular, usa permisos como `READ_PRIVILEGE`, `WRITE_PRIVILEGE`.

3. **Minimizar acceso predeterminado**:
   - Asegúrate de que las rutas sensibles estén protegidas.

4. **Probar reglas de autorización**:
   - Verifica que las reglas funcionan como se espera con diferentes usuarios y roles.

---

### **Resumen**

En esta sección aprendiste:
- Cómo proteger rutas basadas en roles y permisos.
- Cómo usar expresiones avanzadas para reglas complejas.
- Cómo habilitar autorización a nivel de métodos con anotaciones.
- Buenas prácticas para garantizar un control efectivo de acceso.

## **5. Integración con OAuth2 y JWT en Spring Security**

La integración con **OAuth2** y **JWT (JSON Web Tokens)** permite a las aplicaciones Spring manejar autenticación y autorización en sistemas distribuidos, lo que es clave para aplicaciones modernas, especialmente APIs RESTful.

En esta sección, abordaremos:
1. Introducción a OAuth2 y JWT.
2. Configuración de OAuth2 en Spring Security.
3. Uso de JWT para autenticación.
4. Validación de tokens.
5. Implementación paso a paso.

---

### **5.1. Introducción a OAuth2 y JWT**

#### **OAuth2:**
OAuth2 es un marco de autorización que permite a una aplicación obtener acceso a recursos de un usuario en un servidor de recursos sin necesidad de compartir contraseñas directamente.

- **Roles principales en OAuth2:**
  1. **Resource Owner:** El usuario final que posee los recursos protegidos.
  2. **Client:** La aplicación que solicita acceso a los recursos.
  3. **Authorization Server:** Responsable de autenticar usuarios y emitir tokens de acceso.
  4. **Resource Server:** La API que almacena los recursos protegidos y valida los tokens de acceso.

- **Tipos de tokens:** 
  - **Access Token:** Token para acceder a recursos.
  - **Refresh Token:** Token usado para obtener un nuevo access token cuando el anterior expira.

---

#### **JWT:**
Un JWT es un token seguro y auto-contenido usado comúnmente para autenticación. Contiene tres partes codificadas en Base64:
1. **Header (Encabezado):** Especifica el tipo de token y el algoritmo de firma (como HS256 o RS256).
2. **Payload (Carga):** Contiene los datos del usuario, como roles, ID, etc.
3. **Signature (Firma):** Garantiza la integridad y autenticidad del token.

Ejemplo de JWT:
```json
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJ1c2VyIiwicm9sZXMiOlsiUk9MRV9VU0VSIl0sImV4cCI6MTY5MDM1MjAwMH0.DhJ3jFa6RZtCBNpq-rIfbT8D5qJ6dGh5V99ZmU7j3Xc
```

---

### **5.2. Configuración de OAuth2 en Spring Security**

Spring Security tiene soporte incorporado para OAuth2, facilitando la autenticación contra servidores externos como Google, Facebook o un servidor OAuth2 personalizado.

#### **Habilitar OAuth2 Login**
Primero, añade las dependencias necesarias:
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-oauth2-client</artifactId>
</dependency>
```

#### **Configuración básica:**
En `application.yml`, configura un proveedor de OAuth2 (por ejemplo, Google):
```yaml
spring:
  security:
    oauth2:
      client:
        registration:
          google:
            client-id: your-client-id
            client-secret: your-client-secret
            scope: profile, email
            redirect-uri: "{baseUrl}/login/oauth2/code/{registrationId}"
        provider:
          google:
            authorization-uri: https://accounts.google.com/o/oauth2/auth
            token-uri: https://oauth2.googleapis.com/token
            user-info-uri: https://www.googleapis.com/oauth2/v3/userinfo
```

#### **Configurar seguridad en Java:**
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/public/**").permitAll()
                .anyRequest().authenticated()
            .and()
            .oauth2Login();  // Habilita OAuth2 Login
        return http.build();
    }
}
```

Con esta configuración, Spring Security redirige automáticamente a la página de inicio de sesión de Google.

---

### **5.3. Uso de JWT para autenticación**

JWT es una forma común de implementar autenticación en APIs. Spring Security permite crear y validar JWT fácilmente.

#### **Dependencias necesarias:**
```xml
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-api</artifactId>
    <version>0.11.5</version>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-impl</artifactId>
    <version>0.11.5</version>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-jackson</artifactId>
    <version>0.11.5</version>
</dependency>
```

---

#### **Generar un JWT:**
Usa `io.jsonwebtoken.Jwts` para generar tokens.

```java
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;

import java.util.Date;

public class JwtUtil {
    private static final String SECRET_KEY = "your-secret-key";

    public static String generateToken(String username) {
        return Jwts.builder()
                .setSubject(username)
                .setIssuedAt(new Date())
                .setExpiration(new Date(System.currentTimeMillis() + 1000 * 60 * 60)) // 1 hora
                .signWith(SignatureAlgorithm.HS256, SECRET_KEY)
                .compact();
    }
}
```

#### **Validar un JWT:**
```java
import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jwts;

public class JwtUtil {
    public static Claims extractClaims(String token) {
        return Jwts.parser()
                .setSigningKey(SECRET_KEY)
                .parseClaimsJws(token)
                .getBody();
    }

    public static String extractUsername(String token) {
        return extractClaims(token).getSubject();
    }

    public static boolean isTokenExpired(String token) {
        return extractClaims(token).getExpiration().before(new Date());
    }
}
```

---

### **5.4. Configuración de un servidor JWT**

En un sistema basado en JWT, el servidor de autenticación genera el token y el servidor de recursos valida el token en cada solicitud.

#### **Configurar autenticación con JWT:**
1. Intercepta solicitudes y verifica el token.
2. Configura un filtro que valide el token antes de procesar la solicitud.

#### **Filtro de validación de JWT:**
```java
import io.jsonwebtoken.Claims;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.web.filter.OncePerRequestFilter;

import javax.servlet.FilterChain;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class JwtAuthenticationFilter extends OncePerRequestFilter {

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
            throws ServletException, IOException {
        String authorizationHeader = request.getHeader("Authorization");

        if (authorizationHeader != null && authorizationHeader.startsWith("Bearer ")) {
            String token = authorizationHeader.substring(7);
            Claims claims = JwtUtil.extractClaims(token);

            if (claims != null && !JwtUtil.isTokenExpired(token)) {
                String username = claims.getSubject();
                // Crea un contexto de seguridad aquí (opcional)
                SecurityContextHolder.getContext().setAuthentication(new JwtAuthenticationToken(username));
            }
        }
        filterChain.doFilter(request, response);
    }
}
```

---

### **5.5. Buenas prácticas con OAuth2/JWT**

1. **Usar Refresh Tokens:**
   - Implementa refresh tokens para extender sesiones sin pedir al usuario que vuelva a autenticarse.

2. **Revoke Tokens:**
   - Implementa un mecanismo para invalidar tokens comprometidos.

3. **Configura tiempos de expiración cortos:**
   - Los access tokens deben expirar rápidamente.

4. **Usa HTTPS:**
   - Asegúrate de que todos los tokens viajen en conexiones seguras.

5. **Firma segura:**
   - Usa algoritmos seguros como RS256 (firma asimétrica) en lugar de HS256.

---

### **Resumen**

- **OAuth2:** Facilita autenticación con terceros y servidores de autorización.
- **JWT:** Permite autenticación en APIs de forma eficiente y segura.
- **Configuración:** Spring Security facilita la integración tanto para OAuth2 como para JWT.
- **Buenas prácticas:** Implementa mecanismos como refresh tokens, validación y expiración adecuada.


## **6. Personalización de Spring Security**

Spring Security es extremadamente flexible y permite personalizar casi todos sus aspectos para adaptarse a las necesidades específicas de una aplicación. En esta sección, exploraremos cómo modificar las configuraciones predeterminadas para lograr una solución completamente personalizada.

---

### **6.1. Personalizar la página de inicio de sesión**

Por defecto, Spring Security ofrece una página de inicio de sesión básica. Esta página puede ser reemplazada por una personalizada.

#### **Configurar una página de inicio de sesión personalizada**
En el archivo de configuración de seguridad, especifica la ruta de tu página personalizada:
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/public/**").permitAll()
                .anyRequest().authenticated()
            .and()
            .formLogin()
                .loginPage("/login")  // Ruta a tu página personalizada
                .permitAll()          // Permite acceso sin autenticación
            .and()
            .logout()
                .permitAll();         // Permite cerrar sesión
        return http.build();
    }
}
```

#### **Diseñar la página personalizada**
Crea un controlador y un archivo HTML para el inicio de sesión:
```java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class LoginController {

    @GetMapping("/login")
    public String login() {
        return "login";  // Nombre del archivo HTML
    }
}
```

Archivo `login.html` (con Thymeleaf):
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Inicio de Sesión</title>
</head>
<body>
    <form th:action="@{/login}" method="post">
        <div>
            <label for="username">Usuario:</label>
            <input type="text" id="username" name="username" required>
        </div>
        <div>
            <label for="password">Contraseña:</label>
            <input type="password" id="password" name="password" required>
        </div>
        <button type="submit">Ingresar</button>
    </form>
</body>
</html>
```

---

### **6.2. Personalizar el manejo de errores**

Puedes personalizar cómo Spring Security responde a eventos como acceso denegado o errores de autenticación.

#### **Acceso denegado (403 - Forbidden)**
Crea un manejador personalizado para errores de autorización:
```java
import org.springframework.security.access.AccessDeniedException;
import org.springframework.security.web.access.AccessDeniedHandler;
import org.springframework.stereotype.Component;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@Component
public class CustomAccessDeniedHandler implements AccessDeniedHandler {

    @Override
    public void handle(HttpServletRequest request, HttpServletResponse response, AccessDeniedException ex) throws IOException {
        response.sendRedirect("/access-denied");
    }
}
```

Registra el manejador en la configuración de seguridad:
```java
http
    .exceptionHandling()
        .accessDeniedHandler(new CustomAccessDeniedHandler());
```

Crea una página personalizada para errores:
```java
@Controller
public class ErrorController {

    @GetMapping("/access-denied")
    public String accessDenied() {
        return "accessDenied";  // Archivo HTML para 403
    }
}
```

#### **Error de autenticación**
Para manejar errores de autenticación, configura un `AuthenticationFailureHandler`:
```java
import org.springframework.security.web.authentication.AuthenticationFailureHandler;
import org.springframework.stereotype.Component;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@Component
public class CustomAuthenticationFailureHandler implements AuthenticationFailureHandler {

    @Override
    public void onAuthenticationFailure(HttpServletRequest request, HttpServletResponse response, AuthenticationException exception)
            throws IOException {
        response.sendRedirect("/login?error=true");
    }
}
```

Registra el manejador en la configuración:
```java
.formLogin()
    .loginPage("/login")
    .failureHandler(new CustomAuthenticationFailureHandler())
    .permitAll();
```

---

### **6.3. Personalizar usuarios y roles**

#### **Base de datos personalizada**
En lugar de almacenar usuarios en memoria, puedes usar una base de datos para gestionar la autenticación.

Ejemplo de entidad `User`:
```java
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.ManyToMany;
import java.util.Set;

@Entity
public class User {

    @Id
    private String username;
    private String password;
    private boolean enabled;

    @ManyToMany
    private Set<Role> roles;

    // Getters y Setters
}
```

Ejemplo de entidad `Role`:
```java
import javax.persistence.Entity;
import javax.persistence.Id;

@Entity
public class Role {

    @Id
    private String name;

    // Getters y Setters
}
```

#### **Servicio personalizado para usuarios**
Define un servicio que cargue usuarios desde la base de datos:
```java
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

@Service
public class CustomUserDetailsService implements UserDetailsService {

    private final UserRepository userRepository;

    public CustomUserDetailsService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        User user = userRepository.findByUsername(username)
                .orElseThrow(() -> new UsernameNotFoundException("Usuario no encontrado"));

        return org.springframework.security.core.userdetails.User.builder()
                .username(user.getUsername())
                .password(user.getPassword())
                .roles(user.getRoles().stream().map(Role::getName).toArray(String[]::new))
                .build();
    }
}
```

Registra el servicio en la configuración:
```java
http
    .userDetailsService(customUserDetailsService());
```

---

### **6.4. Personalizar filtros de seguridad**

Puedes agregar filtros personalizados para procesar solicitudes antes o después de los filtros predeterminados.

#### **Crear un filtro personalizado**
```java
import org.springframework.web.filter.OncePerRequestFilter;

import javax.servlet.FilterChain;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class CustomFilter extends OncePerRequestFilter {

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
            throws ServletException, IOException {
        // Lógica personalizada
        System.out.println("Filtro personalizado ejecutado");
        filterChain.doFilter(request, response);
    }
}
```

#### **Agregar el filtro a la cadena de seguridad**
```java
http
    .addFilterBefore(new CustomFilter(), UsernamePasswordAuthenticationFilter.class);
```

---

### **6.5. Personalizar la expiración de sesiones**

Spring Security permite controlar la duración de sesiones y el comportamiento cuando un usuario inicia sesión desde múltiples dispositivos.

#### **Configuración de sesiones:**
```java
http
    .sessionManagement()
        .maximumSessions(1)  // Solo permite 1 sesión activa por usuario
        .maxSessionsPreventsLogin(true);  // Bloquea el inicio de sesión adicional
```

---

### **6.6. Personalización de Logout**

Spring Security ofrece un mecanismo predeterminado para cerrar sesión, pero puedes personalizarlo.

#### **Configurar Logout**
```java
http
    .logout()
        .logoutUrl("/logout")  // Ruta personalizada para logout
        .logoutSuccessUrl("/login?logout=true")  // Redirección tras logout
        .deleteCookies("JSESSIONID")  // Borra cookies al cerrar sesión
        .permitAll();
```

---

### **Resumen**

La personalización de Spring Security incluye:
- Configurar páginas personalizadas de inicio de sesión y manejo de errores.
- Usar bases de datos para almacenar usuarios y roles.
- Agregar filtros y manejadores personalizados.
- Controlar sesiones y comportamiento de logout.


## **7. Seguridad avanzada en Spring Security: CSRF, CORS, HTTPS y más**

Spring Security proporciona herramientas avanzadas para proteger aplicaciones contra amenazas comunes como ataques de CSRF, problemas de CORS y garantizar conexiones seguras mediante HTTPS. En esta sección, exploraremos cómo habilitar, personalizar y configurar estas características.

---

### **7.1. Protección contra CSRF (Cross-Site Request Forgery)**

El ataque CSRF ocurre cuando un usuario autenticado es engañado para ejecutar acciones no deseadas en un servidor en el que está autenticado. Spring Security habilita la protección CSRF de forma predeterminada para aplicaciones basadas en formularios.

#### **Cómo funciona la protección CSRF**
1. Spring Security genera un token único para cada sesión de usuario.
2. Este token debe enviarse con cada solicitud que cambie el estado (POST, PUT, DELETE).
3. El servidor valida el token antes de procesar la solicitud.

#### **Configurar protección CSRF**
Por defecto, Spring Security ya la habilita. Si necesitas personalizarla, puedes hacerlo en tu configuración:
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf()
                .csrfTokenRepository(CookieCsrfTokenRepository.withHttpOnlyFalse())  // Almacena el token en una cookie
            .and()
            .authorizeRequests()
                .antMatchers("/public/**").permitAll()
                .anyRequest().authenticated();
        return http.build();
    }
}
```

#### **Deshabilitar CSRF**
Si estás construyendo una API REST (stateless), puedes deshabilitar CSRF, ya que las APIs suelen usar tokens (como JWT) en lugar de cookies:
```java
http.csrf().disable();
```

---

### **7.2. Configuración de CORS (Cross-Origin Resource Sharing)**

CORS define cómo se permite que las aplicaciones de otros dominios accedan a tus recursos. Spring Security puede configurarse para manejar solicitudes CORS automáticamente.

#### **Habilitar CORS en Spring Security**
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.cors.CorsConfiguration;
import org.springframework.web.cors.UrlBasedCorsConfigurationSource;
import org.springframework.web.filter.CorsFilter;

@Configuration
public class SecurityConfig {

    @Bean
    public CorsFilter corsFilter() {
        CorsConfiguration corsConfiguration = new CorsConfiguration();
        corsConfiguration.addAllowedOrigin("https://frontend.com");  // Dominio permitido
        corsConfiguration.addAllowedMethod("*");                     // Métodos HTTP permitidos
        corsConfiguration.addAllowedHeader("*");                     // Cabeceras permitidas

        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        source.registerCorsConfiguration("/**", corsConfiguration);
        return new CorsFilter(source);
    }
}
```

#### **Habilitar CORS en la configuración HTTP**
```java
http.cors().configurationSource(request -> {
    CorsConfiguration config = new CorsConfiguration();
    config.addAllowedOrigin("https://frontend.com");
    config.addAllowedMethod("*");
    config.addAllowedHeader("*");
    return config;
});
```

---

### **7.3. Configurar HTTPS**

HTTPS asegura que las comunicaciones entre el cliente y el servidor sean cifradas, protegiendo la información de ataques de intermediarios.

#### **Redirigir todo el tráfico a HTTPS**
Puedes configurar Spring Security para requerir HTTPS:
```java
http.requiresChannel()
    .anyRequest()
    .requiresSecure();
```

#### **Configurar un certificado SSL**
1. **Genera un certificado SSL (self-signed o con una autoridad certificadora):**
   ```bash
   keytool -genkeypair -alias mycert -keyalg RSA -keysize 2048 -validity 365 -keystore keystore.jks
   ```

2. **Configura el certificado en `application.yml`:**
   ```yaml
   server:
     ssl:
       enabled: true
       key-store: classpath:keystore.jks
       key-store-password: password
       key-store-type: JKS
       key-alias: mycert
   ```

---

### **7.4. Protección contra Clickjacking**

El clickjacking es un ataque donde un atacante engaña al usuario para hacer clic en algo diferente a lo que cree, por ejemplo, incrustando tu página en un iframe malicioso.

#### **Evitar clickjacking con Spring Security**
Spring Security añade automáticamente la cabecera `X-Frame-Options` con el valor `DENY`. Puedes personalizar este comportamiento:
```java
http.headers()
    .frameOptions()
    .sameOrigin();  // Permite iframes solo desde el mismo dominio
```

---

### **7.5. Configuración de cabeceras de seguridad**

Spring Security puede ayudarte a configurar cabeceras HTTP que protegen tu aplicación de varias vulnerabilidades.

#### **Ejemplo de configuración de cabeceras:**
```java
http.headers()
    .contentSecurityPolicy("default-src 'self'")  // Protege contra ataques XSS
    .and()
    .referrerPolicy(ReferrerPolicyHeaderWriter.ReferrerPolicy.NO_REFERRER)  // Limita información del Referer
    .and()
    .xssProtection()
        .block(true)  // Habilita protección contra XSS en navegadores
    .and()
    .httpStrictTransportSecurity()
        .includeSubDomains(true)
        .maxAgeInSeconds(31536000);  // Habilita HSTS (1 año)
```

---

### **7.6. Seguridad basada en sesiones**

#### **Controlar sesiones activas**
Para limitar el número de sesiones activas por usuario:
```java
http.sessionManagement()
    .maximumSessions(1)
    .maxSessionsPreventsLogin(true);
```

#### **Configurar políticas de sesión:**
- **Siempre crea una nueva sesión:**
  ```java
  http.sessionManagement()
      .sessionCreationPolicy(SessionCreationPolicy.ALWAYS);
  ```

- **Stateless (para APIs RESTful):**
  ```java
  http.sessionManagement()
      .sessionCreationPolicy(SessionCreationPolicy.STATELESS);
  ```

---

### **7.7. Protección contra ataques de fuerza bruta**

Spring Security no ofrece esta funcionalidad por defecto, pero puedes implementarla usando un filtro o interceptador.

#### **Ejemplo de contador de intentos fallidos:**
```java
@Component
public class LoginAttemptService {
    private final Map<String, Integer> attemptsCache = new ConcurrentHashMap<>();

    public void loginFailed(String username) {
        attemptsCache.put(username, attemptsCache.getOrDefault(username, 0) + 1);
    }

    public boolean isBlocked(String username) {
        return attemptsCache.getOrDefault(username, 0) >= 5;  // Bloquea tras 5 intentos
    }
}
```

Integra este servicio en un `AuthenticationFailureHandler`.

---

### **7.8. Autenticación multifactor (MFA)**

La autenticación multifactor añade una capa extra de seguridad, como un código enviado al correo o móvil.

#### **Ejemplo básico de MFA:**
1. Genera un código único y envíalo al correo del usuario.
2. Requiere que el usuario ingrese el código antes de completar la autenticación.

```java
@Component
public class MfaService {
    private final Map<String, String> userCodes = new ConcurrentHashMap<>();

    public String generateCode(String username) {
        String code = String.valueOf(new Random().nextInt(900000) + 100000);  // Código de 6 dígitos
        userCodes.put(username, code);
        return code;
    }

    public boolean verifyCode(String username, String code) {
        return code.equals(userCodes.get(username));
    }
}
```

Integra esto en el flujo de autenticación.

---

### **Resumen**

1. **CSRF:** Protege contra solicitudes maliciosas enviadas desde otros sitios.
2. **CORS:** Configura qué dominios pueden acceder a tus recursos.
3. **HTTPS:** Asegura comunicaciones cifradas.
4. **Cabeceras de seguridad:** Mitiga ataques como XSS, clickjacking y más.
5. **Sesiones:** Configura políticas para usuarios concurrentes.
6. **Fuerza bruta:** Implementa medidas para limitar intentos de inicio de sesión.
7. **MFA:** Añade autenticación adicional para mayor seguridad.


## **8. Spring Security y APIs RESTful**

En el desarrollo de APIs RESTful, las consideraciones de seguridad son diferentes a las de aplicaciones tradicionales basadas en sesiones y formularios. Esta sección detalla cómo implementar una seguridad sólida para APIs RESTful utilizando Spring Security.

---

### **8.1. Características de seguridad en APIs RESTful**

1. **Stateless Authentication:** 
   - Las APIs RESTful son "stateless" (sin estado), lo que significa que no se almacenan datos de sesión entre solicitudes.
   - La autenticación se realiza con cada solicitud, generalmente usando tokens como JWT (JSON Web Token).

2. **HTTPS obligatorio:**
   - Asegúrate de que todas las comunicaciones entre el cliente y el servidor estén cifradas usando HTTPS.

3. **Métodos HTTP seguros:**
   - Limita los métodos HTTP utilizados: GET para lectura, POST para creación, PUT para actualización y DELETE para eliminación.

---

### **8.2. Configuración básica para APIs RESTful**

#### **1. Configuración Stateless**
En un entorno RESTful, las sesiones de usuario no deben ser utilizadas. Configura Spring Security para que sea stateless:
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.config.http.SessionCreationPolicy;

@Configuration
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf().disable()  // No necesario para APIs RESTful
            .sessionManagement()
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS)  // Stateless
            .and()
            .authorizeRequests()
                .antMatchers("/api/public/**").permitAll()  // Endpoints públicos
                .anyRequest().authenticated();             // Otros requieren autenticación
        return http.build();
    }
}
```

#### **2. Deshabilitar CSRF**
La protección contra CSRF no es necesaria en APIs RESTful porque no usan cookies de sesión. Para deshabilitar CSRF:
```java
http.csrf().disable();
```

---

### **8.3. Autenticación con JWT**

JWT (JSON Web Token) es un método popular para autenticar usuarios en APIs RESTful. Un JWT es un token compacto y seguro que contiene información del usuario y una firma para garantizar su validez.

#### **1. Flujo básico con JWT:**
1. El usuario envía sus credenciales (usuario y contraseña).
2. El servidor valida las credenciales y genera un token JWT firmado.
3. El cliente usa el token en el encabezado `Authorization` de futuras solicitudes.
4. El servidor valida el token en cada solicitud.

#### **2. Configuración de JWT**
##### **Generación del token**
Crea un servicio para generar JWTs:
```java
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import org.springframework.stereotype.Service;

import java.util.Date;

@Service
public class JwtService {

    private final String SECRET_KEY = "mi_clave_secreta";  // Debe ser segura y almacenada correctamente

    public String generateToken(String username) {
        return Jwts.builder()
            .setSubject(username)
            .setIssuedAt(new Date())
            .setExpiration(new Date(System.currentTimeMillis() + 1000 * 60 * 60))  // Expira en 1 hora
            .signWith(SignatureAlgorithm.HS256, SECRET_KEY)
            .compact();
    }
}
```

##### **Validación del token**
Crea un servicio para validar JWTs:
```java
import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jwts;
import org.springframework.stereotype.Service;

@Service
public class JwtService {

    private final String SECRET_KEY = "mi_clave_secreta";

    public String extractUsername(String token) {
        return extractClaims(token).getSubject();
    }

    public boolean isTokenValid(String token) {
        return extractClaims(token).getExpiration().after(new Date());
    }

    private Claims extractClaims(String token) {
        return Jwts.parser()
            .setSigningKey(SECRET_KEY)
            .parseClaimsJws(token)
            .getBody();
    }
}
```

---

#### **3. Configurar filtros JWT**

Crea un filtro para validar tokens JWT en cada solicitud:
```java
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.web.authentication.WebAuthenticationDetailsSource;
import org.springframework.stereotype.Component;
import org.springframework.web.filter.OncePerRequestFilter;

import javax.servlet.FilterChain;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@Component
public class JwtRequestFilter extends OncePerRequestFilter {

    private final JwtService jwtService;
    private final CustomUserDetailsService userDetailsService;

    public JwtRequestFilter(JwtService jwtService, CustomUserDetailsService userDetailsService) {
        this.jwtService = jwtService;
        this.userDetailsService = userDetailsService;
    }

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain chain)
            throws ServletException, IOException {

        final String authorizationHeader = request.getHeader("Authorization");

        String username = null;
        String jwt = null;

        if (authorizationHeader != null && authorizationHeader.startsWith("Bearer ")) {
            jwt = authorizationHeader.substring(7);
            username = jwtService.extractUsername(jwt);
        }

        if (username != null && SecurityContextHolder.getContext().getAuthentication() == null) {
            UserDetails userDetails = userDetailsService.loadUserByUsername(username);
            if (jwtService.isTokenValid(jwt)) {
                UsernamePasswordAuthenticationToken authToken =
                        new UsernamePasswordAuthenticationToken(userDetails, null, userDetails.getAuthorities());
                authToken.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));
                SecurityContextHolder.getContext().setAuthentication(authToken);
            }
        }
        chain.doFilter(request, response);
    }
}
```

Registra el filtro en la configuración de Spring Security:
```java
http.addFilterBefore(jwtRequestFilter, UsernamePasswordAuthenticationFilter.class);
```

---

### **8.4. Control de acceso a endpoints**

Usa roles y permisos para proteger endpoints específicos en tu API RESTful.

#### **Ejemplo de configuración:**
```java
http.authorizeRequests()
    .antMatchers("/api/admin/**").hasRole("ADMIN")
    .antMatchers("/api/user/**").hasAnyRole("USER", "ADMIN")
    .antMatchers("/api/public/**").permitAll()
    .anyRequest().authenticated();
```

---

### **8.5. Seguridad adicional**

#### **1. Límite de velocidad (Rate Limiting)**
Evita ataques de fuerza bruta o abuso de la API implementando un límite de solicitudes por usuario o dirección IP. Herramientas como Bucket4j o Redis son útiles para esto.

#### **2. Protección contra ataques de fuerza bruta**
Combina un contador de intentos de inicio de sesión fallidos con bloqueo temporal:
```java
@Component
public class LoginAttemptService {
    private final Map<String, Integer> attemptsCache = new ConcurrentHashMap<>();

    public void loginFailed(String username) {
        attemptsCache.put(username, attemptsCache.getOrDefault(username, 0) + 1);
    }

    public boolean isBlocked(String username) {
        return attemptsCache.getOrDefault(username, 0) >= 5;  // Bloquea tras 5 intentos
    }
}
```

---

### **8.6. Documentación de la API**

Asegúrate de documentar claramente los requisitos de autenticación y autorización de tu API RESTful. Swagger es una herramienta popular para esto. Con Spring Boot, puedes integrarlo fácilmente usando `springdoc-openapi`.

---

### **Resumen**

1. Configura Spring Security para un entorno stateless.
2. Usa JWT para autenticación y autorización en APIs RESTful.
3. Implementa protección avanzada como límites de solicitudes y manejo de errores.
4. Documenta tu API para facilitar su uso.


## **Filtros en Spring Security**

Los **filtros** son un componente esencial de Spring Security que permite interceptar, procesar y modificar solicitudes HTTP antes de que lleguen al controlador o después de que se hayan procesado. Los filtros en Spring Security forman parte de una **cadena de filtros (filter chain)** que determina cómo se maneja la autenticación, autorización y otras configuraciones de seguridad.

---

### **¿Qué es un filtro?**

Un **filtro** es una clase que implementa la interfaz `javax.servlet.Filter`. Se ejecuta para cada solicitud entrante y puede realizar tareas como:

1. Autenticación del usuario.
2. Verificación de permisos.
3. Validación de tokens (como JWT).
4. Modificación de cabeceras o datos en la solicitud o respuesta.
5. Registro (logging) de solicitudes.

En el caso de Spring Security, la lógica de seguridad se implementa principalmente en los filtros.

---

### **Cadena de filtros (Filter Chain)**

La **cadena de filtros** de Spring Security es un conjunto de filtros que se ejecutan en orden. Cada filtro tiene una responsabilidad específica y pasa la solicitud al siguiente filtro en la cadena una vez que su tarea ha sido completada.

#### **Estructura típica de la cadena de filtros:**
1. **`SecurityContextPersistenceFilter`**: Administra el contexto de seguridad (`SecurityContext`), que almacena información de autenticación.
2. **`CsrfFilter`**: Maneja la protección contra ataques CSRF.
3. **`UsernamePasswordAuthenticationFilter`**: Procesa las credenciales de inicio de sesión (usuario y contraseña).
4. **`BearerTokenAuthenticationFilter`**: Maneja la autenticación basada en tokens.
5. **`ExceptionTranslationFilter`**: Captura excepciones relacionadas con la seguridad y envía respuestas adecuadas.
6. **`FilterSecurityInterceptor`**: Autoriza el acceso a los recursos solicitados.

El orden de los filtros es importante porque cada filtro depende del resultado de los filtros anteriores.

---

### **Cómo funcionan los filtros en Spring Security**

Cuando llega una solicitud HTTP, Spring Security la pasa a través de la cadena de filtros. Cada filtro realiza una tarea específica y puede:
1. Procesar y modificar la solicitud antes de enviarla al controlador.
2. Procesar y modificar la respuesta después de que el controlador haya ejecutado la lógica del negocio.

#### **Flujo básico:**
1. **Solicitud HTTP entra en el servidor.**
2. **Spring Security redirige la solicitud a la cadena de filtros.**
3. Cada filtro ejecuta su lógica de seguridad y decide si:
   - Permitir que la solicitud continúe hacia el siguiente filtro.
   - Detener la solicitud y enviar una respuesta al cliente (por ejemplo, `403 Forbidden`).
4. Si se pasa toda la cadena, la solicitud llega al controlador.

---

### **Tipos de filtros en Spring Security**

#### **1. Filtros predefinidos**
Spring Security incluye varios filtros listos para usar, cada uno con responsabilidades específicas:

| **Filtro**                           | **Descripción**                                                                 |
|--------------------------------------|---------------------------------------------------------------------------------|
| `UsernamePasswordAuthenticationFilter` | Maneja la autenticación basada en formularios (usuario y contraseña).           |
| `BasicAuthenticationFilter`           | Procesa autenticación HTTP básica.                                              |
| `BearerTokenAuthenticationFilter`     | Verifica y valida tokens Bearer (como JWT).                                     |
| `CsrfFilter`                          | Protege contra ataques de Cross-Site Request Forgery (CSRF).                    |
| `SecurityContextPersistenceFilter`    | Administra el contexto de seguridad entre solicitudes.                          |
| `ExceptionTranslationFilter`          | Maneja excepciones relacionadas con la seguridad (como acceso no autorizado).   |
| `FilterSecurityInterceptor`           | Autoriza el acceso a recursos según las reglas de seguridad configuradas.       |

---

#### **2. Filtros personalizados**
Puedes crear tus propios filtros para manejar casos de seguridad específicos.

#### **Cómo crear un filtro personalizado**
1. **Extiende la clase `OncePerRequestFilter`**:
   - Es una clase base de Spring que garantiza que el filtro se ejecute solo una vez por solicitud.

2. **Sobrescribe el método `doFilterInternal`**:
   - Este método contiene la lógica del filtro.

3. **Registra el filtro en la cadena de filtros**.

##### **Ejemplo: Filtro personalizado para validar tokens JWT**
```java
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.web.authentication.WebAuthenticationDetailsSource;
import org.springframework.stereotype.Component;
import org.springframework.web.filter.OncePerRequestFilter;

import javax.servlet.FilterChain;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@Component
public class JwtAuthenticationFilter extends OncePerRequestFilter {

    private final JwtService jwtService;
    private final CustomUserDetailsService userDetailsService;

    public JwtAuthenticationFilter(JwtService jwtService, CustomUserDetailsService userDetailsService) {
        this.jwtService = jwtService;
        this.userDetailsService = userDetailsService;
    }

    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain) throws IOException, ServletException {

        // Obtener el token de la cabecera "Authorization"
        String authorizationHeader = request.getHeader("Authorization");

        String username = null;
        String jwt = null;

        if (authorizationHeader != null && authorizationHeader.startsWith("Bearer ")) {
            jwt = authorizationHeader.substring(7);
            username = jwtService.extractUsername(jwt);  // Extraer el usuario del token
        }

        // Si el usuario está autenticado, establece el contexto de seguridad
        if (username != null && SecurityContextHolder.getContext().getAuthentication() == null) {
            UserDetails userDetails = userDetailsService.loadUserByUsername(username);

            if (jwtService.isTokenValid(jwt)) {  // Valida el token
                var authentication = new UsernamePasswordAuthenticationToken(
                        userDetails, null, userDetails.getAuthorities());
                authentication.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));
                SecurityContextHolder.getContext().setAuthentication(authentication);
            }
        }

        // Continúa con la cadena de filtros
        filterChain.doFilter(request, response);
    }
}
```

---

### **Registrar un filtro en la cadena de Spring Security**

Puedes registrar un filtro personalizado en tu configuración de seguridad. Por ejemplo:
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;

@Configuration
public class SecurityConfig {

    private final JwtAuthenticationFilter jwtAuthenticationFilter;

    public SecurityConfig(JwtAuthenticationFilter jwtAuthenticationFilter) {
        this.jwtAuthenticationFilter = jwtAuthenticationFilter;
    }

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf().disable()
            .authorizeRequests()
                .antMatchers("/api/public/**").permitAll()
                .anyRequest().authenticated()
            .and()
            .addFilterBefore(jwtAuthenticationFilter, UsernamePasswordAuthenticationFilter.class);  // Registra el filtro

        return http.build();
    }
}
```

---

### **Prioridad de los filtros**

El orden de los filtros es fundamental para el correcto funcionamiento de la seguridad. Puedes controlar el orden usando el método `addFilterBefore` o `addFilterAfter`:

- **`addFilterBefore`:** Coloca el filtro antes de otro filtro especificado.
- **`addFilterAfter`:** Coloca el filtro después de otro filtro especificado.
- **`addFilter`:** Añade el filtro en una posición personalizada.

Por ejemplo:
```java
http.addFilterBefore(myCustomFilter, UsernamePasswordAuthenticationFilter.class);
```

---

### **Ventajas del enfoque basado en filtros**
1. **Reutilización:** Los filtros pueden manejar tareas comunes (como validación de tokens) para múltiples solicitudes.
2. **Separación de responsabilidades:** Cada filtro tiene una responsabilidad específica.
3. **Flexibilidad:** Puedes agregar, quitar o modificar filtros según las necesidades del proyecto.

---

## **9. Integración con OAuth2 y OpenID Connect en Spring Security**

OAuth2 y OpenID Connect (OIDC) son protocolos ampliamente utilizados para manejar la **autenticación** y **autorización** en aplicaciones modernas. Spring Security proporciona un conjunto robusto de herramientas para integrar estos protocolos de manera eficiente.

---

### **9.1. ¿Qué son OAuth2 y OpenID Connect?**

#### **OAuth2 (Autorización)**
OAuth2 es un protocolo estándar que permite a las aplicaciones obtener acceso limitado a los recursos de un usuario en un servidor (como su cuenta de Google, GitHub, etc.) sin necesidad de compartir sus credenciales (contraseña). Proporciona un mecanismo para delegar permisos.

**Flujo básico:**
1. El usuario concede permiso a la aplicación.
2. La aplicación obtiene un "access token" para acceder a los recursos del usuario.

#### **OpenID Connect (Autenticación + Autorización)**
OIDC es una extensión de OAuth2 que agrega capacidades de **autenticación**. Permite que una aplicación verifique la identidad de un usuario y obtenga información sobre él mediante un "ID Token".

**Diferencias clave:**
- OAuth2: "¿La aplicación tiene permiso para acceder a este recurso?"
- OIDC: "¿Quién es el usuario que intenta acceder?"

---

### **9.2. Casos de uso comunes**

1. **Inicio de sesión social** (Google, Facebook, GitHub, etc.).
2. **Integración con proveedores de identidad corporativos** (Azure AD, Okta, Keycloak).
3. **Autorización de terceros** para acceder a recursos protegidos en nombre del usuario.

---

### **9.3. Configuración básica de OAuth2 en Spring Security**

#### **Dependencias necesarias**
Incluye las dependencias en `pom.xml` o `build.gradle`:
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-oauth2-client</artifactId>
</dependency>
```

#### **Configuración del cliente OAuth2**
Configura el proveedor de OAuth2 en el archivo `application.yml` o `application.properties`.

**Ejemplo (Google como proveedor de OAuth2):**
```yaml
spring:
  security:
    oauth2:
      client:
        registration:
          google:                # Identificador del proveedor
            client-id: CLIENT_ID
            client-secret: CLIENT_SECRET
            scope: openid, profile, email
            redirect-uri: "{baseUrl}/login/oauth2/code/google"
            authorization-grant-type: authorization_code
            client-name: Google
        provider:
          google:
            authorization-uri: https://accounts.google.com/o/oauth2/v2/auth
            token-uri: https://oauth2.googleapis.com/token
            user-info-uri: https://openidconnect.googleapis.com/v1/userinfo
            user-name-attribute: sub
```

En este ejemplo:
- **client-id**: ID único que identifica tu aplicación en el proveedor de OAuth2.
- **client-secret**: Clave secreta proporcionada por el proveedor.
- **redirect-uri**: URL donde el proveedor redirige tras la autenticación.

#### **Habilitar autenticación OAuth2**
Spring Security configura automáticamente los endpoints necesarios para manejar OAuth2. Solo necesitas especificar las rutas protegidas.

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/", "/login").permitAll()
                .anyRequest().authenticated()
            .and()
            .oauth2Login();  // Habilita inicio de sesión con OAuth2

        return http.build();
    }
}
```

Con esto:
- El usuario será redirigido al proveedor de OAuth2 para autenticarse.
- Una vez autenticado, el usuario será redirigido a tu aplicación.

---

### **9.4. Personalización del inicio de sesión con OAuth2**

#### **1. Redirigir a una página personalizada después del inicio de sesión**
Puedes personalizar el flujo de redirección utilizando un `SuccessHandler`:
```java
import org.springframework.security.core.Authentication;
import org.springframework.security.web.authentication.SimpleUrlAuthenticationSuccessHandler;
import org.springframework.stereotype.Component;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@Component
public class CustomOAuth2SuccessHandler extends SimpleUrlAuthenticationSuccessHandler {

    @Override
    public void onAuthenticationSuccess(HttpServletRequest request, HttpServletResponse response, Authentication authentication) throws IOException {
        // Lógica personalizada tras el inicio de sesión
        response.sendRedirect("/dashboard");
    }
}
```

Registra este manejador en la configuración de seguridad:
```java
http.oauth2Login().successHandler(customOAuth2SuccessHandler);
```

---

#### **2. Acceder a la información del usuario autenticado**
Spring Security guarda la información del usuario en el contexto de seguridad. Puedes acceder a esta información mediante:
```java
import org.springframework.security.core.annotation.AuthenticationPrincipal;
import org.springframework.security.oauth2.core.user.OAuth2User;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class UserController {

    @GetMapping("/user")
    public String getUserInfo(@AuthenticationPrincipal OAuth2User principal) {
        return principal.getAttributes().toString();  // Devuelve los datos del usuario
    }
}
```

---

### **9.5. Uso de OpenID Connect (OIDC)**

Para integrar OpenID Connect, configura un proveedor compatible con OIDC. Por ejemplo:
```yaml
spring:
  security:
    oauth2:
      client:
        registration:
          google:
            client-id: CLIENT_ID
            client-secret: CLIENT_SECRET
            scope: openid, profile, email
            authorization-grant-type: authorization_code
```

Cuando `openid` está incluido en el alcance (`scope`), el proveedor devuelve un **ID Token** con información adicional sobre el usuario.

#### **Acceder al ID Token**
Puedes extraer el ID Token desde la autenticación:
```java
import org.springframework.security.core.Authentication;
import org.springframework.security.oauth2.core.oidc.user.OidcUser;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class OIDCController {

    @GetMapping("/id-token")
    public String getIdToken(Authentication authentication) {
        OidcUser oidcUser = (OidcUser) authentication.getPrincipal();
        return oidcUser.getIdToken().getTokenValue();  // Devuelve el ID Token
    }
}
```

---

### **9.6. Autorización avanzada con scopes y roles**

#### **Restricción por scope**
Puedes proteger rutas según los permisos (`scopes`) otorgados por el proveedor de OAuth2:
```java
http.authorizeRequests()
    .antMatchers("/api/admin/**").hasAuthority("SCOPE_admin")
    .antMatchers("/api/user/**").hasAuthority("SCOPE_user")
    .anyRequest().authenticated();
```

---

### **9.7. Uso de un servidor de identidad local (como Keycloak)**

Spring Security también puede integrarse con servidores de identidad locales como Keycloak u Okta. La configuración es similar:
1. Registra tu aplicación como cliente en el servidor de identidad.
2. Usa las URL de autorización, token y usuario del servidor.

**Ejemplo: Configuración de Keycloak**
```yaml
spring:
  security:
    oauth2:
      client:
        registration:
          keycloak:
            client-id: my-client-id
            client-secret: my-client-secret
            authorization-grant-type: authorization_code
            scope: openid, profile
        provider:
          keycloak:
            authorization-uri: http://localhost:8080/auth/realms/myrealm/protocol/openid-connect/auth
            token-uri: http://localhost:8080/auth/realms/myrealm/protocol/openid-connect/token
            user-info-uri: http://localhost:8080/auth/realms/myrealm/protocol/openid-connect/userinfo
```

---

### **Resumen**

- **OAuth2** permite la autorización delegada para acceder a recursos del usuario.
- **OIDC** agrega autenticación al flujo OAuth2, permitiendo verificar la identidad del usuario.
- Con Spring Security, puedes implementar autenticación y autorización basadas en estos protocolos con configuraciones simples y extensibles.
- Es posible personalizar el flujo de inicio de sesión, redirecciones, y acceder a datos del usuario autenticado.

¿Te gustaría un ejemplo práctico con un servidor OAuth2 como Keycloak o prefieres avanzar a la siguiente sección?

## **10. Seguridad Reactiva (Spring WebFlux)**

Spring WebFlux es un marco de trabajo reactivo que utiliza el modelo de programación no bloqueante para manejar grandes volúmenes de solicitudes de manera eficiente. Spring Security ofrece soporte integrado para proteger aplicaciones reactivas con características diseñadas específicamente para este paradigma.

---

### **10.1. Diferencias entre seguridad en Spring MVC y WebFlux**

| **Aspecto**              | **Spring MVC (Bloqueante)**                | **Spring WebFlux (Reactivo)**                 |
|--------------------------|-------------------------------------------|-----------------------------------------------|
| **Modelo de programación** | Basado en hilos tradicionales (`Servlet`) | Basado en programación reactiva (Netty, etc.) |
| **Seguridad**            | Cadena de filtros bloqueantes             | Cadena de filtros reactivos                  |
| **Estado**               | Puede mantener sesiones                   | Predomina la seguridad sin estado (JWT, tokens) |
| **API principal**        | `HttpServletRequest`                      | `ServerHttpRequest`, `Mono`, `Flux`          |

---

### **10.2. Configuración básica de seguridad en WebFlux**

#### **Dependencias necesarias**
En un proyecto reactivo, agrega las siguientes dependencias al archivo `pom.xml`:
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-webflux</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt</artifactId>
    <version>0.9.1</version> <!-- O la última versión disponible -->
</dependency>
```

---

#### **Estructura básica del filtro de seguridad reactivo**
El flujo de seguridad en WebFlux utiliza la clase `SecurityWebFilterChain` para definir configuraciones de seguridad. 

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.web.server.ServerHttpSecurity;
import org.springframework.security.web.server.SecurityWebFilterChain;

@Configuration
public class SecurityConfig {

    @Bean
    public SecurityWebFilterChain securityWebFilterChain(ServerHttpSecurity http) {
        return http
            .authorizeExchange()
                .pathMatchers("/public/**").permitAll()  // Acceso libre
                .anyExchange().authenticated()          // Cualquier otra ruta requiere autenticación
            .and()
            .httpBasic()  // Habilita autenticación básica (opcional)
            .csrf().disable() // Deshabilita CSRF para APIs (opcional)
            .build();
    }
}
```

**Explicación:**
- `authorizeExchange()`: Configura permisos para diferentes rutas.
- `httpBasic()`: Habilita la autenticación HTTP básica.
- `csrf().disable()`: Deshabilita CSRF, especialmente útil para APIs sin estado.

---

### **10.3. Uso de JWT en aplicaciones reactivas**

La seguridad reactiva se adapta muy bien al uso de **JSON Web Tokens (JWT)**, ya que es un esquema sin estado que no requiere sesiones en el servidor.

#### **Generación de JWT**
Crea un método para generar tokens al autenticar un usuario.

```java
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;

import java.util.Date;

public class JwtUtil {

    private static final String SECRET_KEY = "mysecretkey";
    private static final long EXPIRATION_TIME = 86400000; // 1 día en milisegundos

    public static String generateToken(String username) {
        return Jwts.builder()
                .setSubject(username)
                .setIssuedAt(new Date())
                .setExpiration(new Date(System.currentTimeMillis() + EXPIRATION_TIME))
                .signWith(SignatureAlgorithm.HS512, SECRET_KEY)
                .compact();
    }
}
```

#### **Validación de JWT**
Verifica el token en un filtro personalizado.

```java
import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jwts;

public class JwtUtil {

    public static Claims validateToken(String token) {
        return Jwts.parser()
                .setSigningKey(SECRET_KEY)
                .parseClaimsJws(token)
                .getBody();
    }
}
```

#### **Filtro para validar JWT**
Crea un filtro que valide el JWT y lo añada al contexto reactivo de seguridad.

```java
import org.springframework.security.core.context.ReactiveSecurityContextHolder;
import org.springframework.security.core.context.SecurityContext;
import org.springframework.security.core.context.SecurityContextImpl;
import org.springframework.security.web.server.authentication.ServerAuthenticationConverter;
import org.springframework.security.web.server.context.ServerSecurityContextRepository;
import org.springframework.stereotype.Component;
import reactor.core.publisher.Mono;

@Component
public class SecurityContextRepository implements ServerSecurityContextRepository {

    @Override
    public Mono<Void> save(ServerWebExchange exchange, SecurityContext context) {
        throw new UnsupportedOperationException("Save is not supported");
    }

    @Override
    public Mono<SecurityContext> load(ServerWebExchange exchange) {
        String authHeader = exchange.getRequest().getHeaders().getFirst("Authorization");
        if (authHeader != null && authHeader.startsWith("Bearer ")) {
            String token = authHeader.substring(7);
            Claims claims = JwtUtil.validateToken(token);

            // Aquí puedes construir el contexto de seguridad
            return Mono.just(new SecurityContextImpl(authentication));
        }
        return Mono.empty();
    }
}
```

Configura este filtro en la cadena de seguridad:

```java
@Bean
public SecurityWebFilterChain securityWebFilterChain(ServerHttpSecurity http) {
    return http
        .authorizeExchange()
            .pathMatchers("/public/**").permitAll()
            .anyExchange().authenticated()
        .and()
        .addFilterAt(jwtAuthenticationWebFilter(), SecurityWebFiltersOrder.AUTHENTICATION)
        .csrf().disable()
        .build();
}
```

---

### **10.4. Personalización del flujo reactivo**

Puedes personalizar aspectos como:
- Redirección tras inicio de sesión o acceso denegado.
- Autenticación personalizada con `ReactiveAuthenticationManager`.

Ejemplo: Gestión personalizada de errores:
```java
@Bean
public SecurityWebFilterChain securityWebFilterChain(ServerHttpSecurity http) {
    return http
        .exceptionHandling()
            .authenticationEntryPoint((exchange, ex) -> {
                exchange.getResponse().setStatusCode(HttpStatus.UNAUTHORIZED);
                return exchange.getResponse().setComplete();
            })
            .accessDeniedHandler((exchange, ex) -> {
                exchange.getResponse().setStatusCode(HttpStatus.FORBIDDEN);
                return exchange.getResponse().setComplete();
            })
        .and()
        .build();
}
```

---

### **10.5. Buenas prácticas en aplicaciones reactivas**

1. **Usar JWT para seguridad sin estado**:
   - Reduce la carga en el servidor.
   - Compatible con servicios distribuidos.

2. **Proteger rutas específicas**:
   - Define claramente qué rutas son públicas y cuáles requieren autenticación.

3. **Evitar operaciones bloqueantes**:
   - Usa métodos no bloqueantes siempre que sea posible.

4. **Auditar y monitorear la seguridad**:
   - Implementa registros detallados de accesos y errores de seguridad.

---

### **Resumen**
- Spring WebFlux utiliza una cadena de filtros reactivos para manejar la seguridad.
- JWT es una solución ideal para la seguridad sin estado en aplicaciones reactivas.
- Spring Security en WebFlux permite una personalización completa de flujos de autenticación y autorización, adaptándose al paradigma no bloqueante.

---

## **11. Cómo Definir Roles y Darles Permisos en Spring Security**

La gestión de roles y permisos es una parte clave de la seguridad en cualquier aplicación. En Spring Security, los **roles** representan un nivel de acceso, mientras que los **permisos** permiten un control más granular sobre los recursos y operaciones que un usuario puede realizar. 

En esta sección, explicaremos cómo definir roles y asignarles permisos utilizando las herramientas de Spring Security, tanto a nivel de código como con bases de datos.

---

### **11.1. ¿Qué son roles y permisos en Spring Security?**

1. **Roles:**
   - Son identificadores generales que agrupan privilegios.
   - Ejemplo: `ADMIN`, `USER`, `MODERATOR`.

2. **Permisos:**
   - Son acciones específicas o privilegios sobre recursos.
   - Ejemplo: `READ_PRIVILEGES`, `WRITE_PRIVILEGES`.

En Spring Security, los roles generalmente tienen el prefijo `ROLE_` por convención (por ejemplo, `ROLE_ADMIN`), pero esto se puede personalizar.

---

### **11.2. Configuración básica de roles**

#### **Definir roles en memoria**
Puedes definir roles y asignarles permisos directamente en la configuración de Spring Security, ideal para aplicaciones simples o de prueba.

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.crypto.password.NoOpPasswordEncoder;

@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        return http
            .authorizeRequests()
                .antMatchers("/admin/**").hasRole("ADMIN")
                .antMatchers("/user/**").hasRole("USER")
                .antMatchers("/public/**").permitAll()
                .anyRequest().authenticated()
            .and()
            .httpBasic()
            .and()
            .build();
    }

    @Bean
    public InMemoryUserDetailsManager userDetailsService() {
        UserDetails admin = User.withUsername("admin")
                .password("{noop}adminpass")  // {noop} indica que no se usa encriptación
                .roles("ADMIN")              // Define el rol
                .build();

        UserDetails user = User.withUsername("user")
                .password("{noop}userpass")
                .roles("USER")
                .build();

        return new InMemoryUserDetailsManager(admin, user);
    }
}
```

#### **Puntos clave:**
1. `hasRole("ADMIN")`: Protege las rutas para que solo usuarios con el rol `ADMIN` puedan acceder.
2. `roles("ADMIN")`: Asigna un rol al usuario.

---

### **11.3. Gestión avanzada de roles y permisos**

En aplicaciones más complejas, es común almacenar roles y permisos en una base de datos y cargarlos dinámicamente.

#### **Definir tablas para roles y permisos**

Modelo de datos básico:

```sql
CREATE TABLE users (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    enabled BOOLEAN DEFAULT TRUE
);

CREATE TABLE roles (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50) NOT NULL UNIQUE
);

CREATE TABLE user_roles (
    user_id BIGINT NOT NULL,
    role_id BIGINT NOT NULL,
    FOREIGN KEY (user_id) REFERENCES users(id),
    FOREIGN KEY (role_id) REFERENCES roles(id)
);

CREATE TABLE permissions (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50) NOT NULL UNIQUE
);

CREATE TABLE role_permissions (
    role_id BIGINT NOT NULL,
    permission_id BIGINT NOT NULL,
    FOREIGN KEY (role_id) REFERENCES roles(id),
    FOREIGN KEY (permission_id) REFERENCES permissions(id)
);
```

#### **Carga dinámica de roles y permisos desde la base de datos**

1. **Configura un servicio `UserDetailsService` personalizado:**

```java
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

import java.util.List;
import java.util.stream.Collectors;

@Service
public class CustomUserDetailsService implements UserDetailsService {

    private final UserRepository userRepository;

    public CustomUserDetailsService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        User user = userRepository.findByUsername(username)
                .orElseThrow(() -> new UsernameNotFoundException("Usuario no encontrado"));

        List<GrantedAuthority> authorities = user.getRoles().stream()
                .flatMap(role -> role.getPermissions().stream())
                .map(permission -> new SimpleGrantedAuthority(permission.getName()))
                .collect(Collectors.toList());

        return new org.springframework.security.core.userdetails.User(
                user.getUsername(),
                user.getPassword(),
                authorities
        );
    }
}
```

2. **Configura el proveedor de autenticación:**

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.config.annotation.authentication.configuration.AuthenticationConfiguration;

@Configuration
public class SecurityConfig {

    private final CustomUserDetailsService customUserDetailsService;

    public SecurityConfig(CustomUserDetailsService customUserDetailsService) {
        this.customUserDetailsService = customUserDetailsService;
    }

    @Bean
    public AuthenticationManager authenticationManager(AuthenticationConfiguration config) throws Exception {
        return config.getAuthenticationManager();
    }
}
```

3. **Define reglas de acceso por permisos:**

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    return http
        .authorizeRequests()
            .antMatchers("/admin/**").hasAuthority("MANAGE_USERS")
            .antMatchers("/user/**").hasAuthority("VIEW_DASHBOARD")
            .antMatchers("/public/**").permitAll()
            .anyRequest().authenticated()
        .and()
        .httpBasic()
        .and()
        .build();
}
```

---

### **11.4. Uso de anotaciones para roles y permisos**

En lugar de configurar las reglas en el archivo de seguridad, puedes usar anotaciones en los controladores.

1. **`@Secured`:**
   - Permite definir roles específicos para acceder a un método.

```java
import org.springframework.security.access.annotation.Secured;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class AdminController {

    @Secured("ROLE_ADMIN")
    @GetMapping("/admin/dashboard")
    public String adminDashboard() {
        return "Panel de administrador";
    }
}
```

2. **`@PreAuthorize`:**
   - Permite definir expresiones más complejas para roles y permisos.

```java
import org.springframework.security.access.prepost.PreAuthorize;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class UserController {

    @PreAuthorize("hasAuthority('VIEW_DASHBOARD')")
    @GetMapping("/user/dashboard")
    public String userDashboard() {
        return "Panel de usuario";
    }
}
```

Para que estas anotaciones funcionen, habilítalas en tu aplicación:

```java
@EnableGlobalMethodSecurity(prePostEnabled = true, securedEnabled = true)
public class MethodSecurityConfig extends GlobalMethodSecurityConfiguration {
}
```

---

### **11.5. Buenas prácticas**

1. **Uso de permisos granulares:**
   - Define permisos claros como `CREATE_POST`, `DELETE_USER`, etc., en lugar de roles genéricos.

2. **Mantenibilidad:**
   - Usa bases de datos para roles y permisos en aplicaciones grandes, ya que facilita la administración.

3. **Evitar roles "monolíticos":**
   - Evita roles que contengan demasiados permisos. Divide privilegios por áreas funcionales.

4. **Pruebas de seguridad:**
   - Verifica que cada ruta o acción esté protegida adecuadamente.

---

### **Resumen**

Spring Security ofrece múltiples enfoques para manejar roles y permisos:
- Configuración en memoria para proyectos simples.
- Bases de datos para sistemas más complejos.
- Uso de anotaciones para facilitar el control a nivel de métodos.


## Algunas anotaciones:
Spring Security ofrece una variedad de anotaciones que facilitan la configuración de seguridad a nivel de clase, método o endpoint. A continuación, te doy un resumen de las principales anotaciones que puedes usar en Spring Security, su propósito y cómo aplicarlas:

---

### **1. `@EnableWebSecurity`**
- **Propósito:** Habilita la configuración personalizada de Spring Security en tu aplicación.
- **Uso común:**
  - Se utiliza en clases de configuración para activar la seguridad web personalizada.
- **Ejemplo:**
  ```java
  @Configuration
  @EnableWebSecurity
  public class SecurityConfig {
      // Configuración de seguridad aquí
  }
  ```
- **Nota:** En Spring Boot no siempre es necesaria, ya que Spring Boot detecta automáticamente la configuración.

---

### **2. `@PreAuthorize`**
- **Propósito:** Restringe el acceso a métodos basándose en roles o authorities antes de que se ejecute el método.
- **Requiere:** Habilitar con `@EnableGlobalMethodSecurity(prePostEnabled = true)`.
- **Uso común:**
  - Se utiliza en servicios o controladores para aplicar reglas de seguridad específicas.
- **Ejemplo:**
  ```java
  @PreAuthorize("hasRole('ADMIN') and hasAuthority('MANAGE_USERS')")
  public void manageUsers() {
      // Lógica del método
  }
  ```

#### **Expresiones más usadas:**
- `hasRole('ROLE_NAME')`: Verifica si el usuario tiene un rol específico.
- `hasAuthority('AUTHORITY_NAME')`: Verifica si el usuario tiene una autoridad específica.
- Operadores lógicos:
  - `and`, `or`, `not`.

---

### **3. `@PostAuthorize`**
- **Propósito:** Restringe el acceso **después** de que se ejecute el método. Útil para validar el resultado de un método.
- **Requiere:** Habilitar con `@EnableGlobalMethodSecurity(prePostEnabled = true)`.
- **Uso común:**
  - Se utiliza para filtrar los datos retornados por un método según las reglas de seguridad.
- **Ejemplo:**
  ```java
  @PostAuthorize("returnObject.owner == authentication.name")
  public User getUserDetails(String id) {
      return userService.findById(id); // Verifica si el usuario autenticado es el propietario
  }
  ```

---

### **4. `@Secured`**
- **Propósito:** Restringe el acceso a métodos basándose en roles.
- **Requiere:** Habilitar con `@EnableGlobalMethodSecurity(securedEnabled = true)`.
- **Uso común:**
  - Más limitada que `@PreAuthorize`, ya que no permite expresiones complejas ni authorities, solo roles.
- **Ejemplo:**
  ```java
  @Secured("ROLE_ADMIN")
  public void deleteUser(Long id) {
      // Lógica del método
  }
  ```

- **Nota:** Usa roles directamente con el prefijo `ROLE_`. No soporta expresiones complejas como `hasAuthority`.

---

### **5. `@RolesAllowed`**
- **Propósito:** Similar a `@Secured`, pero se basa en la especificación de seguridad de Java EE.
- **Requiere:** Habilitar con `@EnableGlobalMethodSecurity(jsr250Enabled = true)`.
- **Uso común:**
  - Se utiliza para restringir acceso basado en roles, generalmente en aplicaciones compatibles con JSR-250.
- **Ejemplo:**
  ```java
  @RolesAllowed("ROLE_USER")
  public void viewProfile() {
      // Lógica del método
  }
  ```

- **Diferencia con `@Secured`:** Es parte de las especificaciones JSR-250 y soporta más interoperabilidad con otras plataformas.

---

### **6. `@WithMockUser`**
- **Propósito:** Crea un usuario simulado para pruebas en métodos de test.
- **Uso común:**
  - En pruebas unitarias o de integración para simular la autenticación de un usuario.
- **Ejemplo:**
  ```java
  @Test
  @WithMockUser(username = "testUser", roles = {"USER"})
  public void testGetProfile() throws Exception {
      mockMvc.perform(get("/profile"))
             .andExpect(status().isOk());
  }
  ```

---

### **7. `@AuthenticationPrincipal`**
- **Propósito:** Permite inyectar el usuario autenticado directamente en un controlador o método.
- **Uso común:**
  - Se usa para acceder al principal (usuario autenticado) en métodos del controlador.
- **Ejemplo:**
  ```java
  @GetMapping("/profile")
  public String getProfile(@AuthenticationPrincipal UserDetails userDetails) {
      return "Hola " + userDetails.getUsername();
  }
  ```

---

### **8. `@PermitAll` y `@DenyAll`**
- **Propósito:** Permiten o deniegan acceso a todos los usuarios.
- **Requiere:** Habilitar con `@EnableGlobalMethodSecurity(jsr250Enabled = true)`.
- **Uso común:**
  - Se utiliza para marcar métodos o clases con acceso abierto o cerrado.
- **Ejemplo:**
  ```java
  @PermitAll
  public String publicResource() {
      return "Recurso público";
  }

  @DenyAll
  public String privateResource() {
      return "Acceso denegado";
  }
  ```

---

### **9. `@EnableGlobalMethodSecurity`**
- **Propósito:** Habilita el uso de anotaciones como `@PreAuthorize`, `@PostAuthorize`, `@Secured` y `@RolesAllowed`.
- **Configuración:**
  - Debe incluirse en la configuración de seguridad.
- **Ejemplo:**
  ```java
  @Configuration
  @EnableGlobalMethodSecurity(prePostEnabled = true, securedEnabled = true, jsr250Enabled = true)
  public class MethodSecurityConfig {
      // Configuración de seguridad
  }
  ```

---

### **10. `@CrossOrigin`**
- **Propósito:** Permite el acceso a recursos desde dominios cruzados (CORS) en controladores específicos.
- **Uso común:**
  - Útil para habilitar CORS en aplicaciones RESTful.
- **Ejemplo:**
  ```java
  @RestController
  @CrossOrigin(origins = "http://example.com")
  public class ApiController {
      @GetMapping("/data")
      public String getData() {
          return "Datos";
      }
  }
  ```

---

### **Resumen general**

| **Anotación**           | **Propósito**                                                                                      |
|-------------------------|--------------------------------------------------------------------------------------------------|
| `@EnableWebSecurity`     | Habilita la configuración de seguridad personalizada.                                            |
| `@PreAuthorize`          | Restringe acceso **antes** de ejecutar un método, con soporte para expresiones complejas.        |
| `@PostAuthorize`         | Restringe acceso **después** de ejecutar un método.                                              |
| `@Secured`               | Restringe acceso basado en roles (simple, sin expresiones).                                      |
| `@RolesAllowed`          | Basada en JSR-250, restringe acceso por roles.                                                  |
| `@WithMockUser`          | Crea usuarios simulados para pruebas.                                                           |
| `@AuthenticationPrincipal` | Inyecta el usuario autenticado en un controlador o método.                                     |
| `@PermitAll` / `@DenyAll`| Permiten o deniegan acceso a todos los usuarios.                                                |
| `@CrossOrigin`           | Habilita el acceso desde dominios cruzados (CORS).                                              |

---

### **¿Cuál usar?**
- Usa **`@PreAuthorize`** para máxima flexibilidad con expresiones lógicas.
- Usa **`@Secured`** o **`@RolesAllowed`** si solo necesitas restricciones simples basadas en roles.
- Usa **`@AuthenticationPrincipal`** si necesitas acceder al usuario autenticado directamente en un método o controlador.

---

## Guía: Uso de `hasPermission` en Spring Security y asignación de permisos a usuarios

Esta guía te llevará paso a paso a implementar el método `hasPermission` en **Spring Security** y configurar la lógica para asignar permisos a usuarios sobre recursos específicos.

---

### **1. Conceptos básicos**
- **`hasPermission`**: Es una expresión de Spring Security que se usa para verificar si un usuario tiene un permiso específico sobre un recurso (por ejemplo, "leer", "editar", "eliminar").
- **Permisos**: Son controles más finos que los roles. En lugar de simplemente verificar "Administrador", puedes validar permisos como "EDITAR_DOCUMENTO" o "VER_REPORTE".
- **Access Control List (ACL)**: Un enfoque que Spring Security utiliza para administrar permisos sobre recursos individuales.

---

### **2. Configuración inicial**
Antes de usar `hasPermission`, necesitas implementar un sistema de permisos en tu aplicación.

#### **2.1. Agregar dependencias**
Asegúrate de tener las dependencias de Spring Security en tu archivo `pom.xml` (Maven):

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

#### **2.2. Habilitar anotaciones de seguridad**
En tu clase de configuración de seguridad, habilita la seguridad a nivel de método:

```java
@Configuration
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class SecurityConfig extends GlobalMethodSecurityConfiguration {
}
```

---

### **3. Crear un `PermissionEvaluator` personalizado**
El `PermissionEvaluator` es el núcleo de `hasPermission`, ya que define cómo se verifica un permiso.

#### **3.1. Implementar el `PermissionEvaluator`**
Crea una clase que implemente la lógica personalizada para verificar permisos.

```java
@Component
public class CustomPermissionEvaluator implements PermissionEvaluator {

    @Override
    public boolean hasPermission(Authentication authentication, Object targetDomainObject, Object permission) {
        if (targetDomainObject == null || !(permission instanceof String)) {
            return false;
        }

        // Lógica personalizada para verificar permisos
        String username = authentication.getName();
        String resource = targetDomainObject.toString();
        String requiredPermission = permission.toString();

        // Aquí podrías consultar una base de datos, un servicio o cualquier lógica de negocio
        return checkUserPermission(username, resource, requiredPermission);
    }

    @Override
    public boolean hasPermission(Authentication authentication, Serializable targetId, String targetType, Object permission) {
        // Otra lógica personalizada basada en el ID del recurso
        return false; // Modifícalo según sea necesario
    }

    private boolean checkUserPermission(String username, String resource, String permission) {
        // Aquí defines cómo verificar los permisos
        // Ejemplo: consulta en una base de datos
        return true; // Cambiar según la lógica de tu negocio
    }
}
```

#### **3.2. Registrar el `PermissionEvaluator`**
Registra tu `CustomPermissionEvaluator` en la configuración de seguridad:

```java
@Configuration
public class SecurityConfig extends GlobalMethodSecurityConfiguration {

    @Override
    protected MethodSecurityExpressionHandler createExpressionHandler() {
        DefaultMethodSecurityExpressionHandler expressionHandler = new DefaultMethodSecurityExpressionHandler();
        expressionHandler.setPermissionEvaluator(new CustomPermissionEvaluator());
        return expressionHandler;
    }
}
```

---

### **4. Uso de `hasPermission` en métodos**
Puedes proteger métodos usando la anotación `@PreAuthorize` con la expresión `hasPermission`.

#### **Ejemplo:**
```java
@PreAuthorize("hasPermission(#document, 'edit')")
public void editDocument(Document document) {
    // Solo los usuarios con el permiso 'edit' pueden ejecutar este método
}
```

En este caso:
- `#document` es el objeto sobre el que se verifica el permiso.
- `'edit'` es el permiso requerido.

---

### **5. Diseño de la base de datos para permisos**
Necesitas una estructura en tu base de datos para almacenar los permisos. Un diseño típico puede incluir:

#### **Tablas sugeridas**
1. **Usuarios (`users`)**
   - `id`
   - `username`
   - `password`

2. **Recursos (`resources`)**
   - `id`
   - `name` (por ejemplo, "documento_1")

3. **Permisos (`permissions`)**
   - `id`
   - `name` (por ejemplo, "read", "edit", "delete")

4. **Asignación de permisos (`user_permissions`)**
   - `user_id` (FK a `users`)
   - `resource_id` (FK a `resources`)
   - `permission_id` (FK a `permissions`)

---

### **6. Asignar permisos a un usuario sobre un recurso**
Crea un servicio para asignar permisos dinámicamente.

```java
@Service
public class PermissionService {

    @Autowired
    private UserPermissionRepository userPermissionRepository;

    public void assignPermission(String username, Long resourceId, String permissionName) {
        // Busca al usuario y el permiso
        User user = userRepository.findByUsername(username);
        Permission permission = permissionRepository.findByName(permissionName);

        // Asocia el permiso con el recurso
        UserPermission userPermission = new UserPermission();
        userPermission.setUser(user);
        userPermission.setResourceId(resourceId);
        userPermission.setPermission(permission);

        userPermissionRepository.save(userPermission);
    }
}
```

---

### **7. Verificación de permisos en la lógica**
Cuando uses `hasPermission`, el `PermissionEvaluator` verificará si el usuario tiene el permiso necesario según lo asignado en tu base de datos.

#### **Ejemplo de método protegido:**
```java
@PreAuthorize("hasPermission(#resourceId, 'com.example.Document', 'read')")
public Document getDocumentById(Long resourceId) {
    // Lógica del método
    return documentRepository.findById(resourceId).orElseThrow();
}
```

---

### **8. Pruebas**
Realiza pruebas unitarias y de integración para asegurarte de que los permisos se asignan y verifican correctamente.

#### **Prueba de asignación:**
```java
@Test
public void testAssignPermission() {
    permissionService.assignPermission("john_doe", 1L, "edit");

    // Verifica que el permiso se haya asignado correctamente
    assertTrue(userPermissionRepository.existsByUserAndResourceAndPermission("john_doe", 1L, "edit"));
}
```

#### **Prueba de acceso a un método protegido:**
```java
@Test
@WithMockUser(username = "john_doe")
public void testProtectedMethod() {
    // Solo debe ejecutarse si el usuario tiene el permiso
    Document doc = documentService.getDocumentById(1L);
    assertNotNull(doc);
}
```

---

### **9. Resumen**
1. Implementa un `PermissionEvaluator` para manejar la lógica de permisos.
2. Define tablas para usuarios, recursos y permisos en la base de datos.
3. Usa `@PreAuthorize` con `hasPermission` para proteger tus métodos.
4. Crea un servicio para asignar permisos dinámicamente.
5. Prueba tu sistema para garantizar que los permisos funcionen correctamente.

¡Con esta guía puedes implementar un sistema de permisos granular utilizando `hasPermission`!



---

## La diferencia principal entre **`@PreAuthorize`** y **`@PostAuthorize`** radica en **cuándo** se realiza la evaluación de la autorización en el flujo de ejecución de un método. Vamos a desglosarlo:

---

### **1. `@PreAuthorize`**
- **¿Cuándo se ejecuta?**  
  La autorización se verifica **antes** de que se ejecute el método. Si el usuario no tiene los permisos necesarios, el método no se ejecuta.

- **Uso típico:**  
  Ideal para restringir el acceso a un método basado en roles, permisos o condiciones antes de realizar la operación.

- **Ejemplo:**
  ```java
  @PreAuthorize("hasRole('ADMIN')")
  public void deleteUser(Long userId) {
      // Solo los administradores pueden ejecutar este método
      userRepository.deleteById(userId);
  }
  ```
  En este caso, si el usuario no tiene el rol `ADMIN`, Spring Security bloqueará la ejecución **antes** de llegar al cuerpo del método.

---

### **2. `@PostAuthorize`**
- **¿Cuándo se ejecuta?**  
  La autorización se verifica **después** de que el método se haya ejecutado, pero antes de que se devuelva el resultado al cliente.

- **Uso típico:**  
  Útil en situaciones donde necesitas validar permisos basándote en el resultado del método. Es decir, el método se ejecuta primero, y luego se revisa si el usuario tiene acceso al resultado devuelto.

- **Ejemplo:**
  ```java
  @PostAuthorize("returnObject.owner == authentication.name")
  public Document getDocument(Long documentId) {
      // Cualquier usuario puede ejecutar este método, pero solo
      // podrá ver el documento si es el propietario
      return documentRepository.findById(documentId).orElseThrow();
  }
  ```
  En este ejemplo:
  - El método `getDocument` se ejecuta y recupera el documento.
  - Después, Spring Security evalúa si el usuario autenticado es el propietario del documento (comparando `owner` con el nombre del usuario autenticado).
  - Si no es el propietario, se lanza una excepción de acceso denegado.

---

### **3. Resumen de diferencias**

| Característica          | `@PreAuthorize`                       | `@PostAuthorize`                     |
|--------------------------|----------------------------------------|---------------------------------------|
| **Momento de evaluación**| Antes de ejecutar el método.           | Después de ejecutar el método.        |
| **Acceso al resultado**  | No se tiene acceso al resultado.       | Sí se tiene acceso al resultado.      |
| **Uso típico**           | Verificar permisos previos a una operación. | Verificar permisos basados en el resultado del método. |
| **Efecto si falla**      | El método no se ejecuta.               | El resultado no