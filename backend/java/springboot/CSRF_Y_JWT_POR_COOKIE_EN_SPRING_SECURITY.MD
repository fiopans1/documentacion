# CSRF Token por Cookie y Autenticación JWT por Cookie en Spring Security

Implementar autenticación con **JWT almacenado en cookies** dentro de una aplicación Spring Security es una estrategia muy útil cuando el cliente es un navegador. Permite evitar almacenar tokens en `localStorage` o `sessionStorage`, reduce exposición frente a ciertos escenarios de XSS y aprovecha mecanismos nativos del navegador para enviar credenciales.

Sin embargo, cuando la autenticación viaja en **cookies**, aparece una consecuencia importante: el navegador enviará esas cookies automáticamente en cada petición al mismo sitio, y eso vuelve a poner sobre la mesa el riesgo de **CSRF (Cross-Site Request Forgery)**. Por ese motivo, si decides usar **JWT por cookie**, normalmente también debes implementar **protección CSRF**.

Esta guía explica el enfoque completo: cómo funciona, por qué se hace así, cómo configurarlo en **Spring Security 6 / Spring Boot 3**, cómo emitir cookies seguras, cómo validar JWT desde cookies, cómo proteger endpoints mutadores con CSRF y qué errores conviene evitar.

---

## 1. Idea general de la arquitectura

El flujo habitual es este:

1. El usuario envía sus credenciales a `/auth/login`.
2. El backend valida usuario y contraseña.
3. El backend genera un **JWT de acceso**.
4. El backend devuelve ese JWT dentro de una **cookie segura** (`HttpOnly`, `Secure`, `SameSite`, etc.).
5. En peticiones posteriores, el navegador envía automáticamente la cookie.
6. Un filtro de Spring Security lee la cookie, extrae el JWT, lo valida y autentica al usuario.
7. Para peticiones que cambian estado (`POST`, `PUT`, `PATCH`, `DELETE`), el cliente también debe enviar un **token CSRF**.

En otras palabras:

- **JWT en cookie** resuelve la autenticación.
- **CSRF token** protege las operaciones sensibles cuando el navegador manda la cookie automáticamente.

---

## 2. ¿Por qué usar JWT en cookie?

Guardar el JWT en cookie tiene varias ventajas:

- Evita exponer el token a JavaScript si la cookie es `HttpOnly`.
- Simplifica el envío de credenciales desde el navegador.
- Encaja bien con frontends web tradicionales y SPAs.
- Facilita combinar autenticación stateless con semántica web clásica.

Pero también implica varias responsabilidades:

- Configurar bien `Secure`, `HttpOnly`, `SameSite`, `Path` y expiración.
- Protegerse frente a **CSRF**.
- Configurar correctamente **CORS** si frontend y backend están en orígenes distintos.

---

## 3. ¿Por qué sigue existiendo CSRF si uso JWT?

Un error muy común es pensar que “si uso JWT, ya no necesito CSRF”. Eso solo es cierto cuando el token se envía manualmente en una cabecera `Authorization: Bearer ...` y **no** en una cookie automática.

Cuando el JWT va dentro de una cookie:

- el navegador la adjunta automáticamente,
- incluso aunque la petición se origine desde una página maliciosa,
- y eso permite que un atacante intente forzar acciones en nombre del usuario autenticado.

Por eso:

- **JWT en header** → normalmente se puede desactivar CSRF.
- **JWT en cookie** → normalmente **debes mantener CSRF activado**.

---

## 4. Estrategia recomendada

La estrategia más habitual en Spring Security es:

### Para autenticación

- Guardar el **access token** JWT en una cookie `HttpOnly`.
- Leer esa cookie en un filtro personalizado.
- Validar el JWT y construir el objeto `Authentication`.

### Para CSRF

- Usar `CookieCsrfTokenRepository`.
- Enviar el token CSRF en una cookie legible por JavaScript.
- Hacer que el frontend lea esa cookie y reenvíe su valor en una cabecera, normalmente `X-XSRF-TOKEN`.

Este patrón se conoce como **double submit cookie pattern**.

---

