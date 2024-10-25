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

@EnableAutoConfiguration Intenta configurar springboot de una forma automática

min 26