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

### 7. **Herencia y Módulos (Multi-module Projects)**

Maven permite estructurar proyectos complejos en una jerarquía de módulos, utilizando la **herencia** y la **estructura de multi-módulos**. Esto facilita la reutilización de configuraciones comunes y el manejo centralizado de dependencias, versiones y configuraciones de plugins.

---

#### 7.1 Concepto de herencia en Maven

La **herencia** en Maven permite que un proyecto "hijo" herede configuraciones del `pom.xml` de un proyecto "padre". Esto se define utilizando el bloque `<parent>` en el `pom.xml` del proyecto hijo:

```xml
<parent>
    <groupId>com.example</groupId>
    <artifactId>proyecto-padre</artifactId>
    <version>1.0.0</version>
    <relativePath>../proyecto-padre/pom.xml</relativePath> <!-- Ruta relativa al pom.xml del padre -->
</parent>
```

##### Configuraciones que se heredan:
- Dependencias
- Plugins
- Propiedades
- Repositorios
- Configuraciones de compilación (`<build>`)

##### Ejemplo:
**Proyecto Padre (`pom.xml`):**
```xml
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>proyecto-padre</artifactId>
    <version>1.0.0</version>
    <packaging>pom</packaging>

    <properties>
        <java.version>17</java.version>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>5.3.29</version>
        </dependency>
    </dependencies>
</project>
```

**Proyecto Hijo (`pom.xml`):**
```xml
<project>
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>com.example</groupId>
        <artifactId>proyecto-padre</artifactId>
        <version>1.0.0</version>
    </parent>

    <artifactId>modulo-hijo</artifactId>
</project>
```

El proyecto hijo hereda:
- Dependencia a `spring-core`
- Propiedades comunes (`java.version`, `project.build.sourceEncoding`)

---

#### 7.2 Proyectos Multi-Módulo

Un proyecto multi-módulo organiza varios subproyectos (módulos) bajo un único proyecto "agregador". El proyecto agregador es responsable de construir todos los módulos.

##### Estructura de un proyecto multi-módulo
```
proyecto-raiz/
├── pom.xml           <-- Proyecto agregador
├── modulo1/
│   └── pom.xml       <-- Módulo 1
├── modulo2/
│   └── pom.xml       <-- Módulo 2
└── modulo3/
    └── pom.xml       <-- Módulo 3
```

##### Proyecto agregador (`pom.xml`):
El `pom.xml` del proyecto raíz debe tener un empaquetado de tipo `pom` y listar todos los módulos en el bloque `<modules>`:

```xml
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>proyecto-raiz</artifactId>
    <version>1.0.0</version>
    <packaging>pom</packaging>

    <modules>
        <module>modulo1</module>
        <module>modulo2</module>
        <module>modulo3</module>
    </modules>
</project>
```

##### Configuración de un módulo (`pom.xml`):
Cada módulo tiene su propio `pom.xml` y puede heredar del proyecto raíz:

```xml
<project>
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>com.example</groupId>
        <artifactId>proyecto-raiz</artifactId>
        <version>1.0.0</version>
    </parent>

    <artifactId>modulo1</artifactId>
</project>
```

---

#### 7.3 Construcción de proyectos multi-módulo

Cuando se ejecuta Maven en el proyecto raíz, automáticamente compila todos los módulos listados en `<modules>`:

```bash
mvn clean install
```

El orden de construcción de los módulos está determinado por sus dependencias. Maven asegura que un módulo requerido por otro se construya primero.

---

#### 7.4 Ventajas de usar multi-módulos

- **Centralización:** Configuraciones comunes se definen en el proyecto raíz.
- **Reutilización:** Múltiples módulos pueden compartir dependencias y configuraciones.
- **Mantenimiento:** Facilita la actualización de versiones y propiedades.
- **Separación de responsabilidades:** Cada módulo puede representar una funcionalidad o capa específica (ej., modelo, servicios, frontend).

---

#### 7.5 Ejemplo práctico de un proyecto multi-módulo

**Estructura del proyecto:**
```
proyecto-empresa/
├── pom.xml           <-- Proyecto agregador
├── servicio-cliente/
│   └── pom.xml       <-- Módulo 1
├── servicio-pedido/
│   └── pom.xml       <-- Módulo 2
└── servicio-producto/
    └── pom.xml       <-- Módulo 3
```

**Proyecto raíz (`pom.xml`):**
```xml
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.empresa</groupId>
    <artifactId>proyecto-empresa</artifactId>
    <version>1.0.0</version>
    <packaging>pom</packaging>

    <modules>
        <module>servicio-cliente</module>
        <module>servicio-pedido</module>
        <module>servicio-producto</module>
    </modules>
</project>
```

**Módulo `servicio-cliente` (`pom.xml`):**
```xml
<project>
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>com.empresa</groupId>
        <artifactId>proyecto-empresa</artifactId>
        <version>1.0.0</version>
    </parent>

    <artifactId>servicio-cliente</artifactId>

    <dependencies>
        <dependency>
            <groupId>com.empresa</groupId>
            <artifactId>servicio-producto</artifactId>
            <version>1.0.0</version>
        </dependency>
    </dependencies>
</project>
```

Aquí, el módulo `servicio-cliente` depende de `servicio-producto`.

---

#### 7.6 Mejoras avanzadas en proyectos multi-módulo

1. **Versión centralizada con `dependencyManagement`:**  
   Las versiones de dependencias se pueden centralizar en el proyecto raíz:
   ```xml
   <dependencyManagement>
       <dependencies>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-core</artifactId>
               <version>5.3.29</version>
           </dependency>
       </dependencies>
   </dependencyManagement>
   ```

2. **Perfiles para compilación parcial:**  
   Puedes construir solo módulos específicos usando perfiles:
   ```bash
   mvn clean install -Pmodulo1
   ```

---

### 8. **Configuraciones Avanzadas**