## 5. Dependencias base

En una aplicación Spring Boot moderna normalmente necesitas, como mínimo:

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>

    <dependency>
        <groupId>io.jsonwebtoken</groupId>
        <artifactId>jjwt-api</artifactId>
        <version>0.12.5</version>
    </dependency>
    <dependency>
        <groupId>io.jsonwebtoken</groupId>
        <artifactId>jjwt-impl</artifactId>
        <version>0.12.5</version>
        <scope>runtime</scope>
    </dependency>
    <dependency>
        <groupId>io.jsonwebtoken</groupId>
        <artifactId>jjwt-jackson</artifactId>
        <version>0.12.5</version>
        <scope>runtime</scope>
    </dependency>
    
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-validation</artifactId>
    </dependency>
</dependencies>
```

Puedes usar cualquier librería JWT equivalente; lo importante es que el backend firme y valide correctamente los tokens.

---

## 6. Configuración principal de Spring Security

Un ejemplo típico de configuración sería este:

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.http.HttpMethod;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.config.Customizer;
import org.springframework.security.config.annotation.authentication.configuration.AuthenticationConfiguration;
import org.springframework.security.config.annotation.method.configuration.EnableMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configurers.AbstractHttpConfigurer;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;
import org.springframework.security.web.csrf.CookieCsrfTokenRepository;

@Configuration
@EnableMethodSecurity
public class SecurityConfig {

    private final JwtAuthenticationFilter jwtAuthenticationFilter;

    public SecurityConfig(JwtAuthenticationFilter jwtAuthenticationFilter) {
        this.jwtAuthenticationFilter = jwtAuthenticationFilter;
    }

    @Bean
    SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .cors(Customizer.withDefaults())
            .csrf(csrf -> csrf
                .csrfTokenRepository(CookieCsrfTokenRepository.withHttpOnlyFalse())
            )
            .sessionManagement(session -> session
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
            )
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/auth/login", "/auth/refresh", "/auth/csrf").permitAll()
                .requestMatchers(HttpMethod.GET, "/public/**").permitAll()
                .anyRequest().authenticated()
            )
            .addFilterBefore(jwtAuthenticationFilter, UsernamePasswordAuthenticationFilter.class)
            .logout(logout -> logout
                .logoutUrl("/auth/logout")
                .deleteCookies("ACCESS_TOKEN", "XSRF-TOKEN")
                .logoutSuccessHandler((request, response, authentication) -> response.setStatus(204))
            );

        return http.build();
    }

    @Bean
    AuthenticationManager authenticationManager(AuthenticationConfiguration configuration) throws Exception {
        return configuration.getAuthenticationManager();
    }
}
```

### Puntos importantes de esta configuración

- `SessionCreationPolicy.STATELESS`: Spring Security no usa sesión de servidor para autenticar.
- `CookieCsrfTokenRepository.withHttpOnlyFalse()`: genera el token CSRF en cookie y permite que el frontend lo lea.
- `addFilterBefore(...)`: inserta el filtro JWT antes del filtro estándar de usuario/contraseña.
- `logout().deleteCookies(...)`: limpia cookies de autenticación y CSRF al cerrar sesión.

> Aunque la app sea stateless, **CSRF sigue siendo relevante** porque la credencial viaja en cookie.

---

## 7. Configuración CORS correcta

Si tu frontend corre en otro origen, debes permitir credenciales explícitamente:

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.cors.CorsConfiguration;
import org.springframework.web.cors.CorsConfigurationSource;
import org.springframework.web.cors.UrlBasedCorsConfigurationSource;

import java.util.List;

@Configuration
public class CorsConfig {

