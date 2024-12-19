Springboot es un framework de la plataforma Spring para Java

Spring Framework es conocido para desarrollar aplicaciones java, pero era mucho lío XML, y para simplificar esto nació SpringBoot

Spring es un ecosistema de proyectos que hace que programar en Java sea más rápido, fácil y seguro para todos. Springboot es un proyecto de spring, como otros como spring data, spring cloud, etc.

## DIFERENCIAS TRABAJAR CON Y SIN SPRINGBOOT

Con Sping Boot ganamos un montor de ventajas como no tener muchas configuraciones XML, uso de dependencias iniciales que facilitan en gran medida la administración de dependencias, aplicaciones independientes con servidor incorporado y que el tiempo de configuración de los proyectos es super rápido



## Spring vs Spring Boot: Conclusión
• Spring es un marco enorme que consta de muchos proyectos.
• Spring Boot es un proyecto de Spring
• Spring Boot proporciona un conjunto de bibliotecas que simplifica enormemente el desarrollo de
aplicaciones basadas en Spring.
• Puede crear aplicaciones basadas en Spring sin o con Spring Boot
• Usar Spring Boot es más fácil y productivo
• Spring Boot es la forma estándar de desarrollar aplicaciones basadas en Sprint

## STARTER
Un starter sería un mecanismo para simplificar el desarrollo de aplicaciones, es un mecanismo que ya viene con varias dependencias preconfiguradas.

Cada starter está destinada a distintos objetivos, y por ejemplo yo si quiero agregar seguridad a mi programa, añado el starter de seguridad y ya se añaden dependencias para ese starter

Es como importar un paquete con varias dependencias, nosotros importamos un starter y incluye varias dependencias ya configuradas, un proyecto springboot tiene varios starter.

Algunos starters:

• spring-boot-starter-parent: define los valores predeterminados comunes, como la versión de Java (1.8), la codificación de fuente UTF-8, varias configuraciones de complementos de Maven para el
desarrollo del marco Java Spring .

• hereda la gestión de dependencias de spring-boot-dependencies que define la información de dependencia para Spring Boot, Spring framework y bibliotecas comunes.

• La dependencia spring-boot-starter-web es un Spring Boot Starter que especifica un grupo de dependencias para desarrollar aplicaciones web Java como Spring Web MVC y el servidor Tomcat.

• spring-boot-maven-plugin permite la creación automática de un archivo JAR ejecutable para una aplicación web Java independiente con un servidor Tomcat incorporado, gracias a su compatibilidad con
archivos jar anidados.

## Anotación en SpringBoot:

Las anotaciones en springboot son como una forma para asignarle un comportamiento especial a cada método. Por ejemplo estamos en clase y a un compañero se le asigna la anotación de delegado, ahí estamos asignando un comportamiento.

Vienen siendo metadatos que nos permite asignar comportamientos a unas ciertas clases o datos que tengamos

Por ejemplo la clase Main va a ser @SpringBootApplication y esta notación sirve para indicar que esta es la clase principal y vas a poder compilarlo.

Básicamente springboot funciona con notaciones, es la principal característica.

@EnableAutoConfiguration Intenta configurar springboot de una forma automática (busca en el classpath todas las clases con la anotación @Entity)

@ComponentScan Se encarga de buscar el resto de anotaciones, por ejemplo Repository, Service, Controller, etc

