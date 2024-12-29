



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















## guia:

# Guía para generar claves privadas y públicas con OpenSSL y usarlas en Spring Security

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