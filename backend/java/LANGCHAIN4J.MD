# Guía Completa de LangChain4j en Java

LangChain4j es una librería Java para construir aplicaciones con modelos de IA generativa (LLMs) desde la JVM. Esta guía está pensada para alguien que parte de cero en IA y quiere llegar hasta un nivel avanzado usando Java y, especialmente, Spring Boot.

---

## 1. Antes de LangChain4j: conceptos de IA que debes entender

Si no sabes nada de IA, empieza por estas ideas:

### ¿Qué es la IA generativa?

Es un tipo de inteligencia artificial capaz de generar texto, código, resúmenes, respuestas, imágenes o audio a partir de instrucciones.

### ¿Qué es un LLM?

Un **LLM** (_Large Language Model_) es un modelo entrenado con grandes cantidades de texto para comprender y generar lenguaje natural.

Ejemplos conocidos:

- GPT de OpenAI
- Claude de Anthropic
- Gemini de Google
- Llama
- Mistral

### ¿Qué hace realmente un LLM?

No “piensa” como una persona. Predice el siguiente token más probable según el contexto recibido.

### ¿Qué es un token?

Un token es una unidad pequeña de texto. No siempre equivale a una palabra completa.

Importa porque:

- Los modelos tienen límite de contexto
- El coste suele medirse por tokens
- Más tokens = más latencia y más precio

### ¿Qué es un prompt?

Es la instrucción que envías al modelo. Puede ser una pregunta, una orden, contexto adicional, ejemplos o reglas.

### ¿Qué es una alucinación?

Es cuando el modelo responde con algo convincente pero falso. Por eso en sistemas reales se usan memoria, herramientas, validación, RAG y salidas estructuradas.

### ¿Qué es un embedding?

Es una representación numérica de un texto. Sirve para comparar significado y hacer búsqueda semántica.

### ¿Qué es RAG?

**RAG** (_Retrieval-Augmented Generation_) consiste en buscar información relevante en tus documentos y añadirla al prompt antes de preguntar al modelo.

Así el LLM responde usando información de tu empresa, tu producto o tu documentación, no solo lo que aprendió en entrenamiento.

---

## 2. ¿Qué es LangChain4j?

LangChain4j es una librería Java idiomática para integrar LLMs en aplicaciones Java.

Su objetivo es simplificar:

- conexión con proveedores de modelos
- creación de prompts
- chat con memoria
- tool calling
- salidas estructuradas
- RAG
- integración con Spring Boot, Quarkus, Micronaut y Helidon

### Idea clave

LangChain4j te evita trabajar directamente con cada API propietaria de OpenAI, Anthropic, Gemini, Ollama, Bedrock, etc.

En lugar de eso, te ofrece una API unificada.

### Ventajas principales

- API homogénea para varios proveedores
- Muy orientado a Java
- Uso de interfaces, anotaciones, POJOs y DI
- Buen encaje con Spring Boot
- Soporte para chat, tools, memoria, RAG y streaming
- Permite empezar simple y escalar a arquitecturas más avanzadas

### Qué no es

LangChain4j **no es una copia directa** de LangChain Python. Está diseñado específicamente para Java.

---

## 3. Cuándo usar LangChain4j

Úsalo cuando quieras crear:

- asistentes conversacionales
- chatbots internos
- buscadores semánticos
- copilotos para backoffice
- automatización con tools
- extracción de datos a JSON o POJOs
- asistentes con memoria por usuario
- aplicaciones Spring Boot con IA

No siempre lo necesitas. Si solo vas a hacer una llamada HTTP sencilla a un proveedor y nada más, quizá baste con el SDK del proveedor. LangChain4j brilla cuando necesitas orquestación y mantenibilidad.

---

## 4. Requisitos

Según la documentación oficial, el mínimo soportado es:

- **JDK 17**

Y normalmente necesitarás:

- Java 17+
- Maven o Gradle
- clave API del proveedor que vayas a usar

---

## 5. Dependencias básicas

## Opción Maven

### Dependencia del proveedor

Ejemplo con OpenAI:

