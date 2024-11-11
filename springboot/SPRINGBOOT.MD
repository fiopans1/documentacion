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