@SpringConfiguration indica que la clase anotada es una fuente de definiciones de Beans (que una clase tiene cierta cantidad de Beans)
## Creación proyecto spring:
Se inicializa desde: [Inicializar proyecto spring-boot](https://start.spring.io)

## BEANS:

Spring va a estar formada por varios Beans, y eso van a estar por toda la aplicación, van a ser ciertas funcionalidades que se van a usar en toda la aplicación como una única instancia.
### Definición Beans:
- ¿Qué es un Bean? En Spring Boot, un bean es un objeto gestionado por el contenedor de Spring. Spring se encarga de crear, inicializar, configurar y destruir estos objetos según sea necesario, facilitando la inyección de dependencias y la gestión automática de configuraciones en la aplicación. Los beans son componentes básicos en Spring, y permiten construir aplicaciones de manera modular y eficiente.

- Declaración de Beans: Para que Spring gestione una clase como un bean, es necesario registrarla en el contenedor. Esto se hace con anotaciones como @Component, @Service, @Repository, @Controller, o definiéndola en una clase de configuración con @Bean. Esto le dice a Spring que debe crear y mantener una instancia de esa clase en su contenedor.

- Inyección con @Autowired: Una vez que una clase está registrada como bean, puede inyectarse en otras clases usando @Autowired. Esto permite que Spring proporcione automáticamente la instancia del bean donde sea necesario, sin necesidad de instanciarla manualmente.

- Relación entre Bean y @Autowired: Usar solo @Autowired no es suficiente para que Spring cree la instancia de una clase. Primero, la clase debe estar registrada como bean. Una vez que está registrada, Spring se encarga de gestionar e inyectar la instancia del bean en cualquier lugar donde se use @Autowired, garantizando la disponibilidad de la instancia en toda la aplicación.

En resumen, los beans en Spring permiten una gestión centralizada y automática de los objetos, facilitando la modularidad y la eficiencia de la aplicación.

## Carpeta recursos:
Cuando autogeneramos un proyecto con springboot tenemos distintas carpetas, vamos a tener la carpeta de resources que va a tener varios elementos:
- Carpeta static: En esta carpeta guardamos elementos estáticos como imágenes,etc.
- En template irían los archivos HTML
- application.properties: Es un archivo de propiedades, por ejemplo el puerto, info base de datos, etc.


## Diferencia RestController y Controller a secas:

En Spring Framework, las anotaciones `@Controller` y `@RestController` se utilizan para definir controladores, pero tienen una diferencia fundamental en el tipo de respuestas que producen:

### 1. `@Controller`

- **Descripción**: La anotación `@Controller` se usa para definir un controlador en Spring MVC. Indica que la clase es un controlador que maneja solicitudes web.
- **Tipo de respuesta**: Por defecto, los métodos de un controlador anotado con `@Controller` devuelven vistas (archivos HTML, JSP, etc.), y la respuesta se pasa a través de un motor de plantillas.
- **Uso de `@ResponseBody`**: Si deseas que el método de un controlador con `@Controller` devuelva datos en formato JSON o XML (sin procesar una vista), puedes agregar la anotación `@ResponseBody` sobre cada método específico, lo que indica que el cuerpo de la respuesta debe serializarse directamente en JSON o XML.

**Ejemplo**:
```java
@Controller
public class HomeController {
    
    @GetMapping("/home")
    public String home() {
        return "home"; // Retorna el nombre de una vista, no JSON.
    }
    
    @GetMapping("/data")
    @ResponseBody // Esto indica que queremos JSON en lugar de una vista.
    public Map<String, String> getData() {
        Map<String, String> data = new HashMap<>();
        data.put("message", "Hello World");
        return data;
    }
}
```

### 2. `@RestController`

- **Descripción**: La anotación `@RestController` es una combinación de `@Controller` y `@ResponseBody`.
- **Tipo de respuesta**: Todos los métodos en una clase con `@RestController` devuelven datos directamente en formato JSON o XML (en lugar de una vista), ya que se asume automáticamente que cada respuesta debe serializarse.
- **Uso simplificado**: No es necesario especificar `@ResponseBody` en cada método, ya que se aplica de forma implícita a todos ellos.

**Ejemplo**:
```java
@RestController
public class ApiController {
    
    @GetMapping("/api/data")
    public Map<String, String> getData() {
        Map<String, String> data = new HashMap<>();
        data.put("message", "Hello World");
        return data; // Automáticamente en JSON.
    }
}
```

### Resumen de diferencias

| Característica                | `@Controller`                      | `@RestController`                 |
|-------------------------------|------------------------------------|-----------------------------------|
| Tipo de respuesta por defecto | Vista (HTML, JSP)                 | JSON o XML                        |
| Serialización automática      | No, requiere `@ResponseBody`      | Sí, todo se convierte a JSON/XML  |
| Uso común                     | Aplicaciones web tradicionales     | APIs REST                         |

### ¿Cuándo usar cada uno?
- Usa `@Controller` si tu aplicación requiere una interfaz de usuario y necesita generar vistas como HTML o JSP.
- Usa `@RestController` cuando desarrolles APIs RESTful y quieras que tus métodos devuelvan datos en formato JSON o XML de manera predeterminada.


Cuando tienes un Controller indica que tiene que devolver un HTML.

## SPRING MVC:
Spring MVC es un framework de Java que ayuda a construir aplicaciones web usando el patrón Modelo-Vista-Controlador (MVC), que separa la lógica de negocio, la interfaz de usuario, y el flujo de la aplicación.

- Modelo: maneja los datos y la lógica de negocio.
- Vista: muestra los datos al usuario (generalmente en HTML).
- Controlador: recibe las solicitudes del usuario, interactúa con el modelo y elige la vista adecuada.

¿Cómo funciona?
- El usuario hace una solicitud (por ejemplo, abre una página web).
- Spring MVC dirige esta solicitud a un controlador específico.
- El controlador obtiene los datos del modelo, los pasa a la vista y envía la respuesta al usuario.

En resumen, Spring MVC organiza el código de una aplicación web para que sea fácil de mantener y escalar.

Principales características de Spring MVC
- Inyección de dependencias: Usa el core de Spring para manejar dependencias.
- Configuración flexible: Aunque admite XML, se puede configurar casi todo mediante anotaciones.
- Soporte de RESTful: Spring MVC facilita la creación de servicios RESTful.
- Motor de plantillas flexible: Compatible con múltiples motores de plantillas para la capa de presentación.
- Internacionalización y validación: Proporciona mecanismos para gestionar la internacionalización y validar datos de entrada.


### CAPAS SPRING MVC:
Tenemos la capa de presentación, que es la capa de 
@Controller, la capa de negocio, que es la capa de @Service
y la capa de acceso a Datos que es el @Repository


## ESTEREOTIPOS:
Los estereotipos son componentes que extienden de la clase @Component. Spring define un conjunto de anotaciones core que categorizan cada uno de los componentes asociandoles una responsabilidad concreta.

Todo lo que tenga @Component te va a permitir usar esa clase en el contenedor de spring, sino no, luego vas a tener otras notaciones como @Service, etc, pero esas son para categorizar, pero si solo usas @Component ya lo vas a poder usar en el contenedor de Spring

Un Bean es un componente o un elemento de Spring, una instancia de un componente

@Repository es el estereotipo que se encarga de dar de alta un bean para que implemente el patrón repositorio. Es el encargado de almacenar datosen una base de datos o repositorio de información que se necesite. Al marcar el ben con esa anotación Spring aporta servicios transversales como conversión de tipos de expceciones de JDBC A JPA, etc.

@Service se encarga de gestionar las operaciones de negocio más inmportantes a nivel de la aplicación y aglutina llamadas a varios repositorios de forma simultánea. Su tarea fundamental es la de agregador implementa el patrón fachada y aglutina varios repositorios y llamadas a otros servicios.

@Controller es el que realiza las tareas de controlador y gestión de la comunicación entre el usuario y el aplicativo. Para ello se apoya habitualmente en algún motor de plantillas o librería de etiquetas que facilitan la creación de páginas.

## Métodos HTTP:
En Spring tenemos las notaciones para los métodos HTTP, tenemos @RequestMapping(donde tenemos que especificar luego la operación), @GetMapping, @PostMapping, @PutMapping, @DeleteMapping

## INYECCION DE DEPENDENCIAS:
La **inyección de dependencias** en Spring Boot es una técnica de diseño de software que permite gestionar las dependencias entre objetos de manera automática y controlada. En lugar de que un objeto cree o gestione directamente sus dependencias, el framework de Spring las proporciona cuando el objeto las necesita. Esto ayuda a reducir el acoplamiento y facilita el mantenimiento, las pruebas, y la reutilización del código.

### Conceptos clave:

1. **Inversión de Control (IoC)**: En lugar de que una clase sea responsable de instanciar sus dependencias, Spring Boot toma el control de esta tarea. Esto se conoce como Inversión de Control, ya que el flujo de la dependencia se invierte y se maneja externamente.
  
2. **Contenedor de Spring**: Spring Boot utiliza un contenedor IoC que crea y administra los objetos, llamados *beans*, a lo largo del ciclo de vida de la aplicación. Cuando se necesita un bean, el contenedor se encarga de proporcionarlo.

3. **Anotaciones comunes**:
   - `@Autowired`: Se usa para marcar una dependencia en una clase. Spring Boot busca automáticamente un bean compatible en el contenedor y lo inyecta en la clase.
   - `@Component`, `@Service`, `@Repository`, `@Controller`: Estas anotaciones indican que una clase debe ser gestionada como un bean de Spring, y el contenedor la detectará automáticamente.

### Tipos de inyección:

- **Inyección por constructor**: La inyección se realiza a través del constructor de la clase. Es el método recomendado ya que garantiza que el objeto esté completamente inicializado al crearse.
  
- **Inyección por setter**: Usa métodos *setter* para inyectar dependencias. Este método se usa menos, pero permite la inyección de dependencias opcionales.

### Ejemplo básico:
Supongamos que tenemos una clase `CarService` que depende de `EngineService`:

```java
@Service
public class CarService {
    
    private final EngineService engineService;

    @Autowired
    public CarService(EngineService engineService) { 
        this.engineService = engineService; 
    }
    
    public void startCar() {
        engineService.startEngine();
    }
}
```

Aquí, Spring Boot se encarga de crear y gestionar la instancia de `EngineService` y la inyecta en `CarService` a través del constructor, gracias a la anotación `@Autowired`.

### Ventajas:
- **Desacoplamiento**: Las clases dependen de interfaces, no de implementaciones específicas.
- **Facilidad de pruebas**: Las dependencias se pueden cambiar o simular fácilmente, lo cual facilita las pruebas unitarias.
- **Mantenimiento y escalabilidad**: Los cambios en una dependencia no afectan directamente a otras clases, lo cual simplifica la gestión y el crecimiento del proyecto.

En resumen, la inyección de dependencias en Spring Boot permite que el contenedor IoC de Spring gestione automáticamente las dependencias entre objetos, promoviendo un diseño de código modular, mantenible y fácilmente testable.

Spring tiene la notación @Autowired, que es una de las anotaciones más habituales cuanod trabajamos con Spring Framework ya que se trata de la anotación que permite inyectar unas dependencias con otras dentro de Spring. 

Otra forma de hacerlo es con:

Por supuesto, en Spring Boot, también se puede usar `@Autowired` directamente en la declaración del atributo. En este caso, no es necesario un constructor o método *setter* para la inyección de dependencias. Spring Boot automáticamente inyectará la dependencia en el atributo marcado con `@Autowired`.

### Ejemplo básico:

Supongamos que tenemos una clase `CarService` que depende de `EngineService`. Aquí inyectaremos `EngineService` directamente en la declaración del atributo usando `@Autowired`:

```java
@Service
public class CarService {

    @Autowired
    private EngineService engineService;

    public void startCar() {
        engineService.startEngine();
    }
}
```

### Explicación:

- La anotación `@Autowired` en la declaración del atributo `engineService` indica a Spring que debe inyectar automáticamente un bean de tipo `EngineService` cuando se crea una instancia de `CarService`.
- El contenedor de Spring busca un bean de tipo `EngineService` y lo inyecta en `CarService` sin necesidad de un constructor o un método *setter*.

### Consideraciones:

Aunque esta forma es válida y sencilla, la **inyección por constructor** suele ser preferida porque:
1. Facilita el diseño de clases inmutables.
2. Permite que las dependencias se verifiquen en el momento de la creación del objeto.
3. Es más adecuada para las pruebas unitarias, ya que permite inyectar dependencias en el constructor al crear instancias en pruebas.

Aun así, la inyección en la declaración del atributo es útil en aplicaciones pequeñas y casos en los que se necesita simplicidad.

Ambas formas son válidas, aunque el primer ejemplo es más estricto.
Para que una clase se pueda instanciar y usar con `@Autowired` en Spring Boot, es necesario que la clase cumpla con algunos requisitos básicos. Aquí están los elementos que necesita:

### 1. Ser un **bean gestionado por el contenedor de Spring**
   La clase debe estar marcada con una anotación que indique al contenedor de Spring que debe gestionarla como un *bean*. Las anotaciones más comunes son:

   - `@Component`: Para clases genéricas.
   - `@Service`: Para clases que implementan lógica de negocio.
   - `@Repository`: Para clases de acceso a datos (DAO).
   - `@Controller` o `@RestController`: Para clases que actúan como controladores en aplicaciones web.

   Estas anotaciones hacen que Spring detecte automáticamente la clase y la registre en su contenedor IoC (Inversión de Control).

   ```java
   @Service
   public class EngineService {
       public void startEngine() {
           System.out.println("Engine started!");
       }
   }
   ```

### 2. **Estar dentro de un paquete escaneado por Spring Boot**
   Spring Boot escanea por defecto el paquete donde está la clase principal (aquella con `@SpringBootApplication`) y todos los subpaquetes de esta. Por lo tanto, la clase con `@Component`, `@Service`, etc., debe estar en el mismo paquete o en uno de los subpaquetes de la clase principal.

   Si la clase está en un paquete distinto, se puede usar `@ComponentScan` en la clase principal para que Spring busque beans en otros paquetes.

   ```java
   @SpringBootApplication
   @ComponentScan(basePackages = "com.example.otherpackage") // Si está en otro paquete
   public class MySpringBootApplication {
       public static void main(String[] args) {
           SpringApplication.run(MySpringBootApplication.class, args);
       }
   }
   ```

### 3. Tener un **constructor público** (o el constructor predeterminado si no hay uno definido)
   Spring necesita acceso a un constructor para instanciar la clase. Aunque en la mayoría de los casos, Spring usará el constructor sin parámetros, también puede usar un constructor con parámetros para inyección de dependencias (inyección por constructor).

   ```java
   @Service
   public class CarService {

       private final EngineService engineService;

       // Inyección de dependencia por constructor
       @Autowired
       public CarService(EngineService engineService) {
           this.engineService = engineService;
       }

       public void startCar() {
           engineService.startEngine();
       }
   }
   ```

### 4. Tener una dependencia compatible disponible en el contenedor de Spring
   La clase que deseas inyectar (por ejemplo, `EngineService`) también debe ser un bean gestionado por Spring. Esto significa que `EngineService` también debe estar anotada con `@Component`, `@Service`, o una anotación similar, para que el contenedor IoC pueda instanciarla y gestionarla.

En resumen, para que una clase se pueda inyectar con `@Autowired`, debe ser un *bean* de Spring (anotado con `@Component`, `@Service`, etc.), estar en un paquete escaneado y tener un constructor público accesible.

## PERSISTENCIA DE DATOS:

**ORM*: Un ORM es una técnica de programación utilizada en el desarrollo de aplicaciones para mapear objetos en lenguajes de programación orientados a objetos, como Java, a tablas en una base de datos relacional. En esencia permite que las aplicaciones interactúen con la base de datos utilizando objetos y métodos en lugar de escribir consultas SQL directamente. JPA es una interfaz de esto, que nos permite mapear objetos a base de datos. Una implementación de esto por ejemplo es Hibernate, que se encarga de convertir los objetos java a la base de datos.

Spring Data es un proyecto dentro del ecosistema Spring que simplifica aún más el acceso a bases de datos y la persistencia en general. No es una implementación de JPA, sino una capa de abstracción adicional que trabaja en conjunto con JPA y otras tecnologías de persistencia. Spring Data proporciona repositorios CRUD predefinidos y facilidades para definir consultas personalizadas utilizando métodos de repositorio. Además, Spring Data ofrece soporte para múltiples tecnologías de almacenamiento, como JPA, MongoDB, Redis y más.
Spring Data JPA es una subparte de Spring Data que se centra en facilitar el uso de JPA. En resumen, Spring Data es una implementación para acceder a base de datos, y que me permite trabajar con distintas tecnlogías, y Spring Data JPA es la que me permite trabajar con JPA.

Esto todo se relaciona con la capa de acceso a datos, de @Repository, es la encargada de manejar el acceso a base de datos

## CONFIGURACIONES:

Cuando creamos un proyecto lo primero es configurar la base de datos y el JPA (si queremos que cada vez cree la base de datos, la actualice, etc) y el dialecto

## REPOSITORIOS:

Los repositorios **siempre** tienen que ser interfaces, siempre, y heredan haciendo extends de otras interfaces de spring
### **¿Qué es `@Repository` en Spring?**

`@Repository` es una anotación de **Spring Framework** que marca una clase como un **componente de acceso a datos**. Indica que la clase es responsable de interactuar con la base de datos o cualquier otra fuente de datos. Es una de las tres anotaciones especializadas de estereotipos en Spring, junto con `@Controller` y `@Service`.

---

### **Características principales de `@Repository`:**
1. **Detección automática de componentes:**
   - Al usar la anotación `@Repository`, Spring puede detectar y registrar automáticamente la clase en el contexto de aplicación durante el escaneo de componentes.
   - Esto significa que puedes inyectar esta clase en otras partes de tu aplicación.

2. **Manejo de excepciones:**
   - Spring traduce automáticamente las excepciones específicas de la base de datos (como `SQLException`) a excepciones genéricas de Spring (`DataAccessException`) cuando se usa `@Repository`.

3. **Facilita el desacoplamiento:**
   - Promueve la separación de responsabilidades al concentrar el acceso a datos en un único lugar.

---

### **Cómo usar `@Repository`**

#### **1. Uso con clases personalizadas**
Puedes usar `@Repository` para marcar una clase donde implementes manualmente la lógica de acceso a datos:

```java
@Repository
public class UserRepository {

    @PersistenceContext
    private EntityManager entityManager;

    public User findUserById(Long id) {
        return entityManager.find(User.class, id);
    }
}
```

#### **2. Uso con interfaces y Spring Data JPA**
Cuando utilizas Spring Data JPA, los repositorios suelen ser **interfaces** que extienden las interfaces de Spring Data (`JpaRepository`, `CrudRepository`, etc.). No necesitas implementar nada manualmente; Spring genera automáticamente la implementación:

```java
@Repository // Opcional, ya que Spring Data la detecta automáticamente
public interface UserRepository extends JpaRepository<User, Long> {

    List<User> findByLastName(String lastName);

    @Query("SELECT u FROM User u WHERE u.email = :email")
    User findByEmail(@Param("email") String email);
}
```

#### **3. Inyección del repositorio**
Puedes inyectar el repositorio en tus servicios o controladores usando `@Autowired` o constructor injection:

```java
@Service
public class UserService {

    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public User getUserById(Long id) {
        return userRepository.findById(id).orElseThrow(() -> new RuntimeException("User not found"));
    }
}
```

---

### **Buenas prácticas**
1. **Mantén la lógica de acceso a datos en el repositorio:**
   - Evita escribir lógica de negocio en las clases marcadas con `@Repository`.
2. **Usa interfaces cuando sea posible:**
   - Aprovecha las capacidades de Spring Data JPA para reducir el código manual.
3. **Define consultas personalizadas con `@Query`:**
   - Úsalo cuando los métodos derivados no sean suficientes.
4. **Gestiona excepciones genéricas:**
   - Deja que Spring maneje las excepciones específicas del proveedor de la base de datos.

---

### **Resumen**
- **`@Repository`** marca una clase como un componente de acceso a datos.
- Puede ser usada con clases personalizadas o interfaces.
- En Spring Data JPA, no necesitas implementaciones manuales gracias a las interfaces como `JpaRepository`.
- Es fundamental para separar la lógica de acceso a datos de otras capas como servicios o controladores.
- Proporciona la mayoría de métodos que vas a necesitar para interactuar con la base de datos




## COMMANDLINERUNNER:

La clase **`CommandLineRunner`** en Spring Boot se utiliza para ejecutar lógica al inicio de la aplicación, justo después de que el contexto de Spring haya sido cargado y configurado. Es una interfaz funcional que permite ejecutar código una sola vez, útil para tareas iniciales como cargar datos, configurar propiedades o realizar pruebas rápidas.

---

### **Definición**
La interfaz `CommandLineRunner` tiene un único método:

```java
@FunctionalInterface
public interface CommandLineRunner {
    void run(String... args) throws Exception;
}
```

El método `run` recibe como parámetro un arreglo de `String` con los argumentos pasados al ejecutar la aplicación.

---

### **Uso principal**
El principal propósito de **`CommandLineRunner`** es ejecutar lógica personalizada inmediatamente después de que el contexto de la aplicación esté completamente inicializado.

#### **Ejemplo básico**
```java
import org.springframework.boot.CommandLineRunner;
import org.springframework.stereotype.Component;

@Component
public class StartupRunner implements CommandLineRunner {

    @Override
    public void run(String... args) throws Exception {
        System.out.println("La aplicación ha iniciado correctamente.");
    }
}
```

- En este caso:
  - Spring Boot detecta automáticamente la clase `StartupRunner` porque está marcada con `@Component`.
  - El método `run` se ejecutará automáticamente después de que la aplicación haya arrancado.

---

### **Usos comunes**
1. **Inicialización de datos:**
   Puedes cargar datos en la base de datos o inicializar estructuras necesarias para la ejecución de la aplicación.
   ```java
   @Component
   public class DataInitializer implements CommandLineRunner {

       private final UserRepository userRepository;

       public DataInitializer(UserRepository userRepository) {
           this.userRepository = userRepository;
       }

       @Override
       public void run(String... args) {
           userRepository.save(new User("admin", "admin@example.com"));
           System.out.println("Datos iniciales cargados.");
       }
   }
   ```

2. **Ejecución de tareas al inicio:**
   Por ejemplo, verificar la conectividad con servicios externos.
   ```java
   @Component
   public class ConnectivityChecker implements CommandLineRunner {

       @Override
       public void run(String... args) {
           System.out.println("Verificando conectividad con servicios externos...");
           // Lógica de verificación aquí
       }
   }
   ```

3. **Procesar argumentos de línea de comando:**
   Puedes trabajar directamente con los argumentos pasados al iniciar la aplicación.
   ```java
   @Component
   public class ArgsProcessor implements CommandLineRunner {

       @Override
       public void run(String... args) {
           for (String arg : args) {
               System.out.println("Argumento recibido: " + arg);
           }
       }
   }
   ```

---

### **Diferencia entre `CommandLineRunner` y `ApplicationRunner`**
Spring Boot también proporciona otra interfaz llamada **`ApplicationRunner`**, que es muy similar pero usa un enfoque más estructurado al manejar los argumentos de la aplicación:

- `CommandLineRunner`: Los argumentos se reciben como un arreglo de `String`.
- `ApplicationRunner`: Los argumentos se encapsulan en un objeto `ApplicationArguments`, lo que permite acceder a los argumentos con nombres y sin nombres.

```java
import org.springframework.boot.ApplicationRunner;
import org.springframework.stereotype.Component;

@Component
public class ApplicationStartup implements ApplicationRunner {

    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println("Argumentos sin nombres: " + args.getNonOptionArgs());
        System.out.println("Argumentos con nombres: " + args.getOptionNames());
    }
}
```

---

### **¿Cuándo usar `CommandLineRunner`?**
- **Inicialización ligera:** Para cargar datos o realizar tareas sencillas al inicio.
- **Depuración rápida:** Para probar funciones o realizar comprobaciones rápidas.
- **Tareas únicas:** Cuando necesitas ejecutar código solo una vez al arrancar la aplicación.

Es una herramienta muy útil para casos simples. Sin embargo, para lógica más compleja, considera usar inicializadores específicos o métodos como eventos de arranque de contexto (`ContextRefreshedEvent`).

## SERVICIOS:

**Una buena práctica es declarar una interfaz del servicio y luego una clase que la impemente**

### **¿Qué es `@Service` en Spring?**

`@Service` es una anotación de **Spring Framework** que marca una clase como un **componente de lógica de negocio o servicio de aplicación**. Es parte de las anotaciones de estereotipo de Spring, junto con `@Controller` y `@Repository`.

La anotación no tiene un impacto técnico distinto al de `@Component` (ambas indican que Spring debe registrar la clase en el contenedor de beans). Sin embargo, **`@Service` tiene un propósito semántico**, ya que deja claro que la clase pertenece a la capa de servicio en la arquitectura de la aplicación.

---

### **Características principales de `@Service`:**

1. **Detección automática de componentes:**
   - Al usar `@Service`, Spring detecta automáticamente la clase durante el escaneo de componentes y la registra como un bean en el contenedor.
   - Esto significa que puedes inyectar esta clase en otras partes de tu aplicación.

2. **Separación de responsabilidades:**
   - Se utiliza para centralizar la lógica de negocio, separándola de otras capas como los controladores (`@Controller`) o los repositorios (`@Repository`).

3. **Soporte para transacciones:**
   - Al combinarse con `@Transactional`, permite manejar transacciones directamente en la capa de servicio.

---

### **Cómo usar `@Service`**

#### **1. Crear un servicio básico**

```java
import org.springframework.stereotype.Service;

@Service
public class UserService {

    public String greetUser(String name) {
        return "¡Hola, " + name + "!";
    }
}
```

En este caso:
- La clase `UserService` está marcada como un servicio con `@Service`.
- Spring registrará automáticamente esta clase como un bean, permitiendo su inyección en otras partes de la aplicación.

#### **2. Inyección en otras capas**

El servicio se puede inyectar en un controlador o en otra clase utilizando **`@Autowired`** o inyección de constructor (recomendado).

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class UserController {

    private final UserService userService;

    // Inyección a través del constructor
    public UserController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping("/greet")
    public String greetUser(@RequestParam String name) {
        return userService.greetUser(name);
    }
}
```

---

### **Usos comunes de `@Service`**

1. **Centralizar la lógica de negocio:**
   - Coloca toda la lógica de negocio en servicios, no en controladores ni repositorios.

   ```java
   @Service
   public class OrderService {

       public double calculateTotal(double price, int quantity) {
           return price * quantity;
       }
   }
   ```

2. **Coordinación entre capas:**
   - Los servicios suelen coordinar llamadas entre la capa de repositorio y otras partes de la aplicación.

   ```java
   @Service
   public class ProductService {

       private final ProductRepository productRepository;

       public ProductService(ProductRepository productRepository) {
           this.productRepository = productRepository;
       }

       public Product getProductById(Long id) {
           return productRepository.findById(id)
                                   .orElseThrow(() -> new RuntimeException("Producto no encontrado"));
       }
   }
   ```

3. **Soporte para transacciones:**
   - Puedes usar `@Transactional` en los métodos de servicio para manejar transacciones de base de datos de forma automática.

   ```java
   import org.springframework.transaction.annotation.Transactional;

   @Service
   public class BankService {

       private final AccountRepository accountRepository;

       public BankService(AccountRepository accountRepository) {
           this.accountRepository = accountRepository;
       }

       @Transactional
       public void transferMoney(Long fromAccountId, Long toAccountId, double amount) {
           Account fromAccount = accountRepository.findById(fromAccountId)
                                                  .orElseThrow(() -> new RuntimeException("Cuenta no encontrada"));
           Account toAccount = accountRepository.findById(toAccountId)
                                                .orElseThrow(() -> new RuntimeException("Cuenta no encontrada"));

           fromAccount.withdraw(amount);
           toAccount.deposit(amount);

           accountRepository.save(fromAccount);
           accountRepository.save(toAccount);
       }
   }
   ```

---

### **Buenas prácticas para servicios**

1. **Evitar dependencias circulares:**
   - Asegúrate de que los servicios no dependan unos de otros en un ciclo infinito. Si necesitas resolver un caso complejo, usa eventos o servicios más generales.

2. **Mantén la lógica de negocio en los servicios:**
   - No pongas lógica de negocio en controladores ni repositorios.

3. **Divide responsabilidades:**
   - Si un servicio crece demasiado, divídelo en varios servicios más pequeños.

4. **Combina con patrones de diseño:**
   - Implementa patrones como **estrategia**, **plantilla** o **fábrica** dentro de tus servicios si es necesario.

---

### **Resumen**
- **`@Service`** indica que una clase pertenece a la capa de servicios y contiene la lógica de negocio.
- Se detecta automáticamente como un bean y puede ser inyectada en otras capas.
- Centraliza la lógica de negocio, coordina capas y maneja transacciones.
- Es fundamental para mantener la arquitectura limpia y modular en aplicaciones Spring.

## CONTROLLERS:

### Guía completa sobre `@RestController` en Spring Framework

El `@RestController` es una anotación en Spring que simplifica el desarrollo de APIs RESTful al combinar la funcionalidad de `@Controller` y `@ResponseBody`. A continuación, te explico en detalle cómo funciona, sus características, ventajas, y un ejemplo práctico para entender su uso.

---

### **1. ¿Qué es `@RestController`?**
La anotación `@RestController` se utiliza para crear controladores que devuelven datos directamente a través de HTTP, usualmente en formato JSON o XML. Está optimizada para APIs RESTful, donde el objetivo principal es enviar y recibir datos estructurados en lugar de vistas HTML.

**Características clave**:
- Combina `@Controller` y `@ResponseBody`.
- Convierte automáticamente los objetos retornados a formato JSON o XML.
- Simplifica la creación de servicios backend para aplicaciones front-end como Angular, React, o clientes móviles.

---

### **2. ¿Cómo funciona?**
Spring convierte el objeto retornado por los métodos de un `@RestController` en una respuesta HTTP adecuada utilizando la biblioteca **Jackson** (predeterminada para JSON) o JAXB (para XML si se configura).

**Flujo típico**:
1. El cliente envía una solicitud HTTP a un endpoint definido en el controlador.
2. El controlador procesa la solicitud y devuelve un objeto.
3. Spring traduce el objeto a un formato de respuesta como JSON y lo envía al cliente.

---

### **3. Configuración básica**
Para utilizar `@RestController` en una aplicación Spring Boot:
1. Asegúrate de tener el starter `spring-boot-starter-web` en tu archivo `pom.xml` o `build.gradle`.
2. Declara tus controladores con `@RestController`.

---

### **4. Ejemplo básico**
Un ejemplo típico de uso de `@RestController` es un servicio que devuelve un mensaje en formato JSON.

#### Código:
```java
@RestController
@RequestMapping("/api")
public class HelloController {

    @GetMapping("/hello")
    public Map<String, String> sayHello() {
        Map<String, String> response = new HashMap<>();
        response.put("message", "Hola, bienvenido a la API RESTful!");
        return response; // Esto se convierte automáticamente a JSON
    }
}
```

#### Respuesta JSON:
```json
{
  "message": "Hola, bienvenido a la API RESTful!"
}
```

---

### **5. Ejemplo avanzado**
Supongamos que queremos construir una API para gestionar usuarios.

#### Modelo:
```java
public class User {
    private Long id;
    private String name;
    private String email;

    // Getters y setters
}
```

#### Controlador:
```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    private List<User> users = new ArrayList<>(List.of(
        new User(1L, "John Doe", "john@example.com"),
        new User(2L, "Jane Doe", "jane@example.com")
    ));

    @GetMapping
    public List<User> getAllUsers() {
        return users;
    }

    @GetMapping("/{id}")
    public ResponseEntity<User> getUserById(@PathVariable Long id) {
        return users.stream()
            .filter(user -> user.getId().equals(id))
            .findFirst()
            .map(ResponseEntity::ok)
            .orElse(ResponseEntity.notFound().build());
    }

    @PostMapping
    public User createUser(@RequestBody User user) {
        user.setId((long) (users.size() + 1));
        users.add(user);
        return user;
    }
}
```

#### Explicación:
1. **`@GetMapping`**: Obtiene todos los usuarios o uno específico basado en el ID.
2. **`@PostMapping`**: Agrega un nuevo usuario a la lista (simulación de base de datos).
3. **`@RequestBody`**: Convierte un cuerpo JSON enviado por el cliente en un objeto Java.

#### Prueba con una herramienta como Postman:
- **GET**: `/api/users` -> Devuelve todos los usuarios.
- **POST**: `/api/users` con cuerpo JSON:
  ```json
  {
    "name": "Nuevo Usuario",
    "email": "nuevo@example.com"
  }
  ```
  Respuesta:
  ```json
  {
    "id": 3,
    "name": "Nuevo Usuario",
    "email": "nuevo@example.com"
  }
  ```

---

### **6. Comparación con `@Controller`**
| Característica            | `@Controller`                      | `@RestController`                   |
|---------------------------|-------------------------------------|-------------------------------------|
| **Tipo de respuesta**      | Vistas renderizadas (HTML, JSP)     | Datos (JSON, XML)                   |
| **Uso principal**          | Aplicaciones web tradicionales      | APIs RESTful                        |
| **Necesidad de `@ResponseBody`** | Sí                            | No, ya está implícito                |

---

### **7. Casos de uso comunes**
- Construcción de **APIs REST** para frontends modernos (React, Angular).
- Exposición de microservicios para integraciones.
- Creación de backends para aplicaciones móviles.

---

### **8. Ventajas**
- **Menos código**: Combina dos anotaciones (`@Controller` y `@ResponseBody`).
- **Compatibilidad JSON/XML**: Soporte predeterminado para formatos estructurados.
- **Escalabilidad**: Fácil integración en arquitecturas basadas en microservicios.

---

### **Conclusión**
`@RestController` es ideal para construir APIs RESTful en aplicaciones Spring. Es simple, eficiente y se adapta perfectamente a los estándares modernos de desarrollo web y móvil. ¡Empieza a usarlo para facilitar la creación de servicios backend robustos y escalables!

## DIFERENCIA ENTRE CONTROLLER Y REST CONTROLLER:

En el ecosistema de Spring Framework, los controladores son componentes fundamentales que manejan solicitudes HTTP. Sin embargo, hay dos anotaciones principales para definir controladores en Spring: **`@Controller`** y **`@RestController`**. Aunque ambas sirven para manejar solicitudes, tienen diferencias importantes en cómo procesan las respuestas.

---

### **1. `@Controller`**
La anotación `@Controller` es utilizada para definir un controlador en aplicaciones web basadas en Spring. Está orientada principalmente a devolver vistas (por ejemplo, HTML) a través de un motor de plantillas como **Thymeleaf** o **JSP**.

#### Características principales:
- **Retorno de vistas**: Por defecto, espera que los métodos retornen nombres de vistas (como plantillas HTML o JSP) que serán renderizadas.
- **Uso con `ModelAndView` o `Model`**: Los datos se pasan a la vista utilizando el objeto `Model` o `ModelAndView`.
- **No convierte automáticamente a JSON**: Si necesitas devolver datos en formato JSON o XML, debes usar la anotación adicional `@ResponseBody` en los métodos.

#### Ejemplo:
```java
@Controller
public class HomeController {
    