```xml
<dependency>
    <groupId>dev.langchain4j</groupId>
    <artifactId>langchain4j-open-ai</artifactId>
    <version>1.13.1</version>
</dependency>
```

### Dependencia principal de LangChain4j

Necesaria para trabajar cómodamente con AI Services y otras capas de alto nivel:

```xml
<dependency>
    <groupId>dev.langchain4j</groupId>
    <artifactId>langchain4j</artifactId>
    <version>1.13.1</version>
</dependency>
```

### BOM recomendado

Si vas a usar varios módulos:

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>dev.langchain4j</groupId>
            <artifactId>langchain4j-bom</artifactId>
            <version>1.13.1</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

## Opción Gradle

```groovy
implementation 'dev.langchain4j:langchain4j-open-ai:1.13.1'
implementation 'dev.langchain4j:langchain4j:1.13.1'
```

> Nota: algunas integraciones y starters de Spring Boot pueden aparecer con versión beta en la documentación oficial. Revisa Maven Central y la documentación oficial antes de fijar versiones en producción.

---

## 6. Tu primer ejemplo

```java
import dev.langchain4j.model.openai.OpenAiChatModel;

public class Main {

    public static void main(String[] args) {
        String apiKey = System.getenv("OPENAI_API_KEY");

        OpenAiChatModel model = OpenAiChatModel.builder()
                .apiKey(apiKey)
                .modelName("gpt-4o-mini")
                .build();

        String respuesta = model.chat("Explícame qué es un endpoint REST en una frase");
        System.out.println(respuesta);
    }
}
```

### Qué hace este ejemplo

1. Lee la API key desde variable de entorno
2. Crea un modelo de chat
3. Envía un prompt
4. Recibe la respuesta como texto

### Buenas prácticas aquí

- No hardcodear API keys
- Empezar con modelos baratos para pruebas
- Loguear con cuidado para no exponer datos sensibles

---

## 7. Arquitectura mental de LangChain4j

Piensa en LangChain4j en capas:

### Capa baja

Trabajas de forma explícita con:

- `ChatModel`
- `StreamingChatModel`
- `ChatRequest`
- `ChatResponse`
- mensajes
- parámetros del modelo

Te da más control.

### Capa alta

Trabajas con:

- `AiServices`
- interfaces Java
- anotaciones como `@SystemMessage`, `@UserMessage`, `@Tool`
- memoria
- RAG
- salidas estructuradas

Te da más productividad.

Para la mayoría de proyectos empresariales, **AI Services** es el punto ideal para empezar.

---

## 8. Modelos e integraciones

LangChain4j soporta múltiples proveedores de modelos e integraciones relacionadas.

Ejemplos frecuentes:

- OpenAI
- Anthropic
- Google Gemini / Vertex AI
- AWS Bedrock
- Azure OpenAI
- Ollama
- Mistral
- Hugging Face

Y también muchos almacenes vectoriales o embedding stores:

- Pinecone
- Milvus
- Weaviate
- Qdrant
- Elasticsearch
- pgvector
- Cassandra
- Chroma

### Beneficio real

Si abstraes bien tu código, cambiar de proveedor puede requerir cambios pequeños de configuración o de wiring, no una reescritura completa.

---

## 9. AI Services: la funcionalidad estrella

AI Services es una de las partes más potentes de LangChain4j. La idea es definir una interfaz Java y dejar que LangChain4j cree una implementación dinámica.

### Ejemplo mínimo

```java
interface Assistant {
    String chat(String userMessage);
}
```

```java
Assistant assistant = AiServices.create(Assistant.class, model);
String respuesta = assistant.chat("¿Qué es programación orientada a objetos?");
```

### Por qué es importante

Con esto escondes:

- formateo de prompts
- conversión de mensajes
- parseo de respuesta
- integración con memoria
- tools
- RAG

Es parecido a cómo Spring Data implementa interfaces automáticamente.

---

## 10. `@SystemMessage` y `@UserMessage`

### `@SystemMessage`

Sirve para definir el comportamiento base del asistente.

```java
interface TutorIA {

    @SystemMessage("Eres un profesor de Java. Responde de forma clara y didáctica.")
    String responder(String pregunta);
}
```

### `@UserMessage`