    @Bean
    CorsConfigurationSource corsConfigurationSource() {
        CorsConfiguration configuration = new CorsConfiguration();
        configuration.setAllowedOrigins(List.of("https://app.midominio.com"));
        configuration.setAllowedMethods(List.of("GET", "POST", "PUT", "PATCH", "DELETE", "OPTIONS"));
        configuration.setAllowedHeaders(List.of("Content-Type", "X-XSRF-TOKEN"));
        configuration.setAllowCredentials(true);

        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        source.registerCorsConfiguration("/**", configuration);
        return source;
    }
}
```

### Reglas importantes

- Si usas cookies, debes enviar peticiones con credenciales (`credentials: 'include'` en frontend).
- Si `allowCredentials(true)` está activo, **no uses `*` como origen permitido**.
- Debes listar explícitamente los orígenes autorizados.

---

## 8. Servicio para generar y validar JWT

Un servicio simple podría verse así:

```java
import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import io.jsonwebtoken.security.Keys;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.stereotype.Service;

import javax.crypto.SecretKey;
import java.nio.charset.StandardCharsets;
import java.time.Instant;
import java.time.temporal.ChronoUnit;
import java.util.Date;

@Service
public class JwtService {

    private final SecretKey secretKey;

    public JwtService(@Value("${security.jwt.secret}") String secret) {
        this.secretKey = Keys.hmacShaKeyFor(secret.getBytes(StandardCharsets.UTF_8));
    }

    public String generateToken(UserDetails userDetails) {
        Instant now = Instant.now();

        return Jwts.builder()
            .subject(userDetails.getUsername())
            .issuedAt(Date.from(now))
            .expiration(Date.from(now.plus(15, ChronoUnit.MINUTES)))
            .claim("roles", userDetails.getAuthorities().stream()
                .map(authority -> authority.getAuthority())
                .toList())
            .signWith(secretKey, SignatureAlgorithm.HS256)
            .compact();
    }

    public String extractUsername(String token) {
        return parseClaims(token).getSubject();
    }

    public boolean isTokenValid(String token, UserDetails userDetails) {
        String username = extractUsername(token);
        return username.equals(userDetails.getUsername()) && !parseClaims(token).getExpiration().before(new Date());
    }

    private Claims parseClaims(String token) {
        return Jwts.parser()
            .verifyWith(secretKey)
            .build()
            .parseSignedClaims(token)
            .getPayload();
    }
}
```

### Recomendaciones

- Usa una clave suficientemente larga y almacenada fuera del código fuente.
- Mantén el access token con vida corta.
- Si necesitas sesiones prolongadas, usa un **refresh token** separado, también en cookie segura.

---

## 9. Filtro para autenticar a partir de la cookie

El siguiente filtro busca el JWT en una cookie llamada `ACCESS_TOKEN`:

```java
import jakarta.servlet.FilterChain;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.Cookie;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.web.authentication.WebAuthenticationDetailsSource;
import org.springframework.stereotype.Component;
import org.springframework.web.filter.OncePerRequestFilter;

import java.io.IOException;
import java.util.Arrays;
import java.util.Optional;

@Component
public class JwtAuthenticationFilter extends OncePerRequestFilter {

    private final JwtService jwtService;
    private final UserDetailsService userDetailsService;

    public JwtAuthenticationFilter(JwtService jwtService, UserDetailsService userDetailsService) {
        this.jwtService = jwtService;
        this.userDetailsService = userDetailsService;
    }

    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain) throws ServletException, IOException {

        Optional<String> jwtOptional = extractTokenFromCookies(request.getCookies());

        if (jwtOptional.isPresent() && SecurityContextHolder.getContext().getAuthentication() == null) {
            String jwt = jwtOptional.get();
            String username = jwtService.extractUsername(jwt);

            UserDetails userDetails = userDetailsService.loadUserByUsername(username);

            if (jwtService.isTokenValid(jwt, userDetails)) {
                UsernamePasswordAuthenticationToken authentication =
                    new UsernamePasswordAuthenticationToken(
                        userDetails,
                        null,
                        userDetails.getAuthorities()
                    );

                authentication.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));
                SecurityContextHolder.getContext().setAuthentication(authentication);
            }
        }

        filterChain.doFilter(request, response);
    }

    private Optional<String> extractTokenFromCookies(Cookie[] cookies) {
        if (cookies == null) {
            return Optional.empty();
        }

        return Arrays.stream(cookies)
            .filter(cookie -> "ACCESS_TOKEN".equals(cookie.getName()))
            .map(Cookie::getValue)
            .findFirst();
    }
}
```

### Buenas prácticas en este filtro

- No confíes solo en que la cookie exista: valida firma y expiración.
- Evita autenticar si ya hay un `Authentication` cargado.
- Si el token es inválido, deja que la cadena continúe y que Spring responda con 401/403 según corresponda.

---

## 10. Login: emitir el JWT en una cookie segura

Tras autenticar usuario y contraseña, debes devolver el JWT dentro de una cookie:

```java
import jakarta.validation.Valid;
import org.springframework.http.HttpHeaders;
import org.springframework.http.ResponseCookie;
import org.springframework.http.ResponseEntity;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/auth")
public class AuthController {