    @GetMapping("/home")
    public String showHomePage(Model model) {
        model.addAttribute("message", "Bienvenido a la página de inicio");
        return "home"; // Renderiza el archivo home.html
    }
}
```

---

### **2. `@RestController`**
La anotación `@RestController` es una combinación de `@Controller` y `@ResponseBody`. Está diseñada para construir APIs RESTful y devolver directamente los datos en formato **JSON** o **XML**, sin necesidad de vistas.

#### Características principales:
- **Retorno de datos**: Por defecto, convierte los objetos devueltos en la respuesta HTTP a formato JSON (usando la librería Jackson, que es predeterminada en Spring).
- **Orientado a APIs RESTful**: Ideal para aplicaciones backend que exponen servicios a través de endpoints HTTP.
- **No necesita `@ResponseBody`**: Los métodos no requieren esta anotación adicional, ya que `@RestController` la incluye implícitamente.

#### Ejemplo:
```java
@RestController
public class ApiController {
    
    @GetMapping("/api/message")
    public Map<String, String> getMessage() {
        Map<String, String> response = new HashMap<>();
        response.put("message", "Hola desde la API");
        return response; // Devuelve {"message": "Hola desde la API"} como JSON
    }
}
```

---

### **Diferencias clave**
| Característica            | `@Controller`                              | `@RestController`                           |
|---------------------------|--------------------------------------------|--------------------------------------------|
| **Tipo de respuesta**      | Renderiza vistas (HTML, JSP, etc.)         | Devuelve datos en formato JSON o XML       |
| **Uso de `@ResponseBody`** | Necesario para datos (sin vistas)          | Implícito, no se requiere                  |
| **Orientación**            | Aplicaciones web tradicionales             | APIs RESTful                                |
| **Motores de plantillas**  | Compatible con motores como Thymeleaf      | No requiere motores de plantillas          |

---

### **Cuándo usar cada uno**
- Usa **`@Controller`** si estás construyendo una aplicación web tradicional donde necesitas renderizar vistas en el navegador.
- Usa **`@RestController`** si estás desarrollando una API REST que sirve datos a clientes como aplicaciones móviles o frontends basados en JavaScript (por ejemplo, Angular, React, o Vue.js).

Con esta distinción clara, puedes elegir la anotación adecuada según los requisitos de tu proyecto.


---

## Guía Completa para Manejar Excepciones en Spring

El manejo de excepciones en Spring es crucial para garantizar que las aplicaciones sean robustas, fáciles de depurar y amigables para los usuarios finales. A continuación, se presenta una guía completa sobre cómo manejar excepciones en Spring de forma efectiva y profesional.

---

### **¿Qué son las excepciones y por qué manejarlas?**

Las excepciones son eventos inesperados que ocurren durante la ejecución de un programa. Manejar excepciones permite:

- Evitar que la aplicación falle abruptamente.
- Proveer mensajes claros y útiles al usuario final.
- Registrar y depurar errores.
- Mejorar la experiencia de usuario y garantizar que la aplicación siga siendo funcional.

---

### **Estrategias para manejar excepciones en Spring**

Spring proporciona varias formas de manejar excepciones, desde estrategias específicas a nivel de controlador hasta enfoques globales.

---

#### **1. Manejo de excepciones específicas con `@ExceptionHandler`**

El decorador `@ExceptionHandler` permite manejar excepciones de manera local en un controlador. Es útil para manejar excepciones específicas lanzadas en el mismo controlador.

##### Ejemplo:
```java
@RestController
@RequestMapping("/api")
public class MiControlador {

