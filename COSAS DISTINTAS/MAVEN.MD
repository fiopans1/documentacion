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



## DIFERENCIA MVN INSTALL Y MVN CLEAN INSTALL
La diferencia principal entre `mvn install` y `mvn clean install` radica en la inclusión de la fase **`clean`** en el ciclo de vida de Maven cuando se utiliza el comando `mvn clean install`. Veamos en detalle:

### 1. **`mvn install`**
- Este comando ejecuta el ciclo de vida de Maven desde la fase inicial hasta la fase **`install`**. 
- **Fase `install`**: En esta fase, Maven compila el código fuente, ejecuta las pruebas, empaqueta el proyecto (en JAR, WAR, etc.) y copia el artefacto generado (junto con las dependencias necesarias) en el repositorio local de Maven (generalmente en `~/.m2/repository`).
- **No elimina archivos previos**: Si ya tienes artefactos generados (como JARs o WARs) y carpetas de compilación en el directorio `target`, Maven los reutilizará, lo que puede resultar en inconsistencias si los artefactos o clases generadas previamente están desactualizados.

### 2. **`mvn clean install`**
- Este comando **incluye la fase `clean` antes de la fase `install`**.
- **Fase `clean`**: Borra la carpeta `target` y cualquier archivo generado anteriormente, asegurando un entorno limpio para la compilación.
- Después de limpiar, Maven ejecuta el ciclo de vida completo desde el principio hasta la fase `install`, compilando el código, ejecutando pruebas, empaquetando y colocando los artefactos en el repositorio local.

### Diferencias clave:
| Aspecto                     | `mvn install`                       | `mvn clean install`                  |
|-----------------------------|--------------------------------------|--------------------------------------|
| **Limpieza previa**         | No elimina archivos previos.        | Limpia los archivos y directorios generados antes de la ejecución. |
| **Reutilización**           | Puede reutilizar archivos obsoletos.| Asegura que el proceso se ejecute desde cero. |
| **Tiempo de ejecución**     | Generalmente más rápido.             | Puede tomar más tiempo debido al paso adicional de limpieza. |
| **Consistencia**            | Puede generar resultados inconsistentes si hay cambios previos. | Garantiza un entorno limpio y resultados confiables. |

### ¿Cuándo usar cada uno?
- **`mvn install`**: Cuando estás trabajando iterativamente y no necesitas realizar una limpieza completa, por ejemplo, si no ha habido cambios en archivos generados previamente o dependencias.
- **`mvn clean install`**: Es preferible cuando:
  - Haces cambios significativos en el código o dependencias.
  - Quieres garantizar que no haya residuos de compilaciones anteriores.
  - Has encontrado problemas inesperados en la ejecución del proyecto y necesitas empezar desde un entorno limpio.

En resumen, **`mvn clean install`** es una opción más segura pero un poco más lenta, mientras que **`mvn install`** es más rápido pero puede resultar en inconsistencias si no se tiene cuidado.

# GUIA SOBRE EL POM.XML:
¡Perfecto! Aquí tienes un índice completo para una guía sobre el uso del `pom.xml` en Maven. 

## Índice de la Guía sobre `pom.xml` de Maven

1. **Introducción a Maven y el `pom.xml`**
   - ¿Qué es Maven?
   - Rol del archivo `pom.xml` en un proyecto Maven

2. **Estructura Básica del `pom.xml`**
   - Elementos esenciales: `project`, `modelVersion`, `groupId`, `artifactId`, `version`
   - Formato XML y su validación

3. **Gestión de Dependencias**
   - Declaración de dependencias
   - Uso de repositorios públicos y privados
   - Exclusiones y conflictos de versiones
   - Dependencias transitivas

4. **Gestión de Plugins**
   - Declaración y configuración de plugins
   - Plugins obligatorios y comunes (como `maven-compiler-plugin`, `maven-surefire-plugin`)
   - Fases del ciclo de vida y su relación con los plugins

5. **Perfiles de Construcción**
   - Creación y uso de perfiles
   - Activación de perfiles (por propiedades, sistema operativo, etc.)
   - Ejemplos prácticos

6. **Propiedades y Variables en el `pom.xml`**
   - Uso de propiedades predefinidas y personalizadas
   - Referencias cruzadas en el archivo `pom.xml`
   - Variables de entorno y configuración externa