Maven ofrece una variedad de configuraciones avanzadas que permiten personalizar y optimizar el comportamiento del proceso de construcción. Estas configuraciones abarcan desde la manipulación de dependencias hasta la personalización del flujo de construcción mediante plugins y perfiles.

---

#### 8.1 Configuración de Dependencias Avanzadas

##### Exclusión de dependencias transitivas
A veces, las dependencias traen otras dependencias transitivas que no son necesarias o generan conflictos. Puedes excluirlas utilizando el bloque `<exclusions>`:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
    <version>3.1.0</version>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-logging</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

##### Dependencias con alcance (scope)
El alcance de las dependencias define cuándo y cómo se usan durante el ciclo de vida de Maven:
- **`compile` (predeterminado):** Disponible en todas las fases.
- **`provided`:** Similar a `compile`, pero se espera que el entorno las proporcione (ej., `servlet-api` en servidores web).
- **`runtime`:** Necesaria solo en tiempo de ejecución (ej., controladores JDBC).
- **`test`:** Solo para la fase de pruebas.

Ejemplo:
```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>4.0.1</version>
    <scope>provided</scope>
</dependency>
```

##### Sistema de dependencias (system scope)
Las dependencias del sistema deben proporcionarse explícitamente con una ruta local:
```xml
<dependency>
    <groupId>com.example</groupId>
    <artifactId>mi-libreria</artifactId>
    <version>1.0.0</version>
    <scope>system</scope>
    <systemPath>${project.basedir}/libs/mi-libreria-1.0.0.jar</systemPath>
</dependency>
```

---

#### 8.2 Configuración Avanzada de Plugins

##### Personalización de plugins
Puedes configurar los plugins de Maven para adaptarlos a tus necesidades. Por ejemplo, configurando el plugin de compilación:

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
                <showWarnings>true</showWarnings>
            </configuration>
        </plugin>
    </plugins>
</build>
```

##### Fase de ejecución personalizada
Puedes configurar plugins para que se ejecuten en fases específicas del ciclo de vida:
```xml
<execution>
    <id>etapa-personalizada</id>
    <phase>process-resources</phase>
    <goals>
        <goal>goal-especifico</goal>
    </goals>
</execution>
```

##### Plugins con múltiples configuraciones
Puedes configurar el mismo plugin varias veces usando identificadores únicos:
```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-resources-plugin</artifactId>
            <version>3.3.1</version>
            <executions>
                <execution>
                    <id>copiar-imagenes</id>
                    <phase>process-resources</phase>
                    <goals>
                        <goal>copy-resources</goal>
                    </goals>
                    <configuration>
                        <outputDirectory>${project.build.directory}/images</outputDirectory>
                        <resources>
                            <resource>
                                <directory>src/main/resources/images</directory>
                            </resource>
                        </resources>
                    </configuration>
                </execution>
                <execution>
                    <id>copiar-configuracion</id>
                    <phase>process-resources</phase>
                    <goals>
                        <goal>copy-resources</goal>
                    </goals>
                    <configuration>
                        <outputDirectory>${project.build.directory}/config</outputDirectory>
                        <resources>
                            <resource>
                                <directory>src/main/resources/config</directory>
                            </resource>
                        </resources>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

---

#### 8.3 Uso Avanzado de Perfiles

Los **perfiles** permiten adaptar el comportamiento de Maven para diferentes entornos o configuraciones. Los perfiles avanzados pueden activarse automáticamente o mediante condiciones personalizadas.

##### Perfiles activados por propiedades
Puedes activar un perfil basado en una propiedad pasada por línea de comandos:
```xml
<profiles>
    <profile>
        <id>produccion</id>
        <activation>
            <property>
                <name>entorno</name>
                <value>produccion</value>
            </property>
        </activation>
        <properties>
            <db.url>jdbc:mysql://prod.server/database</db.url>
        </properties>
    </profile>
</profiles>
```

Activación:
```bash
mvn clean install -Dentorno=produccion
```

##### Perfiles activados por sistema operativo
Puedes activar un perfil dependiendo del sistema operativo:
```xml
<profiles>
    <profile>
        <id>windows-profile</id>
        <activation>
            <os>
                <family>Windows</family>
            </os>
        </activation>
        <properties>
            <path.separator>;</path.separator>
        </properties>
    </profile>
</profiles>
```

##### Perfiles activados por JDK
Puedes activar un perfil basado en la versión del JDK:
```xml
<profiles>
    <profile>
        <id>jdk-11</id>
        <activation>
            <jdk>11</jdk>
        </activation>
    </profile>
</profiles>
```

---

#### 8.4 Configuración de Builds Paralelos

Maven permite la ejecución de builds en paralelo para mejorar el rendimiento en proyectos grandes con múltiples módulos:

```bash
mvn clean install -T 4
```

El argumento `-T` indica el número de hilos (`4` en este caso). También puedes usar un porcentaje para asignar recursos relativos a los núcleos de CPU disponibles:
```bash
mvn clean install -T 1C
```

---

#### 8.5 Configuración de Repositorios Personalizados

Puedes configurar repositorios adicionales (internos o externos) para descargar dependencias:

```xml
<repositories>
    <repository>
        <id>repositorio-interno</id>
        <url>https://artifacts.example.com/repository/maven-releases/</url>
    </repository>
</repositories>
```

También puedes configurar repositorios específicos para snapshots:
```xml
<snapshots>
    <enabled>true</enabled>
</snapshots>
```

---

#### 8.6 Logs y Depuración

Puedes ajustar el nivel de verbosidad de Maven para depurar problemas:
```bash
mvn clean install -X  # Modo de depuración
mvn clean install -q  # Modo silencioso
```

---

#### 8.7 Ejemplo Completo de Configuración Avanzada

```xml
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>proyecto-avanzado</artifactId>
    <version>1.0.0</version>

    <properties>
        <java.version>17</java.version>
    </properties>

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

    <profiles>
        <profile>
            <id>produccion</id>
            <activation>
                <property>
                    <name>entorno</name>
                    <value>produccion</value>
                </property>
            </activation>
            <properties>
                <db.url>jdbc:mysql://prod.server/database</db.url>
            </properties>
        </profile>
    </profiles>
</project>
```

