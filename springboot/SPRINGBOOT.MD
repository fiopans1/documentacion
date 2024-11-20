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

2:30