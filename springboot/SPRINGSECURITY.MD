# CONCEPTOS BASICOS:

## **CORS** (**Cross-Origin Resource Sharing**, o **Intercambio de Recursos de Origen Cruzado**) es un mecanismo de seguridad implementado en navegadores web para controlar cómo un recurso (como datos, imágenes, o archivos) en un servidor puede ser solicitado desde un dominio diferente al que aloja dicho recurso.

### **1. ¿Qué problema resuelve CORS?**
Por defecto, los navegadores aplican la **Política del mismo origen** (*Same-Origin Policy*), que restringe que un sitio web (dominio) haga solicitudes HTTP a otro dominio diferente. Esto protege a los usuarios contra posibles ataques, como el **Cross-Site Request Forgery (CSRF)**.

Por ejemplo:
- Tu aplicación en `https://mi-app.com` no puede acceder directamente a recursos alojados en `https://api-externa.com` (otro origen).
- Pero CORS permite que el servidor externo (`api-externa.com`) especifique en sus cabeceras si acepta solicitudes desde dominios externos como `mi-app.com`.

### **2. ¿Qué es un origen (origin)?**
Un origen está definido por tres componentes:
1. El **protocolo** (HTTP o HTTPS).
2. El **dominio** (por ejemplo, `example.com`).
3. El **puerto** (por ejemplo, `:8080`).

Dos URLs se consideran del **mismo origen** si tienen el mismo protocolo, dominio y puerto. Si cualquiera de estos es diferente, se considera un **origen cruzado** (cross-origin).

---

### **3. ¿Cómo funciona CORS?**
Cuando un navegador hace una solicitud HTTP a un servidor de otro origen, el servidor puede responder con cabeceras específicas de CORS para permitir o denegar el acceso. Algunas de estas cabeceras son:

- **`Access-Control-Allow-Origin`**: Define qué dominios tienen permiso para acceder al recurso. Puede ser un dominio específico (`https://mi-app.com`) o un comodín (`*`) para permitir acceso desde cualquier origen.
- **`Access-Control-Allow-Methods`**: Especifica los métodos HTTP permitidos, como `GET`, `POST`, `PUT`, etc.
- **`Access-Control-Allow-Headers`**: Define qué cabeceras HTTP personalizadas pueden enviarse.
- **`Access-Control-Allow-Credentials`**: Indica si se pueden enviar credenciales (cookies o encabezados de autenticación).

#### Ejemplo de respuesta CORS:
El servidor responde con algo como esto:

```
Access-Control-Allow-Origin: https://mi-app.com
Access-Control-Allow-Methods: GET, POST
Access-Control-Allow-Headers: Content-Type
Access-Control-Allow-Credentials: true
```

Esto indica al navegador que:
- Permita solicitudes desde el dominio `https://mi-app.com`.
- Sólo permita métodos `GET` y `POST`.
- Autorice cabeceras personalizadas como `Content-Type`.
- Acepte el envío de cookies o credenciales.

---

### **4. Tipos de solicitudes CORS**

#### **a. Solicitud simple:**
Una solicitud se considera "simple" si cumple con ciertos criterios (por ejemplo, usa métodos como `GET` o `POST` y no tiene cabeceras personalizadas). En este caso, el navegador envía la solicitud directamente, y el servidor responde con las cabeceras de CORS.

#### **b. Solicitud preflight (preparación):**
Cuando la solicitud incluye métodos no estándar (como `PUT` o `DELETE`), cabeceras personalizadas, o credenciales, el navegador realiza primero una **solicitud preflight** usando el método `OPTIONS`. Esto comprueba si el servidor permite la solicitud antes de enviarla realmente.

**Ejemplo de solicitud preflight:**
```http
OPTIONS /recurso HTTP/1.1
Origin: https://mi-app.com
Access-Control-Request-Method: POST
Access-Control-Request-Headers: Content-Type
```

El servidor responde indicando si la solicitud real será permitida:
```http
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://mi-app.com
Access-Control-Allow-Methods: POST
Access-Control-Allow-Headers: Content-Type
```

---

### **5. Configuración de CORS en el servidor**
El soporte para CORS debe configurarse en el servidor. Aquí tienes ejemplos para algunas tecnologías comunes:

#### **Spring Boot**
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class CorsConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("https://mi-app.com")
                .allowedMethods("GET", "POST", "PUT", "DELETE")
                .allowedHeaders("*")
                .allowCredentials(true);
    }
}
```

#### **Node.js (Express)**
```javascript
const express = require('express');
const cors = require('cors');
const app = express();

app.use(cors({
    origin: 'https://mi-app.com',
    methods: ['GET', 'POST', 'PUT', 'DELETE'],
    credentials: true
}));