    @GetMapping("/recurso")
    public String obtenerRecurso() {
        throw new IllegalArgumentException("Error en el recurso");
    }

    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity<String> manejarIllegalArgumentException(IllegalArgumentException ex) {
        return ResponseEntity.badRequest().body(ex.getMessage());
    }
}
```

**Ventajas:**
- Fácil de implementar.
- ÚTil para manejar errores específicos en controladores individuales.

**Desventajas:**
- No centraliza el manejo de errores.

---

#### **2. Manejo global de excepciones con `@ControllerAdvice`**

Para manejar excepciones en toda la aplicación de manera centralizada, se utiliza `@ControllerAdvice`. Esto permite un enfoque uniforme para gestionar errores.

##### Ejemplo:
```java
@ControllerAdvice
public class ManejadorGlobalExcepciones {

    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity<String> manejarIllegalArgumentException(IllegalArgumentException ex) {
        return ResponseEntity.badRequest().body("Error: " + ex.getMessage());
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> manejarExcepcionesGenerales(Exception ex) {
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body("Ha ocurrido un error inesperado");
    }
}
```

**Ventajas:**
- Centraliza el manejo de excepciones.
- Reduce la repetición de código.

**Desventajas:**
- Puede complicarse si se manejan muchas excepciones diferentes.

---

#### **3. Uso de `ResponseStatusException`**

La clase `ResponseStatusException` permite lanzar excepciones con códigos de estado HTTP específicos.

##### Ejemplo:
```java
@GetMapping("/recurso")
public String obtenerRecurso() {
    throw new ResponseStatusException(HttpStatus.NOT_FOUND, "Recurso no encontrado");
}
```

**Ventajas:**
- Simplicidad y claridad en la implementación.

**Desventajas:**
- Menos flexible en comparación con `@ExceptionHandler`.

---

#### **4. Creación de clases de respuesta para errores**

Es una buena práctica devolver errores en un formato consistente, por ejemplo, en formato JSON. Esto es especialmente útil en APIs REST.

##### Ejemplo:
```java
public class ErrorResponse {
    private String mensaje;
    private int codigo;