---

### 9. **Extensiones y Personalización**

Maven se puede extender y personalizar de diversas maneras para adaptarse a las necesidades específicas de los proyectos. Esta sección cubre el uso de **extensiones de Maven**, el archivo `settings.xml`, y cómo personalizar los ciclos de vida con plugins.

---

#### 9.1 Extensiones de Maven

Las extensiones de Maven son componentes que amplían las capacidades de Maven. Se definen en un archivo `pom.xml` y pueden modificar cómo funciona Maven en términos de construcción, manejo de dependencias o integración con herramientas externas.

##### ¿Qué son las extensiones?
- Son bibliotecas o herramientas que modifican o extienden el comportamiento predeterminado de Maven.
- Ejemplos comunes: controladores para repositorios privados, integración con sistemas CI/CD, herramientas de análisis como JaCoCo o SonarQube.

##### Cómo agregar una extensión
Las extensiones se declaran en el bloque `<build>` dentro de `<extensions>`:

```xml
<build>
    <extensions>
        <extension>
            <groupId>org.apache.maven.wagon</groupId>
            <artifactId>wagon-ssh</artifactId>
            <version>3.5.2</version>
        </extension>
    </extensions>
</build>
```

Este ejemplo agrega soporte para la transferencia de archivos mediante SSH.

##### Ejemplo práctico:
Integración con **Takari Lifecycle**, que mejora el rendimiento del build:
```xml
<build>
    <extensions>
        <extension>
            <groupId>io.takari.maven.plugins</groupId>
            <artifactId>takari-lifecycle-extension</artifactId>
            <version>1.13.0</version>
        </extension>
    </extensions>
</build>
```

---

#### 9.2 Archivos `settings.xml` y su interacción con `pom.xml`

El archivo `settings.xml` proporciona configuraciones específicas del entorno del usuario, como credenciales, repositorios, proxies, y perfiles. Este archivo interactúa con el `pom.xml` para definir configuraciones globales o específicas del entorno.

##### Ubicación del archivo `settings.xml`
- **Global:** `<MAVEN_HOME>/conf/settings.xml` (aplica a todos los usuarios).
- **Usuario:** `~/.m2/settings.xml` (aplica al usuario actual).

##### Ejemplo de un archivo `settings.xml`:
```xml
<settings>
    <mirrors>
        <mirror>
            <id>central-mirror</id>
            <mirrorOf>central</mirrorOf>
            <url>https://repo.example.com/maven2</url>
        </mirror>
    </mirrors>
    <servers>
        <server>
            <id>repositorio-privado</id>
            <username>usuario</username>
            <password>contraseña</password>
        </server>
    </servers>
    <profiles>
        <profile>
            <id>produccion</id>
            <properties>
                <db.url>jdbc:mysql://prod.server/database</db.url>
            </properties>
        </profile>
    </profiles>
    <activeProfiles>
        <activeProfile>produccion</activeProfile>
    </activeProfiles>
</settings>
```

##### Interacción con el `pom.xml`
1. **Repositorios personalizados:**  
   Los repositorios definidos en `settings.xml` se combinan con los del `pom.xml`.
   - Repositorio en `settings.xml`:
     ```xml
     <repositories>
         <repository>
             <id>repositorio-global</id>
             <url>https://global.repo.com</url>
         </repository>
     </repositories>
     ```
   - Repositorio en `pom.xml`:
     ```xml
     <repositories>
         <repository>
             <id>repositorio-local</id>
             <url>https://local.repo.com</url>
         </repository>
     </repositories>
     ```
     Resultado: Ambos repositorios estarán disponibles.

2. **Perfiles:**  
   Los perfiles definidos en `settings.xml` pueden ser activados explícitamente en el `pom.xml` o mediante la línea de comandos.

---

#### 9.3 Personalización de Ciclos de Vida con Plugins

Maven permite personalizar los ciclos de vida predeterminados mediante la configuración de plugins para añadir tareas específicas o reemplazar fases.

##### Fases del ciclo de vida
Cada ciclo de vida en Maven tiene fases, y estas pueden ser personalizadas al asociarles plugins o metas específicas.

###### Ejemplo básico:
Añadir una tarea de verificación estática en la fase `verify`:
```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-checkstyle-plugin</artifactId>
            <version>3.2.0</version>
            <executions>
                <execution>
                    <phase>verify</phase>
                    <goals>
                        <goal>check</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

###### Ejemplo con una tarea personalizada:
Usando el **plugin `exec-maven-plugin`** para ejecutar un script en la fase `validate`:
```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>3.1.0</version>
            <executions>
                <execution>
                    <id>pre-validacion</id>
                    <phase>validate</phase>
                    <goals>
                        <goal>exec</goal>
                    </goals>
                    <configuration>
                        <executable>echo</executable>
                        <arguments>
                            <argument>Iniciando validación del proyecto...</argument>
                        </arguments>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

---

#### 9.4 Ejemplo Completo de Configuración Avanzada con Extensiones y Personalización

```xml
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>proyecto-personalizado</artifactId>
    <version>1.0.0</version>

    <build>
        <extensions>
            <!-- Extensión para soportar SSH en repositorios -->
            <extension>
                <groupId>org.apache.maven.wagon</groupId>
                <artifactId>wagon-ssh</artifactId>
                <version>3.5.2</version>
            </extension>
        </extensions>

        <plugins>
            <!-- Plugin para análisis estático -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-checkstyle-plugin</artifactId>
                <version>3.2.0</version>
                <executions>
                    <execution>
                        <phase>verify</phase>
                        <goals>
                            <goal>check</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>

            <!-- Plugin para ejecución de scripts -->
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>exec-maven-plugin</artifactId>
                <version>3.1.0</version>
                <executions>
                    <execution>
                        <phase>validate</phase>
                        <goals>
                            <goal>exec</goal>
                        </goals>
                        <configuration>
                            <executable>echo</executable>
                            <arguments>
                                <argument>Validando configuración...</argument>
                            </arguments>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>

    <profiles>
        <profile>
            <id>produccion</id>
            <activation>
                <property>
                    <name>entorno</name>
                    <value>produccion</value>
                </property>
            </activation>
            <properties>
                <db.url>jdbc:mysql://prod.server/database</db.url>
            </properties>
        </profile>
    </profiles>
</project>
```