app.listen(3000, () => {
    console.log('Servidor corriendo en puerto 3000');
});
```

---

### **6. Errores comunes relacionados con CORS**
Si el servidor no responde correctamente con las cabeceras CORS necesarias, el navegador bloquea la solicitud. Algunos errores comunes son:

- **Falta la cabecera `Access-Control-Allow-Origin`**.
- El origen no está permitido en el servidor.
- El servidor no admite el método HTTP usado en la solicitud.
- Las credenciales no están configuradas correctamente.

---

### **Resumen:**
- **CORS** permite que un navegador solicite recursos de un origen diferente de manera segura.
- El servidor controla qué orígenes, métodos y cabeceras están permitidos mediante las cabeceras CORS.
- Necesitas configurar el servidor correctamente para permitir solicitudes cross-origin según sea necesario.

---
---
---

# SPRING SECURITY:

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
> ⚠️ **Nota importante:** Para la autenticación spring security utiliza la clase UserDetails, que contiene la información de usuario, en este caso costruye un UserDetails sobre tú User haciendo una conversión, otra opción es si por ejemplo haces que tu user extienda de UserDetails


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


## **TEMARIO IMPORTANTE: Filtros en Spring Security**

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

---
---

## **ANOTACIONES IMPORTANTES EN SPRING SECURITY:**:
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
---
---
---

# COSAS ADICIONALES SOBRE SPRING SECURITY:
> **Nota:** TODO apartir de aquí son información a mayores sobre Spring Security y guías sobre como implementar diferentes cosas






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






# MINIGUÍAS DETALLADAS:

## Guía para generar claves privadas y públicas con OpenSSL y usarlas en Spring Security

En esta guía, aprenderás cómo generar claves privadas y públicas utilizando OpenSSL, configurarlas en una aplicación Spring Security y utilizarlas para tareas como la autenticación JWT.

---

## Paso 1: Generar claves privadas y públicas con OpenSSL

### 1.1 Generar una clave privada RSA de 2048 bits
Ejecuta el siguiente comando en tu terminal para generar una clave privada:

```bash
openssl genrsa -out private_key.pem 2048
```

Esto crea un archivo llamado `private_key.pem`, que contiene la clave privada.

---

### 1.2 Generar la clave pública a partir de la clave privada
Usa el siguiente comando para extraer la clave pública desde `private_key.pem`:

```bash
openssl rsa -pubout -in private_key.pem -out public_key.pem
```

Esto genera un archivo llamado `public_key.pem`, que contiene la clave pública.

---

### 1.3 Almacenar las claves en formato PEM
Ambas claves están en formato PEM, lo cual es necesario para trabajar con JWT en Spring Security.

- **Clave privada (`private_key.pem`)**: Se utiliza para firmar los tokens JWT.
- **Clave pública (`public_key.pem`)**: Se utiliza para verificar la firma de los tokens JWT.

---

## Paso 2: Guardar las claves en rutas específicas

Para organizar las claves, colócalas en directorios accesibles para la aplicación. Por ejemplo:

```plaintext
src/main/resources/jwtKeys/private_key.pem
src/main/resources/jwtKeys/public_key.pem
```

---

## Paso 3: Configurar las rutas en `application.properties`

En el archivo `application.properties`, define las rutas de las claves:

```properties
jwtKeys.privateKeyPath=classpath:jwtKeys/private_key.pem
jwtKeys.publicKeyPath=classpath:jwtKeys/public_key.pem
```

---

## Paso 4: Cargar las claves en Spring Security

Crea un componente en tu proyecto Spring para leer las claves desde las rutas especificadas.

### 4.1 Clase para cargar las claves
```java
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Configuration;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.security.KeyFactory;
import java.security.PrivateKey;
import java.security.PublicKey;
import java.security.spec.PKCS8EncodedKeySpec;
import java.security.spec.X509EncodedKeySpec;
import java.util.Base64;

@Configuration
public class JwtKeyProvider {

    @Value("${jwtKeys.privateKeyPath}")
    private String privateKeyPath;

    @Value("${jwtKeys.publicKeyPath}")
    private String publicKeyPath;

    public PrivateKey getPrivateKey() throws Exception {
        String key = new String(Files.readAllBytes(Paths.get(privateKeyPath)))
                .replace("-----BEGIN PRIVATE KEY-----", "")
                .replace("-----END PRIVATE KEY-----", "")
                .replaceAll("\\s", "");

        byte[] keyBytes = Base64.getDecoder().decode(key);
        PKCS8EncodedKeySpec spec = new PKCS8EncodedKeySpec(keyBytes);
        KeyFactory keyFactory = KeyFactory.getInstance("RSA");
        return keyFactory.generatePrivate(spec);
    }

    public PublicKey getPublicKey() throws Exception {
        String key = new String(Files.readAllBytes(Paths.get(publicKeyPath)))
                .replace("-----BEGIN PUBLIC KEY-----", "")
                .replace("-----END PUBLIC KEY-----", "")
                .replaceAll("\\s", "");

        byte[] keyBytes = Base64.getDecoder().decode(key);
        X509EncodedKeySpec spec = new X509EncodedKeySpec(keyBytes);
        KeyFactory keyFactory = KeyFactory.getInstance("RSA");
        return keyFactory.generatePublic(spec);
    }
}
```

---

### 4.2 Uso del proveedor de claves

Puedes usar esta clase para obtener las claves y configurarlas en tu aplicación, por ejemplo, para firmar y verificar JWT.

---

## Paso 5: Firmar y verificar JWT en Spring Security

Spring Security con **JWT** usa la clave privada para firmar los tokens y la clave pública para verificar las firmas.

### 5.1 Dependencia de JWT
Asegúrate de incluir la dependencia de **jjwt** en tu `pom.xml`:

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
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-jackson</artifactId>
    <version>0.11.5</version>
    <scope>runtime</scope>
</dependency>
```

---

### 5.2 Crear un token firmado
Usa la clave privada para firmar el token JWT:

```java
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;

import java.security.PrivateKey;
import java.util.Date;

public class JwtUtil {

    public static String createToken(PrivateKey privateKey, String subject, long expirationTime) {
        return Jwts.builder()
                .setSubject(subject)
                .setIssuedAt(new Date())
                .setExpiration(new Date(System.currentTimeMillis() + expirationTime))
                .signWith(privateKey, SignatureAlgorithm.RS256)
                .compact();
    }
}
```

---

### 5.3 Verificar un token con la clave pública
Usa la clave pública para verificar la firma del token:

```java
import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jwts;

import java.security.PublicKey;

public class JwtUtil {

    public static Claims parseToken(PublicKey publicKey, String token) {
        return Jwts.parserBuilder()
                .setSigningKey(publicKey)
                .build()
                .parseClaimsJws(token)
                .getBody();
    }
}
```

---

## Paso 6: Pruebas

### Crear un token
```java
@Autowired
private JwtKeyProvider jwtKeyProvider;

public String generarToken() throws Exception {
    PrivateKey privateKey = jwtKeyProvider.getPrivateKey();
    return JwtUtil.createToken(privateKey, "usuario123", 3600000); // 1 hora
}
```

### Verificar un token
```java
@Autowired
private JwtKeyProvider jwtKeyProvider;

public void verificarToken(String token) throws Exception {
    PublicKey publicKey = jwtKeyProvider.getPublicKey();
    Claims claims = JwtUtil.parseToken(publicKey, token);
    System.out.println("Usuario: " + claims.getSubject());
}
```

---

## Conclusión

Con esta configuración, puedes usar claves privadas y públicas generadas con OpenSSL para firmar y verificar JWT en una aplicación Spring Security. Esto proporciona una manera segura y flexible de autenticar usuarios en tus aplicaciones.

--- 
## Guía completa: Configuración de roles en Spring Security con JWT