7. **Herencia y Módulos (Multi-module Projects)**
   - Uso del padre (`parent`) en Maven
   - Estructura de proyectos multi-módulo
   - Compartir configuraciones entre módulos

8. **Configuraciones Avanzadas**
   - Repositorios remotos y locales
   - Configuración de compiladores específicos (Java, Kotlin, etc.)
   - Gestión de perfiles avanzados para entornos (producción, desarrollo)

9. **Extensiones y Personalización**
   - Extensiones de Maven
   - Archivos `settings.xml` y su interacción con `pom.xml`
   - Personalización de ciclos de vida con plugins

10. **Ejemplos Completos y Casos Prácticos**
    - Proyecto simple: aplicación Java
    - Proyecto avanzado: Spring Boot con múltiples módulos
    - Gestión de librerías internas y privadas

11. **Resolución de Problemas Comunes**
    - Resolución de conflictos de dependencias
    - Problemas con plugins
    - Soluciones a errores de configuración del `pom.xml`


---

### 1. **Introducción a Maven y el `pom.xml`**

#### 1.1 ¿Qué es Maven?
Maven es una herramienta de construcción y gestión de proyectos basada en el concepto de un modelo de objeto de proyecto (POM, Project Object Model). Es ampliamente utilizada en el ecosistema Java para automatizar tareas como compilación, prueba, empaquetado, despliegue y gestión de dependencias.

**Características principales de Maven:**
- **Gestión de dependencias:** Resuelve automáticamente las dependencias de un proyecto, incluyendo las transitivas.
- **Modelo estándar de construcción:** Define un ciclo de vida estándar con fases como `compile`, `test`, y `package`.
- **Escalabilidad:** Soporta proyectos pequeños, grandes y multi-módulo.
- **Extensibilidad:** Permite integrar plugins para personalizar y extender su funcionalidad.

#### 1.2 Rol del archivo `pom.xml` en un proyecto Maven
El archivo `pom.xml` es el corazón de un proyecto Maven. Define:
- **La identidad del proyecto:** Incluye información como `groupId`, `artifactId` y `version`, que hacen único al proyecto.
- **Dependencias:** Librerías externas necesarias para el proyecto.
- **Plugins y configuraciones:** Herramientas adicionales para personalizar el proceso de construcción.
- **Perfiles:** Configuraciones específicas para diferentes entornos (desarrollo, producción, etc.).
- **Configuración del ciclo de vida:** Qué debe hacer Maven en cada fase del ciclo de vida.

---

### 2. **Estructura Básica del `pom.xml`**

Un archivo `pom.xml` básico se ve así:

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
                             http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!-- Identificación del proyecto -->
    <groupId>com.ejemplo</groupId>
    <artifactId>mi-aplicacion</artifactId>
    <version>1.0.0</version>
    <packaging>jar</packaging> <!-- Opcional: jar, war, pom, etc. -->

    <!-- Información adicional -->
    <name>Mi Aplicación</name>
    <description>Un ejemplo de proyecto Maven</description>
    <url>http://mi-aplicacion.com</url>

</project>
```

#### 2.1 Elementos esenciales del `pom.xml`

1. **`<modelVersion>`**  
   Define la versión del modelo POM. Actualmente, siempre se usa `4.0.0`.

2. **`<groupId>`**  
   Identifica de manera única al grupo o empresa del proyecto, similar a un nombre de paquete en Java (por ejemplo, `com.miempresa`).

3. **`<artifactId>`**  
   Nombre único del proyecto dentro del grupo. Por ejemplo, `mi-aplicacion`.

4. **`<version>`**  
   Versión del proyecto. Maven sigue el formato de versiones estándar: `MAJOR.MINOR.PATCH` (por ejemplo, `1.0.0`).

5. **`<packaging>`** *(opcional)*  
   Tipo de artefacto generado:  
   - `jar`: Archivo Java ejecutable.
   - `war`: Aplicación web.
   - `pom`: Solo como contenedor (en proyectos multi-módulo).

#### 2.2 Validación y formato XML
El `pom.xml` usa XML estándar, y Maven verifica que sea válido antes de usarlo. Los atributos `xmlns` y `xsi:schemaLocation` aseguran que el archivo sea conforme al esquema XML definido por Maven.


---

### 3. **Gestión de Dependencias**

Una de las características más potentes de Maven es la capacidad de gestionar dependencias de manera eficiente, resolviendo automáticamente conflictos y transitividades.

---

#### 3.1 Declaración de dependencias

Las dependencias son librerías externas que un proyecto necesita para funcionar. En el `pom.xml`, se declaran dentro de la etiqueta `<dependencies>`, como en este ejemplo básico:

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>5.3.29</version>
        <scope>compile</scope> <!-- Opcional: explicamos scopes abajo -->
    </dependency>
</dependencies>
```

