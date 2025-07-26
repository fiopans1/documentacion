# Introducción OAUTH2:

En un mundo donde la seguridad y la privacidad son fundamentales, permitir que las aplicaciones accedan a datos de los usuarios **sin exponer sus credenciales** se ha vuelto una necesidad. Aquí es donde entra en juego **OAuth 2.0**, un protocolo de autorización moderno, seguro y ampliamente adoptado que permite delegar el acceso a recursos protegidos de forma controlada y confiable.

OAuth 2.0 no es un sistema de autenticación en sí, sino un **mecanismo que permite a los usuarios conceder acceso limitado a sus datos** almacenados en un servicio (como Google, Facebook o Authentik), sin necesidad de compartir sus contraseñas con las aplicaciones que solicitan ese acceso. Esto se consigue mediante el uso de **tokens de acceso temporales**, que actúan como llaves de acceso válidas por un tiempo definido.

## Introducción a OAuth 2.0

OAuth 2.0 es un **framework de autorización** que permite a aplicaciones (clientes) obtener acceso limitado a recursos protegidos en nombre de un usuario, sin necesidad de compartir sus credenciales (usuario/contraseña) con la aplicación. Fue desarrollado por la IETF (RFC 6749) y se utiliza ampliamente para habilitar “Login con Google/Facebook/GitHub”, APIs de terceros, y arquitecturas de microservicios.

### Actores principales

1. **Resource Owner (Usuario):** Titular de los datos o recursos protegidos.
2. **Client (Cliente):** Aplicación que quiere acceder a los recursos.
3. **Authorization Server (Servidor de autorización):** Emite tokens de acceso previo consentimiento del usuario.
4. **Resource Server (Servidor de recursos):** Aloja los recursos protegidos y acepta tokens emitidos por el Authorization Server.

### Flujo básico (Grant Types)

1. **Authorization Code (Código de autorización):**

   - Ideal para aplicaciones servidor‑a‑servidor o web.
   - El cliente redirige al usuario al Authorization Server, este autentica y solicita consentimiento, luego devuelve un código que el cliente intercambia por un token de acceso.

2. **Implicit (Explícito):**

   - Para aplicaciones de una sola página (SPA).
   - El token de acceso se entrega directamente al cliente sin pasar por código de autorización (menos seguro, ya no recomendado).

3. **Resource Owner Password Credentials (Credenciales del usuario):**

   - El usuario proporciona usuario y contraseña directamente al cliente, que los envía al Authorization Server.
   - Solo para aplicaciones muy confiables (legacy).

4. **Client Credentials (Credenciales del cliente):**

   - Para acceso machine‑to‑machine: el cliente usa su propio ID y secreto para obtener un token, sin intervención de usuario.

5. **Refresh Token (Token de actualización):**

   - Permite renovar el token de acceso caducado sin autenticación del usuario.

### ¿Para qué sirve OAuth 2.0?

- **Seguridad y confinamiento de credenciales:** El usuario no expone sus credenciales al cliente.
- **Delegación de acceso:** Permite al usuario controlar el nivel de acceso, pudiendo revocar tokens sin cambiar su contraseña.
- **Integración sencilla con proveedores externos:** Google, Facebook, GitHub, Azure AD, etc.
- **Escalabilidad en APIs y microservicios:** Los servicios confían en tokens JWT u otros formatos estándar.

---

## Configuración en Spring Boot

Spring Boot ofrece soporte integral a OAuth 2.0 a través de **Spring Security OAuth 2.0 Client**. La configuración se realiza principalmente en el archivo `application.yml` o `application.properties`, bajo el prefijo:

```yaml
spring:
  security:
    oauth2:
      client:
        registration:
        provider:
```

### `registration`

Define **cada cliente o “aplicación”** que se integra como “relying party” frente al Authorization Server. Aquí declaras:

- **`client-id` y `client-secret`**: Credenciales que la aplicación obtiene al registrarse en el proveedor (e.g., consola de Google).
- **`authorization-grant-type`**: Tipo de flujo (authorization_code, client_credentials, password, refresh_token).
- **`redirect-uri`**: URI donde el Authorization Server devolverá el código o token.
- **`scope`**: Alcances de permisos solicitados (perfil, email, api.read, etc.).
- **`client-name`**: Nombre identificativo para logs y mensajes.

Ejemplo en YAML:

```yaml
spring:
  security:
    oauth2:
      client:
        registration:
          google:
            client-id: tu-google-client-id
            client-secret: tu-google-secret
            authorization-grant-type: authorization_code
            redirect-uri: "{baseUrl}/login/oauth2/code/{registrationId}"
            scope:
              - openid
              - profile
              - email
```

> **Nota:** `{baseUrl}` suele resolver al dominio donde corre tu app, `{registrationId}` al nombre que le des (`google` en este caso).

### `provider`

Define **detalles del Authorization Server** al que apunta una o varias `registration`. Incluye:

- **`authorization-uri`**: Endpoint para solicitar el código de autorización.
- **`token-uri`**: Endpoint para intercambiar código por token de acceso.
- **`user-info-uri`**: (Opcional) Endpoint para obtener información del usuario (profile).
- **`jwk-set-uri`**: (Opcional) URI para obtener claves públicas en formato JWK, usado para verificar JWT.
- **`user-name-attribute`**: (Opcional) Atributo JSON que identifica el nombre de usuario (e.g., `sub`, `id`).

Ejemplo enlazando con el registration anterior:

```yaml
spring:
  security:
    oauth2:
      client:
        provider:
          google:
            authorization-uri: https://accounts.google.com/o/oauth2/v2/auth
            token-uri: https://oauth2.googleapis.com/token
            user-info-uri: https://openidconnect.googleapis.com/v1/userinfo
            user-name-attribute: sub
            jwk-set-uri: https://www.googleapis.com/oauth2/v3/certs
```

### Relación Registration ↔ Provider

- El **`registrationId`** (p. ej. `google`) en `registration` se usa para vincular a la sección `provider` del mismo nombre.
- Puedes usar un mismo proveedor con varios clientes (p. ej. móvil y web), o múltiples proveedores con un mismo cliente.

---

## Flujo de ejemplo en Spring Boot