Esta guía te llevará paso a paso para configurar roles y autorizaciones utilizando Spring Security y autenticación basada en JWT. También incluye ejemplos prácticos y las mejores prácticas.

---

## Requisitos previos
1. **Proyecto Java configurado con Spring Boot.**  
   Asegúrate de tener un proyecto Spring Boot con las siguientes dependencias en tu `pom.xml` o `build.gradle`:
   - `spring-boot-starter-security`
   - `spring-boot-starter-web`
   - `spring-boot-starter-data-jpa` (opcional para usar base de datos)
   - `jjwt` o `io.jsonwebtoken:jjwt-api`

2. **Conocimiento básico de JWT.**  
   Debes saber cómo generar y validar tokens JWT.

---

## Estructura general de roles y autenticación

El flujo es el siguiente:
1. **Autenticación:** Un usuario envía sus credenciales, y el servidor responde con un token JWT que incluye los roles del usuario.
2. **Autorización:** Las solicitudes enviadas con el token JWT son verificadas y se aplican restricciones basadas en roles definidos.

---

## Paso 1: Configura las entidades de usuario y roles

Define las entidades `User` y `Role` en tu modelo.

```java
import jakarta.persistence.*;
import java.util.Set;

@Entity
public class Role {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(unique = true, nullable = false)
    private String name; // Ejemplo: ROLE_ADMIN, ROLE_USER

    // Getters y setters
}

@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, unique = true)
    private String username;

    @Column(nullable = false)
    private String password;

    @ManyToMany(fetch = FetchType.EAGER)
    @JoinTable(
        name = "user_roles",
        joinColumns = @JoinColumn(name = "user_id"),
        inverseJoinColumns = @JoinColumn(name = "role_id")
    )
    private Set<Role> roles;

    // Getters y setters
}
```

---

## Paso 2: Implementa un servicio para la autenticación

Define un servicio que cargue los detalles del usuario desde la base de datos.

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
            .orElseThrow(() -> new UsernameNotFoundException("User not found: " + username));

        return new org.springframework.security.core.userdetails.User(
            user.getUsername(),
            user.getPassword(),
            user.getRoles().stream()
                .map(role -> new SimpleGrantedAuthority(role.getName()))
                .toList()
        );
    }
}
```

---

## Paso 3: Configura JWT para autenticación

### Generación del token JWT

Crea un componente para generar el token JWT.

```java
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import org.springframework.stereotype.Component;

import java.util.Date;
import java.util.stream.Collectors;

@Component
public class JwtUtil {

    private static final String SECRET_KEY = "your_secret_key";
    private static final long EXPIRATION_TIME = 86400000; // 1 día en milisegundos

    public String generateToken(org.springframework.security.core.userdetails.UserDetails userDetails) {
        String roles = userDetails.getAuthorities().stream()
            .map(authority -> authority.getAuthority())
            .collect(Collectors.joining(","));

        return Jwts.builder()
            .setSubject(userDetails.getUsername())
            .claim("roles", roles)
            .setIssuedAt(new Date())
            .setExpiration(new Date(System.currentTimeMillis() + EXPIRATION_TIME))
            .signWith(SignatureAlgorithm.HS512, SECRET_KEY)
            .compact();
    }
}
```

### Validación del token JWT

```java
import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jwts;
import org.springframework.stereotype.Component;

@Component
public class JwtValidator {

    private static final String SECRET_KEY = "your_secret_key";

    public Claims extractClaims(String token) {
        return Jwts.parser()
            .setSigningKey(SECRET_KEY)
            .parseClaimsJws(token)
            .getBody();
    }

    public boolean isTokenValid(String token, UserDetails userDetails) {
        String username = extractClaims(token).getSubject();
        return username.equals(userDetails.getUsername()) && !isTokenExpired(token);
    }

    private boolean isTokenExpired(String token) {
        return extractClaims(token).getExpiration().before(new Date());
    }
}
```

---

## Paso 4: Configura Spring Security

Configura el manejo de JWT en tu aplicación.

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.config.annotation.authentication.configuration.AuthenticationConfiguration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
public class SecurityConfig {

    private final JwtAuthenticationFilter jwtAuthenticationFilter;

    public SecurityConfig(JwtAuthenticationFilter jwtAuthenticationFilter) {
        this.jwtAuthenticationFilter = jwtAuthenticationFilter;
    }

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http.csrf().disable()
            .authorizeHttpRequests()
            .requestMatchers("/api/auth/**").permitAll()
            .anyRequest().authenticated()
            .and()
            .sessionManagement()
            .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
            .and()
            .addFilterBefore(jwtAuthenticationFilter, UsernamePasswordAuthenticationFilter.class);

        return http.build();
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Bean
    public AuthenticationManager authenticationManager(AuthenticationConfiguration configuration) throws Exception {
        return configuration.getAuthenticationManager();
    }
}
```

### Filtro para validar JWT

```java
import jakarta.servlet.FilterChain;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;
import org.springframework.stereotype.Component;

import java.io.IOException;

@Component
public class JwtAuthenticationFilter extends UsernamePasswordAuthenticationFilter {

    private final JwtValidator jwtValidator;
    private final CustomUserDetailsService userDetailsService;

    public JwtAuthenticationFilter(JwtValidator jwtValidator, CustomUserDetailsService userDetailsService) {
        this.jwtValidator = jwtValidator;
        this.userDetailsService = userDetailsService;
    }

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain chain)
        throws IOException, ServletException {

        String authHeader = request.getHeader("Authorization");
        if (authHeader != null && authHeader.startsWith("Bearer ")) {
            String token = authHeader.substring(7);
            String username = jwtValidator.extractClaims(token).getSubject();

            UserDetails userDetails = userDetailsService.loadUserByUsername(username);
            if (jwtValidator.isTokenValid(token, userDetails)) {
                var authToken = new UsernamePasswordAuthenticationToken(userDetails, null, userDetails.getAuthorities());
                SecurityContextHolder.getContext().setAuthentication(authToken);
            }
        }
        chain.doFilter(request, response);
    }
}
```

---

## Paso 5: Controla las rutas con roles

Finalmente, aplica las restricciones según los roles.

```java
http.authorizeHttpRequests()
    .requestMatchers("/admin/**").hasRole("ADMIN")
    .requestMatchers("/user/**").hasAnyRole("USER", "ADMIN")
    .anyRequest().authenticated();
```