**Elementos importantes en una dependencia:**
- **`<groupId>`:** Identifica al proveedor de la librería (por ejemplo, `org.springframework`).
- **`<artifactId>`:** Nombre único del artefacto dentro del grupo (`spring-core`).
- **`<version>`:** Versión específica de la librería (`5.3.29`).
- **`<scope>`** *(opcional)*: Indica en qué fases del ciclo de vida de Maven se usará esta dependencia. (Ver más abajo).

---

#### 3.2 Uso de repositorios públicos y privados

##### Repositorios públicos
Maven descarga automáticamente las dependencias desde repositorios públicos como el **Maven Central Repository**. Este es el repositorio predeterminado en cualquier instalación de Maven.

```xml
<repositories>
    <repository>
        <id>central</id>
        <url>https://repo.maven.apache.org/maven2</url>
    </repository>
</repositories>
```

##### Repositorios privados
En entornos empresariales, se pueden configurar repositorios privados (como Nexus o Artifactory) para gestionar dependencias internas:

```xml
<repositories>
    <repository>
        <id>mi-repositorio</id>
        <url>https://mi-servidor.com/repository/maven-releases</url>
    </repository>
</repositories>
```

---

#### 3.3 Scopes de las dependencias

El elemento `<scope>` define cuándo y cómo se utilizará una dependencia. Los valores posibles son:

- **`compile` (por defecto):** Disponible en todas las fases del ciclo de vida (compilación, prueba, ejecución, empaquetado).
- **`provided`:** Similar a `compile`, pero se asume que estará disponible en el entorno de ejecución (por ejemplo, un servlet container como Tomcat).
- **`runtime`:** Necesario solo en tiempo de ejecución, no durante la compilación (por ejemplo, drivers de bases de datos).
- **`test`:** Disponible solo para pruebas (JUnit, Mockito, etc.).
- **`system`:** Similar a `provided`, pero requiere una ruta explícita al JAR en tu sistema.
- **`import`:** Usado en proyectos multi-módulo con dependencias gestionadas.

---

#### 3.4 Exclusiones y conflictos de versiones

Cuando dos dependencias incluyen diferentes versiones de la misma librería, Maven intentará resolver el conflicto, pero esto puede causar problemas. Puedes usar exclusiones para evitar conflictos:

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <version>3.0.0</version>
        <exclusions>
            <exclusion>
                <groupId>org.slf4j</groupId>
                <artifactId>slf4j-api</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
</dependencies>
```

---

#### 3.5 Dependencias transitivas

Maven resuelve automáticamente las dependencias transitivas, es decir, las dependencias de las dependencias. Por ejemplo:

1. Tu proyecto incluye `spring-boot-starter-web`.
2. Esta dependencia incluye automáticamente `spring-core`, `spring-web`, y más.

Puedes ver las dependencias transitivas ejecutando el siguiente comando:

```bash
mvn dependency:tree
```

Para evitar dependencias transitivas no deseadas, puedes usar el atributo `<scope>` o `<exclusions>`.

---

#### Ejemplo práctico de gestión de dependencias

```xml
<dependencies>
    <!-- Dependencia principal -->
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-core</artifactId>
        <version>2.20.0</version>
        <scope>runtime</scope>
    </dependency>

    <!-- Dependencia con exclusión -->
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.15.0</version>
        <exclusions>
            <exclusion>
                <groupId>com.fasterxml.jackson.core</groupId>
                <artifactId>jackson-annotations</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
</dependencies>
```

---

### 4. **Gestión de Plugins**

Los **plugins** son fundamentales en Maven, ya que permiten personalizar el ciclo de vida del proyecto y añadir funcionalidades adicionales, como compilar código, ejecutar pruebas o generar documentación.

---

#### 4.1 Declaración y configuración de plugins

Los plugins se declaran dentro del elemento `<build>` del `pom.xml`. Un ejemplo básico sería:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.11.0</version>
            <configuration>
                <source>17</source> <!-- Versión del código fuente -->
                <target>17</target> <!-- Versión de salida del bytecode -->
            </configuration>
        </plugin>
    </plugins>
</build>
```