    public ErrorResponse(String mensaje, int codigo) {
        this.mensaje = mensaje;
        this.codigo = codigo;
    }

    // Getters y setters
}

@ControllerAdvice
public class ManejadorGlobalExcepciones {

    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity<ErrorResponse> manejarIllegalArgumentException(IllegalArgumentException ex) {
        ErrorResponse error = new ErrorResponse(ex.getMessage(), HttpStatus.BAD_REQUEST.value());
        return new ResponseEntity<>(error, HttpStatus.BAD_REQUEST);
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> manejarExcepcionesGenerales(Exception ex) {
        ErrorResponse error = new ErrorResponse("Ha ocurrido un error inesperado", HttpStatus.INTERNAL_SERVER_ERROR.value());
        return new ResponseEntity<>(error, HttpStatus.INTERNAL_SERVER_ERROR);
    }
}
```

**Ventajas:**
- Consistencia en las respuestas.
- Mejora la experiencia del cliente.

---

#### **5. Manejo de errores de validación**

Cuando utilizas validaciones con anotaciones como `@Valid`, puedes capturar excepciones como `MethodArgumentNotValidException` para devolver mensajes claros.

##### Ejemplo:
```java
@ExceptionHandler(MethodArgumentNotValidException.class)
public ResponseEntity<Map<String, String>> manejarErroresDeValidacion(MethodArgumentNotValidException ex) {
    Map<String, String> errores = new HashMap<>();
    ex.getBindingResult().getFieldErrors().forEach(error ->
        errores.put(error.getField(), error.getDefaultMessage())
    );
    return ResponseEntity.badRequest().body(errores);
}
```

**Ventajas:**
- Proporciona mensajes detallados para errores de validación.

---

#### **6. Creación de excepciones personalizadas**

Puedes definir tus propias clases de excepciones para representar errores específicos de tu negocio.

#### Ejemplo:
```java
public class MiExcepcionPersonalizada extends RuntimeException {
    public MiExcepcionPersonalizada(String mensaje) {
        super(mensaje);
    }
}

@ExceptionHandler(MiExcepcionPersonalizada.class)
public ResponseEntity<String> manejarMiExcepcionPersonalizada(MiExcepcionPersonalizada ex) {
    return ResponseEntity.status(HttpStatus.CONFLICT).body(ex.getMessage());
}
```

**Ventajas:**
- Mejora la semántica del código.
- Facilita la gestión de errores específicos.

---

### **Buenas Prácticas**

1. **Centraliza el manejo de excepciones**: Usa `@ControllerAdvice` para reducir la repetición de código.
2. **Devuelve códigos de estado HTTP correctos**: Asegúrate de que cada respuesta tenga un código adecuado (por ejemplo, 404 para "no encontrado", 400 para "mala solicitud").
3. **Proporciona mensajes claros**: Los mensajes de error deben ser comprensibles y útiles.
4. **Evita exponer detalles sensibles**: Nunca devuelvas rastros de pila u otra información sensible en respuestas al cliente.
5. **Registra errores**: Utiliza herramientas como `SLF4J` o `Logback` para registrar los errores y facilitar la depuración.
6. **Maneja excepciones específicas antes que generales**: Define primero excepciones específicas en tus manejadores.

---

Con estas técnicas y buenas prácticas, puedes manejar excepciones en Spring de forma efectiva, garantizando una mejor experiencia para los usuarios y un código más limpio y mantenible.


## THYMELEAF:

Nació para hacer aplicaciones monolíticas y poder hacer el frontend con Springboot también. Thymeleaf es un motor de plantillas

### **Guía Completa de Thymeleaf y su Uso en Aplicaciones Spring Boot**

**Thymeleaf** es un motor de plantillas para Java que se utiliza principalmente en aplicaciones **Spring Boot** para renderizar vistas del lado del servidor. Es una alternativa popular a tecnologías como JSP o Freemarker y destaca por ser fácil de usar y compatible con HTML estándar, lo que permite a los desarrolladores trabajar en conjunto con diseñadores de front-end.

A continuación, se explican sus conceptos principales y se ofrecen ejemplos detallados para comprender su uso.

---

### **1. Instalación y Configuración**

Cuando generas un proyecto Spring Boot usando **Spring Initializr**, puedes agregar Thymeleaf como dependencia seleccionando `Spring Web` y `Thymeleaf`.

#### **Dependencia Maven**
En caso de que necesites agregarlo manualmente, incluye la dependencia en tu archivo `pom.xml`:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

Para Gradle, agrega esto al archivo `build.gradle`:
```groovy
implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
```

---

### **2. ¿Cómo funciona Thymeleaf?**

- Thymeleaf procesa archivos **HTML** y permite insertar dinámicamente contenido desde el backend.
- Combina etiquetas y atributos específicos (`th:*`) con datos enviados desde controladores de Spring.
- Permite trabajar directamente con HTML, lo que simplifica la colaboración con diseñadores.

---

### **3. Estructura Básica del Proyecto**

```plaintext
src
├── main
│   ├── java
│   │   └── com.example.demo
│   │       ├── DemoApplication.java
│   │       └── controller
│   │           └── HomeController.java
│   ├── resources
│       ├── static          # Recursos estáticos (CSS, JS, imágenes)
│       ├── templates       # Vistas Thymeleaf (HTML)
│       └── application.properties
```

#### **Configuración predeterminada**
Spring Boot buscará los archivos Thymeleaf en el directorio `src/main/resources/templates`. Si necesitas cambiar este comportamiento, agrega lo siguiente a tu `application.properties`:

```properties
spring.thymeleaf.prefix=classpath:/misplantillas/
spring.thymeleaf.suffix=.html
```

---

### **4. Ejemplo Básico: Renderizar una Página HTML**

#### **4.1. Controlador Simple**
Crea un controlador en Java para manejar solicitudes HTTP y devolver vistas Thymeleaf.

```java
package com.example.demo.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HomeController {

    @GetMapping("/")
    public String home(Model model) {
        model.addAttribute("message", "¡Bienvenido a Thymeleaf con Spring Boot!");
        return "home"; // Renderiza home.html en templates/
    }
}
```

#### **4.2. Plantilla HTML (home.html)**

Guarda este archivo en `src/main/resources/templates/home.html`:

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Home Page</title>
</head>
<body>
    <h1 th:text="${message}">Texto predeterminado</h1>
</body>
</html>
```

#### **¿Qué hace el atributo `th:text`?**
- **`${message}`**: Es una expresión que Thymeleaf evalúa para reemplazar el contenido con el valor del modelo enviado desde el controlador.

Cuando visitas `http://localhost:8080/`, Thymeleaf renderiza el contenido con el mensaje enviado desde el controlador.

---

### **5. Atributos Thymeleaf Comunes**

#### **5.1. Mostrar Contenido**
- **`th:text`**: Reemplaza el texto dentro de un elemento.
- **Ejemplo:**

```html
<p th:text="'Hola, ' + ${nombre} + '!'">Placeholder</p>
```

Si `nombre = "Juan"`, el resultado será:
```html
<p>Hola, Juan!</p>
```

---

#### **5.2. Enlaces y Recursos**
- **`th:href`**: Genera enlaces dinámicos.
- **`th:src`**: Configura rutas de imágenes o recursos.

##### Ejemplo de `th:href`
```html
<a th:href="@{/contacto}">Ir a Contacto</a>
```

Si tu controlador tiene:
```java
@GetMapping("/contacto")
public String contacto() {
    return "contacto"; // Renderiza contacto.html
}
```

El HTML resultante será:
```html
<a href="/contacto">Ir a Contacto</a>
```

---

#### **5.3. Listas y Bucles**
- **`th:each`**: Itera sobre colecciones para generar contenido dinámico.

##### Ejemplo
Controlador:
```java
@GetMapping("/productos")
public String productos(Model model) {
    List<String> productos = Arrays.asList("Laptop", "Mouse", "Teclado");
    model.addAttribute("productos", productos);
    return "productos";
}
```

Plantilla HTML:
```html
<ul>
    <li th:each="producto : ${productos}" th:text="${producto}"></li>
</ul>
```

Resultado:
```html
<ul>
    <li>Laptop</li>
    <li>Mouse</li>
    <li>Teclado</li>
</ul>
```

---

#### **5.4. Formularios**
Thymeleaf facilita la creación de formularios dinámicos.

##### Ejemplo: Crear un formulario y manejar datos
Controlador:
```java
@PostMapping("/submit")
public String submit(@RequestParam String nombre, Model model) {
    model.addAttribute("mensaje", "Hola, " + nombre + "!");
    return "resultado";
}
```

Formulario HTML:
```html
<form action="/submit" method="post">
    <label for="nombre">Nombre:</label>
    <input type="text" id="nombre" name="nombre">
    <button type="submit">Enviar</button>
</form>
```

Resultado (después de enviar):
```html
<h1>Hola, Juan!</h1>
```

---

### **6. Internacionalización**
Thymeleaf soporta archivos de mensajes para traducción.

#### Configuración de mensajes
1. Crea archivos de propiedades, por ejemplo:
   - `messages_en.properties`:
     ```properties
     saludo=Hello!
     ```
   - `messages_es.properties`:
     ```properties
     saludo=¡Hola!
     ```

2. En tu plantilla, usa `th:text` con `#{}`
   ```html
   <p th:text="#{saludo}">Default Text</p>
   ```

El texto se traducirá automáticamente según la configuración de idioma de Spring.

---

### **7. Extensiones y Avanzado**

#### **Fragmentos Reutilizables**
Thymeleaf permite reutilizar fragmentos de HTML como cabeceras o pies de página.

##### Ejemplo
Archivo de fragmento (`header.html`):
```html
<div th:fragment="header">
    <h1>Mi Sitio Web</h1>
</div>
```

Archivo principal (`home.html`):
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<body>
    <div th:include="header :: header"></div>
</body>
</html>
```

---

#### **Condicionales**
- **`th:if`**: Muestra un elemento si se cumple una condición.
- **Ejemplo**:

```html
<p th:if="${usuarioLogeado}">Bienvenido, usuario!</p>
<p th:unless="${usuarioLogeado}">Por favor, inicia sesión.</p>
```

---

### **Conclusión**

Thymeleaf es una herramienta poderosa para crear aplicaciones web con Spring Boot. Proporciona:
- Un lenguaje intuitivo para renderizar contenido dinámico.
- Soporte completo para formularios, listas, internacionalización y fragmentos reutilizables.
- Una integración perfecta con Spring MVC, haciendo que sea ideal para proyectos Java.

---

- Thymelead funciona con plantillas de html

- Usaremos un @Controller para devolver vistas

- Usaremos la clase model para transferir objetos entre el @Controller y la vista

---

En el marco de trabajo Spring, las anotaciones `@Controller` y el objeto **Model** desempeñan roles complementarios en la construcción de aplicaciones web tradicionales. A continuación, se explica cómo funcionan y cómo se relacionan:

---

### **1. ¿Qué es `@Controller` y cómo funciona?**

La anotación **`@Controller`** en Spring se utiliza para marcar una clase como un controlador que maneja solicitudes web. Es una parte fundamental del patrón **MVC (Model-View-Controller)**, donde el controlador actúa como un intermediario entre la vista y el modelo.

#### **Principales características:**
- **Dirige las solicitudes HTTP**: Mapear rutas específicas usando anotaciones como `@GetMapping`, `@PostMapping`, etc.
- **Devuelve vistas**: Por defecto, los métodos en un controlador marcado con `@Controller` retornan el nombre de una vista (como un archivo HTML).
- **Colabora con el modelo**: Utiliza el objeto `Model` para enviar datos desde el backend a la vista.

#### **Ejemplo de uso:**
```java
@Controller
public class HomeController {