---

Con estos pasos, has configurado un sistema completo de autenticación JWT y manejo de roles en Spring Security.







### Relacionando la clase `User` con `UserDetails` y accediendo al usuario autenticado

Para integrar tu entidad `User` con `UserDetails`, debes hacer que tu clase `User` o una clase adaptadora implemente la interfaz `UserDetails`. Esto permitirá que Spring Security maneje tu entidad personalizada como usuario autenticado, manteniendo la compatibilidad con sus componentes.

---

## Paso 1: Haz que la clase `User` implemente `UserDetails`

Modifica tu clase `User` para implementar la interfaz `UserDetails`. Esto requiere que definas ciertos métodos estándar de la interfaz, como `getAuthorities()`, `isEnabled()`, etc.

```java
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;

import java.util.Collection;
import java.util.stream.Collectors;

@Entity
public class User implements UserDetails {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, unique = true)
    private String username;

    @Column(nullable = false)
    private String password;

    @ManyToMany(fetch = FetchType.EAGER)
    @JoinTable(
        name = "user_roles",
        joinColumns = @JoinColumn(name = "user_id"),
        inverseJoinColumns = @JoinColumn(name = "role_id")
    )
    private Set<Role> roles;

    // Otros campos personalizados
    private String email;
    private boolean enabled;

    // Getters y Setters de tus campos personalizados

    // Implementación de métodos de UserDetails
    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return roles.stream()
            .map(role -> (GrantedAuthority) () -> role.getName())
            .collect(Collectors.toSet());
    }

    @Override
    public String getPassword() {
        return password;
    }

    @Override
    public String getUsername() {
        return username;
    }

    @Override
    public boolean isAccountNonExpired() {
        return true; // Personaliza según tu lógica
    }

    @Override
    public boolean isAccountNonLocked() {
        return true; // Personaliza según tu lógica
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return true; // Personaliza según tu lógica
    }

    @Override
    public boolean isEnabled() {
        return enabled; // Basado en el campo `enabled`
    }
}
```

Con esta implementación, tu clase `User` ahora es compatible con Spring Security.

---

## Paso 2: Usa `CustomUserDetailsService` para cargar el usuario autenticado

Crea o ajusta tu servicio de detalles del usuario (`CustomUserDetailsService`) para devolver una instancia de tu clase `User`.

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
        return userRepository.findByUsername(username)
            .orElseThrow(() -> new UsernameNotFoundException("User not found: " + username));
    }
}
```

---

## Paso 3: Almacena el usuario autenticado en el contexto de seguridad

Spring Security almacena el objeto autenticado en el `SecurityContext`. Cuando un usuario se autentica exitosamente (por ejemplo, mediante JWT o autenticación básica), puedes acceder al usuario autenticado desde cualquier parte del código.

---

## Paso 4: Obtén el usuario autenticado en cualquier parte del código

Para obtener el usuario autenticado, utiliza el `SecurityContextHolder`. Si necesitas acceder a la entidad completa de tu clase `User`, puedes realizar un cast.

### Ejemplo de cómo acceder al usuario autenticado:
```java
import org.springframework.security.core.Authentication;
import org.springframework.security.core.context.SecurityContextHolder;

public class AuthUtil {

    public static User getAuthenticatedUser() {
        Authentication authentication = SecurityContextHolder.getContext().getAuthentication();

        if (authentication != null && authentication.getPrincipal() instanceof User) {
            return (User) authentication.getPrincipal();
        }

        throw new IllegalStateException("No authenticated user found");
    }
}
```

### Ejemplo de uso:
```java
User authenticatedUser = AuthUtil.getAuthenticatedUser();
System.out.println("Usuario autenticado: " + authenticatedUser.getUsername());
```

---

## Nota sobre JWT

Cuando usas JWT, debes configurar correctamente el filtro de autenticación para que cargue el usuario y lo almacene en el contexto de seguridad. Esto ya se hace en el ejemplo del filtro de JWT, pero aquí lo resumimos:

### Código del filtro JWT para almacenar el usuario:
```java
@Override
protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain chain)
    throws ServletException, IOException {

    String token = request.getHeader("Authorization");
    if (token != null && token.startsWith("Bearer ")) {
        token = token.substring(7); // Quitar "Bearer "
        String username = jwtValidator.getUsernameFromToken(token);

        UserDetails userDetails = userDetailsService.loadUserByUsername(username);

        if (jwtValidator.isTokenValid(token, userDetails)) {
            UsernamePasswordAuthenticationToken authToken =
                new UsernamePasswordAuthenticationToken(userDetails, null, userDetails.getAuthorities());
            SecurityContextHolder.getContext().setAuthentication(authToken);
        }
    }

    chain.doFilter(request, response);
}
```

---

## Resumen

1. **Integra tu clase `User` con `UserDetails`** para que sea compatible con Spring Security.
2. **Utiliza el contexto de seguridad (`SecurityContextHolder`)** para acceder al usuario autenticado en cualquier parte del código.
3. **Asegúrate de configurar correctamente los filtros de JWT** para que el usuario autenticado se almacene en el contexto de seguridad.

De esta forma, puedes acceder al usuario identificado en toda la aplicación, ya sea para lógica de negocio o auditoría.




¡Exactamente! Una vez que configuras correctamente las **Authorities** en el objeto `UsernamePasswordAuthenticationToken` y las almacenas en el contexto de seguridad (`SecurityContext`), Spring Security tiene conocimiento de los roles del usuario autenticado.

Esto te permite usar métodos como `hasRole()` o `hasAuthority()` en configuraciones de seguridad y en anotaciones, ya que Spring Security verifica automáticamente las autoridades asociadas al usuario autenticado.

---

## Cómo funciona el `hasRole`

En Spring Security, las **Authorities** asociadas al usuario suelen representarse como cadenas de texto, como `ROLE_ADMIN` o `ROLE_USER`. Spring tiene algunas reglas específicas:

1. **Prefijo `ROLE_`:**  
   Spring Security espera que los roles tengan el prefijo `ROLE_` por convención. Por ejemplo:
   - Si tienes un rol `ADMIN`, deberías mapearlo como `ROLE_ADMIN`.
   - Spring Security internamente compara los roles con este prefijo.

2. **Método `hasRole(String role)`:**  
   Al usar este método, no necesitas incluir el prefijo `ROLE_`. Por ejemplo:
   - `hasRole("ADMIN")` buscará internamente `ROLE_ADMIN`.

3. **Método `hasAuthority(String authority)`:**  
   Este método, en cambio, no aplica la convención del prefijo. Debes usar exactamente la cadena que defines como autoridad. Por ejemplo:
   - Si tienes `ROLE_ADMIN`, deberías usar `hasAuthority("ROLE_ADMIN")`.

---

## Ejemplo de configuración de roles

En el método `getAuthorities()` de tu clase `User` (implementación de `UserDetails`), ya defines los roles como `GrantedAuthority`. Si estás agregando los roles con el prefijo `ROLE_`, Spring Security los reconocerá automáticamente:

```java
@Override
public Collection<? extends GrantedAuthority> getAuthorities() {
    return roles.stream()
        .map(role -> (GrantedAuthority) () -> "ROLE_" + role.getName())
        .collect(Collectors.toSet());
}
```

Por ejemplo, si el usuario tiene un rol `ADMIN`, esto genera una autoridad llamada `ROLE_ADMIN`.

---

## Uso en configuraciones de seguridad

En tu clase de configuración de Spring Security, puedes usar los roles en las reglas de acceso:

### Ejemplo con configuración programática:
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/admin/**").hasRole("ADMIN") // Solo para usuarios con ROLE_ADMIN
                .antMatchers("/user/**").hasAnyRole("USER", "ADMIN") // ROLE_USER o ROLE_ADMIN
                .antMatchers("/public/**").permitAll() // Acceso público
                .anyRequest().authenticated()
            .and()
            .httpBasic(); // O cualquier otro mecanismo de autenticación
    }
}
```