---

#### 4.2 Plugins obligatorios y comunes

Maven tiene algunos plugins esenciales que vienen integrados o son ampliamente utilizados en los proyectos.

##### **Plugins obligatorios (internos de Maven):**
1. **`maven-compiler-plugin`**  
   Compila el código fuente.
   ```xml
   <plugin>
       <groupId>org.apache.maven.plugins</groupId>
       <artifactId>maven-compiler-plugin</artifactId>
       <version>3.11.0</version>
       <configuration>
           <source>17</source>
           <target>17</target>
       </configuration>
   </plugin>
   ```

2. **`maven-surefire-plugin`**  
   Ejecuta las pruebas unitarias.
   ```xml
   <plugin>
       <groupId>org.apache.maven.plugins</groupId>
       <artifactId>maven-surefire-plugin</artifactId>
       <version>3.1.2</version>
   </plugin>
   ```

3. **`maven-clean-plugin`**  
   Limpia el directorio `target` antes de construir el proyecto.
   ```xml
   <plugin>
       <groupId>org.apache.maven.plugins</groupId>
       <artifactId>maven-clean-plugin</artifactId>
       <version>3.3.1</version>
   </plugin>
   ```

##### **Plugins comunes:**
1. **`maven-jar-plugin`**  
   Empaqueta el código como un archivo JAR.
   ```xml
   <plugin>
       <groupId>org.apache.maven.plugins</groupId>
       <artifactId>maven-jar-plugin</artifactId>
       <version>3.3.0</version>
   </plugin>
   ```

2. **`maven-deploy-plugin`**  
   Sube el artefacto generado a un repositorio remoto.
   ```xml
   <plugin>
       <groupId>org.apache.maven.plugins</groupId>
       <artifactId>maven-deploy-plugin</artifactId>
       <version>3.1.1</version>
   </plugin>
   ```

3. **`maven-shade-plugin`**  
   Combina múltiples dependencias en un solo archivo JAR (fat/uber JAR).
   ```xml
   <plugin>
       <groupId>org.apache.maven.plugins</groupId>
       <artifactId>maven-shade-plugin</artifactId>
       <version>3.4.0</version>
       <executions>
           <execution>
               <phase>package</phase>
               <goals>
                   <goal>shade</goal>
               </goals>
           </execution>
       </executions>
   </plugin>
   ```

---

#### 4.3 Fases del ciclo de vida y su relación con los plugins

Maven organiza su proceso de construcción en **fases del ciclo de vida**. Los plugins se configuran para ejecutarse en fases específicas. Algunas fases importantes:

1. **`validate`:** Verifica que el proyecto está correctamente configurado.
2. **`compile`:** Compila el código fuente.
3. **`test`:** Ejecuta pruebas unitarias.
4. **`package`:** Empaqueta el código compilado en un JAR, WAR u otro formato.
5. **`install`:** Instala el artefacto en el repositorio local.
6. **`deploy`:** Publica el artefacto en un repositorio remoto.

**Ejemplo:** Configurar un plugin para ejecutarse en la fase `package`:
```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-jar-plugin</artifactId>
            <version>3.3.0</version>
            <executions>
                <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>jar</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

---

#### 4.4 Configuración avanzada de plugins

Los plugins también pueden aceptar configuraciones específicas según las necesidades del proyecto. Por ejemplo:

**Configurar el plugin `maven-surefire-plugin` para usar un marco de pruebas específico:**
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>3.1.2</version>
    <configuration>
        <includes>
            <include>**/*Test.java</include>
        </includes>
        <excludes>
            <exclude>**/IntegrationTest.java</exclude>
        </excludes>
    </configuration>
</plugin>
```

**Personalizar el JAR con `maven-jar-plugin`:**
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-jar-plugin</artifactId>
    <version>3.3.0</version>
    <configuration>
        <finalName>mi-aplicacion</finalName>
    </configuration>
</plugin>
```

---

#### Ejemplo práctico de múltiples plugins

```xml
<build>
    <plugins>
        <!-- Plugin para compilar -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.11.0</version>
            <configuration>
                <source>17</source>
                <target>17</target>
            </configuration>
        </plugin>

        <!-- Plugin para empaquetar -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-jar-plugin</artifactId>
            <version>3.3.0</version>
            <configuration>
                <finalName>mi-aplicacion</finalName>
            </configuration>
        </plugin>

        <!-- Plugin para pruebas -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>3.1.2</version>
        </plugin>
    </plugins>