---
### Profundización en el archivo `settings.xml`

El archivo `settings.xml` de Maven es un componente crucial para personalizar la configuración de Maven en entornos específicos. Se utiliza para definir configuraciones globales o específicas de un usuario sin modificar el `pom.xml` del proyecto. Aquí exploraremos más a fondo su estructura, casos de uso y su interacción con el `pom.xml`.

---

#### 1. **Ubicación y Prioridad del Archivo**

- **Archivo global (`$MAVEN_HOME/conf/settings.xml`):**
  - Este archivo es compartido por todos los usuarios en una máquina.
  - Define configuraciones generales que afectan a todos los proyectos Maven ejecutados en ese entorno.

- **Archivo de usuario (`~/.m2/settings.xml`):**
  - Este archivo es específico para un usuario.
  - Tiene prioridad sobre el archivo global. Si hay configuraciones conflictivas, el archivo de usuario sobrescribe al archivo global.

**Nota:** Si ambos archivos están presentes, Maven combina sus configuraciones, donde el archivo de usuario tiene más peso.

---

#### 2. **Estructura del Archivo `settings.xml`**

El `settings.xml` sigue una estructura XML similar al `pom.xml` pero con un propósito diferente. Aquí tienes una plantilla básica:

```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                              https://maven.apache.org/xsd/settings-1.0.0.xsd">
    
    <mirrors>...</mirrors>
    <servers>...</servers>
    <profiles>...</profiles>
    <activeProfiles>...</activeProfiles>

</settings>
```

Cada sección tiene un propósito específico, que se describe a continuación.

---

#### 3. **Secciones del Archivo `settings.xml`**

##### **3.1 Mirrors**
Los mirrors se utilizan para redirigir las solicitudes de Maven a un repositorio alternativo, como un repositorio privado o una réplica de Maven Central.

```xml
<mirrors>
    <mirror>
        <id>central-mirror</id>
        <mirrorOf>central</mirrorOf>
        <url>https://mirror.example.com/repository/maven-central/</url>
    </mirror>
</mirrors>
```

- **`id`:** Identificador único del mirror.
- **`mirrorOf`:** Indica qué repositorio está siendo replicado (`central`, `*` para todos, o un ID específico).
- **`url`:** Dirección del mirror.

##### **3.2 Servers**
Define credenciales para autenticar conexiones a repositorios, servidores de despliegue o cualquier otra herramienta que requiera autenticación.

```xml
<servers>
    <server>
        <id>repositorio-privado</id>
        <username>mi_usuario</username>
        <password>mi_contraseña</password>
    </server>
</servers>
```

- **`id`:** Debe coincidir con el ID definido en un repositorio del `pom.xml`.
- **`username`:** Usuario para autenticar.
- **`password`:** Contraseña correspondiente.

**Ejemplo de uso con repositorios en el `pom.xml`:**
```xml
<repositories>
    <repository>
        <id>repositorio-privado</id>
        <url>https://repositorio.privado.com/maven2</url>
    </repository>
</repositories>
```

##### **3.3 Profiles**
Los perfiles en el `settings.xml` funcionan igual que en el `pom.xml` pero están orientados al entorno del usuario. Estos perfiles permiten configurar propiedades, repositorios, y otras configuraciones específicas del entorno.

```xml
<profiles>
    <profile>
        <id>desarrollo</id>
        <properties>
            <env>desarrollo</env>
        </properties>
        <repositories>
            <repository>
                <id>dev-repo</id>
                <url>https://repositorio.dev.com/maven2</url>
            </repository>
        </repositories>
    </profile>
</profiles>
```

##### **3.4 ActiveProfiles**
Define qué perfiles deben estar activos por defecto.

```xml
<activeProfiles>
    <activeProfile>desarrollo</activeProfile>
</activeProfiles>
```

**Nota:** También puedes activar perfiles usando la línea de comandos:
```bash
mvn clean install -P desarrollo
```

---

#### 4. **Casos de Uso Común del `settings.xml`**

##### **4.1 Configuración de un repositorio corporativo**
Muchas empresas utilizan repositorios privados (como Nexus o Artifactory) para gestionar dependencias internas.

```xml
<settings>
    <mirrors>
        <mirror>
            <id>corporativo</id>
            <mirrorOf>*</mirrorOf>
            <url>https://artifacts.empresa.com/repository/maven-central</url>
        </mirror>
    </mirrors>
</settings>
```

Este mirror redirige todas las solicitudes (`*`) al repositorio corporativo.

##### **4.2 Configuración de credenciales de despliegue**
Para publicar artefactos en un repositorio privado:

```xml
<servers>
    <server>
        <id>repositorio-privado</id>
        <username>deploy_user</username>
        <password>deploy_password</password>
    </server>
</servers>
```

Configúralo en el `distributionManagement` del `pom.xml`:
```xml
<distributionManagement>
    <repository>
        <id>repositorio-privado</id>
        <url>https://repositorio.privado.com/releases</url>
    </repository>
</distributionManagement>
```

##### **4.3 Configuración de proxies**
Si estás detrás de un proxy corporativo, puedes configurarlo en el `settings.xml`:

```xml
<proxies>
    <proxy>
        <id>proxy-corporativo</id>
        <active>true</active>
        <protocol>http</protocol>
        <host>proxy.empresa.com</host>
        <port>8080</port>
        <username>usuario</username>
        <password>contraseña</password>
    </proxy>
</proxies>
```

---

#### 5. **Buenas Prácticas con `settings.xml`**