---

## Uso en controladores con anotaciones

Puedes usar anotaciones como `@PreAuthorize` o `@Secured` para restringir el acceso a métodos o controladores basándote en roles.

### Ejemplo con `@PreAuthorize`:
Asegúrate de habilitar la anotación con `@EnableGlobalMethodSecurity(prePostEnabled = true)` en tu clase de configuración de seguridad.

```java
@RestController
@RequestMapping("/admin")
public class AdminController {

    @PreAuthorize("hasRole('ADMIN')")
    @GetMapping
    public String adminOnly() {
        return "Solo administradores pueden acceder aquí";
    }
}
```

### Ejemplo con `@Secured`:
Esta anotación también requiere que habilites el soporte con `@EnableGlobalMethodSecurity(securedEnabled = true)`.

```java
@RestController
@RequestMapping("/user")
public class UserController {

    @Secured("ROLE_USER")
    @GetMapping
    public String userOnly() {
        return "Solo usuarios pueden acceder aquí";
    }
}
```

---

## Validación dinámica con `hasRole()` en las vistas Thymeleaf (opcional)

Si estás usando Thymeleaf, puedes restringir secciones de la interfaz de usuario en función de los roles.

### Ejemplo en Thymeleaf:
```html
<div sec:authorize="hasRole('ADMIN')">
    <p>Este contenido solo es visible para administradores.</p>
</div>

<div sec:authorize="hasAnyRole('USER', 'ADMIN')">
    <p>Este contenido es visible para usuarios y administradores.</p>
</div>
```

---

## Resumen

- Si mapeaste correctamente las autoridades con el prefijo `ROLE_` en tu implementación de `UserDetails`, Spring Security ya tiene conocimiento de los roles del usuario autenticado.
- Puedes usar `hasRole()` o `hasAuthority()` en configuraciones de seguridad, anotaciones y vistas.
- Spring Security maneja la verificación de roles de forma automática basándose en las autoridades que configuraste.

Con esta configuración, tienes flexibilidad para aplicar seguridad tanto en el backend como en las vistas.

---

# Añadir permisos a los roles:
Para añadir permisos a los roles y que puedas utilizar el método **`hasPermission`** en tu aplicación con Spring Security, debes integrar un modelo basado en permisos. Esto implica que no solo tengas roles, sino que cada rol tenga permisos asociados. Los permisos pueden ser verificados en el código mediante expresiones como `@PreAuthorize("hasPermission(...))`.

A continuación, te explico cómo lograrlo paso a paso:

---

## **1. Extiende tu Modelo de Seguridad**

### Crea la entidad **Permission**:
Cada permiso representa una acción específica que puede realizar un usuario (como `READ`, `WRITE`, `DELETE`).

```java
@Entity
public class Permission {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name; // Ejemplo: "READ_PRIVILEGES", "WRITE_PRIVILEGES"

    // Getters y setters
}
```

### Relaciona **Permission** con **Role**:
Cada rol tendrá una lista de permisos asociados.

```java
@Entity
public class Role {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name; // Ejemplo: "ROLE_ADMIN", "ROLE_USER"

    @ManyToMany(fetch = FetchType.EAGER)
    @JoinTable(
        name = "role_permissions",
        joinColumns = @JoinColumn(name = "role_id"),
        inverseJoinColumns = @JoinColumn(name = "permission_id")
    )
    private Set<Permission> permissions = new HashSet<>();

    // Getters y setters
}
```

### Relaciona **Role** con **User**:
Los usuarios mantienen sus roles como hasta ahora.

```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String username;
    private String password;

    @ManyToMany(fetch = FetchType.EAGER)
    @JoinTable(
        name = "user_roles",
        joinColumns = @JoinColumn(name = "user_id"),
        inverseJoinColumns = @JoinColumn(name = "role_id")
    )
    private Set<Role> roles = new HashSet<>();

    // Getters y setters
}
```

---

## **2. Implementa un Custom PermissionEvaluator**

Spring Security tiene un mecanismo llamado **`PermissionEvaluator`** que puedes personalizar para implementar la lógica de permisos.

### Clase CustomPermissionEvaluator:

