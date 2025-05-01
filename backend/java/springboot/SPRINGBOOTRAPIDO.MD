En springboot tenemos un archivo pom.xml donde se añaden todas las dependencias de maven

Vamos a tener 4 carpetas: controllers, models, repositories y services


En el archivo appplication.properties tendremos las propiedades del archivo 


Los modelos son los elementos que vamos a guardar en la base de datos  (con JPA ya mapea automaticamente), para eso ponemos @Table(name = "nombretabla")

@Entity decimos que es un modelo en la tabla


* @Id indicamos que parametro es el identificador en la tabla
* @GeneratedValue() : Que se genera automaticamente
* @Column() Propiedades de la columna

FLUJO:
Todo inicia con el controlador, ahí se recibe la petición web, después manda llamar a un servicio que ejecuta la lógica de la aplicación, que este utiliza el repositorio que es el que hace las peticiones con la base de datos y el repositorio utiliza el modelo para saber qué tipo de información va a traer


Cuando creamos un repositorio si extendemos de CrudRepository ya implementa la mayoría de los métodos, solo tenemos que indicarle el modelo y de que tipo es el identificador




Para crear el servicio tenemos que poner la notación @Service y ya se convierte en una clase de tipo servicio

Con @Autowired ya instancia las clases que hagan falta y además usa una sola instancia para todo el programa


Con la etiqueta @RestController se indica que ese es el controlador rest

Con @RequestMapping indicamos que cuando llegue una peticion con esa url se ejecute esa clase

Luego tenemos distintos metodos, como @GetMapping y @PostMapping, etc

En el método post, ponemos poner que como parámetro reciba un @RequestBody

En el repositorio solo con el método con el nombre adecuado podemos hacer x cosultas, por ejemplo si llamamos a un metodo findByNombreatributo buscara por el nombredelatributo

Dentro del GetMapping o PostMapping, podemos indicar su PATH, para pillar por ejemplo como parametro el id del path, tendriamos que poner @PathVariable y indicar le nombre de la variable


Podemos pasar también en las querys los parámetros por consulta, pero para eso en los parámetros de la función ponemos @RequestParam("nombreParametro")

También tenemos etiquetas para otros tipos de servicios como @DeleteMapping


En mvnrepository están todas las dependencias de maven


Para crear un proyecto springboot inicial tenemos start.spring.io


Con @Component puedes decirle a spring que te instancie una clase y que puedas usarla como inyección en servicios u otras cosas, es cómoda para hacer un commonsUtils o algo por el estilo


El DTO lo que se manda a través de REST2

El @Entity viene siendo la entidad

El DAO vienen siendo las operaciones de acceso a base de datos, las de @Repository