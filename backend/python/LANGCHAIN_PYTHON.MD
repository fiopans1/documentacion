# 🦜⛓️ Guía Completa de LangChain para Python

LangChain es un framework para construir aplicaciones con modelos de lenguaje en Python. Te ayuda a organizar prompts, modelos, parsing de respuestas, recuperación de documentos, tools y flujos más complejos sin tener que pegar llamadas sueltas a APIs.

Esta guía está pensada para que puedas pasar de **entender los conceptos** a **tener una base real para un proyecto**.

---

## 📋 Índice

1. [Qué es LangChain y cuándo usarlo](#1-qué-es-langchain-y-cuándo-usarlo)
2. [Conceptos que debes entender antes](#2-conceptos-que-debes-entender-antes)
3. [Instalación y estructura mínima](#3-instalación-y-estructura-mínima)
4. [Tu primer ejemplo con un modelo](#4-tu-primer-ejemplo-con-un-modelo)
5. [Prompt templates](#5-prompt-templates)
6. [Encadenar pasos con LCEL](#6-encadenar-pasos-con-lcel)
7. [Trabajar con mensajes de chat](#7-trabajar-con-mensajes-de-chat)
8. [Salidas estructuradas](#8-salidas-estructuradas)
9. [Streaming de respuestas](#9-streaming-de-respuestas)
10. [RAG: responder con tus documentos](#10-rag-responder-con-tus-documentos)
11. [Tools y llamadas a funciones](#11-tools-y-llamadas-a-funciones)
12. [Agents: cuándo usarlos y cuándo no](#12-agents-cuándo-usarlos-y-cuándo-no)
13. [Memoria y estado conversacional](#13-memoria-y-estado-conversacional)
14. [Estructura recomendada para un proyecto real](#14-estructura-recomendada-para-un-proyecto-real)
15. [Buenas prácticas](#15-buenas-prácticas)
16. [Errores comunes](#16-errores-comunes)
17. [Qué proyecto puedes hacer para aprender](#17-qué-proyecto-puedes-hacer-para-aprender)

---

## 1. Qué es LangChain y cuándo usarlo

LangChain te sirve cuando tu aplicación con IA necesita algo más que enviar un prompt y mostrar una respuesta.

Úsalo si necesitas:

- prompts reutilizables
- varias etapas de procesamiento
- salidas con formato controlado
- búsqueda sobre tus documentos
- tools externas como APIs o base de datos
- observabilidad y trazas
- flujos mantenibles a medida que el proyecto crece

### Cuándo sí compensa usarlo

- chatbot con conocimiento de tu empresa
- asistente que consulta APIs externas
- extractor de datos desde texto a JSON
- buscador semántico sobre PDFs o documentación
- copiloto interno para soporte o backoffice

### Cuándo quizá no lo necesitas

Si solo vas a hacer una llamada simple a OpenAI o Anthropic y devolver el texto, quizá con el SDK oficial del proveedor basta.

### Idea importante

LangChain no es el modelo. LangChain es la **capa de orquestación** alrededor del modelo.

---

## 2. Conceptos que debes entender antes

### LLM

Es el modelo que genera texto a partir de instrucciones y contexto.

Ejemplos:

- GPT
- Claude
- Gemini
- Llama
- Mistral

### Prompt

Es la instrucción que le pasas al modelo.

Ejemplo simple:

```text
Resume este correo en dos líneas y con tono profesional.
```

### Tokens

Los modelos no “leen palabras” como una persona. Trabajan con tokens. Esto importa porque:

- hay límite de contexto
- el coste suele medirse por tokens
- más tokens implican más latencia

### Embeddings

Son representaciones numéricas del texto. Sirven para comparar significado y hacer búsqueda semántica.

Ejemplo mental:

- “factura de abril”
- “invoice for april”

Aunque no son la misma frase, sus embeddings pueden quedar cerca si expresan ideas parecidas.

### Retriever

Es el componente que busca fragmentos relevantes antes de preguntar al modelo.

### RAG

RAG significa **Retrieval-Augmented Generation**. Primero recuperas información de tus documentos y luego se la das al modelo para que responda mejor.

### Tool

Es una función o acción que el modelo puede solicitar ejecutar, por ejemplo:

- consultar el tiempo
- buscar un pedido
- leer una API interna
- ejecutar una query controlada

---

## 3. Instalación y estructura mínima

### Requisitos

- Python 3.10+
- una API key del proveedor que vayas a usar
- entorno virtual

### Instalación básica

```bash
python -m venv .venv
source .venv/bin/activate
pip install -U langchain langchain-openai python-dotenv
```

### Instalación para ejemplos de RAG

```bash
pip install -U langchain-community faiss-cpu pypdf
```

### Variable de entorno

```bash
export OPENAI_API_KEY="tu_api_key"
```

En Windows PowerShell:

```powershell
$env:OPENAI_API_KEY="tu_api_key"
```

### Estructura mínima recomendada

```text
mi_proyecto_ia/
├── app/
│   ├── main.py
│   ├── prompts/
│   ├── services/
│   ├── rag/
│   └── tools/
├── data/
├── .env
├── requirements.txt
└── README.md
```

### Por qué separar carpetas

Porque en proyectos reales mezclar prompts, RAG, tools y lógica HTTP en un solo archivo se vuelve inmantenible muy rápido.

---

## 4. Tu primer ejemplo con un modelo

```python
from langchain_openai import ChatOpenAI

model = ChatOpenAI(
    model="gpt-4o-mini",
    temperature=0
)

respuesta = model.invoke("Explícame qué es una API REST en una frase")
print(respuesta.content)
```

### Qué hace este código

1. Crea un cliente de chat.
2. Elige un modelo.
3. Envía una instrucción.
4. Devuelve un mensaje con contenido.

### Por qué `temperature=0`

Porque cuando estás aprendiendo, depurando o extrayendo información, te interesa que la salida sea más estable.

### Ejemplo de salida

```text
Una API REST es una interfaz que permite que sistemas se comuniquen mediante HTTP usando recursos y operaciones estándar.
```

### Cuándo usar este patrón

Úsalo cuando solo necesites una respuesta simple y todavía no haya plantillas, documentos ni tools.

---

## 5. Prompt templates

Cuando empiezas a repetir prompts con variables, no conviene concatenar strings a mano.

```python
from langchain_core.prompts import ChatPromptTemplate
from langchain_openai import ChatOpenAI

prompt = ChatPromptTemplate.from_template(
    "Explica el concepto de {tema} para un nivel {nivel} y añade un ejemplo corto"
)

model = ChatOpenAI(model="gpt-4o-mini", temperature=0)
chain = prompt | model

respuesta = chain.invoke({"tema": "decoradores en Python", "nivel": "junior"})
print(respuesta.content)
```

### Qué ventaja tiene

Separas:

- la plantilla
- las variables de entrada
- el modelo

Eso hace que el prompt sea más fácil de probar, cambiar y reutilizar.

### Ejemplo de uso real

Imagina una plataforma educativa:

- `tema = "SQL joins"`
- `nivel = "intermedio"`

Con el mismo prompt puedes generar explicaciones adaptadas sin duplicar lógica.

---

## 6. Encadenar pasos con LCEL

LCEL es la forma moderna y cómoda de componer piezas en LangChain usando el operador `|`.

```python
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser
from langchain_openai import ChatOpenAI

prompt = ChatPromptTemplate.from_template(
    "Resume el siguiente texto en 3 bullets:\n\n{texto}"
)

model = ChatOpenAI(model="gpt-4o-mini", temperature=0)
parser = StrOutputParser()

chain = prompt | model | parser

resultado = chain.invoke({
    "texto": "LangChain permite orquestar modelos, prompts, retrievers y tools para crear aplicaciones de IA más mantenibles."
})

print(resultado)
```

### Qué hace cada parte

- `prompt`: construye el mensaje
- `model`: genera la respuesta
- `parser`: transforma la salida al formato que quieres

### Por qué esto es importante

En proyectos reales rara vez haces solo una llamada al modelo. Normalmente necesitas:

- preparar entrada
- invocar el modelo
- transformar salida
- pasar resultado a otra capa

LCEL hace esto muy limpio.

---

## 7. Trabajar con mensajes de chat

Cuando tu app es conversacional, es mejor pensar en mensajes que en un único string.

```python
from langchain_core.messages import HumanMessage, SystemMessage
from langchain_openai import ChatOpenAI

model = ChatOpenAI(model="gpt-4o-mini", temperature=0)

mensajes = [
    SystemMessage(content="Eres un profesor de Python claro y directo."),
    HumanMessage(content="¿Qué diferencia hay entre una lista y una tupla?")
]

respuesta = model.invoke(mensajes)
print(respuesta.content)
```

### Por qué usar mensajes

Porque separas roles:

- `SystemMessage`: reglas de comportamiento
- `HumanMessage`: entrada del usuario
- `AIMessage`: respuesta del modelo

### Ejemplo práctico

En un asistente de soporte puedes usar `SystemMessage` para fijar reglas como:

- responde en español
- no inventes datos
- si falta contexto, pide aclaraciones

---

## 8. Salidas estructuradas

Uno de los casos más útiles es pedirle al modelo datos con forma clara para luego procesarlos en tu backend.

```python
from pydantic import BaseModel, Field
from langchain_openai import ChatOpenAI

class TicketSoporte(BaseModel):
    prioridad: str = Field(description="alta, media o baja")
    area: str = Field(description="facturacion, tecnico o ventas")
    resumen: str

model = ChatOpenAI(model="gpt-4o-mini", temperature=0)
structured_model = model.with_structured_output(TicketSoporte)

resultado = structured_model.invoke(
    "El cliente dice que pagó dos veces la misma factura y quiere una devolución urgente."
)

print(resultado)
print(resultado.prioridad)
print(resultado.area)
```

### Por qué esto es tan útil

Porque tu backend normalmente no quiere un párrafo bonito. Quiere datos utilizables.

### Ejemplo real

Si recibes correos de clientes, puedes convertirlos en:

- prioridad
- categoría
- resumen
- sentimiento

Y luego guardarlo en base de datos o enviarlo a otro servicio.

---

## 9. Streaming de respuestas

Si tu aplicación tiene interfaz de chat, el streaming mejora mucho la experiencia porque el usuario ve la respuesta por partes.

```python
from langchain_openai import ChatOpenAI

model = ChatOpenAI(model="gpt-4o-mini", temperature=0)

for chunk in model.stream("Explícame paso a paso qué es Docker"):
    if chunk.content:
        print(chunk.content, end="", flush=True)
```

### Cuándo usar streaming

- chat web
- consola interactiva
- asistentes donde la latencia importa

### Cuándo no es necesario

- tareas batch
- extracción estructurada
- procesos offline

---

## 10. RAG: responder con tus documentos

Si quieres hacer un proyecto serio con documentación propia, este es uno de los bloques más importantes.

### Flujo mental de RAG

1. cargas documentos
2. los troceas
3. generas embeddings
4. los guardas en un vector store
5. recuperas fragmentos relevantes
6. preguntas al modelo usando ese contexto

### Ejemplo simple con documentos en memoria

```python
from langchain_core.documents import Document
from langchain_text_splitters import RecursiveCharacterTextSplitter
from langchain_community.vectorstores import FAISS
from langchain_openai import ChatOpenAI, OpenAIEmbeddings
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser

# 1. Documentos
raw_docs = [
    Document(page_content="LangChain permite crear flujos con prompts, modelos, retrievers y tools."),
    Document(page_content="FAISS es una opción común para búsquedas vectoriales locales en prototipos."),
    Document(page_content="RAG combina recuperación de contexto y generación de respuestas con un LLM.")
]

# 2. Troceado
splitter = RecursiveCharacterTextSplitter(chunk_size=150, chunk_overlap=30)
docs = splitter.split_documents(raw_docs)

# 3. Embeddings + vector store
embeddings = OpenAIEmbeddings()
vectorstore = FAISS.from_documents(docs, embeddings)
retriever = vectorstore.as_retriever(search_kwargs={"k": 2})

# 4. Prompt
prompt = ChatPromptTemplate.from_template(
    """
    Responde solo con el contexto proporcionado.

    Contexto:
    {context}

    Pregunta:
    {question}
    """
)

model = ChatOpenAI(model="gpt-4o-mini", temperature=0)
parser = StrOutputParser()

# 5. Consulta
question = "¿Para qué sirve FAISS en un proyecto con LangChain?"
context_docs = retriever.invoke(question)
context = "\n\n".join(doc.page_content for doc in context_docs)

chain = prompt | model | parser
respuesta = chain.invoke({"context": context, "question": question})

print(respuesta)
```

### Qué debes entender de verdad aquí

#### `Document`

Es la unidad base de texto con la que trabajarás.

#### `RecursiveCharacterTextSplitter`

Parte textos largos en fragmentos más manejables. Esto es clave porque los modelos y embeddings funcionan mejor con chunks razonables.

#### `FAISS`

Guarda vectores para hacer búsqueda semántica.

#### `retriever`

Dado un texto de consulta, trae los fragmentos más relevantes.

### Ejemplo real de proyecto

Supón que tienes documentación interna de una empresa:

- políticas de devoluciones
- manuales de soporte
- preguntas frecuentes
- guías técnicas

Sin RAG, el modelo puede inventar.
Con RAG, primero recuperas el contenido real y luego respondes con base en él.

### Dependencia útil para este ejemplo

Si no tienes instalado el splitter por separado:

```bash
pip install -U langchain-text-splitters
```

---

## 11. Tools y llamadas a funciones

Los tools sirven para sacar al modelo del puro texto y permitirle usar capacidades externas.

```python
from langchain_core.tools import tool
from langchain_openai import ChatOpenAI

@tool
def obtener_precio_producto(nombre: str) -> str:
    """Devuelve el precio actual de un producto."""
    precios = {
        "portatil": "999 euros",
        "teclado": "80 euros",
        "raton": "35 euros"
    }
    return precios.get(nombre.lower(), "Producto no encontrado")

model = ChatOpenAI(model="gpt-4o-mini", temperature=0)
model_with_tools = model.bind_tools([obtener_precio_producto])

respuesta = model_with_tools.invoke("¿Cuál es el precio del teclado?")
print(respuesta.tool_calls)
```

### Qué debes entender

Aquí el modelo no está ejecutando magia. Está indicando que quiere usar una herramienta concreta con ciertos argumentos.

Después, tu aplicación ejecuta esa herramienta y continúa el flujo.

### Casos de uso reales

- consultar stock
- crear tickets
- leer una API de envíos
- buscar pedidos
- lanzar acciones controladas

### Regla importante

Nunca expongas tools peligrosos sin validación.

Ejemplos de riesgo:

- ejecutar SQL libre
- borrar registros
- llamar APIs críticas sin permisos

---

## 12. Agents: cuándo usarlos y cuándo no

Un agent decide qué pasos dar y qué tools usar según la tarea.

### Cuándo tienen sentido

- cuando la secuencia de acciones no es fija
- cuando el modelo debe elegir entre varias herramientas
- cuando quieres más autonomía en la resolución

### Cuándo no conviene empezar por aquí

Si aún estás aprendiendo, no empieces creando un agent complejo. Primero domina:

- prompts
- cadenas
- salidas estructuradas
- RAG
- tools simples

### Recomendación práctica

Para muchos proyectos reales, un flujo explícito y controlado funciona mejor que un agent “demasiado libre”.

### Idea importante

Si tu aplicación necesita orquestación avanzada con estado, ramas y ciclos, hoy suele tener más sentido mirar también **LangGraph** además de LangChain.

---

## 13. Memoria y estado conversacional

Muchos principiantes creen que el modelo “recuerda” solo. No. Si quieres continuidad, tu aplicación debe volver a enviar el contexto relevante.

### Ejemplo simple de historial manual

```python
from langchain_core.messages import HumanMessage, SystemMessage, AIMessage
from langchain_openai import ChatOpenAI

model = ChatOpenAI(model="gpt-4o-mini", temperature=0)

historial = [
    SystemMessage(content="Eres un asistente técnico que responde en español."),
    HumanMessage(content="Mi nombre es Ana y trabajo con FastAPI."),
    AIMessage(content="Perfecto, Ana. ¿En qué puedo ayudarte con FastAPI?")
]

historial.append(HumanMessage(content="Recuérdame qué te dije sobre mí"))
respuesta = model.invoke(historial)
print(respuesta.content)
```

### Qué enseña este ejemplo

El “recuerdo” no es mágico. Se logra reenviando mensajes previos o un resumen de ellos.

### En un proyecto real

Puedes guardar el historial en:

- memoria temporal en backend
- Redis
- base de datos
- un resumen persistido por conversación

---

## 14. Estructura recomendada para un proyecto real

Si quieres hacer un proyecto con LangChain, esta estructura te evita muchos problemas desde el principio.

```text
app/
├── api/
│   └── routes.py
├── core/
│   ├── config.py
│   └── logging.py
├── llm/
│   ├── models.py
│   ├── prompts.py
│   └── parsers.py
├── rag/
│   ├── ingest.py
│   ├── retriever.py
│   └── chains.py
├── tools/
│   ├── catalogo.py
│   └── pedidos.py
├── services/
│   └── chatbot_service.py
└── main.py
```

### Qué pondrías en cada sitio

- `config.py`: variables de entorno y configuración
- `models.py`: inicialización de modelos
- `prompts.py`: plantillas reutilizables
- `parsers.py`: salidas estructuradas
- `ingest.py`: carga e indexación de documentos
- `retriever.py`: acceso a búsqueda semántica
- `tools/`: funciones externas disponibles
- `chatbot_service.py`: flujo principal del caso de uso

### Consejo práctico

No metas el código RAG dentro del endpoint HTTP directamente. Mantenlo en servicios reutilizables.

---

## 15. Buenas prácticas

### 1. Empieza simple

Primero haz que funcione:

- un prompt
- una respuesta
- una salida estable

Y luego añade RAG, tools o memoria.

### 2. No hardcodees claves

Usa variables de entorno o `.env`.

### 3. Reduce alucinaciones

- usa contexto recuperado
- limita la instrucción al material disponible
- pide que diga “no lo sé” si falta información

### 4. Separa prompts del código de negocio

Si los mezclas todo el tiempo, luego es muy difícil mantenerlos.

### 5. Observa lo que está pasando

Guarda:

- prompt enviado
- documentos recuperados
- coste estimado
- tiempos de respuesta
- errores

### 6. Evalúa con ejemplos reales

No pruebes solo con casos fáciles. Usa preguntas ambiguas, entradas largas y datos ruidosos.

### 7. Controla el contexto

Enviar demasiado historial o demasiados documentos empeora el coste y a veces también la calidad.

---

## 16. Errores comunes

### Error 1: querer empezar por agents

Suele parecer lo más potente, pero también es donde más complejidad aparece.

### Error 2: no separar documentos por chunks

Si indexas textos enormes sin estrategia, el retrieval empeora.

### Error 3: no validar la salida

Si necesitas JSON o campos concretos, no dependas de texto libre.

### Error 4: usar RAG sin revisar qué fragmentos recupera

No basta con que “exista un vector store”. Tienes que comprobar si trae el contexto correcto.

### Error 5: confiar demasiado en la primera demo

Que un ejemplo funcione dos veces no significa que ya esté listo para producción.

---

## 17. Qué proyecto puedes hacer para aprender

### Opción recomendada: asistente sobre tu propia documentación

Es el mejor proyecto para aprender porque tocas casi todo:

- carga de documentos
- chunking
- embeddings
- vector store
- retriever
- prompt
- respuesta final

### Fase 1

Haz un chatbot que responda sobre tus archivos Markdown o PDFs.

### Fase 2

Añade citas o referencias al fragmento usado.

### Fase 3

Añade historial por usuario.

### Fase 4

Añade tools, por ejemplo una API para consultar estado de pedidos o tickets.

### Fase 5

Añade evaluación y logging.

### Stack sugerido

- FastAPI para exponer API
- LangChain para la orquestación
- FAISS o Chroma para prototipos
- PostgreSQL o Redis para estado si crece
- LangSmith si quieres trazas y observabilidad

---

## ✅ Resumen final

Si te quieres preparar bien para un proyecto con LangChain en Python, el orden más útil suele ser este:

1. entender prompts y mensajes
2. dominar `invoke` y `ChatPromptTemplate`
3. componer cadenas con LCEL
4. generar salidas estructuradas
5. implementar RAG
6. añadir tools
7. solo después evaluar si realmente necesitas agents

Si sigues este orden, aprenderás lo importante sin meterte demasiado pronto en la parte más compleja.