Sirve para definir o personalizar el prompt del usuario.

```java
interface Traductor {

    @UserMessage("Traduce al inglés el siguiente texto: {{it}}")
    String traducir(String texto);
}
```

### Cuándo usar cada uno

- `@SystemMessage`: reglas, tono, rol, restricciones
- `@UserMessage`: plantilla concreta de entrada

### Recomendación

Pon en el system prompt solo reglas estables. El contenido dinámico debería vivir en el user prompt o en parámetros.

---

## 11. Variables en prompts

Puedes parametrizar prompts con variables:

```java
interface GeneradorEmails {

    @SystemMessage("Eres experto en comunicación profesional.")
    @UserMessage("Escribe un email para {{tipoCliente}} sobre {{tema}}")
    String generar(@V("tipoCliente") String tipoCliente, @V("tema") String tema);
}
```

Esto permite prompts reutilizables y más limpios.

---

## 12. Salidas estructuradas

Una necesidad muy común no es recibir texto libre, sino datos estructurados.

Por ejemplo:

- extraer campos de una factura
- clasificar tickets
- devolver JSON validable
- convertir respuesta a un POJO

### Ejemplo con record

```java
record Persona(String nombre, int edad, String ciudad) {}
```

```java
interface ExtractorPersona {
    Persona extraer(String texto);
}
```

LangChain4j puede parsear la salida del modelo a tipos Java compatibles.

### Beneficios

- menos postprocesado manual
- menos regexs frágiles
- respuestas más integrables con backend
- mejor base para automatización

### Casos típicos

- devolver enums
- listas de objetos
- DTOs
- records
- resultados enriquecidos con metadatos

### Precaución

Aunque la salida estructurada mejora mucho la fiabilidad, siempre conviene validar los datos antes de persistirlos o ejecutar acciones críticas.

---

## 13. Herramientas / Function Calling / `@Tool`

Los LLMs son buenos generando texto, pero no ejecutan acciones reales por sí solos. Para eso están las **tools**.

Una tool puede ser:

- consultar una base de datos
- llamar a un microservicio
- obtener el clima
- enviar un email
- calcular precios
- ejecutar una búsqueda

### Ejemplo

```java
class CalculadoraTools {

    @Tool("Suma dos números")
    double sumar(double a, double b) {
        return a + b;
    }

    @Tool("Calcula la raíz cuadrada de un número")
    double raizCuadrada(double x) {
        return Math.sqrt(x);
    }
}
```

### Qué ocurre por detrás

1. El usuario pregunta algo
2. El modelo detecta que necesita una tool
3. LangChain4j prepara la invocación
4. Se ejecuta el método Java
5. El resultado vuelve al modelo
6. El modelo genera la respuesta final

### Buenas prácticas con tools

- nombres claros
- descripciones precisas
- parámetros bien definidos
- no exponer operaciones peligrosas sin validación
- registrar auditoría si una tool modifica estado

### Muy importante

No dejes que una tool sensible:

- borre datos
- haga transferencias
- mande emails
- ejecute acciones de negocio

sin autorización, validación y trazabilidad.

---

## 14. Chat Memory

La memoria sirve para que el modelo “recuerde” el contexto reciente de la conversación.

### Diferencia entre historial y memoria

- **Historial**: todo lo que se dijo
- **Memoria**: lo que decides reenviar al modelo

No siempre deben coincidir.

### Implementaciones comunes

- `MessageWindowChatMemory`
- `TokenWindowChatMemory`

### Ejemplo

```java
ChatMemory chatMemory = MessageWindowChatMemory.withMaxMessages(10);

Assistant assistant = AiServices.builder(Assistant.class)
        .chatModel(model)
        .chatMemory(chatMemory)
        .build();
```

### Cuándo usar cada una

- `MessageWindowChatMemory`: simple, útil para prototipos
- `TokenWindowChatMemory`: mejor control del contexto real y del coste

### Memoria persistente

LangChain4j permite implementar un `ChatMemoryStore` para guardar mensajes en base de datos o almacenamiento persistente.

Esto es clave si necesitas:

- conversaciones por usuario
- continuidad entre sesiones
- soporte multiusuario
- auditoría