    @GetMapping("/home")
    public String showHomePage(Model model) {
        model.addAttribute("message", "Bienvenido a Spring MVC");
        return "home"; // Renderiza el archivo home.html en resources/templates
    }
}
```

En este ejemplo:
1. Se define una ruta `/home` que, al ser llamada, ejecuta el método `showHomePage`.
2. El objeto `Model` pasa datos a la vista.
3. Se devuelve el nombre de la vista (`home`), que será renderizada usando un motor como Thymeleaf.

---

### **2. ¿Qué es el objeto Model y cómo funciona?**

El objeto **Model** en Spring actúa como un contenedor para almacenar y transferir datos desde el controlador hacia la vista. Es una abstracción que simplifica el intercambio de datos entre estas dos capas.

#### **Principales características:**
- **Almacén de datos**: Permite agregar atributos (como variables) que estarán disponibles en la vista.
- **Clave-Valor**: Los datos se almacenan en pares clave-valor, donde la clave será el identificador en la vista.
- **Colaboración con motores de plantillas**: Los datos del modelo son utilizados por motores como Thymeleaf o JSP para renderizar el contenido dinámico.

#### **Ejemplo de uso:**
```java
@GetMapping("/greeting")
public String greeting(Model model) {
    model.addAttribute("title", "Saludos");
    model.addAttribute("content", "Este es un mensaje desde el backend.");
    return "greeting"; // Renderiza greeting.html
}
```

#### **En la vista (Thymeleaf):**
```html
<!DOCTYPE html>
<html>
<head>
    <title th:text="${title}">Default Title</title>
</head>
<body>
    <p th:text="${content}">Default Content</p>
</body>
</html>
```

En este ejemplo:
- El controlador pasa dos atributos (`title` y `content`) al modelo.
- La vista utiliza esos atributos para renderizar el contenido dinámicamente.

---

### **3. Diferencia entre `@Controller` y `@RestController`**

| Característica            | `@Controller`                              | `@RestController`                           |
|---------------------------|--------------------------------------------|--------------------------------------------|
| **Propósito principal**   | Renderizar vistas (HTML, JSP)              | Devolver datos (JSON, XML)                 |
| **Uso de `Model`**        | Necesario para pasar datos a la vista      | No se utiliza, ya que se devuelven datos directamente |
| **Anotación combinada**   | No combina `@ResponseBody`                 | Combina `@Controller` + `@ResponseBody`    |

---

### **4. Relación entre `@Controller` y Model**

- **`@Controller`** gestiona la lógica de la solicitud HTTP y delega a una vista para renderizar.
- **Model** permite enviar datos desde el controlador a esa vista, facilitando la renderización dinámica.

---

### **Conclusión**
- **`@Controller`** es ideal para aplicaciones web tradicionales que necesitan renderizar vistas HTML dinámicas.
- **Model** es el puente que permite a los controladores compartir datos con las vistas, haciéndolas interactivas y personalizadas para cada usuario.

Estas herramientas combinadas son esenciales para aplicaciones basadas en el patrón MVC.

---

- Todas las vistas de thymeleaf se guardar en la carpeta templates


---
  
Para enviar datos de un formulario desde el front-end al back-end usando **Thymeleaf** y **Spring Boot**, puedes seguir estos pasos detallados:

---

### **1. Crear la clase del modelo (Entidad o DTO):**
Define una clase en el backend que represente los datos del formulario.

```java
public class FormData {
    private String nombre;
    private String email;
    private int edad;