```java
@Component
public class CustomPermissionEvaluator implements PermissionEvaluator {

    @Override
    public boolean hasPermission(Authentication authentication, Object targetDomainObject, Object permission) {
        if (authentication == null || permission == null) {
            return false;
        }

        // Obtener el usuario autenticado
        UserDetails userDetails = (UserDetails) authentication.getPrincipal();
        Set<Role> roles = ((CustomUserDetails) userDetails).getRoles(); // Reemplaza con tu implementación

        // Verificar si algún rol tiene el permiso requerido
        for (Role role : roles) {
            for (Permission perm : role.getPermissions()) {
                if (perm.getName().equals(permission.toString())) {
                    return true;
                }
            }
        }
        return false;
    }

    @Override
    public boolean hasPermission(Authentication authentication, Serializable targetId, String targetType, Object permission) {
        // No implementado para este caso
        return false;
    }
}
```

---

## **3. Registra el PermissionEvaluator en tu Configuración de Seguridad**

En tu clase de configuración de Spring Security, indica que usarás tu implementación personalizada del evaluador de permisos.

```java
@Configuration
@EnableMethodSecurity
public class WebSecurityConfig {

    @Bean
    public MethodSecurityExpressionHandler expressionHandler(PermissionEvaluator permissionEvaluator) {
        DefaultMethodSecurityExpressionHandler handler = new DefaultMethodSecurityExpressionHandler();
        handler.setPermissionEvaluator(permissionEvaluator);
        return handler;
    }
}
```

---

## **4. Usa `@PreAuthorize` con `hasPermission`**

Ahora puedes usar la anotación `@PreAuthorize` en tus controladores y servicios para verificar permisos.

### Ejemplo:

```java
@RestController
@RequestMapping("/api")
public class MyController {

    @GetMapping("/admin")
    @PreAuthorize("hasPermission(null, 'READ_PRIVILEGES')")
    public ResponseEntity<String> adminAccess() {
        return ResponseEntity.ok("Acceso permitido con permisos de lectura");
    }

    @PostMapping("/admin")
    @PreAuthorize("hasPermission(null, 'WRITE_PRIVILEGES')")
    public ResponseEntity<String> adminWriteAccess() {
        return ResponseEntity.ok("Acceso permitido con permisos de escritura");
    }
}
```

---

## **5. Inicializa Roles y Permisos**

Utiliza un `CommandLineRunner` o alguna migración para cargar datos iniciales en tu base de datos.

```java
@Component
public class DataInitializer implements CommandLineRunner {

    @Autowired
    private RoleRepository roleRepository;

    @Autowired
    private PermissionRepository permissionRepository;

    @Override
    public void run(String... args) throws Exception {
        Permission readPermission = new Permission();
        readPermission.setName("READ_PRIVILEGES");
        permissionRepository.save(readPermission);

        Permission writePermission = new Permission();
        writePermission.setName("WRITE_PRIVILEGES");
        permissionRepository.save(writePermission);

        Role adminRole = new Role();
        adminRole.setName("ROLE_ADMIN");
        adminRole.getPermissions().add(readPermission);
        adminRole.getPermissions().add(writePermission);
        roleRepository.save(adminRole);

        Role userRole = new Role();
        userRole.setName("ROLE_USER");
        userRole.getPermissions().add(readPermission);
        roleRepository.save(userRole);
    }
}
```

---

## **6. Carga los Roles y Permisos en `UserDetails`**

Tu implementación de `UserDetails` debe cargar los roles y sus permisos. Por ejemplo:

```java
@Override
public Collection<? extends GrantedAuthority> getAuthorities() {
    Set<GrantedAuthority> authorities = new HashSet<>();
    for (Role role : user.getRoles()) {
        authorities.add(new SimpleGrantedAuthority(role.getName()));
        for (Permission perm : role.getPermissions()) {
            authorities.add(new SimpleGrantedAuthority(perm.getName()));
        }
    }
    return authorities;
}
```

---

## **Resumen**

1. Crea la relación entre roles y permisos.
2. Implementa un `PermissionEvaluator` personalizado.
3. Registra el evaluador en la configuración de seguridad.
4. Usa `@PreAuthorize("hasPermission(...)")` para proteger rutas según permisos.
5. Inicializa roles y permisos en tu base de datos.
6. Carga los permisos en los roles al inicializar el contexto de seguridad.

Esto te permitirá manejar roles y permisos en tu aplicación con Spring Security de forma granular y flexible.

---

¡Sí, tienes toda la razón! Si los permisos ya están incluidos en el método **`getAuthorities()`** dentro de tu implementación de `UserDetails`, **no es necesario implementar un `PermissionEvaluator` personalizado**. Puedes aprovechar Spring Security para verificar directamente los permisos utilizando `@PreAuthorize` con `hasAuthority`.

---

### **1. Modificar `getAuthorities` para incluir los permisos**
Puedes hacer que **`getAuthorities()`** incluya tanto los roles como los permisos asociados a cada usuario. Así, Spring Security podrá verificar tanto roles (con `hasRole`) como permisos (con `hasAuthority`).

```java
@Override
public Collection<? extends GrantedAuthority> getAuthorities() {
    Set<GrantedAuthority> authorities = new HashSet<>();

    // Agregar los roles
    for (Role role : user.getRoles()) {
        authorities.add(new SimpleGrantedAuthority(role.getName())); // Ejemplo: "ROLE_ADMIN"
        
        // Agregar los permisos asociados al rol
        for (Permission permission : role.getPermissions()) {
            authorities.add(new SimpleGrantedAuthority(permission.getName())); // Ejemplo: "READ_PRIVILEGES"
        }
    }

    return authorities;
}
```

En este caso, todos los permisos asociados a los roles se añadirán como **`GrantedAuthority`**, lo cual es suficiente para que Spring Security los reconozca en las expresiones de autorización.

---

### **2. Usar `hasAuthority` en lugar de `hasPermission`**
Con los permisos ya disponibles en el contexto de seguridad a través de `getAuthorities()`, puedes usar `@PreAuthorize` con **`hasAuthority`** para verificar permisos específicos.

#### Ejemplo:
```java
@RestController
@RequestMapping("/api")
public class MyController {

    @GetMapping("/admin")
    @PreAuthorize("hasAuthority('READ_PRIVILEGES')")
    public ResponseEntity<String> adminAccess() {
        return ResponseEntity.ok("Acceso permitido con permisos de lectura");
    }

    @PostMapping("/admin")
    @PreAuthorize("hasAuthority('WRITE_PRIVILEGES')")
    public ResponseEntity<String> adminWriteAccess() {
        return ResponseEntity.ok("Acceso permitido con permisos de escritura");
    }
}
```