</build>
```

#### Guía sobre los Plugins de Maven y Configuración de Build

##### **¿Qué son los Plugins en Maven?**
Los plugins en Maven son componentes que amplían la funcionalidad del proceso de construcción (build) de un proyecto. Estos plugins son ejecutados durante las fases del ciclo de vida de Maven y realizan tareas específicas, como compilar código, ejecutar tests, generar documentación o empaquetar el proyecto.

##### **Tipos de Plugins en Maven**
1. **Plugins Integrados**: Maven ya incluye algunos plugins esenciales por defecto, como el `maven-compiler-plugin` para compilar código o el `maven-surefire-plugin` para ejecutar tests.
2. **Plugins Personalizados**: Son plugins externos que puedes añadir en tu archivo `pom.xml` para tareas específicas, como generar reportes o manejar configuraciones avanzadas.

##### **¿Es necesario añadir todos los plugins al `pom.xml`?**
No, **no es necesario añadir todos los plugins** al `pom.xml`. Maven viene con configuraciones predeterminadas para los plugins más comunes. Esto significa que si no especificas un plugin (como el `maven-compiler-plugin`), Maven usará su versión predeterminada.

**Cuándo añadir plugins:**
- Si necesitas cambiar la configuración predeterminada de un plugin.
- Si necesitas ejecutar tareas específicas que no están incluidas en los plugins predeterminados.

Por ejemplo:
- Si quieres usar una versión específica de Java para compilar tu proyecto, puedes personalizar el `maven-compiler-plugin`.
- Si necesitas generar un archivo JAR o WAR de forma específica, puedes configurar el `maven-jar-plugin`.

##### **¿Qué es la configuración de `build` en Maven?**
La sección `<build>` en `pom.xml` es donde defines configuraciones relacionadas con el proceso de construcción de tu proyecto. Esto incluye:
1. **Plugins**: Define y configura los plugins que deseas usar.
2. **Directory Settings**: Configura directorios de salida como `target`.
3. **Resources**: Define recursos que deben incluirse en el paquete del proyecto.

###### **Ejemplo básico de configuración de `<build>`**
```xml
<build>
    <plugins>
        <!-- Plugin para compilar el código fuente -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.10.1</version>
            <configuration>
                <source>11</source> <!-- Versión de Java fuente -->
                <target>11</target> <!-- Versión de Java objetivo -->
            </configuration>
        </plugin>

        <!-- Plugin para empaquetar un archivo JAR -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-jar-plugin</artifactId>
            <version>3.2.2</version>
            <configuration>
                <archive>
                    <manifest>
                        <mainClass>com.ejemplo.Main</mainClass>
                    </manifest>
                </archive>
            </configuration>
        </plugin>
    </plugins>
</build>
```

##### **¿Por qué es importante la configuración en `build`?**
La configuración en `<build>` te permite personalizar y controlar cómo se realiza el ciclo de vida del proyecto. Algunos beneficios son:
- Personalizar el proceso de compilación y empaquetado.
- Añadir tareas específicas para la fase de despliegue o desarrollo.
- Optimizar los procesos automatizados en entornos de integración continua.

---

##### **Consejos Prácticos**
1. **Reutiliza configuraciones comunes**: Si tienes varios proyectos con configuraciones similares, considera usar un **parent POM** para centralizar los plugins y configuraciones.
2. **No abuses de los plugins**: Añade solo los plugins necesarios. Una configuración innecesaria puede hacer tu proyecto más complejo.
3. **Consulta la documentación de Maven**: Los plugins tienen configuraciones avanzadas; revisa siempre la documentación oficial para entender sus capacidades.

---


### 5. **Perfiles de Construcción**

Los **perfiles de construcción** en Maven permiten definir configuraciones específicas que pueden activarse según el entorno, como desarrollo, pruebas o producción. Esto es útil para ajustar propiedades, dependencias y configuraciones de plugins sin modificar directamente el archivo `pom.xml` principal.

---

#### 5.1 ¿Qué es un perfil?

Un perfil es una sección dentro del `pom.xml` que permite personalizar:
- Dependencias
- Propiedades
- Plugins
- Configuraciones del `build`

Se declara dentro de la etiqueta `<profiles>`:

```xml
<profiles>
    <profile>
        <id>desarrollo</id>
        <properties>
            <entorno>desarrollo</entorno>
        </properties>
    </profile>
