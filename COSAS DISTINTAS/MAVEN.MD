Aquí tienes una **guía completa sobre Maven** para proyectos en Java. Maven es una herramienta de construcción y gestión de dependencias ampliamente utilizada en el ecosistema Java.

---

## **1. ¿Qué es Maven?**

Maven es una herramienta de automatización y gestión de proyectos que facilita la construcción, reporte y documentación de proyectos Java (y otros lenguajes). Su característica principal es la gestión de dependencias y la estandarización de proyectos mediante un formato XML llamado `pom.xml`.

### **Ventajas de Maven:**
- Gestión automática de dependencias.
- Estandarización de la estructura del proyecto.
- Facilidad para construir, probar y empaquetar proyectos.
- Integración con herramientas populares (IDE, CI/CD).

---

## **2. Instalación de Maven**

### **Requisitos previos:**
- **Java JDK** (al menos versión 8 o superior). Configura la variable `JAVA_HOME`.
- **Maven**. Descárgalo desde [https://maven.apache.org/download.cgi](https://maven.apache.org/download.cgi).

### **Pasos de instalación:**
1. Descarga el archivo comprimido de Maven y extráelo en una carpeta.
2. Configura la variable de entorno `MAVEN_HOME` apuntando al directorio donde descomprimiste Maven.
3. Agrega `MAVEN_HOME/bin` al `PATH` de tu sistema.
4. Verifica la instalación ejecutando el comando:
   ```bash
   mvn -version
   ```
   Deberías ver información sobre Maven y la versión de Java.

---

## **3. Estructura de un proyecto Maven**

Cuando creas un proyecto Maven, sigue esta estructura estándar:

```
my-app/
│
├── src/
│   ├── main/
│   │   ├── java/            # Código fuente Java
│   │   └── resources/       # Recursos como archivos de configuración
│   ├── test/
│       ├── java/            # Pruebas unitarias
│       └── resources/       # Recursos para pruebas
│
├── pom.xml                  # Archivo de configuración de Maven
└── target/                  # Archivos generados (compilados, empaquetados, etc.)
```

---

## **4. Creación de un proyecto Maven**

### **Crear un proyecto desde cero**
Usa el siguiente comando para crear un nuevo proyecto:
```bash
mvn archetype:generate -DgroupId=com.miempresa.app -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

- **`groupId`**: Nombre del grupo o empresa (como el paquete base en Java).
- **`artifactId`**: Nombre del proyecto o módulo.
- **`archetypeArtifactId`**: Tipo de plantilla para el proyecto.

### **Generar un proyecto basado en una plantilla específica**
Maven incluye varios *arquetipos* (plantillas). Para listar los disponibles:
```bash
mvn archetype:generate
```

---

## **5. Archivo POM (`pom.xml`)**

El `pom.xml` es el corazón de Maven. Contiene:
- Información del proyecto.
- Dependencias.
- Configuraciones de complementos.
- Perfiles de construcción.

### **Ejemplo básico de `pom.xml`:**
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.miempresa.app</groupId>
    <artifactId>my-app</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <!-- Dependencia de ejemplo -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
            <version>3.1.2</version>
        </dependency>
    </dependencies>
</project>
```

---

## **6. Gestión de Dependencias**

Maven usa el repositorio de dependencias **Maven Central** por defecto, pero puedes configurar otros repositorios.

### **Agregar una dependencia:**
Busca las dependencias en [Maven Central Repository](https://mvnrepository.com/) e inclúyelas en el bloque `<dependencies>`.

Ejemplo:
```xml
<dependency>
    <groupId>com.google.guava</groupId>
    <artifactId>guava</artifactId>
    <version>32.1-jre</version>
</dependency>
```

### **Tipos de alcance (`scope`):**
- `compile` (por defecto): Disponible para compilar y ejecutar.
- `provided`: Disponible para compilar, pero no se empaqueta.
- `runtime`: Necesaria solo para ejecutar.
- `test`: Usada solo en pruebas.

---

## **7. Comandos básicos de Maven**

### **Compilar:**
```bash
mvn compile
```

### **Ejecutar pruebas:**
```bash
mvn test
```

### **Construir (empaquetar en un JAR o WAR):**
```bash
mvn package
```

### **Limpiar (eliminar la carpeta `target/`):**
```bash
mvn clean
```

### **Ejecutar el proyecto:**
Si usas el complemento de ejecución:
```bash
mvn exec:java -Dexec.mainClass="com.miempresa.app.Main"
```

### **Instalar en el repositorio local (`.m2/repository`):**
```bash
mvn install
```

---

## **8. Gestión de Perfiles**

Los perfiles permiten configurar diferentes entornos (desarrollo, producción, etc.).

Ejemplo en `pom.xml`:
```xml
<profiles>
    <profile>
        <id>dev</id>
        <properties>
            <env>development</env>
        </properties>
    </profile>
    <profile>
        <id>prod</id>
        <properties>
            <env>production</env>
        </properties>
    </profile>
</profiles>
```

Ejecutar un perfil:
```bash
mvn clean install -Pdev
```

---

## **9. Complementos útiles de Maven**

### **Maven Compiler Plugin:**
Configura la versión de Java:
```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.11.0</version>
            <configuration>
                <source>17</source>
                <target>17</target>
            </configuration>
        </plugin>
    </plugins>
</build>
```

### **Maven Shade Plugin:**
Empaqueta dependencias en un JAR ejecutable:
```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>3.4.1</version>
            <executions>
                <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>shade</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

---

## **10. Configuración de Repositorios**

### **Repositorios adicionales:**
```xml
<repositories>
    <repository>
        <id>my-repo</id>
        <url>https://example.com/maven-repo</url>
    </repository>
</repositories>
```

### **Repositorios locales:**
Configúralos en `~/.m2/settings.xml`.

---

## **11. Problemas comunes**

### **Error al descargar dependencias:**
Solución: Forzar la actualización:
```bash
mvn clean install -U
```

### **Versión de Java incompatible:**
Verifica las versiones configuradas en `pom.xml` y el entorno.

---

Con esta guía, tienes todo lo necesario para usar Maven con confianza en tus proyectos Java. ¡Adelante!

## Comandos maven:

¡Claro! Aquí tienes una **guía sencilla de comandos de Maven**, explicados para que cualquiera pueda entenderlos, incluso si eres nuevo en esto:

---

## **Comandos básicos de Maven**

### **1. Compilar el proyecto**
```bash
mvn compile
```
- **¿Qué hace?** Compila el código fuente del proyecto (archivos `.java`) y los convierte en archivos `.class`.
- **¿Para qué sirve?** Asegura que tu código no tiene errores de compilación y lo prepara para ejecutarse.
- El comando mvn compile compila el código fuente de tu proyecto, no solo las dependencias. Aquí está el desglose:

Compila el código fuente de tu proyecto ubicado en el directorio src/main/java.
Resuelve y descarga las dependencias necesarias especificadas en el archivo pom.xml, si aún no están presentes en el repositorio local.

- El comando mvn compile guarda los archivos compilados en una estructura de carpetas dentro de la carpeta target. Específicamente:

Los archivos .class resultantes de la compilación del código fuente se almacenan en target/classes.
Si tienes recursos adicionales (como archivos de configuración), estos se copian a target/classes también.
Por ejemplo, si tienes una clase com.ejemplo.MiClase, el archivo compilado se guardará en target/classes/com/ejemplo/MiClase.class.

---

### **2. Limpiar el proyecto**
```bash
mvn clean
```
- **¿Qué hace?** Borra todos los archivos generados por Maven (como la carpeta `target/`).
- **¿Para qué sirve?** Limpia tu proyecto para empezar desde cero. Útil cuando hay problemas y necesitas "borrar y reiniciar".

---

### **3. Empaquetar el proyecto**
```bash
mvn package
```
- **¿Qué hace?** Compila tu proyecto y lo empaqueta en un archivo **JAR** (Java ARchive) o **WAR** (Web ARchive), según el tipo de proyecto.
- **¿Para qué sirve?** Te da un archivo listo para ejecutar o desplegar en un servidor.

---

### **4. Instalar el proyecto**
```bash
mvn install
```
- **¿Qué hace?** Instala el archivo generado (JAR o WAR) en tu repositorio local (`~/.m2/repository`).
- **¿Para qué sirve?** Te permite usar este proyecto como una dependencia en otros proyectos Maven de tu máquina.

---

### **5. Ejecutar las pruebas**
```bash
mvn test
```
- **¿Qué hace?** Ejecuta las pruebas unitarias de tu proyecto (archivos en `src/test/java`).
- **¿Para qué sirve?** Asegura que el código funciona correctamente y pasa todas las pruebas configuradas.

---

### **6. Ejecutar la aplicación**
```bash
mvn exec:java -Dexec.mainClass="com.miempresa.Main"
```
- **¿Qué hace?** Ejecuta directamente tu aplicación Java desde Maven.
- **¿Para qué sirve?** Te permite probar tu aplicación sin complicarte creando comandos manuales para Java.

---

### **7. Forzar la actualización de dependencias**
```bash
mvn clean install -U
```
- **¿Qué hace?** Descarga las dependencias más recientes de los repositorios remotos, incluso si ya las tienes en tu repositorio local.
- **¿Para qué sirve?** Útil cuando tus dependencias están corruptas o han cambiado en el servidor.

---

### **8. Resolver dependencias**
```bash
mvn dependency:resolve
```
- **¿Qué hace?** Descarga todas las dependencias de tu archivo `pom.xml`.
- **¿Para qué sirve?** Te asegura que todo lo que tu proyecto necesita esté listo, sin compilar nada.

---

### **9. Generar un proyecto nuevo**
```bash
mvn archetype:generate -DgroupId=com.miempresa -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```
- **¿Qué hace?** Crea una estructura básica de proyecto Maven.
- **¿Para qué sirve?** Para empezar un nuevo proyecto con todo lo necesario ya configurado.

---

## **Comandos avanzados de Maven**

### **10. Construir el proyecto**
```bash
mvn install
```
- **¿Qué hace?** Es un paso completo: limpia, compila, ejecuta pruebas, empaqueta y lo guarda en el repositorio local.
- **¿Para qué sirve?** Es como hacer todo el proceso de desarrollo de una sola vez.

---

### **11. Ejecutar Maven en modo offline**
```bash
mvn install -o
```
- **¿Qué hace?** Usa solo las dependencias locales (sin conexión a Internet).
- **¿Para qué sirve?** Si no tienes Internet pero ya descargaste las dependencias, puedes seguir trabajando.

---

### **12. Ver el árbol de dependencias**
```bash
mvn dependency:tree
```
- **¿Qué hace?** Muestra todas las dependencias de tu proyecto en forma de árbol, incluidas las dependencias transitivas.
- **¿Para qué sirve?** Útil para ver qué bibliotecas usa tu proyecto y resolver conflictos entre versiones.

---

### **13. Descargar dependencias y copiarlas a una carpeta**
```bash
mvn dependency:copy-dependencies -DoutputDirectory=./lib
```
- **¿Qué hace?** Copia todas las dependencias a una carpeta específica.
- **¿Para qué sirve?** Ideal si necesitas las dependencias para ejecutarlas manualmente o compartirlas con alguien.

---

### **14. Crear un sitio web con documentación del proyecto**
```bash
mvn site
```
- **¿Qué hace?** Genera una página web con información del proyecto, como dependencias, informes de pruebas, etc.
- **¿Para qué sirve?** Crear reportes profesionales para tu proyecto.

---

### **15. Saltar las pruebas**
```bash
mvn install -DskipTests
```
- **¿Qué hace?** Salta las pruebas unitarias al construir el proyecto.
- **¿Para qué sirve?** Acelera el proceso si sabes que las pruebas no son necesarias (¡pero no abuses de esto!).

---

## **Cómo recordar los comandos**
Piensa en Maven como una máquina con pasos lógicos:
1. **Primero limpia:** `mvn clean`
2. **Después compila:** `mvn compile`
3. **Prueba tu código:** `mvn test`
4. **Empaqueta el proyecto:** `mvn package`
5. **Guárdalo para usar en otros proyectos:** `mvn install`