1. Usuario accede a `/oauth2/authorization/google` (ruta automática que genera Spring).
2. Spring Security redirige al usuario a `authorization-uri`.
3. Usuario se autentica y consiente.
4. Google redirige a tu `redirect-uri` con un código.
5. Spring intercambia código por token en `token-uri`.
6. Spring obtiene perfil en `user-info-uri` y llena el `Authentication` en el contexto de seguridad.
7. Tu aplicación recibe al usuario autenticado con datos (nombre, email, roles, etc.).

---

## Conclusión

- **OAuth 2.0** es el estándar de facto para delegar autorización sin exponer credenciales.
- **Spring Boot** centraliza la configuración en `spring.security.oauth2.client.registration` (clientes) y `…provider` (servidores).
- Definir correctamente ambos bloques en `application.yml`/`.properties` permite integrar cualquier proveedor (Google, Facebook, Okta, Auth0, etc.) de manera declarativa y segura.

# Guía completa: Integrar OAuth2 (Google) en Spring Boot junto a login clásico

## Introducción

Esta guía te llevará paso a paso a entender cómo funciona OAuth 2.0, cuáles son sus componentes clave, sus distintos flujos (grant types) y cómo aplicarlo correctamente en tus propios proyectos, tanto en frontend como en backend.

La guía incluirá:

- Configuración desde cero del inicio de sesión clásico con usuario y contraseña.
- Configuración de OAuth2 con Google.
- Dos escenarios distintos:

  1. Cómo alternar entre login clásico y OAuth2 en función de una propiedad o criterio.
  2. Cómo permitir que ambos sistemas de autenticación convivan y el usuario pueda elegir.

Te avisaré en cuanto tenga la guía lista para tu revisión.

En esta guía paso a paso aprenderemos cómo **integrar OAuth2 (inicio de sesión con Google)** en una aplicación **Spring Boot (monolítica)** que ya dispone de un mecanismo de autenticación clásico mediante usuario y contraseña. Explicaremos cómo configurar el **login social con Google** usando Spring Security (en su versión más reciente), manteniendo al mismo tiempo el **login tradicional**. Además, veremos dos enfoques de configuración en secciones separadas:

- Cómo alternar entre **autenticación clásica u OAuth2** según una propiedad de configuración (u otro criterio).
- Cómo **habilitar ambos métodos** de autenticación simultáneamente en la misma aplicación.

Al final, tendrás una referencia completa para integrar un proveedor OAuth2 (usaremos Google por simplicidad) junto con tu inicio de sesión convencional.

## Prerrequisitos y dependencias

Antes de comenzar, asegúrate de contar con lo siguiente:

- **Proyecto Spring Boot** ya configurado con Spring Security para login clásico (usuario/contraseña). Esto implica que ya tienes las dependencias de Spring Security añadidas (por ejemplo, `spring-boot-starter-security`) y posiblemente una página de login personalizada con sus URLs de autenticación.
- **Java 17+** (requerido por las últimas versiones de Spring Boot 3.x/Spring Security 6).
- **Dependencia de cliente OAuth2**: agrega la dependencia de Spring Security OAuth2 Client a tu proyecto. En Maven, por ejemplo, añade en el `pom.xml` lo siguiente:

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-oauth2-client</artifactId>
  </dependency>
  ```

  Esta dependencia (`spring-boot-starter-oauth2-client`) incluye el soporte para OAuth2/OIDC que usaremos para integrar el login con Google. _(Si usas Gradle, agrega la dependencia equivalente)._

## Paso 1: Registrar la aplicación OAuth2 en Google

El primer paso es obtener las credenciales OAuth2 de Google (Client ID y Client Secret) registrando tu aplicación en **Google API Console**. Sigue estos pasos:

1. Ingresa a la [Google API Console](https://console.developers.google.com/) con tu cuenta de Google. Ve a la sección **Credenciales** y crea un **ID de cliente de OAuth 2.0** (tipo de aplicación: Aplicación web).
2. Al crear las credenciales, especifica un **Nombre** para tu cliente OAuth (por ejemplo, "Spring Boot OAuth2 App") y configura la **URI de redirección autorizada** a tu aplicación. Usa la URL de callback predeterminada de Spring Security para Google: `http://localhost:8080/login/oauth2/code/google` . Es importante incluir exactamente esta ruta, ya que es donde Spring Security espera la respuesta de Google tras la autenticación. Si tu aplicación utiliza un contexto diferente o está desplegada en otra ruta, debes ajustar la URI de redirección en Google en consecuencia (por ejemplo, `http://localhost:8080/miapp/login/oauth2/code/google`) .
3. Completa el registro. Google te proporcionará un **Client ID** y un **Client Secret**. Anota estos valores de forma segura, ya que los necesitarás en la configuración de tu aplicación Spring Boot.

> **Nota:** Asegúrate de tener activado el API de Google OAuth2 si es necesario y de haber configurado correctamente las pantallas de consentimiento según las políticas de Google. Para nuestra guía usaremos Google como proveedor externo por sencillez, pero el procedimiento sería similar con otros proveedores (Facebook, GitHub, etc., cambiando la configuración correspondiente).

## Paso 2: Configurar las propiedades de OAuth2 en Spring Boot

Con las credenciales de Google en mano, configuraremos nuestra aplicación Spring Boot para que conozca estos valores y pueda usarlos en el flujo OAuth2. Agrega las siguientes propiedades en tu archivo de configuración (por ejemplo, en **`application.properties`** o **`application.yml`**):

- **ID y secreto de cliente:** indicar el Client ID y Client Secret proporcionados por Google.
- **Scopes (alcances):** opcionalmente especifica qué información solicitaremos. Para login con Google suele usarse los alcances básicos de perfil e email.

Si utilizas `application.properties`, podrías añadir:

```properties
spring.security.oauth2.client.registration.google.client-id = TU_CLIENT_ID_DE_GOOGLE
spring.security.oauth2.client.registration.google.client-secret = TU_CLIENT_SECRET_DE_GOOGLE
spring.security.oauth2.client.registration.google.scope = email, profile
```

En formato YAML, la configuración sería equivalente a:

