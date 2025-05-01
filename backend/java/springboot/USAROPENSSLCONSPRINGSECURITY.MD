



crear claves

openssl genrsa -out private_key.pem 2048  

openssl rsa -pubout -in private_key.pem -out public_key.pem




Creamos LoginDto con user y pass

Creamos ResponseDto con numofErrors y messages

Creamos en validations un UserValidation con las diferentes validaciones, lo necesitamos instanciar con un @Bean


Configurar los cors, para adMapping /** y alowed origins la url de origen y alowedmethods lso metodos permitidos, y allowedheaders
origins, contenttype  y accept y authorization, y allowCredentialstrue para indicar que tiene que haber credenciales, y maxAge 3600, si en allowedorigins ponemos un * se acepta cualquier ruta

Crear una interfaz de IJWTUtilityService, este debe cargar la clave privada y publica, y crear el jwt y parsearlo para validarlo

Luego creamos una clase que extienda de OncePerRequestFilter y Overide a doFilterInternal 

Luego configuramos todo e la clase SecurityConfig

Luego creamos la clase AuthService (un UserService bien hecho)

Luego creamos el authRestControler

## empezamos

@RestController
public class UserController {

    @GetMapping("/user")
    public ResponseEntity<String> getUser() {
        Authentication authentication = SecurityContextHolder.getContext().getAuthentication();
        String username = authentication.getName();
        return ResponseEntity.ok("Authenticated user: " + username);
    }
}




# Cambios a hacer:
-Cambiar en el token el id por el username y que en el login se haga con username

-Que User extienda a UserDetails

-Crear la clase CustomUserDetailsService implements UserDetailsService

-Cambiar el generateToken y el ValidToken

-Cambiar el filter de login










## guia:

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