    private final AuthenticationManager authenticationManager;
    private final JwtService jwtService;

    public AuthController(AuthenticationManager authenticationManager, JwtService jwtService) {
        this.authenticationManager = authenticationManager;
        this.jwtService = jwtService;
    }

    @PostMapping("/login")
    public ResponseEntity<Void> login(@Valid @RequestBody LoginRequest request) {
        Authentication authentication = authenticationManager.authenticate(
            new UsernamePasswordAuthenticationToken(request.username(), request.password())
        );

        UserDetails user = (UserDetails) authentication.getPrincipal();
        String jwt = jwtService.generateToken(user);

        ResponseCookie accessCookie = ResponseCookie.from("ACCESS_TOKEN", jwt)
            .httpOnly(true)
            .secure(true)
            .sameSite("Strict")
            .path("/")
            .maxAge(15 * 60)
            .build();

        return ResponseEntity.noContent()
            .header(HttpHeaders.SET_COOKIE, accessCookie.toString())
            .build();
    }
}
```

### Qué significa cada atributo

- `httpOnly(true)`: impide que JavaScript lea la cookie.
- `secure(true)`: solo se envía por HTTPS.
- `sameSite("Strict")`: la cookie no viaja en contextos cross-site. Es la opción más restrictiva.
- `path("/")`: disponible para toda la aplicación.
- `maxAge(...)`: tiempo de vida de la cookie.

### ¿Strict, Lax o None?

- **Strict**: máxima protección CSRF, pero más restrictivo.
- **Lax**: equilibrio razonable para muchos casos web.
- **None**: necesario en escenarios cross-site reales, pero exige `Secure`.

Si tu frontend y tu backend viven en dominios distintos y necesitas que la cookie viaje entre ambos, probablemente debas usar:

- `SameSite=None`
- `Secure=true`

Y eso hace todavía más importante una protección CSRF correcta.

---

## 11. Exponer o inicializar el token CSRF

Con `CookieCsrfTokenRepository`, Spring genera el token CSRF y lo espera en cabecera. En muchos casos basta con que el cliente haga una primera petición al backend para recibir la cookie `XSRF-TOKEN`.

También puedes exponer un endpoint para forzar su creación:

```java
import org.springframework.security.web.csrf.CsrfToken;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class CsrfController {

    @GetMapping("/auth/csrf")
    public CsrfToken csrf(CsrfToken csrfToken) {
        return csrfToken;
    }
}
```

Esto permite que el frontend:

1. llame a `/auth/csrf`,
2. reciba la cookie `XSRF-TOKEN`,
3. lea su valor,
4. lo envíe en `X-XSRF-TOKEN` en cada petición mutadora.

---

## 12. Cómo debe trabajar el frontend

Si el frontend es una SPA, el flujo suele ser:

1. Hacer `GET /auth/csrf` con credenciales incluidas.
2. Guardar el valor de la cookie `XSRF-TOKEN` leyendo `document.cookie`.
3. Enviar ese valor en la cabecera `X-XSRF-TOKEN`.
4. Hacer login.
5. El backend devuelve la cookie `ACCESS_TOKEN`.
6. A partir de ahí, el navegador envía automáticamente esa cookie en cada petición.

Ejemplo con `fetch`:

```javascript
await fetch("https://api.midominio.com/auth/csrf", {
  method: "GET",
  credentials: "include"
});