    // Getters y Setters
    public String getNombre() {
        return nombre;
    }

    public void setNombre(String nombre) {
        this.nombre = nombre;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public int getEdad() {
        return edad;
    }

    public void setEdad(int edad) {
        this.edad = edad;
    }
}
```

---

### **2. Crear el controlador en el back-end:**
Crea un controlador que procese el formulario.

```java
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.ModelAttribute;

@Controller
public class FormController {

    // Muestra el formulario
    @GetMapping("/formulario")
    public String mostrarFormulario(Model model) {
        model.addAttribute("formData", new FormData());
        return "formulario"; // Nombre del archivo HTML Thymeleaf
    }

    // Procesa el formulario
    @PostMapping("/procesarFormulario")
    public String procesarFormulario(@ModelAttribute FormData formData, Model model) {
        // Aquí puedes manejar los datos enviados
        System.out.println("Nombre: " + formData.getNombre());
        System.out.println("Email: " + formData.getEmail());
        System.out.println("Edad: " + formData.getEdad());

        model.addAttribute("mensaje", "Formulario enviado correctamente.");
        return "resultado"; // Redirige a una página de resultados
    }
}
```

---

### **3. Crear la vista del formulario (HTML Thymeleaf):**
El archivo `formulario.html` mostrará el formulario y enviará los datos al backend.

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Formulario</title>
</head>
<body>
    <h1>Formulario de ejemplo</h1>
    <form th:action="@{/procesarFormulario}" th:object="${formData}" method="post">
        <label for="nombre">Nombre:</label>
        <input type="text" id="nombre" th:field="*{nombre}" /><br/>

        <label for="email">Email:</label>
        <input type="email" id="email" th:field="*{email}" /><br/>

        <label for="edad">Edad:</label>
        <input type="number" id="edad" th:field="*{edad}" /><br/>

        <button type="submit">Enviar</button>
    </form>
</body>
</html>
```

---

### **4. Crear la vista para mostrar el resultado:**
El archivo `resultado.html` mostrará el mensaje o la respuesta.

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Resultado</title>
</head>
<body>
    <h1>Resultado del Formulario</h1>
    <p th:text="${mensaje}"></p>
</body>
</html>
```

---

### **5. Ejecutar la aplicación:**
1. Corre tu aplicación Spring Boot.
2. Abre el navegador y visita `http://localhost:8080/formulario`.
3. Llena el formulario y envíalo. Los datos se procesarán en el backend, y la página de resultado mostrará el mensaje de confirmación.

---

### **Notas adicionales:**
- **Validación de datos:** Puedes agregar validaciones usando anotaciones de Java como `@NotNull`, `@Size`, o `@Email` en la clase del modelo y manejar errores en el controlador.
- **Estilo y diseño:** Puedes usar CSS o frameworks como Bootstrap para darle un mejor aspecto al formulario.

¿Te gustaría agregar alguna funcionalidad adicional como validación o mostrar los datos enviados en la página de resultados?

---

En **Thymeleaf**, el **Model** y el **@ModelAttribute** son conceptos fundamentales en el flujo de datos entre el controlador y la vista en aplicaciones basadas en **Spring MVC**. Aquí te lo explico:

---

### **1. El `Model` en Spring MVC**

El **`Model`** es una interfaz que Spring proporciona para pasar datos desde el controlador a la vista (en este caso, una plantilla Thymeleaf). Es un contenedor de datos que permite añadir atributos que luego serán accesibles en la vista.

#### Uso del `Model`:
Cuando añades datos al objeto **`Model`**, estos estarán disponibles en la vista con los nombres que les hayas asignado.

**Ejemplo de uso en un controlador:**

```java
@Controller
public class MiControlador {

    @GetMapping("/saludo")
    public String mostrarSaludo(Model model) {
        model.addAttribute("mensaje", "¡Hola desde Spring MVC!");
        return "saludo"; // Nombre de la plantilla Thymeleaf (saludo.html)
    }
}
```

**Correspondiente en Thymeleaf:**

En el archivo **`saludo.html`** puedes acceder a los atributos del modelo usando la sintaxis de Thymeleaf:

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Saludo</title>
</head>
<body>
    <h1 th:text="${mensaje}">Mensaje por defecto</h1>
</body>
</html>
```

- **`${mensaje}`**: Recupera el valor del atributo llamado `mensaje` en el modelo.

---

### **2. La anotación `@ModelAttribute`**

La anotación **`@ModelAttribute`** tiene dos principales usos en Spring:

#### **a. Para agregar atributos al modelo globalmente**
Si anotas un método con `@ModelAttribute`, el valor devuelto por ese método se agregará automáticamente al modelo **para todas las vistas**. Esto es útil para datos comunes que deben estar disponibles en muchas vistas.

**Ejemplo:**

```java
@Controller
public class MiControlador {