</profiles>
```

El perfil con `id="desarrollo"` puede activarse cuando se ejecute Maven, permitiendo usar configuraciones específicas para ese entorno.

---

#### 5.2 Creación y uso de perfiles

##### Ejemplo básico de perfiles:
```xml
<profiles>
    <profile>
        <id>desarrollo</id>
        <properties>
            <entorno>desarrollo</entorno>
        </properties>
    </profile>

    <profile>
        <id>produccion</id>
        <properties>
            <entorno>produccion</entorno>
        </properties>
    </profile>
</profiles>
```

##### Activación de un perfil en línea de comandos:
Puedes activar un perfil con el parámetro `-P` al ejecutar Maven:

```bash
mvn clean install -Pdesarrollo
```

---

#### 5.3 Configuraciones comunes dentro de un perfil

Un perfil puede incluir configuraciones específicas para:
- Dependencias
- Plugins
- Repositorios
- Propiedades

##### Ejemplo: Dependencias específicas por perfil
```xml
<profiles>
    <profile>
        <id>desarrollo</id>
        <dependencies>
            <dependency>
                <groupId>org.hsqldb</groupId>
                <artifactId>hsqldb</artifactId>
                <version>2.7.1</version>
            </dependency>
        </dependencies>
    </profile>
    <profile>
        <id>produccion</id>
        <dependencies>
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>8.1.0</version>
            </dependency>
        </dependencies>
    </profile>
</profiles>
```

En este caso:
- En desarrollo, Maven usará una base de datos embebida (`hsqldb`).
- En producción, usará MySQL.

##### Ejemplo: Configuración de plugins según el perfil
```xml
<profiles>
    <profile>
        <id>desarrollo</id>
        <build>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <configuration>
                        <debug>true</debug>
                    </configuration>
                </plugin>
            </plugins>
        </build>
    </profile>
    <profile>
        <id>produccion</id>
        <build>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <configuration>
                        <optimize>true</optimize>
                    </configuration>
                </plugin>
            </plugins>
        </build>
    </profile>
</profiles>
```

---

#### 5.4 Activación de perfiles

Los perfiles pueden activarse automáticamente con ciertas condiciones:

##### Por propiedades del sistema:
```xml
<activation>
    <property>
        <name>entorno</name>
        <value>produccion</value>
    </property>
</activation>
```
Ejecutar Maven con la propiedad activa este perfil:
```bash
mvn clean install -Dentorno=produccion
```

##### Por sistema operativo:
```xml
<activation>
    <os>
        <name>Linux</name>
    </os>
</activation>
```
Este perfil se activa automáticamente en sistemas Linux.

##### Por existencia de un archivo:
```xml
<activation>
    <file>
        <exists>config/produccion.yml</exists>
    </file>
</activation>
```
El perfil se activa si el archivo `config/produccion.yml` existe.

##### Por un valor predeterminado:
Se puede activar un perfil por defecto si no se especifica otro:
```xml
<profiles>
    <profile>
        <id>default</id>
        <activation>
            <activeByDefault>true</activeByDefault>
        </activation>
    </profile>
</profiles>
```

---

#### 5.5 Ejemplo práctico de perfiles completos

```xml
<profiles>
    <profile>
        <id>desarrollo</id>
        <activation>
            <property>
                <name>entorno</name>
                <value>desarrollo</value>
            </property>
        </activation>
        <properties>
            <db.url>jdbc:h2:mem:testdb</db.url>
        </properties>
    </profile>

    <profile>
        <id>produccion</id>
        <activation>
            <property>
                <name>entorno</name>
                <value>produccion</value>
            </property>
        </activation>
        <properties>
            <db.url>jdbc:mysql://mi-servidor.com/prod</db.url>
        </properties>
    </profile>
</profiles>
```

Cuando se usa el perfil `desarrollo`, la base de datos será H2 en memoria. En `producción`, la base de datos será MySQL.

---


### 6. **Gestión de Propiedades**

Las **propiedades** en Maven son claves para parametrizar configuraciones y evitar la repetición de valores en el `pom.xml`. Esto permite que los proyectos sean más mantenibles, reutilizables y fáciles de personalizar en diferentes entornos.

---

#### 6.1 Declaración de propiedades en Maven

Las propiedades se definen en el bloque `<properties>` del `pom.xml`:

```xml
<properties>
    <java.version>17</java.version>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.plugin.version>3.11.0</maven.compiler.plugin.version>