---

## 15. Streaming

Si no quieres esperar la respuesta completa, puedes usar streaming para ir recibiendo texto por fragmentos.

Útil para:

- UIs tipo chat
- sensación de menor latencia
- respuestas largas

En aplicaciones web modernas esto suele exponerse por:

- SSE
- WebSocket
- streams HTTP

---

## 16. Parámetros importantes del modelo

Al configurar un modelo, suelen importar:

- `modelName`
- `temperature`
- `maxTokens`
- `timeout`
- logging de requests y responses

### Regla rápida de temperatura

- **baja**: respuestas más predecibles
- **alta**: respuestas más creativas

Ejemplos:

- extracción de datos: baja
- generación creativa: media/alta
- clasificación: baja

---

## 17. RAG: dar contexto real al modelo

RAG es una de las piezas más importantes en sistemas empresariales.

### Problema que resuelve

Un LLM no conoce:

- tu base documental interna
- manuales de tu empresa
- datos de producto recientes
- normativa específica privada

### Solución

1. troceas documentos
2. generas embeddings
3. los guardas en un embedding store
4. cuando llega una pregunta, buscas fragmentos relevantes
5. añades esos fragmentos al prompt
6. el modelo responde con ese contexto

### En LangChain4j hay varios niveles

- **Easy RAG**: el camino más simple para empezar
- **Naive RAG**: pipeline básico clásico
- **Advanced RAG**: más modular y personalizable

### Ejemplo simplificado

```java
InMemoryEmbeddingStore<TextSegment> embeddingStore = new InMemoryEmbeddingStore<>();

Assistant assistant = AiServices.builder(Assistant.class)
        .chatModel(model)
        .chatMemory(MessageWindowChatMemory.withMaxMessages(10))
        .contentRetriever(EmbeddingStoreContentRetriever.from(embeddingStore))
        .build();
```

### Cuándo usar RAG

- documentación técnica
- FAQ de negocio
- políticas internas
- soporte al cliente
- contratos, manuales, procedimientos

### Riesgos de un mal RAG

- troceado malo
- embeddings pobres
- demasiado contexto irrelevante
- ausencia de filtros por metadata
- respuestas basadas en fragmentos desactualizados

---

## 18. Easy RAG

Easy RAG está pensado para arrancar muy rápido.

Te evita decidir desde el primer minuto:

- parser de documentos
- splitter
- modelo de embeddings
- detalles del pipeline de ingestión

### Cuándo encaja

- pruebas de concepto
- aprendizaje
- demos internas

### Cuándo se queda corto

- producción exigente
- documentos complejos
- necesidad de filtros avanzados
- ranking y re-ranking fino
- control detallado del pipeline

La estrategia habitual es:

1. comenzar con Easy RAG
2. validar utilidad
3. evolucionar a un RAG personalizado

---

## 19. Spring Boot + LangChain4j

Si trabajas con Spring Boot, LangChain4j es especialmente cómodo.

### Starters

LangChain4j ofrece starters para:

- modelos
- embedding models
- embedding stores
- AI Services

### Ejemplo de dependencia para OpenAI con Spring Boot 3

```xml
<dependency>
    <groupId>dev.langchain4j</groupId>
    <artifactId>langchain4j-open-ai-spring-boot-starter</artifactId>
    <version>1.13.1-beta23</version>
</dependency>
```

Y el starter general:

```xml
<dependency>
    <groupId>dev.langchain4j</groupId>
    <artifactId>langchain4j-spring-boot-starter</artifactId>
    <version>1.13.1-beta23</version>
</dependency>
```

### Configuración en `application.properties`

```properties
langchain4j.open-ai.chat-model.api-key=${OPENAI_API_KEY}
langchain4j.open-ai.chat-model.model-name=gpt-4o-mini
langchain4j.open-ai.chat-model.log-requests=true
langchain4j.open-ai.chat-model.log-responses=true
```

### AI Service declarativo

```java
@AiService
interface Assistant {

    @SystemMessage("Eres un asistente técnico amable y preciso")
    String chat(String userMessage);
}
```

Luego lo inyectas como cualquier bean:

```java
@RestController
class AssistantController {

    private final Assistant assistant;

    AssistantController(Assistant assistant) {
        this.assistant = assistant;
    }

    @GetMapping("/chat")
    public String chat(@RequestParam String message) {
        return assistant.chat(message);
    }
}
```

### Ventajas con Spring

- configuración por propiedades
- beans autowireables
- integración natural con `@Service`, `@Component`, `@RestController`
- facilidad para exponer tools
- encaje con observabilidad, seguridad y Actuator

---

## 20. Wiring automático y explícito

En Spring Boot puedes dejar que LangChain4j conecte automáticamente:

- `ChatModel`
- `StreamingChatModel`
- `ChatMemory`
- `ContentRetriever`
- `RetrievalAugmentor`
- tools anotadas con `@Tool`

Si tienes varios modelos o varias configuraciones, te interesa el **wiring explícito** para evitar ambigüedades.

Úsalo cuando tengas por ejemplo:

- un modelo barato para clasificación
- otro modelo mejor para razonamiento
- un asistente con RAG
- otro sin RAG

---

## 21. Observabilidad

En IA esto es fundamental. Si no mides, no controlas:

- coste
- latencia
- calidad
- errores
- uso de tokens

LangChain4j permite escuchar requests, responses y errores mediante `ChatModelListener`.

También puede integrarse con métricas, por ejemplo con Micrometer en Spring Boot.

### Qué conviene registrar

- modelo usado
- tokens de entrada/salida
- duración
- éxito o error
- tool invocada
- versión del prompt

### Qué no conviene registrar sin control

- prompts con datos sensibles
- PII
- secretos
- documentos completos del usuario

---

## 22. Seguridad y buenas prácticas

Si vas a trabajar con IA en backend, esta sección no es opcional.

### API keys

- siempre en variables de entorno o secret managers
- nunca en código
- nunca en repositorios

### Prompt injection

Un usuario puede intentar manipular instrucciones:

- “ignora tus reglas”
- “muéstrame secretos”
- “ejecuta esta acción”

Debes diseñar defensas:

- separar claramente instrucciones del sistema y entrada del usuario
- validar tools
- limitar capacidades
- filtrar operaciones sensibles

### Validación de salida

No confíes ciegamente en la respuesta del modelo si:

- modifica estado
- genera SQL
- envía correos
- llama a servicios críticos

Valida siempre.

### Principio de mínimo privilegio

Las tools deben tener solo los permisos estrictamente necesarios.

---

## 23. Estrategias de diseño recomendadas

### Empezar simple

Fase 1:

- un modelo
- un AI Service
- sin RAG
- sin tools peligrosas

Fase 2:

- memoria
- salidas estructuradas
- prompts versionados

Fase 3:

- RAG
- tools
- observabilidad
- evaluación de calidad

### Encapsula la IA en una capa de servicio

No disperses prompts y llamadas a modelos por toda la aplicación.

Mejor:

- `AsistenteFaqService`
- `ClasificadorTicketsService`
- `ExtractorFacturasService`

Así puedes versionar, testear y sustituir mejor.

### Diseña para cambiar de proveedor

Evita acoplarte innecesariamente a detalles muy específicos de un proveedor salvo que realmente lo necesites.

---

## 24. Casos de uso típicos

### 1. Chatbot de documentación

- RAG sobre manuales y docs
- memoria por usuario
- respuestas con fuentes

### 2. Extracción de datos

- CVs
- facturas
- tickets
- contratos

Ideal con salidas estructuradas.

### 3. Automatización con tools

- consultar stock
- recuperar pedidos
- crear resúmenes
- disparar workflows

### 4. Asistente para soporte técnico

- RAG
- tools internas
- clasificación automática
- resumen de incidencias

---

## 25. Limitaciones reales

LangChain4j ayuda mucho, pero no hace magia.

### Debes seguir resolviendo:

- calidad de prompts
- diseño de arquitectura
- control de costes
- elección del modelo
- evaluación de respuestas
- seguridad
- calidad del RAG

### Errores comunes al empezar