const xsrfToken = document.cookie
  .split("; ")
  .find(row => row.startsWith("XSRF-TOKEN="))
  ?.split("=")[1];

await fetch("https://api.midominio.com/auth/login", {
  method: "POST",
  credentials: "include",
  headers: {
    "Content-Type": "application/json",
    "X-XSRF-TOKEN": xsrfToken
  },
  body: JSON.stringify({
    username: "admin",
    password: "secreta"
  })
});
```

Y para un `POST` protegido:

```javascript
await fetch("https://api.midominio.com/orders", {
  method: "POST",
  credentials: "include",
  headers: {
    "Content-Type": "application/json",
    "X-XSRF-TOKEN": xsrfToken
  },
  body: JSON.stringify({
    productId: 10,
    quantity: 2
  })
});
```

---

## 13. Logout correcto

Cerrar sesión no consiste solo en “olvidar” el token en cliente. Debes invalidar la cookie en el navegador:

```java
@PostMapping("/logout")
public ResponseEntity<Void> logout() {
    ResponseCookie deleteAccessCookie = ResponseCookie.from("ACCESS_TOKEN", "")
        .httpOnly(true)
        .secure(true)
        .sameSite("Strict")
        .path("/")
        .maxAge(0)
        .build();

    ResponseCookie deleteCsrfCookie = ResponseCookie.from("XSRF-TOKEN", "")
        .httpOnly(false)
        .secure(true)
        .sameSite("Strict")
        .path("/")
        .maxAge(0)
        .build();

    return ResponseEntity.noContent()
        .header(HttpHeaders.SET_COOKIE, deleteAccessCookie.toString())
        .header(HttpHeaders.SET_COOKIE, deleteCsrfCookie.toString())
        .build();
}
```

Si además manejas refresh tokens, también debes eliminarlos.

---

## 14. Refresh token por cookie

En sistemas reales, el access token suele durar poco. Para evitar que el usuario haga login constantemente, se usa un **refresh token** con una cookie aparte.

Recomendaciones:

- `ACCESS_TOKEN`: vida corta, por ejemplo 10-15 minutos.
- `REFRESH_TOKEN`: vida más larga, por ejemplo varios días.
- Ambos en cookies `HttpOnly`.
- El refresh token debe ser revocable y, preferiblemente, persistido o versionado en servidor.

Flujo habitual:

1. El access token expira.
2. El frontend llama a `/auth/refresh`.
3. El backend lee la cookie `REFRESH_TOKEN`.
4. Si es válida, emite nuevo `ACCESS_TOKEN`.
5. Devuelve una nueva cookie.

**Importante:** el endpoint de refresh, al usar cookies automáticas, también debe estar protegido de acuerdo con tu política CSRF si puede ser invocado desde navegador.

---

## 15. Qué endpoints requieren CSRF y cuáles no

Por defecto, Spring Security exige CSRF en métodos que cambian estado:

- `POST`
- `PUT`
- `PATCH`
- `DELETE`

Normalmente:

- `GET`, `HEAD`, `OPTIONS`, `TRACE` no requieren token CSRF.
- Login, logout, refresh y cualquier operación mutadora vía navegador **sí deberían considerarse** para CSRF.

Puedes excluir rutas concretas, pero conviene hacerlo solo cuando entiendes perfectamente el riesgo:

```java
.csrf(csrf -> csrf
    .csrfTokenRepository(CookieCsrfTokenRepository.withHttpOnlyFalse())
    .ignoringRequestMatchers("/webhooks/**")
)
```

Solo deberías ignorar CSRF en endpoints que:

- no dependan de cookies automáticas de usuario,
- sean invocados por terceros,
- o usen otro mecanismo de autenticación no vulnerable a CSRF.

---

## 16. Diferencia entre XSS y CSRF en este enfoque

Conviene separar ambos riesgos:

### JWT en cookie ayuda contra ciertos escenarios de XSS

Si la cookie es `HttpOnly`, un script malicioso no puede leer fácilmente el token.

### Pero no elimina XSS

Un XSS sigue siendo grave porque puede:

- ejecutar acciones en nombre del usuario,
- leer datos visibles en la página,
- disparar peticiones autenticadas desde el navegador.

### Y tampoco elimina CSRF

Como la cookie viaja sola, necesitas el token CSRF para probar que la petición proviene de tu frontend legítimo.

La seguridad real aparece cuando combinas:

- cookies seguras,
- CSRF token,
- validaciones de entrada,
- escape/sanitización frente a XSS,
- CORS correcto,
- expiraciones cortas,
- refresh controlado.

---

## 17. Errores comunes

### 1. Desactivar CSRF “porque ya uso JWT”

Si el JWT está en cookie, esta decisión suele ser incorrecta.

### 2. Guardar el JWT en cookie pero también permitir `SameSite=None` sin revisar CSRF

Eso amplía la superficie de ataque si no has blindado bien la app.

### 3. Marcar la cookie CSRF como `HttpOnly`

Si el frontend necesita leerla para reenviarla en cabecera, no podrá hacerlo.

### 4. Usar `Access-Control-Allow-Origin: *` con credenciales

Los navegadores no lo aceptan, y además es inseguro.

### 5. No enviar `credentials: 'include'` desde el frontend

Entonces las cookies no viajarán.

### 6. No invalidar cookies en logout

El navegador puede seguir conservándolas hasta su expiración.

### 7. Tokens demasiado largos o sin rotación

Aumenta el impacto de una filtración o reutilización indebida.

---

## 18. Recomendaciones de seguridad

1. Usa siempre **HTTPS**.
2. Marca cookies sensibles con `Secure`.
3. Usa `HttpOnly` para access y refresh tokens.
4. Usa expiraciones cortas en access tokens.
5. Valora rotación de refresh token.
6. Mantén CSRF habilitado cuando la autenticación dependa de cookies.
7. Restringe CORS a orígenes concretos.
8. No metas información sensible innecesaria dentro del JWT.
9. Considera blacklist, versionado o revocación si el caso de uso lo requiere.
10. Registra eventos de autenticación, refresh y logout.

---

## 19. Flujo completo recomendado

Una implementación robusta suele quedar así:

1. El frontend solicita un token CSRF.
2. El backend responde con cookie `XSRF-TOKEN`.
3. El frontend hace login enviando `X-XSRF-TOKEN`.
4. El backend autentica y devuelve cookie `ACCESS_TOKEN`.
5. En cada petición protegida, el navegador envía `ACCESS_TOKEN`.
6. El frontend agrega `X-XSRF-TOKEN` en peticiones mutadoras.
7. El filtro JWT autentica al usuario.
8. Si el access token expira, el frontend llama a refresh.
9. El backend emite un nuevo access token.
10. En logout, el backend elimina cookies.

---

## 20. ¿Cuándo conviene este enfoque?

Este enfoque suele encajar muy bien cuando:

- el cliente principal es un navegador,
- quieres evitar `localStorage` para el token,
- necesitas un flujo web clásico o SPA,
- puedes controlar bien CORS, cookies y CSRF.

Puede no ser la mejor opción si:

- el cliente principal es móvil o machine-to-machine,
- prefieres un esquema puro `Authorization: Bearer`,
- no quieres depender del comportamiento de cookies del navegador.

---

## 21. Resumen final

Implementar **JWT por cookie** en Spring Security es perfectamente viable y, bien hecho, es un enfoque sólido para aplicaciones web. La clave es entender que mover el token a una cookie cambia el modelo de riesgo:

- ganas protección frente a la lectura directa del token desde JavaScript si usas `HttpOnly`,
- pero recuperas el problema de **CSRF** porque el navegador envía la cookie automáticamente.

Por eso, la implementación correcta no es solo “guardar el JWT en una cookie”, sino combinar:

- **cookie segura para el JWT**,
- **protección CSRF mediante cookie + header**,
- **filtro JWT en Spring Security**,
- **CORS estricto**,
- **logout y refresh bien diseñados**.

Si aplicas estas piezas de forma coherente, obtendrás una autenticación moderna, robusta y adecuada para aplicaciones Spring Boot con frontend web.