---

### **3. Diferencia entre `hasAuthority` y `hasRole`**

- **`hasRole('ROLE_ADMIN')`**:
  Busca un `GrantedAuthority` que comience con el prefijo `ROLE_` (por convención en Spring Security). 
  - Si tienes un rol como `"ROLE_ADMIN"`, lo usarías con `hasRole('ADMIN')`.

- **`hasAuthority('WRITE_PRIVILEGES')`**:
  Busca un `GrantedAuthority` exactamente igual al string que proporcionas. Esto es ideal para manejar permisos más granulares.

---

### **4. Inicializar Roles y Permisos**
Al inicializar roles y permisos, asegúrate de incluir tanto los permisos como los roles al usuario. Este es un ejemplo usando un `CommandLineRunner`:

```java
@Component
public class DataInitializer implements CommandLineRunner {

    @Autowired
    private UserRepository userRepository;

    @Autowired
    private RoleRepository roleRepository;

    @Autowired
    private PermissionRepository permissionRepository;

    @Override
    public void run(String... args) throws Exception {
        // Crear permisos
        Permission readPermission = new Permission();
        readPermission.setName("READ_PRIVILEGES");
        permissionRepository.save(readPermission);

        Permission writePermission = new Permission();
        writePermission.setName("WRITE_PRIVILEGES");
        permissionRepository.save(writePermission);

        // Crear roles con permisos
        Role adminRole = new Role();
        adminRole.setName("ROLE_ADMIN");
        adminRole.getPermissions().add(readPermission);
        adminRole.getPermissions().add(writePermission);
        roleRepository.save(adminRole);

        Role userRole = new Role();
        userRole.setName("ROLE_USER");
        userRole.getPermissions().add(readPermission);
        roleRepository.save(userRole);

        // Crear usuario admin
        User adminUser = new User();
        adminUser.setUsername("admin");
        adminUser.setPassword(new BCryptPasswordEncoder().encode("password"));
        adminUser.getRoles().add(adminRole);
        userRepository.save(adminUser);
    }
}
```

---

### **5. Verificar el Contexto del Usuario Actual**
Si deseas obtener el usuario actual con sus roles y permisos en cualquier punto de tu aplicación, puedes usar el contexto de seguridad:

#### Desde el `SecurityContextHolder`:
```java
Authentication authentication = SecurityContextHolder.getContext().getAuthentication();
CustomUserDetails userDetails = (CustomUserDetails) authentication.getPrincipal();
Set<Role> roles = userDetails.getRoles(); // Roles del usuario
Collection<? extends GrantedAuthority> authorities = userDetails.getAuthorities(); // Roles y permisos
```

#### Inyectando directamente con `@AuthenticationPrincipal`:
```java
@GetMapping("/current-user")
public ResponseEntity<?> getCurrentUser(@AuthenticationPrincipal CustomUserDetails userDetails) {
    return ResponseEntity.ok(userDetails);
}
```

---

### **6. Ventajas de este Enfoque**
1. **Simplifica la configuración**:
   - No necesitas implementar un `PermissionEvaluator` personalizado.
   - Aprovechas la funcionalidad nativa de `GrantedAuthority`.

2. **Reutilizable**:
   - Los permisos y roles están disponibles para cualquier expresión de seguridad (`@PreAuthorize`, `@PostAuthorize`, etc.).

3. **Granularidad**:
   - Puedes combinar roles (`hasRole`) y permisos (`hasAuthority`) para un control más detallado.

---

### **Resumen**
- Sí, es suficiente enviar los permisos en `getAuthorities()` para usar `hasAuthority`.
- Esto elimina la necesidad de un `PermissionEvaluator` personalizado.
- Puedes usar tanto `hasRole` como `hasAuthority` para verificar roles y permisos respectivamente.
- Asegúrate de cargar correctamente roles y permisos en los usuarios durante el inicio o la persistencia en la base de datos.


# Authentication Manager:

## **Guía Completa sobre `AuthenticationManager` y `AuthenticationProvider` en Spring Security**

Spring Security utiliza un sistema basado en **`AuthenticationManager`** y **`AuthenticationProvider`** para autenticar usuarios en aplicaciones. Este mecanismo es central en la configuración de seguridad y proporciona una arquitectura flexible que permite soportar diversos métodos de autenticación, como contraseñas, tokens, OAuth2, etc.

---

### **1. ¿Qué es `AuthenticationManager`?**
El **`AuthenticationManager`** es el componente principal en Spring Security responsable de procesar las solicitudes de autenticación. Su tarea es verificar si las credenciales proporcionadas por un usuario son válidas. Si son válidas, devuelve un objeto **`Authentication`** completo (autenticado). Si no, lanza una excepción de autenticación.

#### **Método clave:**
```java
Authentication authenticate(Authentication authentication) throws AuthenticationException;
```

- **Entrada:** Un objeto `Authentication` con las credenciales (por ejemplo, nombre de usuario y contraseña).
- **Salida:** Un objeto `Authentication` completamente autenticado.
- **Excepción:** Lanza una `AuthenticationException` si las credenciales no son válidas.

---

### **2. ¿Qué es `AuthenticationProvider`?**
El **`AuthenticationProvider`** es una interfaz que proporciona una lógica concreta para autenticar usuarios. El `AuthenticationManager` delega el proceso de autenticación a uno o más `AuthenticationProvider`. Esto permite agregar diferentes estrategias de autenticación (por ejemplo, contraseñas, JWT, OAuth, etc.).

#### **Métodos clave:**
```java
Authentication authenticate(Authentication authentication) throws AuthenticationException;
boolean supports(Class<?> authentication);
```

- **`authenticate`**: Procesa la autenticación.
- **`supports`**: Verifica si este proveedor soporta el tipo de autenticación que se le pasa.

---

### **3. Arquitectura General**
1. El cliente (usuario) envía credenciales al servidor.
2. El filtro de seguridad de Spring Security crea un objeto `Authentication` inicial con las credenciales.
3. Este objeto se pasa al **`AuthenticationManager`**.
4. El `AuthenticationManager` delega la autenticación a uno o más **`AuthenticationProvider`**.
5. Si el proceso tiene éxito, el `Authentication` se completa y se almacena en el contexto de seguridad.

---