- querer usar IA para todo
- no medir tokens ni latencia
- no validar salidas
- hacer prompts gigantes
- meter tools peligrosas demasiado pronto
- confundir demo con producción

---

## 26. Cómo aprender LangChain4j de forma progresiva

### Nivel 1: fundamentos

Aprende:

- qué es un LLM
- prompts
- tokens
- temperatura
- contexto

### Nivel 2: uso básico

Practica:

- crear un `ChatModel`
- enviar prompts
- cambiar de modelo
- controlar parámetros

### Nivel 3: productividad

Aprende:

- `AiServices`
- `@SystemMessage`
- `@UserMessage`
- POJOs de salida

### Nivel 4: asistentes útiles

Aprende:

- memoria
- tools
- streaming

### Nivel 5: nivel profesional

Aprende:

- RAG
- Spring Boot starters
- observabilidad
- seguridad
- evaluación

---

## 27. Ejemplo de recorrido recomendado en un proyecto real

### Paso 1

Crear un endpoint REST que reciba una pregunta y la mande a un `ChatModel`.

### Paso 2

Sustituir la llamada manual por un `AiService`.

### Paso 3

Añadir `@SystemMessage` para definir rol y límites.

### Paso 4

Si el caso lo necesita, devolver DTOs con salida estructurada.

### Paso 5

Añadir memoria por usuario.

### Paso 6

Conectar tools internas.

### Paso 7

Añadir RAG con documentación propia.

### Paso 8

Medir latencia, coste, calidad y errores.

---

## 28. Ejemplo mental de stack moderno con LangChain4j

- **Spring Boot** para API y negocio
- **LangChain4j** para orquestación IA
- **OpenAI / Gemini / Claude / Ollama** como modelo
- **PostgreSQL / Redis / MongoDB** para persistencia auxiliar
- **pgvector / Qdrant / Elasticsearch / Pinecone** para búsqueda semántica
- **Micrometer + Actuator** para métricas
- **tu sistema de autenticación** para controlar acceso a tools y endpoints

---

## 29. Consejos para producción

- usa prompts pequeños y claros
- versiona prompts importantes
- separa entorno dev y prod
- no registres datos sensibles sin control
- crea tests para casos críticos
- evalúa respuestas con ejemplos reales
- limita tools peligrosas
- usa RAG cuando la información cambie mucho
- revisa costes por modelo y por flujo

---

## 30. Resumen final

LangChain4j es una de las mejores puertas de entrada para usar IA generativa desde Java porque:

- habla el lenguaje natural de Java
- abstrae proveedores
- encaja muy bien con Spring Boot
- permite empezar con un chat simple
- escala hacia memoria, tools, salidas estructuradas y RAG

Si empiezas desde cero, la ruta más razonable es:

1. entender LLMs y prompts
2. hacer llamadas básicas a un modelo
3. pasar a AI Services
4. añadir salida estructurada
5. incorporar memoria
6. usar tools
7. llegar a RAG y producción

---

## 31. Checklist práctico para empezar hoy

- [ ] Instalar Java 17+
- [ ] Añadir dependencias de LangChain4j
- [ ] Configurar API key en variables de entorno
- [ ] Probar un `ChatModel`
- [ ] Crear tu primer `AiService`
- [ ] Definir un buen `@SystemMessage`
- [ ] Probar una salida estructurada
- [ ] Añadir memoria si tu caso es conversacional
- [ ] Evaluar si necesitas tools
- [ ] Evaluar si necesitas RAG
- [ ] Añadir métricas y seguridad antes de producción

---

## 32. Qué estudiar después de esta guía

Cuando ya domines esta base, profundiza en:

- prompt engineering
- evaluación de prompts
- chunking y estrategia de RAG
- ranking y re-ranking
- guardrails
- observabilidad de IA
- testing de asistentes
- modelos locales con Ollama
- integración avanzada con Spring Boot

---

## Conclusión

Si tu ecosistema es Java, LangChain4j es una opción muy sólida para construir aplicaciones con IA generativa de forma profesional. Te permite pasar de una simple llamada a un modelo a un sistema completo con memoria, herramientas, recuperación de documentos e integración empresarial sin abandonar el estilo de desarrollo habitual de Java y Spring.
