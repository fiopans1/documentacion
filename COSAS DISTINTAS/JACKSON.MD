# Guía Completa sobre Jackson en Java

Jackson es una de las librerías más utilizadas en Java para convertir objetos en JSON y viceversa. Es especialmente útil en aplicaciones Spring Boot, donde se usa por defecto para la serialización y deserialización de datos en APIs REST.

---

## Instalación de Jackson

Si usas Maven, agrega la dependencia en tu `pom.xml`:

```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.15.0</version>
</dependency>
```

Para Spring Boot, no es necesario incluir manualmente esta dependencia, ya que está incluida en `spring-boot-starter-web`.

---

## Conversión Básica de Objetos a JSON y Viceversa

La clase principal de Jackson para manejar conversiones es `ObjectMapper`.

### Serialización (Java a JSON)

```java
import com.fasterxml.jackson.databind.ObjectMapper;

public class Main {
    public static void main(String[] args) throws Exception {
        ObjectMapper objectMapper = new ObjectMapper();

        Persona persona = new Persona("Juan", 30);
        String json = objectMapper.writeValueAsString(persona);

        System.out.println(json);
    }
}

class Persona {
    private String nombre;
    private int edad;

    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }

    // Getters y Setters son necesarios para Jackson
    public String getNombre() { return nombre; }
    public void setNombre(String nombre) { this.nombre = nombre; }
    
    public int getEdad() { return edad; }
    public void setEdad(int edad) { this.edad = edad; }
}
```

Salida:

```json
{"nombre":"Juan","edad":30}
```

### Deserialización (JSON a Java)

```java
String json = "{\"nombre\":\"Juan\",\"edad\":30}";
Persona persona = objectMapper.readValue(json, Persona.class);

System.out.println(persona.getNombre()); // Juan
```

---

## Uso de Jackson en Spring Boot

Spring Boot usa Jackson automáticamente para convertir objetos a JSON en controladores REST:

```java
@RestController
@RequestMapping("/personas")
public class PersonaController {

    @GetMapping
    public Persona obtenerPersona() {
        return new Persona("Juan", 30);
    }
}
```

Si haces una petición `GET /personas`, la respuesta será:

```json
{"nombre":"Juan","edad":30}
```

---

## Anotaciones Importantes en Jackson

Jackson proporciona varias anotaciones para personalizar la serialización/deserialización.

### `@JsonProperty`
Permite renombrar propiedades en JSON.

```java
class Persona {
    @JsonProperty("full_name")
    private String nombre;

    @JsonProperty("years_old")
    private int edad;
}
```

Salida:

```json
{"full_name":"Juan","years_old":30}
```

---

### `@JsonIgnore`
Omite propiedades en la serialización.

```java
class Persona {
    @JsonIgnore
    private int edad;
}
```

Salida:

```json
{"nombre":"Juan"}
```

---

### `@JsonInclude`
Excluye valores nulos o predeterminados.

```java
@JsonInclude(JsonInclude.Include.NON_NULL)
class Persona {
    private String nombre;
    private Integer edad;
}
```

Si `edad` es `null`, no aparecerá en el JSON.

---

### `@JsonFormat`
Define el formato de fechas.

```java
class Persona {
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "dd-MM-yyyy")
    private Date fechaNacimiento;
}
```

Salida:

```json
{"fechaNacimiento":"15-03-1990"}
```

---

### `@JsonCreator`
Permite definir un constructor específico para la deserialización.

```java
class Persona {
    private String nombre;
    private int edad;

    @JsonCreator
    public Persona(@JsonProperty("nombre") String nombre, @JsonProperty("edad") int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }
}
```

---

## Conclusión

Jackson es una herramienta poderosa para manejar JSON en Java y se integra de manera sencilla con Spring Boot. Con sus anotaciones, puedes personalizar fácilmente la serialización y deserialización según tus necesidades.