```yaml
spring:
  security:
    oauth2:
      client:
        registration:
          google:
            clientId: TU_CLIENT_ID_DE_GOOGLE
            clientSecret: TU_CLIENT_SECRET_DE_GOOGLE
            scope:
              - email
              - profile
```

Como se muestra en el ejemplo, definimos un registro de cliente OAuth2 llamado **google** con su ID de cliente, secreto y los scopes deseados. Spring Security reconoce "google" como un proveedor _preconfigurado_ (trae endpoints por defecto), por lo que con solo estos datos es capaz de manejar el proceso de autenticación con Google.

> **Nota:** Si no especificas los scopes, Spring usará valores por defecto apropiados. En el caso de Google, Spring Security solicitará permisos de perfil básico. Incluir `email` y `profile` explícitamente asegura que obtengamos acceso al email verificado del usuario y a su información de perfil público.

## Paso 3: Configurar Spring Security para soportar OAuth2 y login clásico

Ahora viene la parte central: ajustar la configuración de seguridad (`SecurityConfig`) para integrar el login de Google **además** del login clásico. Utilizaremos la API moderna de Spring Security (SecurityFilterChain en lugar de la clase obsoleta `WebSecurityConfigurerAdapter`). En tu clase de configuración de seguridad, define el `SecurityFilterChain` de la siguiente manera:

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        // Autorizaciones: URLs públicas y protegidas
        http.authorizeHttpRequests(auth -> auth
            .requestMatchers("/", "/login", "/oauth2/**", "/login/oauth2/**", "/css/**", "/js/**").permitAll()  // permitir acceso sin auth a home, login y URLs OAuth2
            .anyRequest().authenticated()  // el resto requiere autenticación
        );
        // Configurar login form clásico
        http.formLogin(form -> form
            .loginPage("/login")        // página de login personalizada (ruta)
            .permitAll()               // permitir acceso a la página de login a cualquiera
        );
        // Configurar login OAuth2 (Google)
        http.oauth2Login(oauth -> oauth
            .loginPage("/login")       // usar la misma página de login para OAuth2
        );
        return http.build();
    }
}
```

En esta configuración:

- Definimos qué rutas son públicas (`permitAll`) y cuáles requieren autenticación. Debes **permitir sin autenticación** tanto la URL de la página de login (`/login`) como las URLs usadas por el flujo OAuth2: en particular `*/oauth2/**` (que cubre la URL de inicio del login social, e.g. `/oauth2/authorization/google`) y `*/login/oauth2/**` (que cubre la URL de callback a la que Google redirige de vuelta con el código de autenticación). Esto es importante para que el proceso OAuth2 funcione correctamente.
- Habilitamos el **form login** tradicional mediante `http.formLogin()`. Aquí indicamos una página de login custom (`/login`) que ya existe en la aplicación (por ejemplo, un formulario HTML donde el usuario ingresa usuario y contraseña). La opción `.permitAll()` permite el acceso a esa página a cualquier usuario (no autenticado).
- Habilitamos el **OAuth2 login** mediante `http.oauth2Login()`. Con solo invocar este método, Spring Security activa el soporte para OAuth2/OIDC. Usamos `.loginPage("/login")` para que Spring utilice la misma página de inicio de sesión personalizada. Esto significa que nuestra página de login servirá tanto para el formulario tradicional como para presentar la opción de "Login con Google".

Al incluir **ambos** `formLogin()` y `oauth2Login()` en la configuración, le indicamos a Spring Security que acepte ambos métodos de autenticación. De hecho, **ofrecer ambos métodos a la vez es tan sencillo como invocar ambos métodos en la configuración**. Spring Security se encargará de generar una página de inicio de sesión por defecto que contenga el formulario de usuario/contraseña **y** un enlace para el proveedor OAuth2 configurado, si no tienes una página personalizada.

Si en tu caso ya tienes una página de login personalizada (por ejemplo, una plantilla Thymeleaf), deberás **añadir manualmente un enlace** para la opción de "Iniciar sesión con Google". Este enlace debe dirigir al endpoint de autorización OAuth2 proporcionado por Spring Security, típicamente `/oauth2/authorization/google`. Por ejemplo, en Thymeleaf podrías agregar algo como:

```html
<a th:href="@{/oauth2/authorization/google}">Iniciar sesión con Google</a>
```

Asegúrate de mostrar este enlace en tu página de login junto al formulario tradicional, indicando que el usuario puede iniciar sesión con Google como alternativa. En el código de ejemplo anterior, se muestra un encabezado "Login with Google" seguido de "OR" (o) y luego el formulario de login local, ilustrando cómo presentar ambas opciones al usuario.

> **Nota:** Si no personalizas la página de login, Spring Security generará automáticamente una página por defecto en la ruta `/login` que incluirá tanto el formulario de usuario/contraseña como un botón/enlace "Login with Google" (usando el nombre del cliente configurado). Esta página por defecto es útil para pruebas, pero en una aplicación real normalmente querrás diseñar la pantalla de login. En tal caso, recuerda registrar un controlador para servir tu página `/login` y permitir el acceso público a dicha ruta.

Hasta este punto, hemos configurado la aplicación para que cualquier usuario pueda autenticarse **ya sea** mediante el login clásico (con las credenciales de nuestra base de datos de usuarios) **o** mediante Google OAuth2. Spring Security manejará internamente la lógica para cada caso:

- Si el usuario envía el formulario de login, se procesará con la autenticación estándar (usando tu `UserDetailsService`, etc.).
- Si el usuario hace clic en "Iniciar con Google", Spring redirigirá al usuario a Google, gestionará el callback en `/login/oauth2/code/google` y, si la autenticación es exitosa, creará una sesión autenticada en la aplicación.

A continuación, abordaremos dos escenarios de configuración según tus necesidades: usar **uno u otro método de login** (mutuamente excluyentes) o **ofrecer ambos a la vez**.

## Alternativa 1: Alternar entre login clásico y OAuth2 según una propiedad

En algunos casos, puede que quieras habilitar **solo uno de los métodos de autenticación a la vez** (por ejemplo, permitir el login clásico _o_ el social, dependiendo de una configuración o entorno). Esto se puede lograr introduciendo una propiedad en tu configuración que controle el modo de autenticación y ajustando la seguridad condicionalmente.

Por ejemplo, definamos en `application.properties` una propiedad personalizada llamada `app.auth.mode` que pueda tomar valores como `"classic"`, `"oauth2"` o `"both"`. Según su valor, la configuración de seguridad habilitará uno u otro método:

```properties
app.auth.mode = classic    # valores posibles: classic, oauth2, both
```

En la clase de seguridad, podemos leer esta propiedad (usando `@Value` o el `Environment`) y condicionar la configuración:

```java
// Supongamos que obtenemos el modo de auth de la propiedad:
String authMode = env.getProperty("app.auth.mode", "both");

if (!"oauth2".equalsIgnoreCase(authMode)) {
    // Si el modo NO es solo oauth2, habilitamos login clásico
    http.formLogin(form -> form
        .loginPage("/login")
        .permitAll());
}
if (!"classic".equalsIgnoreCase(authMode)) {
    // Si el modo NO es solo clásico, habilitamos login OAuth2
    http.oauth2Login(oauth -> oauth
        .loginPage("/login"));
}
```

Con esta lógica:

- Si `app.auth.mode` vale **`classic`**, solo configuramos `formLogin` (y omitimos `oauth2Login`), de modo que el login será únicamente con usuario/clave.
- Si vale **`oauth2`**, solo configuramos `oauth2Login` (omitimos `formLogin`), habilitando únicamente el inicio con Google.
- Si vale **`both`** (o cualquier otro valor no reconocido, que por defecto consideramos ambos), se ejecutarán ambas configuraciones, quedando habilitados los dos métodos.

De esta manera, con un simple cambio en el archivo de propiedades podemos activar o desactivar el login vía Google sin tocar el código. Puedes adaptar la condición o la propiedad a tus necesidades (por ejemplo, usar perfiles de Spring (`@Profile`) para mantener dos configuraciones de seguridad separadas, aunque para la mayoría de casos el enfoque anterior es suficiente).

**Importante:** Si deshabilitas el login clásico pero tu aplicación sigue teniendo rutas que requieren autenticación, asegúrate de ofrecer al usuario alguna forma de autenticarse vía OAuth2; de lo contrario, no podrá iniciar sesión. De igual forma, si deshabilitas OAuth2 pero referencias enlaces de "Login con Google" en la página, esos enlaces no funcionarán (podrías ocultarlos condicionalmente según la propiedad).

## Alternativa 2: Habilitar login clásico y OAuth2 simultáneamente

El escenario más completo (y el principal descrito en esta guía) es permitir **ambos métodos de autenticación a la vez**. Esto ya lo logramos en la configuración del SecurityFilterChain al invocar `formLogin()` y `oauth2Login()` juntos. En este modo "mixto":

- La página de inicio de sesión (ya sea la predeterminada de Spring Security o tu propia) mostrará **las dos opciones**: el formulario tradicional y el enlace de OAuth2. Por ejemplo, la página por defecto genera un enlace con el nombre del cliente ("Google") que apunta a `/oauth2/authorization/google`.
- El usuario puede elegir ingresar sus credenciales locales **o** usar su cuenta de Google. En cualquiera de los casos, tras autenticarse correctamente, la aplicación establecerá una sesión de usuario autenticado de forma unificada. No hay conflicto en mantener ambas posibilidades; Spring Security sabe manejarlo.
- Desde el punto de vista de tu lógica de negocio, puedes tratar a los usuarios autenticados de forma transparente. Tanto si iniciaron vía formulario como vía Google, Spring Security creará un objeto `Authentication` en el contexto de seguridad. En el caso de OAuth2, será un `OAuth2AuthenticationToken` que implementa `Authentication`; en el caso clásico, normalmente un `UsernamePasswordAuthenticationToken`. Pero para fines de autorización (`@PreAuthorize`, `SecurityContextHolder`, etc.) ambos son válidos.

Si deseas **personalizar la experiencia** en la página de login combinada, considera detalles como:

- Mostrar un botón con el logo de Google y texto "Continuar con Google" para mejor UX en lugar de un simple enlace.
- Colocar el formulario y el botón de Google de forma destacada o usando modales, etc.
- Incluir un indicador de "o" separando ambas opciones (como en el ejemplo agregado en la sección anterior con "OR").

En caso de tener **múltiples proveedores OAuth2** (por ejemplo Google y GitHub), Spring Security listará cada uno en la página de login predeterminada o, si es personalizada, podrías agregar varios enlaces/botones. Spring soporta múltiples clientes simultáneamente, solo tendrías que añadir más entradas bajo `spring.security.oauth2.client.registration` (por ejemplo, otra sección `github:` con sus credenciales).

Por último, recuerda que cuando ofreces login social, es posible que quieras **sincronizar esos usuarios con tu sistema interno**. Por ejemplo, quizás quieras crear automáticamente un registro local de usuario la primera vez que alguien inicia con Google, o al menos guardar su email en tu base de datos. En la sección siguiente comentamos brevemente cómo manejar eso.

## Nota: Sincronización con la base de datos de usuarios local (opcional)

Tener dos métodos de autenticación no implica necesariamente tener dos almacenes de usuarios separados. Muchas aplicaciones combinan _OAuth2 login_ con usuarios locales guardando cierta información en la base de datos interna. Por ejemplo, puedes querer distinguir qué usuarios se registraron localmente versus con Google, o poblar más datos de perfil.

Si tu aplicación ya tiene una entidad `User` en la base de datos (para el login clásico), puedes adaptarla para registrar logins de Google. Una práctica común es añadir un campo a la entidad de usuario que indique el **proveedor** o tipo de autenticación (por ejemplo, un campo enum con valores `LOCAL` o `GOOGLE`). También podrías usar el email del usuario de Google como identificador para enlazarlo con tu tabla de usuarios.

Spring Security nos permite **extender el proceso de OAuth2 login** para manejar estos casos. En particular, puedes implementar un **OAuth2UserService** personalizado que, tras la autenticación con Google, se encargue de buscar o crear el usuario en tu base de datos local. La idea general sería:

1. **Extender** la clase `DefaultOAuth2UserService` y sobreescribir el método `loadUser(...)` para interceptar los datos del usuario de Google una vez autenticado.
2. Dentro de `loadUser`, utilizar los datos obtenidos (por ejemplo, email) para buscar en tu repositorio local de usuarios:

   - Si existe un usuario con ese email/proveedor, podrías actualizar detalles o simplemente continuar.
   - Si no existe, crear un nuevo usuario local con esos datos (asignándole quizás un password aleatorio o un flag de que es externo) y marcar su proveedor como Google.

3. Devolver un `OAuth2User` (puede ser un objeto custom que envuelve al user de Spring, si necesitas añadir info adicional).

Spring Security luego usará tu `OAuth2UserService` personalizado durante la autenticación OAuth2. Ten en cuenta que puedes apoyarte en la implementación por defecto (delegar en `super.loadUser(...)` para obtener el `OAuth2User` de Google y luego adaptarlo). La propia documentación de Spring sugiere este enfoque de combinar la consulta al **proveedor externo** y a tu **BBDD local** en un servicio personalizado.

Integrar el OAuth2UserService a la configuración es tan fácil como registrarlo en el builder: por ejemplo `http.oauth2Login().userInfoEndpoint().userService(customOAuth2UserService)`. En nuestro código de ejemplo arriba aparece comentada la línea donde se podría inyectar (`userService(oauthUserService)`). Al hacerlo, Spring llamaría a tu lógica personalizada después de la autenticación con Google.

En resumen, con un **poco de código extra** puedes lograr que al iniciar sesión con Google, tu aplicación:

- _registre_ al usuario en tu sistema (la primera vez) o
- _lo enlace_ con un usuario existente (por ejemplo, si coincide el correo electrónico),

y así mantener una única base de datos de usuarios ya sea que hayan usado login tradicional u OAuth. Este tema es avanzado y fuera del alcance principal de esta guía, pero vale la pena mencionarlo para completar la referencia.

## Conclusión

Integrar OAuth2 en una aplicación Spring Boot con Spring Security es relativamente sencillo gracias al soporte nativo que brinda el framework. Recapitulando lo que hicimos:

- Registramos la aplicación en un proveedor OAuth2 (Google) para obtener credenciales.
- Configuramos las propiedades de cliente OAuth2 en Spring Boot (Client ID, secreto, scopes).
- Ajustamos la configuración de Spring Security para habilitar el login clásico y el login con Google en paralelo.
- Vimos cómo alternar entre modos de autenticación mediante propiedades, y cómo mostrar ambas opciones a la vez al usuario.
- Mencionamos cómo se podría sincronizar usuarios de Google en la base de datos local si es necesario.

Con esta configuración, tu aplicación permite a los usuarios iniciar sesión con sus credenciales tradicionales **o** autenticarse mediante Google, aprovechando lo mejor de ambos mundos. Spring Security se encarga de gran parte del trabajo pesado: redirigir al proveedor, manejar tokens, sesiones, etc., facilitando la implementación de **social login** de forma segura. Esperamos que esta guía te sirva como referencia completa para integrar OAuth2 en tu proyecto Spring Boot de forma exitosa. ¡Manos a la obra y feliz codificación!

---

---

# Guía para integrar OAuth2 en aplicación React (frontend) + Spring Boot (backend) con JWT

En esta guía completa explicaremos cómo agregar inicio de sesión con OAuth2 (proveedor genérico) a una aplicación **React + Spring Boot** que ya cuenta con autenticación clásica mediante JWT. Mantendremos el uso de **JWT** para las peticiones (emitido por el backend y almacenado en el frontend, usando Axios para enviarlo en cada solicitud). Además, mostraremos cómo conservar la gestión de **roles** de usuarios desde la base de datos en este nuevo flujo. A continuación, se detallan los pasos con fragmentos de código y explicación.

## 1. Configuración del backend con Spring Security (OAuth2 + JWT)

### 1.1 Dependencias necesarias

Asegúrate de tener en tu proyecto Spring Boot las dependencias de **Spring Security** y **OAuth2 Client**, además de la biblioteca para JWT que uses. Por ejemplo, en Maven:

```xml
<dependencies>
    <!-- Spring Security y OAuth2 Client -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-oauth2-client</artifactId>
    </dependency>
    <!-- Biblioteca JWT (ejemplo usando jjwt) -->
    <dependency>
        <groupId>io.jsonwebtoken</groupId>
        <artifactId>jjwt</artifactId>
        <version>0.9.1</version>
    </dependency>
    <!-- ... otras dependencias ... -->
</dependencies>
```

### 1.2 Configuración del proveedor OAuth2 en `application.yml`

Registra la información del proveedor OAuth2 (por ejemplo, cliente de Google, GitHub, Auth0, etc.) en el archivo de propiedades. Incluye al menos el `client-id`, `client-secret` y scopes necesarios. Por ejemplo, si fuera Google OAuth2 (como caso genérico):

```yaml
spring:
  security:
    oauth2:
      client:
        registration:
          google:
            client-id: TU_CLIENT_ID
            client-secret: TU_CLIENT_SECRET
            scope: [email, profile]
            redirect-uri: "{baseUrl}/login/oauth2/code/{registrationId}"
        provider:
          google:
            authorization-uri: https://accounts.google.com/o/oauth2/auth
            token-uri: https://oauth2.googleapis.com/token
            user-info-uri: https://www.googleapis.com/oauth2/v3/userinfo
            user-name-attribute: email
```

> **Nota:** En el caso de proveedores conocidos (Google, GitHub, etc.), Spring Boot a veces configura automáticamente las URLs de autorización, token y user info. Solo asegúrate de que la URI de **redirect** en tu configuración coincida con la autorizada en el proveedor (normalmente `/login/oauth2/code/{registrationId}` en el backend).

En este ejemplo usamos `"google"` como proveedor; para otro proveedor genérico cambiarías estos valores según la documentación de su API OAuth2. Lo importante es registrar correctamente el cliente OAuth2.

### 1.3 Configuración de Spring Security (permitiendo OAuth2 y JWT)

A continuación, define la configuración de seguridad. Dado que estás en Spring Boot **última versión** (Spring Security 6), usaremos la forma actual basada en beans `SecurityFilterChain` en lugar de la obsoleta `WebSecurityConfigurerAdapter`. Necesitamos:

- **Deshabilitar** CSRF (ya que utilizamos JWT) y posiblemente CORS según tu caso.
- Permitir el acceso sin autenticar a las rutas relacionadas con OAuth2 (por ejemplo, la URL para iniciar el login externo y la callback).
- Configurar `oauth2Login` para que use un **SuccessHandler** personalizado (veremos en la siguiente sección) que genere el JWT tras login exitoso.
- Registrar el filtro JWT existente para que siga protegiendo las rutas de tu API (autenticación por token en cada request).

Por ejemplo, tu clase de configuración podría quedar así:

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Autowired
    private JwtAuthenticationFilter jwtFilter; // tu filtro JWT existente
    @Autowired
    private OAuth2LoginSuccessHandler oAuth2LoginSuccessHandler; // (Definido más abajo)
    @Autowired
    private OAuth2LoginFailureHandler oAuth2LoginFailureHandler; // (Opcional: manejador de errores OAuth2)

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .cors().and().csrf().disable()
            .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)  // stateless, usamos JWT
            .and().authorizeHttpRequests(auth -> auth
                .requestMatchers("/", "/login", "/oauth2/**").permitAll()  // permitir URLs de login y OAuth2 sin auth
                .anyRequest().authenticated()  // proteger el resto
            )
            .oauth2Login(oauth2 -> oauth2
                // Podemos personalizar endpoints si se desea; por defecto /oauth2/authorization/{registratIonId} inicia login
                .authorizationEndpoint(cfg -> cfg.baseUri("/oauth2/authorize"))
                .redirectionEndpoint(cfg -> cfg.baseUri("/login/oauth2/code/*"))
                .userInfoEndpoint(cfg -> cfg.userService(customOAuth2UserService())) // opcional, para mapear usuarios
                .successHandler(oAuth2LoginSuccessHandler)   // nuestro manejador de éxito
                .failureHandler(oAuth2LoginFailureHandler)   // manejador de error (opcional)
            );

        // Añadir filtro JWT antes del filtro de autenticación por formulario
        http.addFilterBefore(jwtFilter, UsernamePasswordAuthenticationFilter.class);

        return http.build();
    }

    // ... (bean customOAuth2UserService si se usa, ver sección 1.5)
}
```

En lo anterior, hemos indicado que `/oauth2/**` (que incluye las URLs de **autorización** y **callback** de OAuth2) no requieren autenticación previa (se permite el acceso público). También registramos el **SuccessHandler** para el login OAuth2, y mantenemos el filtro JWT existente en la cadena de filtros. La sesión se marca **STATELESS** para seguir usando JWT en cada petición (no se guardará sesión de usuario en el servidor).

### 1.4 Manejador de éxito OAuth2 (generación del JWT)

El componente clave es el **AuthenticationSuccessHandler** personalizado que se ejecutará cuando el login OAuth2 sea exitoso. Su responsabilidad es generar un **JWT** para nuestro sistema y retornarlo al frontend.

Implementemos `OAuth2LoginSuccessHandler` (como `AuthenticationSuccessHandler`). En `onAuthenticationSuccess`, podemos realizar lo siguiente:

1. **Obtener datos del usuario autenticado** (principal de tipo `OAuth2User` u `OidcUser` según el proveedor).
2. **Mapear o registrar el usuario** en nuestra base de datos interna, obteniendo sus roles:

   - Por ejemplo, usar el email u otra clave única del perfil OAuth2 para buscar un usuario existente en la BD.
   - Si no existe, crear uno nuevo con datos básicos (y asignarle un rol por defecto, p. ej. ROLE_USER).
   - Si ya existe, actualizar info si lo deseas (nombre, avatar, etc.) y obtener sus roles actuales.

3. **Generar un JWT** utilizando tu servicio utilitario (el mismo que usas en login clásico) con la identidad del usuario y sus roles embebidos.
4. **Enviar el JWT al frontend**. Lo común es realizar una redirección a una URL del frontend e incluir el token como parámetro de consulta (por ejemplo `?token=...`). Así el navegador, tras autenticarse con el proveedor, será redirigido a tu aplicación React con el token listo para usar.

A continuación un ejemplo simplificado de este manejador de éxito:

```java
@Component
public class OAuth2LoginSuccessHandler implements AuthenticationSuccessHandler {

    @Autowired
    private UserRepository userRepository;       // Repo de usuarios en BD
    @Autowired
    private JwtTokenProvider tokenProvider;      // Servicio para generar JWT (propio de tu app)
    private String frontEndRedirectUri = "http://localhost:3000/oauth2-redirect";  // URL del front para recibir el token

    @Override
    public void onAuthenticationSuccess(HttpServletRequest request, HttpServletResponse response,
                                        Authentication authentication) throws IOException {
        // 1. Obtener detalles del usuario de OAuth2
        OAuth2User oAuth2User = (OAuth2User) authentication.getPrincipal();
        String email = oAuth2User.getAttribute("email");          // atributo de email (depende del proveedor)
        String provider = "";
        if (authentication instanceof OAuth2AuthenticationToken authToken) {
            provider = authToken.getAuthorizedClientRegistrationId();
        }

        // 2. Buscar o registrar usuario en BD local
        User user = userRepository.findByEmail(email).orElseGet(() -> {
            // Si no existe, registrar nuevo
            User newUser = new User();
            newUser.setEmail(email);
            newUser.setProvider(provider);
            newUser.setNombre(oAuth2User.getAttribute("name"));   // ejemplo de obtener nombre
            newUser.setRoles(Collections.singletonList("ROLE_USER"));  // rol por defecto
            return userRepository.save(newUser);
        });
        // (Si ya existía, podríamos actualizar algún campo si es necesario)

        // 3. Generar JWT con la info del usuario (incluyendo roles)
        String tokenJWT = tokenProvider.generarToken(user);  // este método debe construir el JWT con claims de username/roles

        // 4. Redirigir al frontend con el token como parámetro
        String redirectUrl = frontEndRedirectUri + "?token=" + tokenJWT;
        response.sendRedirect(redirectUrl);
    }
}
```

En este código, luego de autenticar al usuario vía OAuth2, localizamos su registro interno (por email). Si no existe, se crea uno nuevo asignándole un rol predeterminado. Luego generamos un JWT para nuestra aplicación (usando posiblemente una secret key propia y las claims deseadas, igual que en tu flujo de login clásico). Finalmente, construimos una URL de retorno al frontend incluyendo el token como query param, por ejemplo `http://localhost:3000/oauth2-redirect?token=eyJhbGciOi...`. Hacemos un `response.sendRedirect` a dicha URL.

> **Importante:** Para seguridad, conviene validar que el `redirectUri` de destino es permitido (evitando redirecciones abiertas). En el ejemplo, usamos una constante; una implementación más robusta podría recoger de una cookie segura o configuración las URLs frontend válidas.

Con este paso, el flujo backend está completo: el usuario se autentica con el proveedor externo, Spring Security captura el **code** de OAuth2, tu success handler crea el JWT y redirige al front con dicho token.

### 1.5 Mapeo de roles de usuario en OAuth2

Ya que tu aplicación maneja **roles** desde la base de datos (y los incluye en el JWT), es fundamental conservar eso en el flujo OAuth2. En el código de arriba, después de obtener/crear el `User` desde la BD, obtuvimos sus roles y los usamos en la generación del JWT. Asegúrate de que `tokenProvider.generarToken(user)` incluya los roles (por ejemplo, como claim `roles` o en el payload) tal como lo hace en el login clásico.

Opcionalmente, podrías integrar la asignación de roles en el propio proceso de autenticación OAuth2. Una forma es implementando un `OAuth2UserService` personalizado que, al **cargar el usuario OAuth2**, también cargue los roles desde tu BD y los agregue a las autoridades **antes** de que Spring Security considere la autenticación completa. Por simplicidad, muchos proyectos realizan la asociación de usuario local y roles en el **SuccessHandler**, como hemos hecho. Esto es válido ya que seguimos trabajando en un contexto autenticado y podemos realizar operaciones adicionales (guardar usuario, etc.).

En resumen, el backend ahora soporta dos formas de login:

- **Login clásico** con usuario/contraseña -> genera JWT (como ya tenías).
- **Login social/OAuth2** -> genera JWT a través del flujo descrito.

Ambos emiten un JWT válido para nuestras APIs protegidas, y el filtro JWT (registrado en la configuración) seguirá verificando dicho token en cada petición entrante.

## 2. Integración del frontend React para OAuth2

Desde el lado del frontend (React), necesitamos ajustar el flujo de autenticación para incluir la opción de OAuth2:

### 2.1 Enlace/botón de "Login con proveedor"

En tu componente de login de React, agrega un botón o enlace que redirija al usuario al **endpoint de autorización OAuth2** expuesto por tu backend. Por ejemplo, si definiste un proveedor con id `"google"`, la URL suele ser `http://TU_BACKEND/oauth2/authorization/google` (Spring Boot expone por defecto `/oauth2/authorization/{registrationId}` para iniciar el flujo).

Ejemplo en React (JSX):

```jsx
<button
  onClick={() =>
    (window.location.href = `${process.env.REACT_APP_API_URL}/oauth2/authorization/google`)
  }
>
  Iniciar sesión con Google
</button>
```

_(Aquí `REACT_APP_API_URL` sería la base URL de tu API backend)_.

En el ejemplo anterior, al hacer click se redirige el navegador a tu backend, que a su vez iniciará el proceso OAuth2 (redirigiendo al proveedor). También podrías usar un `<a>` con `href` directamente. La clave es que **no** usarás Axios para esta petición, ya que el login OAuth2 no es una llamada AJAX sino una navegación/redirect del navegador completo (involucra salirse a la página del proveedor).

> En tu caso mencionas que usas Axios para las peticiones autenticadas con JWT, lo cual está perfecto para las APIs. Sin embargo, para el **inicio de sesión con OAuth2** necesitaremos hacer un redirect normal (el flujo no ocurre via XHR). Tras completar el login, volveremos a la aplicación y allí retomaremos el uso de Axios con el nuevo token.

### 2.2 Recepción del JWT en el navegador tras el login OAuth2

Si el backend redirige correctamente con el token JWT en la URL (como configuramos en el success handler), el usuario terminará en una URL de tu frontend, por ejemplo:

```
http://localhost:3000/oauth2-redirect?token=<JWT_AQUI>
```

Debes crear un componente o página (digamos `Oauth2RedirectHandler`) que maneje esta URL de retorno. Este componente hará lo siguiente al montarse:

- Leer el parámetro `token` de la URL.
- Almacenar el JWT en donde normalmente guardas el token (por ejemplo en `localStorage` o en un estado global).
- Opcionalmente, configurar Axios para usar ese token en adelante (p.ej., establecer header Authorization por defecto).
- Redirigir al usuario a la pantalla principal o dashboard de la aplicación ya autenticado.

Ejemplo de componente React usando hooks (asumiendo React Router v6 y que usas localStorage para simplicidad):

```jsx
import { useEffect } from "react";
import { useNavigate } from "react-router-dom";
import axios from "axios";

function OAuth2RedirectHandler() {
  const navigate = useNavigate();

  useEffect(() => {
    // Obtener token de la URL
    const queryParams = new URLSearchParams(window.location.search);
    const token = queryParams.get("token");
    if (token) {
      // Guardar el token en almacenamiento local
      localStorage.setItem("authToken", token);
      // Opcional: configurar header por defecto de Axios
      axios.defaults.headers.common["Authorization"] = `Bearer ${token}`;
      // Redirigir al home/dashboard ya logueado
      navigate("/dashboard");
    } else {
      // Si no viene token, algo falló: redirigir al login o mostrar error
      navigate("/login?error=oauth");
    }
  }, [navigate]);

  return <p>Procesando inicio de sesión...</p>;
}

export default OAuth2RedirectHandler;
```

En este fragmento, en cuanto React carga la página de redirección, extrae el token JWT de la URL. Luego lo almacena (aquí usamos `localStorage`, pero podrías usar Context, Redux, etc. según tu arquitectura) y configura Axios globalmente para incluirlo en adelante en las cabeceras (así cada llamada autenticada lleva `Authorization: Bearer <token>` automáticamente). Finalmente, navega al componente principal (por ejemplo `/dashboard`). Desde este punto, la app considera al usuario autenticado, igual que si hubiera hecho login clásico.

**Consideraciones de seguridad:** almacenar el JWT en `localStorage` funciona pero tiene implicaciones (por ejemplo, el token puede ser leído por JavaScript en caso de XSS). Una alternativa es que el backend establezca el JWT en una **cookie HttpOnly**. En esta guía seguimos el enfoque de token en cliente por similitud con tu login existente. Asegúrate de las medidas de seguridad habituales: el JWT debería tener expiración y usarse sobre **HTTPS** en producción.

### 2.3 Uso del JWT en las siguientes peticiones (Axios)

Dado que ya tenías implementado el uso de JWT en Axios, probablemente cuentas con un interceptor o agregas el header manualmente en cada request. Con el token almacenado (sea en localStorage o cookie), las peticiones de Axios a tus endpoints protegidos deben incluirlo.

Por ejemplo, si guardaste en localStorage, un interceptor global podría ser:

```jsx
axios.interceptors.request.use((config) => {
  const token = localStorage.getItem("authToken");
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});
```

De esta forma, tu filtro JWT en Spring Security seguirá reconociendo al usuario en cada request entrante gracias al token en el header. La validación y autorización de roles ocurre igual que antes (el backend extraerá del JWT la identidad y roles para decidir el acceso).

## 3. Resumen del flujo OAuth2 integrado

- **Frontend:** El usuario hace click en "Login con <Proveedor>" → el navegador es redirigido a `backend/oauth2/authorization/{proveedor}`.
- **Backend:** Spring Security inicia el **authorization code flow** con el proveedor OAuth2 → el usuario se autentica en la página del proveedor y autoriza.
- **Proveedor OAuth2:** Redirige de vuelta a `backend/login/oauth2/code/{proveedor}` con un código de autorización.
- **Backend:** Spring Security intercambia el código por un **access token** (internamente) y obtiene los datos del perfil del usuario. Luego invoca nuestro **SuccessHandler**:

  - Se busca/crea el usuario en la BD interna y se obtienen sus roles.
  - Se genera un JWT propio (incluyendo dichos roles).
  - Se redirige al frontend con el JWT adjunto en la URL de retorno.

- **Frontend:** Captura el `token` de la URL, lo almacena (ej. localStorage) y lo usa para futuras llamadas API con Axios. Desde aquí, el flujo es idéntico a estar autenticado por login tradicional (Axios envía el JWT en cada petición).

Con esta integración, tu aplicación mantiene el esquema **JWT + roles** que ya tenía, pero ofreciendo también login mediante un proveedor externo. Hemos abstraído el proveedor como "genérico"; en la práctica podrías configurarlo para Google, Facebook, GitHub u otro IdP siguiendo el mismo patrón. Recuerda ajustar detalles específicos (URLs de autorización, atributos como email/nombre según el proveedor, etc.).

## DUDAS:

### Cuál es la diferencia en las properties de springboot entre el registration y el provider?

En Spring Boot, bajo `spring.security.oauth2.client`, existen dos secciones principales:

1. **`registration`**
   Define **tu aplicación cliente** frente al Authorization Server. Aquí configuras **qué** va a pedir tu aplicación y **cómo**:

   - **`client-id` / `client-secret`**: credenciales que obtienes al registrar tu app en el proveedor.

   - **`authorization-grant-type`**: flujo OAuth (por ejemplo `authorization_code`, `client_credentials`).

   - **`redirect-uri`**: dónde recibes la respuesta (código o token).

   - **`scope`**: permisos o ámbitos que solicitas (`openid`, `profile`, `email`, `api.read`, …).

   - **`client-name`**: nombre legible para logs o IU interna.

   > Básicamente, aquí defines **quién eres** (tu ID/secret) y **qué quieres** (scopes y flujo).

2. **`provider`**
   Describe **el Authorization Server** al que apuntas. Aquí indicas **dónde** están los endpoints y cómo resolver la información:

   - **`authorization-uri`**: URL para iniciar el flujo y obtener el código de autorización.

   - **`token-uri`**: URL para intercambiar el código por un access token.

   - **`user-info-uri`**: (opcional) URL para recuperar datos del usuario autenticado.

   - **`jwk-set-uri`**: (opcional) URL para descargar la clave pública y validar JWTs.

   - **`user-name-attribute`**: parámetro JSON que identifica al usuario (por ejemplo `sub`, `id`).

   > Aquí defines **dónde hablar** con el servicio de autorización y **cómo** extraer/validar los tokens y la información de usuario.

---

#### ¿Cómo se vinculan?

- El **`registrationId`** (por ejemplo `"google"` o `"my-app"`) que usas en `registration` debe coincidir con el **nombre** bajo `provider`.
- Spring Security lee primero tu `registration` (qué client-id/secret y scopes usar) y luego consulta el bloque `provider` con el mismo ID para saber **a qué URLs** llamar.

```yaml
spring:
  security:
    oauth2:
      client:
        registration:
          google: # <-- registrationId = "google"
            client-id: ...
            client-secret: ...
            authorization-grant-type: authorization_code
            redirect-uri: "{baseUrl}/login/oauth2/code/{registrationId}"
            scope:
              - openid
              - profile
              - email

        provider:
          google: # <-- providerId = "google"
            authorization-uri: https://accounts.google.com/o/oauth2/v2/auth
            token-uri: https://oauth2.googleapis.com/token
            user-info-uri: https://openidconnect.googleapis.com/v1/userinfo
            user-name-attribute: sub
            jwk-set-uri: https://www.googleapis.com/oauth2/v3/certs
```

- **Sin `provider`**, Spring usará valores por defecto **solo** para proveedores preconfigurados (Google, GitHub, Okta, Azure, …).
- **Con un proveedor custom**, debes definir tú mismo todos los URIs en `provider`.

---

**En resumen:**

- **`registration`** = la configuración de **tu cliente** (credenciales, scopes, flujo, redirect).
- **`provider`** = la configuración del **servidor de autorización** (endpoints, formato de usuario, claves).