### **4. Implementación Paso a Paso**

#### **4.1 Configurar un `AuthenticationManager` Personalizado**
El `AuthenticationManager` puede configurarse con múltiples `AuthenticationProvider`.

```java
@Configuration
public class SecurityConfig {

    @Bean
    public AuthenticationManager authenticationManager(AuthenticationProvider customAuthenticationProvider) {
        return new ProviderManager(customAuthenticationProvider);
    }
}
```

---

#### **4.2 Implementar un `AuthenticationProvider` Personalizado**
Puedes crear tu propio `AuthenticationProvider` para autenticar usuarios según tu lógica.

```java
@Component
public class CustomAuthenticationProvider implements AuthenticationProvider {

    @Autowired
    private CustomUserDetailsService userDetailsService;

    @Autowired
    private PasswordEncoder passwordEncoder;

    @Override
    public Authentication authenticate(Authentication authentication) throws AuthenticationException {
        // Obtener el nombre de usuario y contraseña
        String username = authentication.getName();
        String password = authentication.getCredentials().toString();

        // Cargar el usuario desde la base de datos
        UserDetails userDetails = userDetailsService.loadUserByUsername(username);

        // Verificar la contraseña
        if (!passwordEncoder.matches(password, userDetails.getPassword())) {
            throw new BadCredentialsException("Credenciales inválidas");
        }

        // Retornar un objeto Authentication autenticado
        return new UsernamePasswordAuthenticationToken(userDetails, password, userDetails.getAuthorities());
    }

    @Override
    public boolean supports(Class<?> authentication) {
        // Este proveedor soporta UsernamePasswordAuthenticationToken
        return UsernamePasswordAuthenticationToken.class.isAssignableFrom(authentication);
    }
}
```

---

#### **4.3 Configurar un `AuthenticationManager` con Varios `AuthenticationProvider`**
Si tienes múltiples métodos de autenticación (por ejemplo, contraseñas y tokens JWT), puedes agregar varios `AuthenticationProvider` al `AuthenticationManager`.

```java
@Configuration
public class SecurityConfig {

    @Bean
    public AuthenticationManager authenticationManager(List<AuthenticationProvider> providers) {
        return new ProviderManager(providers);
    }
}
```

---

### **5. Usar `AuthenticationManager` en un Controller**
Puedes usar el `AuthenticationManager` directamente en un controlador para autenticar usuarios.

#### Ejemplo:
```java
@RestController
@RequestMapping("/auth")
public class AuthController {

    @Autowired
    private AuthenticationManager authenticationManager;

    @PostMapping("/login")
    public ResponseEntity<?> login(@RequestBody AuthRequest authRequest) {
        try {
            // Crear un objeto Authentication inicial
            Authentication authentication = new UsernamePasswordAuthenticationToken(authRequest.getUsername(), authRequest.getPassword());

            // Autenticar al usuario
            Authentication authenticated = authenticationManager.authenticate(authentication);

            // Devolver un JWT o respuesta de éxito
            return ResponseEntity.ok("Usuario autenticado correctamente");
        } catch (AuthenticationException ex) {
            return ResponseEntity.status(HttpStatus.UNAUTHORIZED).body("Credenciales inválidas");
        }
    }
}

class AuthRequest {
    private String username;
    private String password;
    // Getters y setters
}
```

---

### **6. Usar `AuthenticationManager` en un Filtro**
El `AuthenticationManager` se usa comúnmente en filtros de seguridad, como en el caso de la autenticación basada en JWT.

#### Ejemplo de un Filtro con JWT:
```java
@Component
public class JWTAuthenticationFilter extends OncePerRequestFilter {

    @Autowired
    private JWTUtility jwtUtility;

    @Autowired
    private AuthenticationManager authenticationManager;

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
        String authorizationHeader = request.getHeader("Authorization");

        if (authorizationHeader != null && authorizationHeader.startsWith("Bearer ")) {
            String token = authorizationHeader.substring(7);

            // Validar el token y crear un objeto Authentication
            String username = jwtUtility.extractUsername(token);
            Authentication authentication = new UsernamePasswordAuthenticationToken(username, null);

            // Autenticar el usuario
            Authentication authenticated = authenticationManager.authenticate(authentication);

            // Guardar el usuario autenticado en el contexto de seguridad
            SecurityContextHolder.getContext().setAuthentication(authenticated);
        }

        filterChain.doFilter(request, response);
    }
}
```

---

### **7. Implementar un Proveedor para JWT**
Puedes implementar un `AuthenticationProvider` para manejar autenticación basada en JWT.

```java
@Component
public class JWTAuthenticationProvider implements AuthenticationProvider {

    @Autowired
    private JWTUtility jwtUtility;

    @Override
    public Authentication authenticate(Authentication authentication) throws AuthenticationException {
        String token = authentication.getCredentials().toString();

        if (!jwtUtility.validateToken(token)) {
            throw new BadCredentialsException("Token inválido");
        }

        String username = jwtUtility.extractUsername(token);

        return new UsernamePasswordAuthenticationToken(username, null, List.of());
    }

    @Override
    public boolean supports(Class<?> authentication) {
        return UsernamePasswordAuthenticationToken.class.isAssignableFrom(authentication);
    }
}
```

---

### **8. Buenas Prácticas**
1. **Separa responsabilidades:**
   - Usa un `AuthenticationProvider` para cada estrategia de autenticación (contraseñas, tokens, OAuth2, etc.).

2. **Usa un `PasswordEncoder`:**
   - Nunca almacenes contraseñas en texto plano; usa un `PasswordEncoder` como BCrypt.

3. **Registra múltiples `AuthenticationProvider`:**
   - Permite agregar soporte para múltiples métodos de autenticación.

4. **Maneja las excepciones de autenticación:**
   - Personaliza los mensajes de error para mejorar la experiencia del usuario.

---

### **Resumen**
- **`AuthenticationManager`** es el punto de entrada principal para autenticar usuarios.
- **`AuthenticationProvider`** contiene la lógica de autenticación específica.
- Puedes usar múltiples proveedores de autenticación dentro de un `AuthenticationManager`.
- `AuthenticationManager` se puede usar directamente en controladores o filtros para autenticar usuarios.

Con esta arquitectura, puedes implementar cualquier estrategia de autenticación de forma modular y flexible.