</properties>
```

Estas propiedades se pueden usar en cualquier parte del `pom.xml` mediante la notación `${nombre.propiedad}`:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>${maven.compiler.plugin.version}</version>
            <configuration>
                <source>${java.version}</source>
                <target>${java.version}</target>
            </configuration>
        </plugin>
    </plugins>
</build>
```

---

#### 6.2 Propiedades predeterminadas de Maven

Maven proporciona propiedades predefinidas que se pueden utilizar directamente:

1. **Propiedades del proyecto**  
   Estas propiedades reflejan los valores configurados en el `pom.xml`:
   - `${project.groupId}`: El ID del grupo.
   - `${project.artifactId}`: El ID del artefacto.
   - `${project.version}`: La versión del proyecto.
   - `${project.build.directory}`: El directorio de compilación (por defecto, `target`).
   - `${project.build.outputDirectory}`: El directorio donde se colocan los archivos compilados.

2. **Propiedades del entorno**  
   Estas propiedades dependen del entorno en el que se ejecuta Maven:
   - `${user.home}`: El directorio del usuario.
   - `${java.home}`: El directorio del JDK en uso.
   - `${os.name}`: El sistema operativo.

---

#### 6.3 Sobrescritura de propiedades

Las propiedades declaradas en el bloque `<properties>` se pueden sobrescribir en tiempo de ejecución usando la línea de comandos:

```bash
mvn clean install -Djava.version=11
```

Esto permite, por ejemplo, compilar el proyecto con una versión diferente de Java sin modificar el archivo `pom.xml`.

---

#### 6.4 Uso de propiedades en perfiles

Las propiedades también pueden configurarse dentro de perfiles para adaptarse a diferentes entornos:

```xml
<profiles>
    <profile>
        <id>desarrollo</id>
        <properties>
            <db.url>jdbc:h2:mem:testdb</db.url>
            <db.username>dev_user</db.username>
            <db.password>dev_password</db.password>
        </properties>
    </profile>
    <profile>
        <id>produccion</id>
        <properties>
            <db.url>jdbc:mysql://prod.server/database</db.url>
            <db.username>prod_user</db.username>
            <db.password>prod_password</db.password>
        </properties>
    </profile>
</profiles>
```

Al activar el perfil correspondiente, las propiedades se actualizan dinámicamente.

---

#### 6.5 Propiedades definidas en archivos externos

Maven también permite cargar propiedades desde archivos externos usando el plugin `maven-antrun-plugin`:

1. Crea un archivo `config.properties` con las propiedades necesarias:
   ```properties
   db.url=jdbc:mysql://localhost:3306/mydb
   db.username=root
   db.password=secret
   ```

2. Configura el plugin en el `pom.xml`:
   ```xml
   <build>
       <plugins>
           <plugin>
               <groupId>org.apache.maven.plugins</groupId>
               <artifactId>maven-antrun-plugin</artifactId>
               <version>3.1.0</version>
               <executions>
                   <execution>
                       <phase>initialize</phase>
                       <configuration>
                           <tasks>
                               <property file="config.properties" />
                           </tasks>
                       </configuration>
                   </execution>
               </executions>
           </plugin>
       </plugins>
   </build>
   ```

3. Usa las propiedades en el `pom.xml`:
   ```xml
   <properties>
       <db.url>${db.url}</db.url>
       <db.username>${db.username}</db.username>
       <db.password>${db.password}</db.password>
   </properties>
   ```

---

#### 6.6 Ejemplo práctico de propiedades

**Estructura del `pom.xml`:**
```xml
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>mi-proyecto</artifactId>
    <version>1.0.0</version>

    <properties>
        <java.version>17</java.version>
        <spring.version>5.3.29</spring.version>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring.version}</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.11.0</version>
                <configuration>
                    <source>${java.version}</source>
                    <target>${java.version}</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

---

#### 6.7 Beneficios de usar propiedades

- **Reutilización:** Evita la duplicación de valores comunes como versiones o configuraciones.
- **Mantenibilidad:** Facilita la actualización de valores en un solo lugar.
- **Personalización:** Adapta el proyecto a diferentes entornos mediante perfiles y sobrescritura en tiempo de ejecución.

---