    @ModelAttribute("nombreApp")
    public String nombreDeLaAplicacion() {
        return "Mi Aplicación Spring";
    }

    @GetMapping("/pagina")
    public String mostrarPagina(Model model) {
        model.addAttribute("mensaje", "Contenido de la página");
        return "pagina";
    }
}
```

En la vista **`pagina.html`**, puedes acceder tanto a `mensaje` como a `nombreApp`:

```html
<p th:text="${nombreApp}"></p>
<p th:text="${mensaje}"></p>
```

#### **b. Para vincular datos a un objeto del modelo**
Cuando usas **`@ModelAttribute`** como parámetro de un método controlador, le dices a Spring que vincule los datos de una solicitud HTTP (como datos enviados desde un formulario) a un objeto específico.

**Ejemplo de un formulario:**

```java
@Controller
public class UsuarioControlador {

    @GetMapping("/formulario")
    public String mostrarFormulario(Model model) {
        model.addAttribute("usuario", new Usuario());
        return "formulario";
    }

    @PostMapping("/formulario")
    public String procesarFormulario(@ModelAttribute Usuario usuario) {
        System.out.println("Datos del usuario: " + usuario);
        return "resultado";
    }
}
```

En este caso:

1. El controlador crea un objeto vacío de la clase `Usuario` y lo añade al modelo.
2. En la vista **`formulario.html`**, puedes usar Thymeleaf para llenar los datos del objeto `Usuario`:
    ```html
    <form action="#" th:action="@{/formulario}" th:object="${usuario}" method="post">
        <label>Nombre:</label>
        <input type="text" th:field="*{nombre}" />
        <br/>
        <label>Email:</label>
        <input type="email" th:field="*{email}" />
        <br/>
        <button type="submit">Enviar</button>
    </form>
    ```

3. Cuando se envía el formulario, Spring automáticamente llena el objeto `Usuario` con los datos enviados y lo pasa al controlador como parámetro.

---

### Resumen:
- **`Model`**: Contenedor para pasar datos desde el controlador a la vista.
  - Ejemplo: `model.addAttribute("clave", valor)`.
- **`@ModelAttribute`**:
  - **Método**: Agrega datos al modelo de forma global.
  - **Parámetro**: Vincula datos de entrada (como formularios) a un objeto del modelo.