1. **Separación de responsabilidades:**
   - Usa el `settings.xml` para configuraciones específicas del usuario o del entorno (credenciales, proxies, etc.).
   - Mantén el `pom.xml` enfocado en la definición del proyecto y sus dependencias.

2. **Seguridad:**
   - Evita almacenar contraseñas en texto plano. Usa herramientas como [Maven Password Encryption](https://maven.apache.org/guides/mini/guide-encryption.html) para cifrar contraseñas:
     ```bash
     mvn --encrypt-password
     ```

3. **Uso de perfiles:**
   - Define perfiles en el `settings.xml` para configurar entornos como desarrollo, staging o producción.

4. **Versionado:**
   - No versiones el archivo `settings.xml`. Especifica en la documentación del proyecto qué configuraciones necesitan los desarrolladores en su entorno local.

---

### 10. **Ejemplos Completos y Casos Prácticos**

En esta sección se presentan ejemplos completos y prácticos de configuraciones `pom.xml` para distintos escenarios, desde un proyecto Java básico hasta un proyecto avanzado con múltiples módulos y gestión de dependencias internas.

---

#### **10.1 Proyecto Simple: Aplicación Java**

Este ejemplo muestra cómo configurar un proyecto básico en Java con Maven, sin configuraciones avanzadas. La estructura del proyecto incluye dependencias, plugins esenciales y perfiles básicos.

##### **Estructura del Proyecto:**
```
simple-java-app/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/example/App.java
│   └── test/
│       └── java/
│           └── com/example/AppTest.java
├── pom.xml
```

##### **Archivo `pom.xml`:**
```xml
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>simple-java-app</artifactId>
    <version>1.0.0</version>
    <packaging>jar</packaging>

    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <!-- Biblioteca para pruebas unitarias -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <!-- Plugin para compilar el código -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.11.0</version>
                <configuration>
                    <source>${maven.compiler.source}</source>
                    <target>${maven.compiler.target}</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

##### **Uso del Proyecto:**
1. **Compilar:**
   ```bash
   mvn compile
   ```
2. **Ejecutar pruebas:**
   ```bash
   mvn test
   ```
3. **Empaquetar:**
   ```bash
   mvn package
   ```

---

#### **10.2 Proyecto Avanzado: Spring Boot con Múltiples Módulos**

Este ejemplo utiliza un proyecto Spring Boot modularizado, en el que cada módulo tiene su propio propósito (por ejemplo, API, lógica de negocio, base de datos).

##### **Estructura del Proyecto:**
```
spring-boot-multi-module/
├── pom.xml (pom principal)
├── api/
│   └── pom.xml
├── service/
│   └── pom.xml
├── data/
│   └── pom.xml
```

##### **Archivo `pom.xml` Principal:**
```xml
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>spring-boot-multi-module</artifactId>
    <version>1.0.0</version>
    <packaging>pom</packaging>

    <modules>
        <module>api</module>
        <module>service</module>
        <module>data</module>
    </modules>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>3.1.0</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
</project>
```

##### **Archivo `pom.xml` del Módulo `api`:**
```xml
<project>
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>com.example</groupId>
        <artifactId>spring-boot-multi-module</artifactId>
        <version>1.0.0</version>
    </parent>

    <artifactId>api</artifactId>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>com.example</groupId>
            <artifactId>service</artifactId>
        </dependency>
    </dependencies>
</project>
```

##### **Archivo `pom.xml` del Módulo `service`:**
```xml
<project>
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>com.example</groupId>
        <artifactId>spring-boot-multi-module</artifactId>
        <version>1.0.0</version>
    </parent>

    <artifactId>service</artifactId>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>com.example</groupId>
            <artifactId>data</artifactId>
        </dependency>
    </dependencies>
</project>
```

##### **Archivo `pom.xml` del Módulo `data`:**
```xml
<project>
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>com.example</groupId>
        <artifactId>spring-boot-multi-module</artifactId>
        <version>1.0.0</version>
    </parent>

    <artifactId>data</artifactId>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>runtime</scope>
        </dependency>
    </dependencies>
</project>
```

---

#### **10.3 Gestión de Librerías Internas y Privadas**

En proyectos corporativos es común usar librerías internas almacenadas en un repositorio privado como Nexus o Artifactory.

##### **Ejemplo de Configuración para un Repositorio Privado:**
```xml
<repositories>
    <repository>
        <id>repositorio-privado</id>
        <url>https://repo.empresa.com/repository/maven-releases</url>
    </repository>
</repositories>

<dependencies>
    <dependency>
        <groupId>com.empresa.libreria</groupId>
        <artifactId>libreria-interna</artifactId>
        <version>1.2.3</version>
    </dependency>
</dependencies>
```

##### **Archivo `settings.xml` para Credenciales:**
```xml
<servers>
    <server>
        <id>repositorio-privado</id>
        <username>mi_usuario</username>
        <password>mi_contraseña</password>
    </server>
</servers>
```

---

### 11. **Resolución de Problemas Comunes**

En esta sección abordaremos algunos de los problemas más comunes que pueden surgir al trabajar con Maven y su archivo `pom.xml`, junto con estrategias y soluciones prácticas.

---

#### **11.1 Resolución de Conflictos de Dependencias**

Los conflictos de dependencias ocurren cuando diferentes versiones de la misma librería son requeridas por múltiples dependencias de un proyecto. Esto puede generar errores en tiempo de ejecución o compilación.

##### **Síntomas Comunes:**
- Clases ausentes o métodos no encontrados (NoSuchMethodError).
- Incompatibilidades de API.
- El proyecto compila pero falla al ejecutarse.

##### **Soluciones:**

1. **Usar el comando `mvn dependency:tree`:**
   Este comando permite visualizar las dependencias del proyecto y sus relaciones jerárquicas.
   ```bash
   mvn dependency:tree
   ```
   Esto ayuda a identificar qué dependencia está causando el conflicto.

2. **Forzar una versión específica usando `<dependencyManagement>`:**
   Define una versión única de la dependencia conflictiva en la sección `<dependencyManagement>` del `pom.xml`.
   ```xml
   <dependencyManagement>
       <dependencies>
           <dependency>
               <groupId>org.apache.commons</groupId>
               <artifactId>commons-lang3</artifactId>
               <version>3.12.0</version>
           </dependency>
       </dependencies>
   </dependencyManagement>
   ```

3. **Excluir dependencias transitivas conflictivas:**
   Si un módulo trae una versión no deseada de una librería, exclúyela explícitamente.
   ```xml
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-context</artifactId>
       <version>5.3.15</version>
       <exclusions>
           <exclusion>
               <groupId>org.apache.commons</groupId>
               <artifactId>commons-lang3</artifactId>
           </exclusion>
       </exclusions>
   </dependency>
   ```

4. **Usar el plugin `maven-enforcer-plugin`:**
   Este plugin ayuda a asegurar que no existan dependencias duplicadas o conflictos.
   ```xml
   <build>
       <plugins>
           <plugin>
               <groupId>org.apache.maven.plugins</groupId>
               <artifactId>maven-enforcer-plugin</artifactId>
               <version>3.2.1</version>
               <executions>
                   <execution>
                       <id>enforce-dependency-convergence</id>
                       <goals>
                           <goal>enforce</goal>
                       </goals>
                       <configuration>
                           <rules>
                               <DependencyConvergence />
                           </rules>
                       </configuration>
                   </execution>
               </executions>
           </plugin>
       </plugins>
   </build>
   ```

---

#### **11.2 Problemas con Plugins**

Los plugins son fundamentales en Maven, pero pueden generar problemas si no están configurados correctamente o si hay incompatibilidades con versiones de Maven o dependencias.

##### **Síntomas Comunes:**
- Errores como `Plugin execution not covered by lifecycle configuration`.
- Mensajes indicando que el plugin no se encuentra.
- Problemas al ejecutar comandos como `mvn clean`, `mvn install`, etc.

##### **Soluciones:**

1. **Verificar la configuración del plugin:**
   Revisa que el plugin esté correctamente configurado en el `pom.xml` con una versión específica.
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

2. **Actualizar el plugin:**
   Si hay un problema con la versión del plugin, actualiza a una versión compatible.
   ```bash
   mvn versions:display-plugin-updates
   ```

3. **Forzar la descarga de plugins:**
   Si el plugin no se descarga correctamente, fuerza su actualización.
   ```bash
   mvn clean install -U
   ```

4. **Solucionar `Plugin execution not covered by lifecycle configuration`:**
   Este error es común en IDEs como Eclipse. Agrega la configuración en la sección `<build>` del `pom.xml` para cubrir la ejecución del plugin.
   ```xml
   <pluginManagement>
       <plugins>
           <plugin>
               <groupId>org.apache.maven.plugins</groupId>
               <artifactId>maven-surefire-plugin</artifactId>
               <version>3.0.0-M7</version>
           </plugin>
       </plugins>
   </pluginManagement>
   ```

---

#### **11.3 Soluciones a Errores de Configuración del `pom.xml`**

Los errores de configuración en el `pom.xml` suelen surgir debido a configuraciones inválidas, dependencias mal declaradas o problemas con versiones.

##### **Errores Comunes y Soluciones:**

1. **`Cannot resolve symbol` o `Dependency not found`:**
   - Verifica que la dependencia esté disponible en el repositorio (como Maven Central).
   - Usa el comando `mvn dependency:resolve` para confirmar.
   - Si se trata de un repositorio privado, asegúrate de configurarlo en el `pom.xml` o en el `settings.xml`.

   ```xml
   <repositories>
       <repository>
           <id>repositorio-privado</id>
           <url>https://repositorio.privado.com/maven2</url>
       </repository>
   </repositories>
   ```

2. **Error de Ciclo de Vida:**
   - Mensaje típico: `Failed to execute goal ...`.
   - Solución: Verifica que el plugin configurado sea compatible con tu versión de Maven. Actualiza Maven si es necesario.

3. **Errores de Dependencias con Alcance Incorrecto:**
   Si una dependencia requerida no está disponible en tiempo de ejecución:
   - Cambia su `scope` a `compile` o `runtime` según corresponda.
   ```xml
   <dependency>
       <groupId>org.example</groupId>
       <artifactId>libreria</artifactId>
       <version>1.0.0</version>
       <scope>compile</scope>
   </dependency>
   ```

4. **Errores de Sintaxis en el `pom.xml`:**
   - Usa un editor con validación XML o ejecuta el comando:
     ```bash
     mvn validate
     ```
   - Esto ayuda a identificar errores estructurales.

---

#### **Consejos Generales para Resolver Problemas**

1. **Logs Detallados:**
   Ejecuta Maven con mayor detalle para identificar problemas exactos:
   ```bash
   mvn clean install -X
   ```

2. **Limpiar el Repositorio Local:**
   Si Maven usa artefactos corruptos, elimina el repositorio local:
   ```bash
   rm -rf ~/.m2/repository
   ```

3. **Actualizar Maven:**
   Asegúrate de usar una versión actualizada de Maven.

4. **Buscar en la Documentación y Comunidad:**
   Los errores de Maven suelen ser documentados ampliamente en foros como StackOverflow o en la [documentación oficial de Maven](https://maven.apache.org/).

---


# PREGUNTAS COMUNES:



## Cuando tienes un proyecto Maven que necesita obtener dependencias de un repositorio externo, puedes configurarlo tanto en el `pom.xml` como en el `settings.xml`. Sin embargo, cada archivo tiene un propósito específico:

### **1. Declaración del repositorio en el `pom.xml`**
El archivo `pom.xml` debe contener la declaración del repositorio, ya que este define el repositorio donde Maven buscará las dependencias. Por ejemplo:

```xml
<repositories>
    <repository>
        <id>mi-repositorio</id>
        <url>https://url-del-repositorio.com/maven2</url>
    </repository>
</repositories>
```

Esto es necesario para que Maven sepa dónde buscar las dependencias específicas.

---

### **2. Configuración de las credenciales en el `settings.xml`**
El archivo `settings.xml` (ubicado en `${MAVEN_HOME}/conf/settings.xml` o en el directorio del usuario `~/.m2/settings.xml`) es el lugar adecuado para almacenar propiedades sensibles, como las credenciales de inicio de sesión, ya que no deberían estar presentes en el `pom.xml` por razones de seguridad.

Aquí es donde configuras el `server` con las credenciales necesarias para acceder al repositorio:

```xml
<servers>
    <server>
        <id>mi-repositorio</id>
        <username>usuario</username>
        <password>contraseña</password>
    </server>
</servers>
```

El `<id>` aquí debe coincidir con el `<id>` del repositorio declarado en el `pom.xml`.

---

### **¿Por qué separar estas configuraciones?**
1. **Seguridad:** El `settings.xml` no se incluye en el control de versiones, lo que evita que las credenciales sean expuestas.
2. **Portabilidad:** Al declarar el repositorio en el `pom.xml`, todos los usuarios del proyecto podrán obtener las dependencias, pero cada usuario deberá configurar sus propias credenciales en su `settings.xml`.

---

### **Proceso Completo**
1. **En el `pom.xml`:** Declaras el repositorio y su ID.
2. **En el `settings.xml`:** Configuras las credenciales asociadas al ID del repositorio.

Con esto, cuando Maven necesita descargar una dependencia del repositorio, utiliza las credenciales configuradas en el `settings.xml`.

Pero si te lo preguntas, puedes declarar repositorios directamente en el archivo settings.xml, y Maven los usará automáticamente, aunque no estén explícitamente definidos en el pom.xml. Esto es útil cuando deseas mantener la configuración de los repositorios fuera del control de versiones o centralizarla en el settings.xml.

## MANAGEMENT:

En Maven, las secciones **`Management`** como `dependencyManagement`, `pluginManagement` y `distributionManagement` son configuraciones avanzadas que proporcionan una forma centralizada de definir versiones y comportamientos para dependencias, plugins y distribuciones. Estas secciones son particularmente útiles en proyectos multi-módulo o en entornos donde se desea mantener consistencia entre configuraciones.

A continuación, explicamos cada una de estas secciones en detalle:

---

### **1. dependencyManagement**
La sección `dependencyManagement` permite definir versiones y configuraciones comunes de dependencias que pueden ser heredadas o utilizadas por módulos secundarios en un proyecto multi-módulo. Esto ayuda a centralizar el control de las dependencias.

#### **¿Cómo Funciona?**
- No agrega automáticamente las dependencias a los módulos; solo define versiones y configuraciones que los módulos pueden usar.
- Los módulos secundarios deben declarar explícitamente las dependencias, pero no necesitan especificar la versión si ya está definida en `dependencyManagement`.

#### **Ejemplo:**
Archivo `pom.xml` del módulo principal:
```xml
<dependencyManagement>
    <dependencies>
        <!-- Definimos una versión fija de una librería -->
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.12.0</version>
        </dependency>
    </dependencies>
</dependencyManagement>
```

Archivo `pom.xml` de un módulo secundario:
```xml
<dependencies>
    <!-- Usamos la dependencia definida en dependencyManagement -->
    <dependency>
        <groupId>org.apache.commons</groupId>
        <artifactId>commons-lang3</artifactId>
    </dependency>
</dependencies>
```

**Ventaja:** Si en el futuro cambias la versión de `commons-lang3` en el `pom.xml` principal, los módulos secundarios no necesitan actualizar manualmente la versión.

---

### **2. pluginManagement**
La sección `pluginManagement` permite definir configuraciones y versiones predeterminadas para los plugins utilizados en el proyecto. Es similar a `dependencyManagement`, pero específicamente para plugins.

#### **¿Cómo Funciona?**
- No ejecuta ni configura automáticamente los plugins en los módulos secundarios.
- Los módulos secundarios deben declarar explícitamente los plugins, pero heredan las configuraciones definidas en `pluginManagement`.

#### **Ejemplo:**
Archivo `pom.xml` del módulo principal:
```xml
<build>
    <pluginManagement>
        <plugins>
            <!-- Definimos una versión específica del plugin de compilación -->
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
    </pluginManagement>
</build>
```

Archivo `pom.xml` de un módulo secundario:
```xml
<build>
    <plugins>
        <!-- Usamos el plugin definido en pluginManagement -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

**Ventaja:** Centraliza la gestión de versiones de plugins, evitando inconsistencias entre módulos.

---

### **3. distributionManagement**
La sección `distributionManagement` especifica cómo y dónde se distribuirán los artefactos de un proyecto, como repositorios remotos para snapshots y releases, además de configuraciones para la generación de sitios web de Maven.

#### **¿Cómo Funciona?**
- Define los repositorios donde se subirán las versiones `release` y `snapshot` del artefacto.
- Se utiliza cuando ejecutas comandos como `mvn deploy`.

#### **Ejemplo:**
```xml
<distributionManagement>
    <repository>
        <!-- Repositorio para versiones finales (releases) -->
        <id>releases-repo</id>
        <url>https://repo.miempresa.com/releases</url>
    </repository>
    <snapshotRepository>
        <!-- Repositorio para versiones de desarrollo (snapshots) -->
        <id>snapshots-repo</id>
        <url>https://repo.miempresa.com/snapshots</url>
    </snapshotRepository>
    <site>
        <!-- Configuración del sitio web generado con Maven -->
        <id>site-repo</id>
        <url>scp://miempresa.com/www/maven-site</url>
    </site>
</distributionManagement>
```

#### **Cuándo Usarlo:**
- **`repository`**: Para subir versiones finales (`release`) al repositorio remoto.
- **`snapshotRepository`**: Para subir versiones de desarrollo (`snapshot`) al repositorio remoto.
- **`site`**: Para generar y desplegar un sitio web con documentación del proyecto.

**Ventaja:** Facilita la distribución del proyecto en entornos corporativos o en sistemas de integración continua (CI/CD).

---

### **Comparación de las Secciones**

| Sección               | Propósito                                                                                      | Usado en...                   |
|-----------------------|-----------------------------------------------------------------------------------------------|--------------------------------|
| `dependencyManagement` | Centralizar versiones de dependencias para proyectos secundarios.                            | Proyectos multi-módulo         |
| `pluginManagement`     | Centralizar versiones y configuraciones de plugins.                                          | Proyectos multi-módulo         |
| `distributionManagement` | Especificar repositorios para distribuir artefactos y generar sitios web.                    | Deploy en repositorios remotos |

---

### **Casos de Uso Combinados**
Un proyecto multi-módulo puede usar `dependencyManagement` para unificar dependencias, `pluginManagement` para configurar plugins comunes y `distributionManagement` para distribuir artefactos al repositorio remoto. Esto simplifica la gestión del proyecto y evita redundancias.


El uso de las secciones `dependencyManagement`, `pluginManagement` y `distributionManagement` **no está limitado exclusivamente a proyectos multi-módulo**, pero suelen ser más útiles y comunes en esos entornos debido a la necesidad de centralizar configuraciones entre múltiples módulos. Sin embargo, dependiendo de las necesidades de tu proyecto, también pueden ser utilizados en un `pom.xml` de un proyecto simple.

Aquí te detallo cuándo y por qué usarlos en cada caso:

---

### **Proyectos Simples**
En proyectos simples (es decir, aquellos con un único módulo y sin módulos secundarios), el uso de estas secciones es menos común pero **puede ser válido** en situaciones específicas:

#### **1. dependencyManagement**
No es particularmente necesario en proyectos simples porque las versiones de dependencias suelen definirse directamente en la sección `<dependencies>`. Sin embargo, puede ser útil si tienes dependencias transitivas complejas o deseas prepararte para escalar a un proyecto multi-módulo en el futuro.

**Ejemplo de uso en un proyecto simple:**
```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-core</artifactId>
            <version>2.20.0</version>
        </dependency>
    </dependencies>
</dependencyManagement>

<dependencies>
    <!-- Usamos la dependencia definida en dependencyManagement -->
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-core</artifactId>
    </dependency>
</dependencies>
```

**Ventaja en proyectos simples:**
- Facilita mantener consistencia en versiones si decides añadir más dependencias relacionadas en el futuro.

---

#### **2. pluginManagement**
En proyectos simples, no es obligatorio utilizar `pluginManagement` porque los plugins generalmente se configuran directamente dentro de la sección `<plugins>`. Sin embargo, si quieres mantener centralizadas las configuraciones de plugins que podrían reutilizarse en un futuro (por ejemplo, al dividir el proyecto en múltiples módulos), puede ser útil.

**Ejemplo de uso en un proyecto simple:**
```xml
<build>
    <pluginManagement>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>3.0.0-M7</version>
            </plugin>
        </plugins>
    </pluginManagement>

    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

**Ventaja en proyectos simples:**
- Mantiene centralizadas las configuraciones de plugins para una mejor organización.
- Puede servir como base si el proyecto se convierte en un proyecto multi-módulo en el futuro.

---

#### **3. distributionManagement**
`distributionManagement` es útil tanto en proyectos simples como multi-módulo si el proyecto necesita ser distribuido a un repositorio remoto o desplegar un sitio web de Maven. Por ejemplo, si trabajas en un entorno corporativo o necesitas subir artefactos a un repositorio como Nexus, Artifactory o un repositorio propio, esta sección es imprescindible.

**Ejemplo en un proyecto simple:**
```xml
<distributionManagement>
    <repository>
        <id>releases</id>
        <url>https://repo.miempresa.com/releases</url>
    </repository>
    <snapshotRepository>
        <id>snapshots</id>
        <url>https://repo.miempresa.com/snapshots</url>
    </snapshotRepository>
</distributionManagement>
```

**Ventaja en proyectos simples:**
- Permite la distribución de artefactos (`deploy`) a repositorios remotos, incluso si el proyecto tiene un solo módulo.

---

### **Proyectos Multi-módulo**
En proyectos multi-módulo, el uso de estas secciones **es mucho más relevante** porque permiten gestionar configuraciones y dependencias comunes para todos los módulos desde el `pom.xml` principal (padre). Esto evita redundancias y facilita la consistencia entre los módulos.

#### **Ventajas en proyectos multi-módulo:**
1. **`dependencyManagement`:** Centraliza las versiones de dependencias, evitando conflictos entre módulos.
2. **`pluginManagement`:** Garantiza que todos los módulos usen las mismas versiones y configuraciones de plugins.
3. **`distributionManagement`:** Permite distribuir todos los módulos a un repositorio remoto de forma centralizada.

**Ejemplo en proyectos multi-módulo:**
- **`pom.xml` principal (padre):**
  ```xml
  <dependencyManagement>
      <dependencies>
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter</artifactId>
              <version>3.1.3</version>
          </dependency>
      </dependencies>
  </dependencyManagement>
  <pluginManagement>
      <plugins>
          <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-compiler-plugin</artifactId>
              <version>3.11.0</version>
          </plugin>
      </plugins>
  </pluginManagement>
  <distributionManagement>
      <repository>
          <id>releases</id>
          <url>https://repo.miempresa.com/releases</url>
      </repository>
  </distributionManagement>
  ```

- **Módulo secundario (`pom.xml`):**
  ```xml
  <dependencies>
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter</artifactId>
      </dependency>
  </dependencies>
  ```

---

### **Conclusión:**

- **Proyectos simples:** Estas secciones no son imprescindibles, pero pueden ser útiles para mantener configuraciones centralizadas o prepararse para escalar el proyecto en el futuro.
- **Proyectos multi-módulo:** Son prácticamente esenciales, ya que permiten gestionar versiones, configuraciones y distribuciones de forma centralizada para todos los módulos.
