# ⚡ Guía Completa de FastAPI

## 📋 Índice

1. [Introducción a FastAPI](#1-introducción-a-fastapi)
2. [Instalación y Configuración](#2-instalación-y-configuración)
3. [Primera Aplicación](#3-primera-aplicación)
4. [Rutas y Métodos HTTP](#4-rutas-y-métodos-http)
5. [Parámetros de Ruta](#5-parámetros-de-ruta)
6. [Parámetros de Query](#6-parámetros-de-query)
7. [Request Body y Modelos Pydantic](#7-request-body-y-modelos-pydantic)
8. [Validación de Datos](#8-validación-de-datos)
9. [Modelos de Respuesta](#9-modelos-de-respuesta)
10. [Manejo de Formularios y Archivos](#10-manejo-de-formularios-y-archivos)
11. [Manejo de Errores y Excepciones](#11-manejo-de-errores-y-excepciones)
12. [Dependency Injection](#12-dependency-injection)
13. [Middleware](#13-middleware)
14. [CORS (Cross-Origin Resource Sharing)](#14-cors-cross-origin-resource-sharing)
15. [Autenticación y Autorización](#15-autenticación-y-autorización)
16. [Bases de Datos con SQLAlchemy](#16-bases-de-datos-con-sqlalchemy)
17. [Bases de Datos con SQLModel](#17-bases-de-datos-con-sqlmodel)
18. [Async y Await](#18-async-y-await)
19. [WebSockets](#19-websockets)
20. [Background Tasks](#20-background-tasks)
21. [Testing](#21-testing)
22. [Documentación Automática](#22-documentación-automática)
23. [Estructura de Proyecto](#23-estructura-de-proyecto)
24. [Despliegue](#24-despliegue)
25. [Buenas Prácticas](#25-buenas-prácticas)

---

## 1. Introducción a FastAPI

FastAPI es un framework moderno y de alto rendimiento para construir APIs con Python 3.7+ basado en las anotaciones de tipo estándar de Python. Fue creado por Sebastián Ramírez y es uno de los frameworks más populares del ecosistema Python.

### Características principales:

- **Rápido**: Rendimiento a la par de Node.js y Go gracias a Starlette y Pydantic
- **Rápido de programar**: Aumenta la velocidad de desarrollo entre un 200% y 300%
- **Menos errores**: Reduce los errores humanos gracias al sistema de tipos
- **Intuitivo**: Gran soporte en editores con autocompletado en todas partes
- **Fácil de aprender**: Diseñado para ser fácil de usar y aprender
- **Robusto**: Código listo para producción con documentación automática interactiva
- **Basado en estándares**: Compatible con OpenAPI y JSON Schema

### ¿Por qué FastAPI sobre otros frameworks?

| Característica | FastAPI | Flask | Django REST |
|----------------|---------|-------|-------------|
| Rendimiento | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ |
| Async nativo | ✅ | ❌ | ❌ |
| Validación automática | ✅ | ❌ | Parcial |
| Documentación auto | ✅ | ❌ | Parcial |
| Tipado | ✅ | ❌ | Parcial |
| Curva aprendizaje | Baja | Baja | Alta |

### Arquitectura de FastAPI

FastAPI se construye sobre dos pilares fundamentales:

- **Starlette**: Framework ASGI de alto rendimiento que proporciona el sistema de rutas, middleware, WebSockets y más
- **Pydantic**: Librería de validación de datos que utiliza las anotaciones de tipo de Python para validar y serializar datos

---

## 2. Instalación y Configuración

### Requisitos previos

- Python 3.7 o superior
- pip (gestor de paquetes de Python)

### Instalación

```bash
# Crear y activar entorno virtual
python -m venv venv

# Activar entorno (Linux/Mac)
source venv/bin/activate

# Activar entorno (Windows)
venv\Scripts\activate

# Instalar FastAPI con todas las dependencias
pip install "fastapi[all]"

# O instalar por separado
pip install fastapi
pip install "uvicorn[standard]"
```

### ¿Qué incluye `fastapi[all]`?

- **uvicorn**: Servidor ASGI de alto rendimiento
- **pydantic**: Validación de datos
- **python-multipart**: Soporte para formularios
- **jinja2**: Motor de plantillas
- **httpx**: Cliente HTTP async

### Verificar instalación

```bash
# Verificar FastAPI
python -c "import fastapi; print(fastapi.__version__)"

# Verificar Uvicorn
uvicorn --version
```

### Dependencias útiles adicionales

```bash
# ORM para bases de datos
pip install sqlalchemy

# SQLModel (ORM creado por el mismo autor de FastAPI)
pip install sqlmodel

# Driver para PostgreSQL
pip install psycopg2-binary

# Driver para MySQL
pip install pymysql

# Autenticación JWT
pip install python-jose[cryptography]
pip install passlib[bcrypt]

# Testing
pip install pytest httpx

# Variables de entorno
pip install python-dotenv

# Migraciones de base de datos
pip install alembic
```

---

## 3. Primera Aplicación

### Hola Mundo con FastAPI

```python
# main.py
from fastapi import FastAPI

# Crear instancia de la aplicación
app = FastAPI()

# Definir una ruta
@app.get("/")
def root():
    return {"mensaje": "¡Hola, Mundo!"}
```

### Ejecutar la aplicación

```bash
# Ejecutar con uvicorn
uvicorn main:app --reload

# main = nombre del archivo (main.py)
# app = nombre de la instancia FastAPI
# --reload = recarga automática al detectar cambios (solo desarrollo)
```

### Acceder a la aplicación

- **API**: `http://127.0.0.1:8000`
- **Documentación Swagger**: `http://127.0.0.1:8000/docs`
- **Documentación ReDoc**: `http://127.0.0.1:8000/redoc`

### Configurar la aplicación con metadatos

```python
from fastapi import FastAPI

app = FastAPI(
    title="Mi API",
    description="API de ejemplo con FastAPI",
    version="1.0.0",
    docs_url="/docs",
    redoc_url="/redoc",
    contact={
        "name": "Desarrollador",
        "email": "dev@example.com",
    },
    license_info={
        "name": "MIT",
    },
)

@app.get("/")
def root():
    return {"mensaje": "¡Hola, Mundo!"}
```

### Ejecutar con configuración personalizada

```bash
# Cambiar puerto y host
uvicorn main:app --host 0.0.0.0 --port 8080 --reload

# Con múltiples workers (producción)
uvicorn main:app --host 0.0.0.0 --port 8000 --workers 4
```

---

## 4. Rutas y Métodos HTTP

### Métodos HTTP disponibles

```python
from fastapi import FastAPI

app = FastAPI()

# GET - Obtener datos
@app.get("/items")
def obtener_items():
    return {"items": ["item1", "item2"]}

# POST - Crear datos
@app.post("/items")
def crear_item():
    return {"mensaje": "Item creado"}

# PUT - Actualizar datos (reemplazo completo)
@app.put("/items/{item_id}")
def actualizar_item(item_id: int):
    return {"mensaje": f"Item {item_id} actualizado"}

# PATCH - Actualizar datos (parcial)
@app.patch("/items/{item_id}")
def actualizar_parcial_item(item_id: int):
    return {"mensaje": f"Item {item_id} parcialmente actualizado"}

# DELETE - Eliminar datos
@app.delete("/items/{item_id}")
def eliminar_item(item_id: int):
    return {"mensaje": f"Item {item_id} eliminado"}
```

### Agrupar rutas con Tags

```python
@app.get("/usuarios", tags=["Usuarios"])
def listar_usuarios():
    return {"usuarios": []}

@app.post("/usuarios", tags=["Usuarios"])
def crear_usuario():
    return {"mensaje": "Usuario creado"}

@app.get("/productos", tags=["Productos"])
def listar_productos():
    return {"productos": []}
```

### Agrupar rutas con APIRouter

```python
# routers/usuarios.py
from fastapi import APIRouter

router = APIRouter(
    prefix="/usuarios",
    tags=["Usuarios"],
    responses={404: {"description": "No encontrado"}},
)

@router.get("/")
def listar_usuarios():
    return {"usuarios": []}

@router.get("/{usuario_id}")
def obtener_usuario(usuario_id: int):
    return {"usuario_id": usuario_id}

@router.post("/")
def crear_usuario():
    return {"mensaje": "Usuario creado"}
```

```python
# main.py
from fastapi import FastAPI
from routers import usuarios

app = FastAPI()

# Incluir el router
app.include_router(usuarios.router)
```

### Múltiples routers

```python
# main.py
from fastapi import FastAPI
from routers import usuarios, productos, pedidos

app = FastAPI()

app.include_router(usuarios.router)
app.include_router(productos.router)
app.include_router(pedidos.router)
```

---

## 5. Parámetros de Ruta

### Parámetros básicos

```python
from fastapi import FastAPI

app = FastAPI()

# Parámetro de ruta con tipo
@app.get("/usuarios/{usuario_id}")
def obtener_usuario(usuario_id: int):
    return {"usuario_id": usuario_id}

# Parámetro de tipo string
@app.get("/archivos/{nombre_archivo}")
def obtener_archivo(nombre_archivo: str):
    return {"nombre": nombre_archivo}
```

### Orden de las rutas (importa)

```python
# ⚠️ El orden de las rutas importa
# Rutas fijas ANTES que rutas con parámetros

@app.get("/usuarios/me")     # Esta va PRIMERO
def obtener_usuario_actual():
    return {"usuario": "Yo mismo"}

@app.get("/usuarios/{usuario_id}")   # Esta va DESPUÉS
def obtener_usuario(usuario_id: int):
    return {"usuario_id": usuario_id}

# Si se invierte el orden, /usuarios/me intentaría
# convertir "me" a int y daría error
```

### Parámetros con Enum

```python
from enum import Enum
from fastapi import FastAPI

class RolUsuario(str, Enum):
    admin = "admin"
    usuario = "usuario"
    moderador = "moderador"

app = FastAPI()

@app.get("/usuarios/rol/{rol}")
def obtener_por_rol(rol: RolUsuario):
    if rol == RolUsuario.admin:
        return {"rol": rol, "permisos": "todos"}
    return {"rol": rol, "permisos": "limitados"}

# Solo acepta: admin, usuario, moderador
# Cualquier otro valor devuelve un error de validación automático
```

### Parámetros de ruta con validación

```python
from fastapi import FastAPI, Path

app = FastAPI()

@app.get("/items/{item_id}")
def obtener_item(
    item_id: int = Path(
        title="ID del Item",
        description="El ID único del item a buscar",
        ge=1,          # Mayor o igual a 1
        le=1000,       # Menor o igual a 1000
        examples=[42]
    )
):
    return {"item_id": item_id}
```

### Parámetros de ruta tipo path

```python
# Capturar rutas completas como parámetro
@app.get("/archivos/{ruta_archivo:path}")
def obtener_archivo(ruta_archivo: str):
    return {"ruta": ruta_archivo}

# GET /archivos/home/user/documento.txt
# -> {"ruta": "home/user/documento.txt"}
```

---

## 6. Parámetros de Query

### Parámetros de query básicos

```python
from fastapi import FastAPI

app = FastAPI()

# Los parámetros de query se definen como argumentos de la función
# que NO están en la ruta
@app.get("/items")
def listar_items(skip: int = 0, limit: int = 10):
    return {"skip": skip, "limit": limit}

# GET /items?skip=0&limit=10
# GET /items?skip=20&limit=50
# GET /items  -> usa valores por defecto: skip=0, limit=10
```

### Parámetros opcionales

```python
from typing import Optional

@app.get("/items")
def listar_items(
    skip: int = 0,
    limit: int = 10,
    buscar: Optional[str] = None    # Parámetro opcional
):
    resultado = {"skip": skip, "limit": limit}
    if buscar:
        resultado["buscar"] = buscar
    return resultado

# GET /items?buscar=laptop
# GET /items  -> buscar será None
```

### Parámetros de query con validación

```python
from fastapi import FastAPI, Query

app = FastAPI()

@app.get("/items")
def listar_items(
    q: str = Query(
        default=None,
        min_length=3,           # Mínimo 3 caracteres
        max_length=50,          # Máximo 50 caracteres
        pattern="^[a-zA-Z]+$",  # Solo letras
        title="Búsqueda",
        description="Texto para buscar items",
        examples=["laptop"]
    ),
    skip: int = Query(default=0, ge=0),        # >= 0
    limit: int = Query(default=10, ge=1, le=100),  # Entre 1 y 100
):
    return {"q": q, "skip": skip, "limit": limit}
```

### Parámetros de query requeridos

```python
@app.get("/items")
def listar_items(
    categoria: str,               # Requerido (sin valor por defecto)
    orden: str = "asc",           # Opcional con valor por defecto
    pagina: Optional[str] = None  # Opcional, puede ser None
):
    return {
        "categoria": categoria,
        "orden": orden,
        "pagina": pagina
    }

# GET /items?categoria=electrónica         -> OK
# GET /items                                -> Error: falta 'categoria'
```

### Lista de parámetros de query

```python
from typing import List

@app.get("/items")
def listar_items(
    tags: List[str] = Query(default=[])
):
    return {"tags": tags}

# GET /items?tags=python&tags=fastapi&tags=api
# -> {"tags": ["python", "fastapi", "api"]}
```

### Combinar parámetros de ruta y query

```python
@app.get("/usuarios/{usuario_id}/items")
def obtener_items_usuario(
    usuario_id: int,             # Parámetro de ruta
    skip: int = 0,               # Parámetro de query
    limit: int = 10,             # Parámetro de query
    activo: bool = True          # Parámetro de query
):
    return {
        "usuario_id": usuario_id,
        "skip": skip,
        "limit": limit,
        "activo": activo
    }

# GET /usuarios/1/items?skip=0&limit=5&activo=true
```

---

## 7. Request Body y Modelos Pydantic

### Modelos Pydantic básicos

```python
from fastapi import FastAPI
from pydantic import BaseModel
from typing import Optional

app = FastAPI()

# Definir un modelo Pydantic
class Item(BaseModel):
    nombre: str
    descripcion: Optional[str] = None
    precio: float
    en_oferta: bool = False

# Usar el modelo como request body
@app.post("/items")
def crear_item(item: Item):
    return {
        "mensaje": "Item creado",
        "item": item
    }

# FastAPI automáticamente:
# 1. Lee el body de la petición como JSON
# 2. Valida los tipos de datos
# 3. Convierte los datos al modelo
# 4. Genera documentación automática
```

### JSON que acepta el endpoint

```json
{
    "nombre": "Laptop Gaming",
    "descripcion": "Laptop para juegos de alta gama",
    "precio": 1299.99,
    "en_oferta": true
}
```

### Modelos anidados

```python
from pydantic import BaseModel
from typing import Optional, List

class Direccion(BaseModel):
    calle: str
    ciudad: str
    codigo_postal: str
    pais: str = "España"

class Usuario(BaseModel):
    nombre: str
    email: str
    edad: Optional[int] = None
    direccion: Direccion                    # Modelo anidado
    intereses: List[str] = []              # Lista de strings

@app.post("/usuarios")
def crear_usuario(usuario: Usuario):
    return {"usuario": usuario}
```

```json
{
    "nombre": "María García",
    "email": "maria@example.com",
    "edad": 28,
    "direccion": {
        "calle": "Calle Mayor 1",
        "ciudad": "Madrid",
        "codigo_postal": "28001",
        "pais": "España"
    },
    "intereses": ["programación", "música"]
}
```

### Combinar body con parámetros de ruta y query

```python
@app.put("/items/{item_id}")
def actualizar_item(
    item_id: int,                    # Parámetro de ruta
    item: Item,                      # Request body
    importancia: int = Query(ge=1)   # Parámetro de query
):
    return {
        "item_id": item_id,
        "item": item,
        "importancia": importancia
    }
```

### Múltiples modelos en el body

```python
from fastapi import Body

class Item(BaseModel):
    nombre: str
    precio: float

class Usuario(BaseModel):
    nombre: str
    email: str

@app.post("/pedidos")
def crear_pedido(
    item: Item,
    usuario: Usuario,
    cantidad: int = Body(ge=1)   # Valor embebido en el body
):
    return {
        "item": item,
        "usuario": usuario,
        "cantidad": cantidad
    }
```

```json
{
    "item": {
        "nombre": "Laptop",
        "precio": 999.99
    },
    "usuario": {
        "nombre": "Juan",
        "email": "juan@example.com"
    },
    "cantidad": 2
}
```

### Modelo con ejemplo en la documentación

```python
class Item(BaseModel):
    nombre: str
    descripcion: Optional[str] = None
    precio: float
    en_oferta: bool = False

    model_config = {
        "json_schema_extra": {
            "examples": [
                {
                    "nombre": "Laptop Gaming",
                    "descripcion": "Laptop de alta gama",
                    "precio": 1299.99,
                    "en_oferta": True
                }
            ]
        }
    }
```

---

## 8. Validación de Datos

### Validación con Pydantic

```python
from pydantic import BaseModel, Field, field_validator, model_validator
from typing import Optional

class Producto(BaseModel):
    nombre: str = Field(
        min_length=2,
        max_length=100,
        description="Nombre del producto"
    )
    precio: float = Field(
        gt=0,
        description="Precio del producto (mayor que 0)"
    )
    descuento: float = Field(
        default=0,
        ge=0,
        le=100,
        description="Porcentaje de descuento (0-100)"
    )
    stock: int = Field(
        ge=0,
        description="Cantidad en stock"
    )
    categoria: str
    tags: list[str] = Field(default_factory=list)
```

### Validadores personalizados

```python
from pydantic import BaseModel, field_validator, model_validator

class Usuario(BaseModel):
    nombre: str
    email: str
    edad: int
    password: str
    confirmar_password: str

    # Validador de un campo específico
    @field_validator("nombre")
    @classmethod
    def nombre_no_vacio(cls, v):
        if not v.strip():
            raise ValueError("El nombre no puede estar vacío")
        return v.strip().title()

    @field_validator("email")
    @classmethod
    def email_valido(cls, v):
        if "@" not in v:
            raise ValueError("El email debe contener @")
        return v.lower()

    @field_validator("edad")
    @classmethod
    def edad_valida(cls, v):
        if v < 18 or v > 120:
            raise ValueError("La edad debe estar entre 18 y 120")
        return v

    @field_validator("password")
    @classmethod
    def password_segura(cls, v):
        if len(v) < 8:
            raise ValueError("La contraseña debe tener al menos 8 caracteres")
        if not any(c.isupper() for c in v):
            raise ValueError("La contraseña debe tener al menos una mayúscula")
        if not any(c.isdigit() for c in v):
            raise ValueError("La contraseña debe tener al menos un número")
        return v

    # Validador que accede a múltiples campos
    @model_validator(mode="after")
    def passwords_coinciden(self):
        if self.password != self.confirmar_password:
            raise ValueError("Las contraseñas no coinciden")
        return self
```

### Validación en parámetros de ruta y query

```python
from fastapi import FastAPI, Path, Query

app = FastAPI()

@app.get("/items/{item_id}")
def obtener_item(
    item_id: int = Path(
        ge=1,
        le=10000,
        description="ID del item"
    ),
    q: str = Query(
        default=None,
        min_length=3,
        max_length=50,
        pattern="^[a-zA-Z0-9 ]+$",
        description="Búsqueda"
    ),
    precio_min: float = Query(default=0, ge=0),
    precio_max: float = Query(default=99999, le=99999),
):
    return {
        "item_id": item_id,
        "q": q,
        "precio_min": precio_min,
        "precio_max": precio_max
    }
```

### Respuestas de error de validación

Cuando la validación falla, FastAPI devuelve automáticamente un error 422 con detalles:

```json
{
    "detail": [
        {
            "type": "value_error",
            "loc": ["body", "email"],
            "msg": "Value error, El email debe contener @",
            "input": "email-invalido",
            "url": "https://errors.pydantic.dev/2/v/value_error"
        }
    ]
}
```

---

## 9. Modelos de Respuesta

### Definir modelos de respuesta

```python
from pydantic import BaseModel
from typing import Optional, List

# Modelo de entrada (lo que envía el usuario)
class UsuarioCrear(BaseModel):
    nombre: str
    email: str
    password: str

# Modelo de salida (lo que devuelve la API)
class UsuarioRespuesta(BaseModel):
    id: int
    nombre: str
    email: str
    # ⚠️ NO incluye password por seguridad

# Usar response_model para filtrar la respuesta
@app.post("/usuarios", response_model=UsuarioRespuesta)
def crear_usuario(usuario: UsuarioCrear):
    # Simular guardado en base de datos
    nuevo_usuario = {
        "id": 1,
        "nombre": usuario.nombre,
        "email": usuario.email,
        "password": usuario.password  # Esto NO se devuelve
    }
    return nuevo_usuario
# FastAPI filtra automáticamente y solo devuelve los campos de UsuarioRespuesta
```

### Respuestas con listas

```python
class ItemRespuesta(BaseModel):
    id: int
    nombre: str
    precio: float

@app.get("/items", response_model=List[ItemRespuesta])
def listar_items():
    return [
        {"id": 1, "nombre": "Laptop", "precio": 999.99},
        {"id": 2, "nombre": "Mouse", "precio": 29.99},
    ]
```

### Códigos de estado HTTP

```python
from fastapi import FastAPI, status

app = FastAPI()

@app.post("/items", status_code=status.HTTP_201_CREATED)
def crear_item(item: Item):
    return {"mensaje": "Item creado", "item": item}

@app.delete("/items/{item_id}", status_code=status.HTTP_204_NO_CONTENT)
def eliminar_item(item_id: int):
    # Eliminar item...
    return None

@app.get("/items/{item_id}", status_code=200)
def obtener_item(item_id: int):
    return {"item_id": item_id}
```

### Excluir valores no establecidos

```python
class ItemActualizar(BaseModel):
    nombre: Optional[str] = None
    precio: Optional[float] = None
    descripcion: Optional[str] = None

@app.patch(
    "/items/{item_id}",
    response_model=ItemRespuesta,
    response_model_exclude_unset=True   # Excluye campos no enviados
)
def actualizar_parcial(item_id: int, item: ItemActualizar):
    item_guardado = obtener_item_db(item_id)
    datos_actualizar = item.model_dump(exclude_unset=True)
    item_actualizado = item_guardado.model_copy(update=datos_actualizar)
    return item_actualizado
```

### Múltiples respuestas posibles

```python
from fastapi.responses import JSONResponse

@app.get(
    "/items/{item_id}",
    response_model=ItemRespuesta,
    responses={
        200: {"description": "Item encontrado"},
        404: {"description": "Item no encontrado"},
        500: {"description": "Error interno del servidor"},
    }
)
def obtener_item(item_id: int):
    item = buscar_item(item_id)
    if not item:
        return JSONResponse(
            status_code=404,
            content={"mensaje": "Item no encontrado"}
        )
    return item
```

---

## 10. Manejo de Formularios y Archivos

### Recibir datos de formularios

```python
from fastapi import FastAPI, Form

app = FastAPI()

# Recibir datos de un formulario HTML
@app.post("/login")
def login(
    username: str = Form(),
    password: str = Form()
):
    return {"username": username}

# Requiere instalar python-multipart:
# pip install python-multipart
```

### Subir archivos

```python
from fastapi import FastAPI, File, UploadFile

app = FastAPI()

# Subir un archivo
@app.post("/subir-archivo")
async def subir_archivo(archivo: UploadFile):
    contenido = await archivo.read()
    return {
        "nombre": archivo.filename,
        "tipo": archivo.content_type,
        "tamaño": len(contenido)
    }

# Subir múltiples archivos
@app.post("/subir-archivos")
async def subir_archivos(archivos: list[UploadFile]):
    nombres = [archivo.filename for archivo in archivos]
    return {"archivos_subidos": nombres}
```

### Guardar archivo en disco

```python
import shutil
from pathlib import Path

DIRECTORIO_SUBIDAS = Path("subidas")
DIRECTORIO_SUBIDAS.mkdir(exist_ok=True)

@app.post("/subir")
async def subir_y_guardar(archivo: UploadFile):
    ruta_destino = DIRECTORIO_SUBIDAS / archivo.filename

    with open(ruta_destino, "wb") as buffer:
        shutil.copyfileobj(archivo.file, buffer)

    return {
        "nombre": archivo.filename,
        "ruta": str(ruta_destino)
    }
```

### Formulario con archivos combinados

```python
@app.post("/crear-perfil")
async def crear_perfil(
    nombre: str = Form(),
    email: str = Form(),
    foto: UploadFile = File(default=None)
):
    resultado = {"nombre": nombre, "email": email}
    if foto:
        resultado["foto"] = foto.filename
    return resultado
```

---

## 11. Manejo de Errores y Excepciones

### HTTPException

```python
from fastapi import FastAPI, HTTPException, status

app = FastAPI()

items_db = {"1": "Laptop", "2": "Mouse"}

@app.get("/items/{item_id}")
def obtener_item(item_id: str):
    if item_id not in items_db:
        raise HTTPException(
            status_code=status.HTTP_404_NOT_FOUND,
            detail="Item no encontrado",
            headers={"X-Error": "Item no existe"}  # Headers opcionales
        )
    return {"item": items_db[item_id]}
```

### Excepciones personalizadas

```python
from fastapi import FastAPI, Request
from fastapi.responses import JSONResponse

app = FastAPI()

# Definir excepción personalizada
class ItemNoEncontradoError(Exception):
    def __init__(self, item_id: int):
        self.item_id = item_id

# Registrar manejador de excepción
@app.exception_handler(ItemNoEncontradoError)
async def item_no_encontrado_handler(request: Request, exc: ItemNoEncontradoError):
    return JSONResponse(
        status_code=404,
        content={
            "error": "Item no encontrado",
            "item_id": exc.item_id,
            "mensaje": f"El item con ID {exc.item_id} no existe"
        }
    )

@app.get("/items/{item_id}")
def obtener_item(item_id: int):
    item = buscar_item_en_db(item_id)
    if not item:
        raise ItemNoEncontradoError(item_id=item_id)
    return item
```

### Manejador global de errores

```python
from fastapi import FastAPI, Request
from fastapi.responses import JSONResponse
import traceback

app = FastAPI()

@app.exception_handler(Exception)
async def error_global_handler(request: Request, exc: Exception):
    return JSONResponse(
        status_code=500,
        content={
            "error": "Error interno del servidor",
            "detalle": str(exc),
            "ruta": str(request.url)
        }
    )
```

### Sobrescribir manejadores por defecto

```python
from fastapi.exceptions import RequestValidationError
from starlette.exceptions import HTTPException as StarletteHTTPException

@app.exception_handler(StarletteHTTPException)
async def http_exception_handler(request: Request, exc: StarletteHTTPException):
    return JSONResponse(
        status_code=exc.status_code,
        content={
            "error": True,
            "codigo": exc.status_code,
            "mensaje": exc.detail
        }
    )

@app.exception_handler(RequestValidationError)
async def validation_exception_handler(request: Request, exc: RequestValidationError):
    return JSONResponse(
        status_code=422,
        content={
            "error": True,
            "mensaje": "Error de validación",
            "detalles": exc.errors()
        }
    )
```

---

## 12. Dependency Injection

La inyección de dependencias es uno de los sistemas más potentes de FastAPI. Permite reutilizar lógica, gestionar recursos y organizar el código de forma limpia.

### Dependencia básica

```python
from fastapi import FastAPI, Depends

app = FastAPI()

# Definir una dependencia como función
def parametros_comunes(skip: int = 0, limit: int = 100):
    return {"skip": skip, "limit": limit}

# Usar la dependencia con Depends()
@app.get("/items")
def listar_items(params: dict = Depends(parametros_comunes)):
    return {"params": params}

@app.get("/usuarios")
def listar_usuarios(params: dict = Depends(parametros_comunes)):
    return {"params": params}

# Ambos endpoints comparten la misma lógica de paginación
```

### Dependencias como clases

```python
class Paginacion:
    def __init__(self, skip: int = 0, limit: int = 10):
        self.skip = skip
        self.limit = limit

@app.get("/items")
def listar_items(paginacion: Paginacion = Depends()):
    return {
        "skip": paginacion.skip,
        "limit": paginacion.limit
    }
```

### Dependencias anidadas

```python
def obtener_db():
    """Obtener conexión a la base de datos"""
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

def obtener_usuario_actual(db = Depends(obtener_db)):
    """Obtener el usuario actual de la base de datos"""
    usuario = db.query(Usuario).first()
    if not usuario:
        raise HTTPException(status_code=401, detail="No autenticado")
    return usuario

@app.get("/perfil")
def obtener_perfil(usuario = Depends(obtener_usuario_actual)):
    return {"usuario": usuario.nombre}

# FastAPI resuelve la cadena:
# obtener_db -> obtener_usuario_actual -> obtener_perfil
```

### Dependencias con yield (recursos que necesitan limpieza)

```python
from fastapi import Depends

# Dependencia con yield para gestionar recursos
async def obtener_conexion_db():
    conexion = await crear_conexion()
    try:
        yield conexion         # El endpoint usa la conexión
    finally:
        await conexion.close() # Se ejecuta después del endpoint

@app.get("/datos")
async def obtener_datos(db = Depends(obtener_conexion_db)):
    datos = await db.fetch("SELECT * FROM tabla")
    return {"datos": datos}
```

### Dependencias a nivel de ruta, router o aplicación

```python
# Dependencia que verifica una API key
async def verificar_api_key(api_key: str = Query()):
    if api_key != "mi-api-key-secreta":
        raise HTTPException(status_code=403, detail="API key inválida")

# A nivel de ruta
@app.get("/protegido", dependencies=[Depends(verificar_api_key)])
def ruta_protegida():
    return {"mensaje": "Acceso concedido"}

# A nivel de router
router = APIRouter(
    prefix="/admin",
    dependencies=[Depends(verificar_api_key)]
)

# A nivel de aplicación (afecta a TODAS las rutas)
app = FastAPI(dependencies=[Depends(verificar_api_key)])
```

---

## 13. Middleware

Los middleware son funciones que se ejecutan antes y después de cada petición. Son útiles para logging, CORS, autenticación global, etc.

### Middleware personalizado

```python
from fastapi import FastAPI, Request
import time

app = FastAPI()

@app.middleware("http")
async def middleware_tiempo_respuesta(request: Request, call_next):
    inicio = time.time()

    # Procesar la petición
    response = await call_next(request)

    # Calcular tiempo de proceso
    tiempo_proceso = time.time() - inicio
    response.headers["X-Tiempo-Proceso"] = str(tiempo_proceso)

    return response
```

### Middleware de logging

```python
import logging

logger = logging.getLogger("api")

@app.middleware("http")
async def middleware_logging(request: Request, call_next):
    logger.info(f"Petición: {request.method} {request.url}")

    response = await call_next(request)

    logger.info(f"Respuesta: {response.status_code}")

    return response
```

### Middleware con clases (BaseHTTPMiddleware)

```python
from starlette.middleware.base import BaseHTTPMiddleware
from fastapi import Request

class MiMiddleware(BaseHTTPMiddleware):
    def __init__(self, app, parametro_custom: str = "valor"):
        super().__init__(app)
        self.parametro_custom = parametro_custom

    async def dispatch(self, request: Request, call_next):
        # Lógica antes de la petición
        print(f"Parámetro: {self.parametro_custom}")

        response = await call_next(request)

        # Lógica después de la petición
        response.headers["X-Custom"] = self.parametro_custom

        return response

# Registrar el middleware
app.add_middleware(MiMiddleware, parametro_custom="mi-valor")
```

### Middleware de Trusted Hosts

```python
from starlette.middleware.trustedhost import TrustedHostMiddleware

app.add_middleware(
    TrustedHostMiddleware,
    allowed_hosts=["example.com", "*.example.com"]
)
```

---

## 14. CORS (Cross-Origin Resource Sharing)

CORS es necesario cuando tu frontend y backend están en dominios diferentes. Sin configurar CORS, el navegador bloqueará las peticiones del frontend a la API.

### Configurar CORS

```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

# Orígenes permitidos
origenes = [
    "http://localhost:3000",       # React en desarrollo
    "http://localhost:5173",       # Vite en desarrollo
    "https://mi-frontend.com",    # Frontend en producción
]

app.add_middleware(
    CORSMiddleware,
    allow_origins=origenes,              # Orígenes permitidos
    allow_credentials=True,              # Permitir cookies
    allow_methods=["*"],                 # Métodos HTTP permitidos
    allow_headers=["*"],                 # Headers permitidos
    expose_headers=["X-Custom-Header"],  # Headers visibles al frontend
    max_age=600,                         # Cache de preflight (segundos)
)
```

### CORS permisivo (solo desarrollo)

```python
# ⚠️ Solo para desarrollo, NUNCA en producción
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],         # Permite TODO
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

### CORS restrictivo (producción)

```python
app.add_middleware(
    CORSMiddleware,
    allow_origins=["https://mi-dominio.com"],
    allow_credentials=True,
    allow_methods=["GET", "POST", "PUT", "DELETE"],
    allow_headers=["Authorization", "Content-Type"],
)
```

---

## 15. Autenticación y Autorización

### Autenticación con JWT (JSON Web Tokens)

```bash
# Instalar dependencias
pip install python-jose[cryptography] passlib[bcrypt]
```

### Configuración de seguridad

```python
from datetime import datetime, timedelta, timezone
from typing import Optional
from jose import JWTError, jwt
from passlib.context import CryptContext
from pydantic import BaseModel
from fastapi import Depends, HTTPException, status
from fastapi.security import OAuth2PasswordBearer, OAuth2PasswordRequestForm

# Configuración
SECRET_KEY = "tu-clave-secreta-muy-segura-cambiar-en-produccion"
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30

# Contexto para hash de contraseñas
pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")

# Esquema OAuth2
oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")
```

### Modelos de autenticación

```python
class Token(BaseModel):
    access_token: str
    token_type: str

class TokenData(BaseModel):
    username: Optional[str] = None

class Usuario(BaseModel):
    username: str
    email: str
    nombre_completo: Optional[str] = None
    deshabilitado: Optional[bool] = None

class UsuarioDB(Usuario):
    hashed_password: str
```

### Funciones de utilidad

```python
# Verificar contraseña
def verificar_password(password_plano: str, password_hash: str) -> bool:
    return pwd_context.verify(password_plano, password_hash)

# Crear hash de contraseña
def crear_hash_password(password: str) -> str:
    return pwd_context.hash(password)

# Crear token JWT
def crear_token_acceso(data: dict, expires_delta: Optional[timedelta] = None) -> str:
    to_encode = data.copy()
    if expires_delta:
        expire = datetime.now(timezone.utc) + expires_delta
    else:
        expire = datetime.now(timezone.utc) + timedelta(minutes=15)
    to_encode.update({"exp": expire})
    encoded_jwt = jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)
    return encoded_jwt
```

### Dependencia para obtener usuario actual

```python
async def obtener_usuario_actual(token: str = Depends(oauth2_scheme)):
    exception_credenciales = HTTPException(
        status_code=status.HTTP_401_UNAUTHORIZED,
        detail="Credenciales inválidas",
        headers={"WWW-Authenticate": "Bearer"},
    )
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        username: str = payload.get("sub")
        if username is None:
            raise exception_credenciales
        token_data = TokenData(username=username)
    except JWTError:
        raise exception_credenciales

    usuario = obtener_usuario_db(username=token_data.username)
    if usuario is None:
        raise exception_credenciales
    return usuario

async def obtener_usuario_activo(
    usuario_actual: Usuario = Depends(obtener_usuario_actual)
):
    if usuario_actual.deshabilitado:
        raise HTTPException(status_code=400, detail="Usuario deshabilitado")
    return usuario_actual
```

### Endpoints de autenticación

```python
from fastapi import FastAPI

app = FastAPI()

@app.post("/token", response_model=Token)
async def login(form_data: OAuth2PasswordRequestForm = Depends()):
    # Buscar usuario en la base de datos
    usuario = autenticar_usuario(form_data.username, form_data.password)
    if not usuario:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Usuario o contraseña incorrectos",
            headers={"WWW-Authenticate": "Bearer"},
        )

    # Crear token de acceso
    access_token_expires = timedelta(minutes=ACCESS_TOKEN_EXPIRE_MINUTES)
    access_token = crear_token_acceso(
        data={"sub": usuario.username},
        expires_delta=access_token_expires
    )
    return {"access_token": access_token, "token_type": "bearer"}

# Ruta protegida
@app.get("/usuarios/me", response_model=Usuario)
async def leer_perfil(usuario_actual: Usuario = Depends(obtener_usuario_activo)):
    return usuario_actual

# Ruta protegida con rol específico
@app.get("/admin")
async def panel_admin(usuario_actual: Usuario = Depends(obtener_usuario_activo)):
    if usuario_actual.rol != "admin":
        raise HTTPException(
            status_code=status.HTTP_403_FORBIDDEN,
            detail="No tienes permisos de administrador"
        )
    return {"mensaje": "Bienvenido al panel de administración"}
```

### Autorización por roles

```python
from functools import wraps

def requiere_rol(roles_permitidos: list[str]):
    """Dependencia que verifica el rol del usuario"""
    async def verificar_rol(usuario: Usuario = Depends(obtener_usuario_activo)):
        if usuario.rol not in roles_permitidos:
            raise HTTPException(
                status_code=status.HTTP_403_FORBIDDEN,
                detail=f"Se requiere uno de los siguientes roles: {roles_permitidos}"
            )
        return usuario
    return verificar_rol

# Usar la dependencia
@app.get("/admin/usuarios")
async def listar_usuarios(
    usuario: Usuario = Depends(requiere_rol(["admin"]))
):
    return {"usuarios": []}

@app.get("/contenido")
async def contenido_premium(
    usuario: Usuario = Depends(requiere_rol(["admin", "premium"]))
):
    return {"contenido": "premium"}
```

---

## 16. Bases de Datos con SQLAlchemy

### Configuración inicial

```python
# database.py
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# URL de conexión
# SQLite
SQLALCHEMY_DATABASE_URL = "sqlite:///./app.db"

# PostgreSQL
# SQLALCHEMY_DATABASE_URL = "postgresql://user:password@localhost/dbname"

# MySQL
# SQLALCHEMY_DATABASE_URL = "mysql+pymysql://user:password@localhost/dbname"

# Crear el motor
engine = create_engine(
    SQLALCHEMY_DATABASE_URL,
    connect_args={"check_same_thread": False}  # Solo para SQLite
)

# Crear la sesión
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

# Base para los modelos
Base = declarative_base()
```

### Definir modelos (ORM)

```python
# models.py
from sqlalchemy import Column, Integer, String, Float, Boolean, ForeignKey, DateTime
from sqlalchemy.orm import relationship
from sqlalchemy.sql import func
from database import Base

class Usuario(Base):
    __tablename__ = "usuarios"

    id = Column(Integer, primary_key=True, index=True)
    nombre = Column(String(100), nullable=False)
    email = Column(String(100), unique=True, index=True, nullable=False)
    hashed_password = Column(String(200), nullable=False)
    activo = Column(Boolean, default=True)
    creado_en = Column(DateTime(timezone=True), server_default=func.now())

    # Relación con items
    items = relationship("Item", back_populates="propietario")

class Item(Base):
    __tablename__ = "items"

    id = Column(Integer, primary_key=True, index=True)
    nombre = Column(String(100), nullable=False)
    descripcion = Column(String(500))
    precio = Column(Float, nullable=False)
    propietario_id = Column(Integer, ForeignKey("usuarios.id"))

    # Relación con usuario
    propietario = relationship("Usuario", back_populates="items")
```

### Schemas Pydantic

```python
# schemas.py
from pydantic import BaseModel
from typing import Optional, List
from datetime import datetime

# --- Schemas de Item ---
class ItemBase(BaseModel):
    nombre: str
    descripcion: Optional[str] = None
    precio: float

class ItemCrear(ItemBase):
    pass

class ItemRespuesta(ItemBase):
    id: int
    propietario_id: int

    model_config = {"from_attributes": True}

# --- Schemas de Usuario ---
class UsuarioBase(BaseModel):
    nombre: str
    email: str

class UsuarioCrear(UsuarioBase):
    password: str

class UsuarioRespuesta(UsuarioBase):
    id: int
    activo: bool
    items: List[ItemRespuesta] = []

    model_config = {"from_attributes": True}
```

### CRUD Operations

```python
# crud.py
from sqlalchemy.orm import Session
import models
import schemas

# --- CRUD de Usuarios ---
def obtener_usuario(db: Session, usuario_id: int):
    return db.query(models.Usuario).filter(
        models.Usuario.id == usuario_id
    ).first()

def obtener_usuario_por_email(db: Session, email: str):
    return db.query(models.Usuario).filter(
        models.Usuario.email == email
    ).first()

def obtener_usuarios(db: Session, skip: int = 0, limit: int = 100):
    return db.query(models.Usuario).offset(skip).limit(limit).all()

def crear_usuario(db: Session, usuario: schemas.UsuarioCrear):
    hashed_password = crear_hash_password(usuario.password)
    db_usuario = models.Usuario(
        nombre=usuario.nombre,
        email=usuario.email,
        hashed_password=hashed_password
    )
    db.add(db_usuario)
    db.commit()
    db.refresh(db_usuario)
    return db_usuario

def actualizar_usuario(db: Session, usuario_id: int, datos: dict):
    db_usuario = db.query(models.Usuario).filter(
        models.Usuario.id == usuario_id
    ).first()
    if db_usuario:
        for key, value in datos.items():
            setattr(db_usuario, key, value)
        db.commit()
        db.refresh(db_usuario)
    return db_usuario

def eliminar_usuario(db: Session, usuario_id: int):
    db_usuario = db.query(models.Usuario).filter(
        models.Usuario.id == usuario_id
    ).first()
    if db_usuario:
        db.delete(db_usuario)
        db.commit()
    return db_usuario

# --- CRUD de Items ---
def crear_item(db: Session, item: schemas.ItemCrear, usuario_id: int):
    db_item = models.Item(**item.model_dump(), propietario_id=usuario_id)
    db.add(db_item)
    db.commit()
    db.refresh(db_item)
    return db_item

def obtener_items(db: Session, skip: int = 0, limit: int = 100):
    return db.query(models.Item).offset(skip).limit(limit).all()
```

### Endpoints con base de datos

```python
# main.py
from fastapi import FastAPI, Depends, HTTPException
from sqlalchemy.orm import Session
import models
import schemas
import crud
from database import SessionLocal, engine

# Crear las tablas
models.Base.metadata.create_all(bind=engine)

app = FastAPI()

# Dependencia para obtener la sesión de BD
def obtener_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

# --- Endpoints de Usuarios ---
@app.post("/usuarios", response_model=schemas.UsuarioRespuesta)
def crear_usuario(usuario: schemas.UsuarioCrear, db: Session = Depends(obtener_db)):
    db_usuario = crud.obtener_usuario_por_email(db, email=usuario.email)
    if db_usuario:
        raise HTTPException(status_code=400, detail="Email ya registrado")
    return crud.crear_usuario(db=db, usuario=usuario)

@app.get("/usuarios", response_model=list[schemas.UsuarioRespuesta])
def listar_usuarios(skip: int = 0, limit: int = 100, db: Session = Depends(obtener_db)):
    return crud.obtener_usuarios(db, skip=skip, limit=limit)

@app.get("/usuarios/{usuario_id}", response_model=schemas.UsuarioRespuesta)
def obtener_usuario(usuario_id: int, db: Session = Depends(obtener_db)):
    db_usuario = crud.obtener_usuario(db, usuario_id=usuario_id)
    if db_usuario is None:
        raise HTTPException(status_code=404, detail="Usuario no encontrado")
    return db_usuario

# --- Endpoints de Items ---
@app.post("/usuarios/{usuario_id}/items", response_model=schemas.ItemRespuesta)
def crear_item_usuario(
    usuario_id: int,
    item: schemas.ItemCrear,
    db: Session = Depends(obtener_db)
):
    return crud.crear_item(db=db, item=item, usuario_id=usuario_id)

@app.get("/items", response_model=list[schemas.ItemRespuesta])
def listar_items(skip: int = 0, limit: int = 100, db: Session = Depends(obtener_db)):
    return crud.obtener_items(db, skip=skip, limit=limit)
```

---

## 17. Bases de Datos con SQLModel

SQLModel es una librería creada por el mismo autor de FastAPI (Sebastián Ramírez) que combina SQLAlchemy y Pydantic en una sola clase, simplificando mucho el código.

### Instalación

```bash
pip install sqlmodel
```

### Configuración

```python
# database.py
from sqlmodel import SQLModel, create_engine, Session

DATABASE_URL = "sqlite:///./app.db"

engine = create_engine(DATABASE_URL, echo=True)

def crear_tablas():
    SQLModel.metadata.create_all(engine)

def obtener_session():
    with Session(engine) as session:
        yield session
```

### Modelos con SQLModel

```python
# models.py
from sqlmodel import SQLModel, Field, Relationship
from typing import Optional, List
from datetime import datetime

# Un solo modelo sirve como tabla de BD Y como schema Pydantic

class UsuarioBase(SQLModel):
    nombre: str = Field(min_length=2, max_length=100)
    email: str = Field(unique=True, index=True)

class Usuario(UsuarioBase, table=True):  # table=True -> crea tabla en BD
    id: Optional[int] = Field(default=None, primary_key=True)
    hashed_password: str
    activo: bool = Field(default=True)

    items: List["Item"] = Relationship(back_populates="propietario")

class UsuarioCrear(UsuarioBase):
    password: str

class UsuarioRespuesta(UsuarioBase):
    id: int
    activo: bool

class ItemBase(SQLModel):
    nombre: str = Field(min_length=1, max_length=100)
    descripcion: Optional[str] = None
    precio: float = Field(gt=0)

class Item(ItemBase, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    propietario_id: int = Field(foreign_key="usuario.id")

    propietario: Optional[Usuario] = Relationship(back_populates="items")

class ItemCrear(ItemBase):
    pass

class ItemRespuesta(ItemBase):
    id: int
    propietario_id: int
```

### CRUD con SQLModel

```python
# main.py
from fastapi import FastAPI, Depends, HTTPException
from sqlmodel import Session, select
from database import crear_tablas, obtener_session
from models import Usuario, UsuarioCrear, UsuarioRespuesta, Item, ItemCrear, ItemRespuesta

app = FastAPI()

@app.on_event("startup")
def on_startup():
    crear_tablas()

# --- Usuarios ---
@app.post("/usuarios", response_model=UsuarioRespuesta)
def crear_usuario(usuario: UsuarioCrear, session: Session = Depends(obtener_session)):
    # Verificar si el email ya existe
    existente = session.exec(
        select(Usuario).where(Usuario.email == usuario.email)
    ).first()
    if existente:
        raise HTTPException(status_code=400, detail="Email ya registrado")

    db_usuario = Usuario(
        nombre=usuario.nombre,
        email=usuario.email,
        hashed_password=crear_hash_password(usuario.password)
    )
    session.add(db_usuario)
    session.commit()
    session.refresh(db_usuario)
    return db_usuario

@app.get("/usuarios", response_model=list[UsuarioRespuesta])
def listar_usuarios(
    skip: int = 0,
    limit: int = 100,
    session: Session = Depends(obtener_session)
):
    usuarios = session.exec(select(Usuario).offset(skip).limit(limit)).all()
    return usuarios

@app.get("/usuarios/{usuario_id}", response_model=UsuarioRespuesta)
def obtener_usuario(usuario_id: int, session: Session = Depends(obtener_session)):
    usuario = session.get(Usuario, usuario_id)
    if not usuario:
        raise HTTPException(status_code=404, detail="Usuario no encontrado")
    return usuario

@app.patch("/usuarios/{usuario_id}", response_model=UsuarioRespuesta)
def actualizar_usuario(
    usuario_id: int,
    datos: UsuarioBase,
    session: Session = Depends(obtener_session)
):
    usuario = session.get(Usuario, usuario_id)
    if not usuario:
        raise HTTPException(status_code=404, detail="Usuario no encontrado")

    datos_dict = datos.model_dump(exclude_unset=True)
    for key, value in datos_dict.items():
        setattr(usuario, key, value)

    session.add(usuario)
    session.commit()
    session.refresh(usuario)
    return usuario

@app.delete("/usuarios/{usuario_id}")
def eliminar_usuario(usuario_id: int, session: Session = Depends(obtener_session)):
    usuario = session.get(Usuario, usuario_id)
    if not usuario:
        raise HTTPException(status_code=404, detail="Usuario no encontrado")

    session.delete(usuario)
    session.commit()
    return {"mensaje": "Usuario eliminado"}
```

---

## 18. Async y Await

FastAPI soporta funciones asíncronas de forma nativa gracias a ASGI y Starlette.

### Cuándo usar async

```python
from fastapi import FastAPI

app = FastAPI()

# Función SÍNCRONA - para operaciones CPU-bound
# o cuando usas librerías síncronas (como SQLAlchemy sin async)
@app.get("/sync")
def endpoint_sincrono():
    # FastAPI lo ejecuta en un thread pool automáticamente
    return {"tipo": "síncrono"}

# Función ASÍNCRONA - para operaciones I/O-bound
# como llamadas a APIs externas, bases de datos async, etc.
@app.get("/async")
async def endpoint_asincrono():
    return {"tipo": "asíncrono"}
```

### Llamadas HTTP asíncronas

```python
import httpx
from fastapi import FastAPI

app = FastAPI()

@app.get("/datos-externos")
async def obtener_datos_externos():
    async with httpx.AsyncClient() as client:
        respuesta = await client.get("https://api.example.com/datos")
        return respuesta.json()

# Múltiples llamadas en paralelo
import asyncio

@app.get("/datos-multiples")
async def obtener_datos_multiples():
    async with httpx.AsyncClient() as client:
        # Ejecutar múltiples peticiones en paralelo
        resultados = await asyncio.gather(
            client.get("https://api.example.com/usuarios"),
            client.get("https://api.example.com/productos"),
            client.get("https://api.example.com/pedidos"),
        )
        return {
            "usuarios": resultados[0].json(),
            "productos": resultados[1].json(),
            "pedidos": resultados[2].json(),
        }
```

### Base de datos asíncrona con SQLAlchemy

```python
from sqlalchemy.ext.asyncio import create_async_engine, AsyncSession
from sqlalchemy.orm import sessionmaker

# Motor asíncrono
async_engine = create_async_engine(
    "postgresql+asyncpg://user:password@localhost/dbname",
    echo=True
)

# Sesión asíncrona
AsyncSessionLocal = sessionmaker(
    async_engine,
    class_=AsyncSession,
    expire_on_commit=False
)

# Dependencia asíncrona
async def obtener_db():
    async with AsyncSessionLocal() as session:
        yield session

# Uso en endpoint
@app.get("/usuarios")
async def listar_usuarios(db: AsyncSession = Depends(obtener_db)):
    result = await db.execute(select(Usuario))
    usuarios = result.scalars().all()
    return usuarios
```

### Eventos de vida de la aplicación

```python
from contextlib import asynccontextmanager

@asynccontextmanager
async def lifespan(app: FastAPI):
    # Código de inicialización (startup)
    print("Iniciando aplicación...")
    crear_tablas()
    yield
    # Código de limpieza (shutdown)
    print("Cerrando aplicación...")

app = FastAPI(lifespan=lifespan)
```

---

## 19. WebSockets

FastAPI soporta WebSockets para comunicación bidireccional en tiempo real.

### WebSocket básico

```python
from fastapi import FastAPI, WebSocket

app = FastAPI()

@app.websocket("/ws")
async def websocket_endpoint(websocket: WebSocket):
    await websocket.accept()
    while True:
        # Recibir mensaje del cliente
        datos = await websocket.receive_text()
        # Enviar respuesta
        await websocket.send_text(f"Mensaje recibido: {datos}")
```

### Cliente HTML para WebSocket

```python
from fastapi.responses import HTMLResponse

@app.get("/")
async def pagina_principal():
    return HTMLResponse("""
    <!DOCTYPE html>
    <html>
    <body>
        <h1>WebSocket Chat</h1>
        <input type="text" id="mensaje" placeholder="Escribe un mensaje">
        <button onclick="enviar()">Enviar</button>
        <ul id="mensajes"></ul>

        <script>
            const ws = new WebSocket("ws://localhost:8000/ws");

            ws.onmessage = function(event) {
                const li = document.createElement("li");
                li.textContent = event.data;
                document.getElementById("mensajes").appendChild(li);
            };

            function enviar() {
                const input = document.getElementById("mensaje");
                ws.send(input.value);
                input.value = "";
            }
        </script>
    </body>
    </html>
    """)
```

### Chat con múltiples usuarios

```python
from fastapi import FastAPI, WebSocket, WebSocketDisconnect

app = FastAPI()

class GestorConexiones:
    def __init__(self):
        self.conexiones_activas: list[WebSocket] = []

    async def conectar(self, websocket: WebSocket):
        await websocket.accept()
        self.conexiones_activas.append(websocket)

    def desconectar(self, websocket: WebSocket):
        self.conexiones_activas.remove(websocket)

    async def enviar_a_todos(self, mensaje: str):
        for conexion in self.conexiones_activas:
            await conexion.send_text(mensaje)

gestor = GestorConexiones()

@app.websocket("/ws/{nombre_usuario}")
async def websocket_chat(websocket: WebSocket, nombre_usuario: str):
    await gestor.conectar(websocket)
    await gestor.enviar_a_todos(f"{nombre_usuario} se ha conectado")

    try:
        while True:
            datos = await websocket.receive_text()
            await gestor.enviar_a_todos(f"{nombre_usuario}: {datos}")
    except WebSocketDisconnect:
        gestor.desconectar(websocket)
        await gestor.enviar_a_todos(f"{nombre_usuario} se ha desconectado")
```

---

## 20. Background Tasks

Las Background Tasks permiten ejecutar tareas después de enviar la respuesta al cliente. Son útiles para enviar emails, procesar datos, escribir logs, etc.

### Background Task básica

```python
from fastapi import FastAPI, BackgroundTasks

app = FastAPI()

def escribir_log(mensaje: str):
    with open("log.txt", "a") as f:
        f.write(f"{mensaje}\n")

@app.post("/enviar-notificacion/{email}")
async def enviar_notificacion(
    email: str,
    background_tasks: BackgroundTasks
):
    # Agregar tarea al background
    background_tasks.add_task(escribir_log, f"Notificación enviada a {email}")

    # La respuesta se envía inmediatamente
    return {"mensaje": f"Notificación enviada a {email}"}
```

### Enviar email en background

```python
import smtplib
from email.message import EmailMessage

def enviar_email(destinatario: str, asunto: str, cuerpo: str):
    msg = EmailMessage()
    msg["Subject"] = asunto
    msg["From"] = "noreply@miapp.com"
    msg["To"] = destinatario
    msg.set_content(cuerpo)

    with smtplib.SMTP("smtp.miapp.com", 587) as server:
        server.starttls()
        server.login("usuario", "contraseña")
        server.send_message(msg)

@app.post("/registrar")
async def registrar_usuario(
    usuario: UsuarioCrear,
    background_tasks: BackgroundTasks
):
    # Crear usuario en la base de datos
    nuevo_usuario = crear_usuario(usuario)

    # Enviar email de bienvenida en background
    background_tasks.add_task(
        enviar_email,
        destinatario=usuario.email,
        asunto="Bienvenido",
        cuerpo=f"Hola {usuario.nombre}, tu cuenta ha sido creada."
    )

    return {"mensaje": "Usuario registrado correctamente"}
```

### Múltiples tareas en background

```python
@app.post("/procesar")
async def procesar_datos(
    datos: dict,
    background_tasks: BackgroundTasks
):
    background_tasks.add_task(guardar_en_bd, datos)
    background_tasks.add_task(enviar_notificacion, datos)
    background_tasks.add_task(actualizar_cache, datos)

    return {"mensaje": "Procesamiento iniciado"}
```

### Background Tasks en dependencias

```python
def tarea_comun(background_tasks: BackgroundTasks):
    background_tasks.add_task(escribir_log, "Endpoint accedido")

@app.get("/items", dependencies=[Depends(tarea_comun)])
def listar_items():
    return {"items": []}
```

---

## 21. Testing

### Configuración de tests

```bash
# Instalar dependencias de testing
pip install pytest httpx
```

### Tests básicos

```python
# test_main.py
from fastapi.testclient import TestClient
from main import app

client = TestClient(app)

def test_root():
    response = client.get("/")
    assert response.status_code == 200
    assert response.json() == {"mensaje": "¡Hola, Mundo!"}

def test_crear_item():
    response = client.post(
        "/items",
        json={
            "nombre": "Laptop",
            "precio": 999.99,
            "descripcion": "Laptop de prueba"
        }
    )
    assert response.status_code == 201
    data = response.json()
    assert data["nombre"] == "Laptop"
    assert data["precio"] == 999.99

def test_obtener_item():
    response = client.get("/items/1")
    assert response.status_code == 200
    assert "nombre" in response.json()

def test_item_no_encontrado():
    response = client.get("/items/99999")
    assert response.status_code == 404
```

### Tests con base de datos de prueba

```python
# conftest.py
import pytest
from fastapi.testclient import TestClient
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker
from database import Base
from main import app, obtener_db

# Base de datos de prueba en memoria
SQLALCHEMY_DATABASE_URL = "sqlite:///./test.db"
engine = create_engine(
    SQLALCHEMY_DATABASE_URL,
    connect_args={"check_same_thread": False}
)
TestingSessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

# Crear tablas
Base.metadata.create_all(bind=engine)

# Sobrescribir dependencia de BD
def override_obtener_db():
    db = TestingSessionLocal()
    try:
        yield db
    finally:
        db.close()

app.dependency_overrides[obtener_db] = override_obtener_db

@pytest.fixture
def client():
    # Crear tablas antes de cada test
    Base.metadata.create_all(bind=engine)
    yield TestClient(app)
    # Limpiar después de cada test
    Base.metadata.drop_all(bind=engine)
```

### Tests asíncronos

```python
# test_async.py
import pytest
from httpx import AsyncClient, ASGITransport
from main import app

@pytest.mark.anyio
async def test_root_async():
    transport = ASGITransport(app=app)
    async with AsyncClient(transport=transport, base_url="http://test") as client:
        response = await client.get("/")
        assert response.status_code == 200
```

### Tests con autenticación

```python
def test_ruta_protegida_sin_token():
    response = client.get("/usuarios/me")
    assert response.status_code == 401

def test_ruta_protegida_con_token():
    # Primero obtener token
    response = client.post(
        "/token",
        data={"username": "testuser", "password": "testpass"}
    )
    token = response.json()["access_token"]

    # Usar token en la petición
    response = client.get(
        "/usuarios/me",
        headers={"Authorization": f"Bearer {token}"}
    )
    assert response.status_code == 200
    assert response.json()["username"] == "testuser"
```

### Ejecutar tests

```bash
# Ejecutar todos los tests
pytest

# Con verbose
pytest -v

# Un archivo específico
pytest test_main.py

# Un test específico
pytest test_main.py::test_crear_item

# Con cobertura
pip install pytest-cov
pytest --cov=app --cov-report=html
```

---

## 22. Documentación Automática

Una de las mayores ventajas de FastAPI es la documentación automática generada a partir de tus modelos y endpoints.

### Swagger UI y ReDoc

```python
app = FastAPI(
    title="Mi API de Tienda",
    description="""
    ## API para gestión de tienda online

    Esta API permite:
    * **Gestionar usuarios** - Crear, leer, actualizar y eliminar usuarios
    * **Gestionar productos** - CRUD completo de productos
    * **Realizar pedidos** - Sistema de pedidos
    """,
    version="2.0.0",
    terms_of_service="https://miapi.com/terminos",
    contact={
        "name": "Soporte API",
        "url": "https://miapi.com/soporte",
        "email": "soporte@miapi.com",
    },
    license_info={
        "name": "MIT",
        "url": "https://opensource.org/licenses/MIT",
    },
)

# Acceder a la documentación:
# Swagger UI: http://localhost:8000/docs
# ReDoc:      http://localhost:8000/redoc
```

### Tags para organizar endpoints

```python
tags_metadata = [
    {
        "name": "Usuarios",
        "description": "Operaciones con usuarios. Login y registro.",
    },
    {
        "name": "Productos",
        "description": "Gestión de productos de la tienda.",
    },
    {
        "name": "Pedidos",
        "description": "Sistema de pedidos y compras.",
        "externalDocs": {
            "description": "Documentación externa",
            "url": "https://miapi.com/docs/pedidos",
        },
    },
]

app = FastAPI(openapi_tags=tags_metadata)

@app.get("/usuarios", tags=["Usuarios"])
def listar_usuarios():
    """
    Obtener lista de todos los usuarios.

    - **skip**: Número de registros a saltar
    - **limit**: Número máximo de resultados
    """
    return []
```

### Documentar endpoints con detalle

```python
@app.post(
    "/items",
    response_model=ItemRespuesta,
    status_code=status.HTTP_201_CREATED,
    summary="Crear un nuevo item",
    description="Crea un nuevo item en la base de datos con los datos proporcionados.",
    response_description="El item creado exitosamente",
    tags=["Items"],
    responses={
        201: {"description": "Item creado exitosamente"},
        400: {"description": "Datos inválidos"},
        409: {"description": "Item ya existe"},
    }
)
def crear_item(item: ItemCrear):
    """
    Crear un item con toda la información:

    - **nombre**: nombre del item (obligatorio)
    - **descripcion**: descripción larga del item
    - **precio**: precio del item (mayor a 0)
    - **en_oferta**: si el item está en oferta
    """
    return {"id": 1, **item.model_dump()}
```

### Deprecar endpoints

```python
@app.get("/items-v1", deprecated=True, tags=["Items"])
def listar_items_v1():
    """
    ⚠️ **Deprecated**: Usar /items-v2 en su lugar.
    """
    return []
```

---

## 23. Estructura de Proyecto

### Estructura básica recomendada

```
mi-proyecto/
├── app/
│   ├── __init__.py
│   ├── main.py              # Punto de entrada de la aplicación
│   ├── config.py             # Configuración y variables de entorno
│   ├── database.py           # Conexión a la base de datos
│   ├── dependencies.py       # Dependencias compartidas
│   │
│   ├── models/               # Modelos de base de datos (ORM)
│   │   ├── __init__.py
│   │   ├── usuario.py
│   │   ├── item.py
│   │   └── pedido.py
│   │
│   ├── schemas/              # Schemas Pydantic (validación)
│   │   ├── __init__.py
│   │   ├── usuario.py
│   │   ├── item.py
│   │   └── pedido.py
│   │
│   ├── routers/              # Rutas/endpoints
│   │   ├── __init__.py
│   │   ├── usuarios.py
│   │   ├── items.py
│   │   └── pedidos.py
│   │
│   ├── services/             # Lógica de negocio
│   │   ├── __init__.py
│   │   ├── usuario_service.py
│   │   ├── item_service.py
│   │   └── email_service.py
│   │
│   ├── repositories/         # Acceso a datos (CRUD)
│   │   ├── __init__.py
│   │   ├── usuario_repo.py
│   │   └── item_repo.py
│   │
│   └── utils/                # Utilidades
│       ├── __init__.py
│       ├── security.py
│       └── helpers.py
│
├── tests/                    # Tests
│   ├── __init__.py
│   ├── conftest.py
│   ├── test_usuarios.py
│   └── test_items.py
│
├── alembic/                  # Migraciones de BD
│   ├── versions/
│   └── env.py
│
├── .env                      # Variables de entorno
├── .gitignore
├── alembic.ini
├── requirements.txt
└── README.md
```

### Archivo de configuración

```python
# app/config.py
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    app_name: str = "Mi API"
    debug: bool = False
    database_url: str
    secret_key: str
    algorithm: str = "HS256"
    access_token_expire_minutes: int = 30

    # Configuración de email
    smtp_host: str = "smtp.gmail.com"
    smtp_port: int = 587
    smtp_user: str = ""
    smtp_password: str = ""

    model_config = {
        "env_file": ".env"
    }

settings = Settings()
```

```bash
# .env
DATABASE_URL=postgresql://user:password@localhost/miapp
SECRET_KEY=mi-clave-secreta-super-segura
DEBUG=true
SMTP_USER=mi@email.com
SMTP_PASSWORD=mi-password
```

### Main con routers

```python
# app/main.py
from fastapi import FastAPI
from contextlib import asynccontextmanager
from app.config import settings
from app.database import crear_tablas
from app.routers import usuarios, items, pedidos

@asynccontextmanager
async def lifespan(app: FastAPI):
    crear_tablas()
    yield

app = FastAPI(
    title=settings.app_name,
    lifespan=lifespan
)

# Registrar routers
app.include_router(usuarios.router)
app.include_router(items.router)
app.include_router(pedidos.router)

@app.get("/")
def root():
    return {"mensaje": f"Bienvenido a {settings.app_name}"}

@app.get("/health")
def health_check():
    return {"status": "ok"}
```

### Ejemplo de router completo

```python
# app/routers/usuarios.py
from fastapi import APIRouter, Depends, HTTPException, status
from sqlalchemy.orm import Session
from app.database import obtener_db
from app.schemas.usuario import UsuarioCrear, UsuarioRespuesta, UsuarioActualizar
from app.services.usuario_service import UsuarioService

router = APIRouter(
    prefix="/usuarios",
    tags=["Usuarios"],
    responses={404: {"description": "No encontrado"}},
)

@router.get("/", response_model=list[UsuarioRespuesta])
def listar_usuarios(
    skip: int = 0,
    limit: int = 100,
    db: Session = Depends(obtener_db)
):
    servicio = UsuarioService(db)
    return servicio.listar_usuarios(skip=skip, limit=limit)

@router.post("/", response_model=UsuarioRespuesta, status_code=status.HTTP_201_CREATED)
def crear_usuario(usuario: UsuarioCrear, db: Session = Depends(obtener_db)):
    servicio = UsuarioService(db)
    return servicio.crear_usuario(usuario)

@router.get("/{usuario_id}", response_model=UsuarioRespuesta)
def obtener_usuario(usuario_id: int, db: Session = Depends(obtener_db)):
    servicio = UsuarioService(db)
    usuario = servicio.obtener_por_id(usuario_id)
    if not usuario:
        raise HTTPException(status_code=404, detail="Usuario no encontrado")
    return usuario

@router.put("/{usuario_id}", response_model=UsuarioRespuesta)
def actualizar_usuario(
    usuario_id: int,
    datos: UsuarioActualizar,
    db: Session = Depends(obtener_db)
):
    servicio = UsuarioService(db)
    return servicio.actualizar_usuario(usuario_id, datos)

@router.delete("/{usuario_id}", status_code=status.HTTP_204_NO_CONTENT)
def eliminar_usuario(usuario_id: int, db: Session = Depends(obtener_db)):
    servicio = UsuarioService(db)
    servicio.eliminar_usuario(usuario_id)
```

### Ejemplo de servicio

```python
# app/services/usuario_service.py
from sqlalchemy.orm import Session
from fastapi import HTTPException
from app.models.usuario import Usuario
from app.schemas.usuario import UsuarioCrear, UsuarioActualizar
from app.utils.security import crear_hash_password

class UsuarioService:
    def __init__(self, db: Session):
        self.db = db

    def listar_usuarios(self, skip: int = 0, limit: int = 100):
        return self.db.query(Usuario).offset(skip).limit(limit).all()

    def obtener_por_id(self, usuario_id: int):
        return self.db.query(Usuario).filter(Usuario.id == usuario_id).first()

    def crear_usuario(self, datos: UsuarioCrear):
        # Verificar si el email ya existe
        existente = self.db.query(Usuario).filter(
            Usuario.email == datos.email
        ).first()
        if existente:
            raise HTTPException(status_code=400, detail="Email ya registrado")

        usuario = Usuario(
            nombre=datos.nombre,
            email=datos.email,
            hashed_password=crear_hash_password(datos.password)
        )
        self.db.add(usuario)
        self.db.commit()
        self.db.refresh(usuario)
        return usuario

    def actualizar_usuario(self, usuario_id: int, datos: UsuarioActualizar):
        usuario = self.obtener_por_id(usuario_id)
        if not usuario:
            raise HTTPException(status_code=404, detail="Usuario no encontrado")

        datos_dict = datos.model_dump(exclude_unset=True)
        for key, value in datos_dict.items():
            setattr(usuario, key, value)

        self.db.commit()
        self.db.refresh(usuario)
        return usuario

    def eliminar_usuario(self, usuario_id: int):
        usuario = self.obtener_por_id(usuario_id)
        if not usuario:
            raise HTTPException(status_code=404, detail="Usuario no encontrado")

        self.db.delete(usuario)
        self.db.commit()
```

---

## 24. Despliegue

### Con Docker

```dockerfile
# Dockerfile
FROM python:3.11-slim

WORKDIR /app

# Instalar dependencias
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copiar código
COPY ./app ./app

# Exponer puerto
EXPOSE 8000

# Comando de inicio
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

```yaml
# docker-compose.yml
version: "3.8"

services:
  api:
    build: .
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://postgres:password@db:5432/miapp
      - SECRET_KEY=clave-secreta
    depends_on:
      - db

  db:
    image: postgres:15
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=miapp
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

volumes:
  postgres_data:
```

```bash
# Construir y ejecutar
docker compose up --build

# En segundo plano
docker compose up -d --build
```

### Con Gunicorn + Uvicorn (producción)

```bash
# Instalar gunicorn
pip install gunicorn

# Ejecutar con múltiples workers
gunicorn app.main:app -w 4 -k uvicorn.workers.UvicornWorker --bind 0.0.0.0:8000
```

```dockerfile
# Dockerfile para producción con Gunicorn
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY ./app ./app

EXPOSE 8000

CMD ["gunicorn", "app.main:app", "-w", "4", "-k", "uvicorn.workers.UvicornWorker", "--bind", "0.0.0.0:8000"]
```

### Variables de entorno en producción

```python
# app/config.py
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    app_name: str = "Mi API"
    debug: bool = False
    database_url: str
    secret_key: str

    model_config = {
        "env_file": ".env"
    }

# Las variables de entorno del sistema tienen prioridad sobre .env
# En producción, configurar las variables directamente en el entorno
```

### Con Nginx como reverse proxy

```nginx
# nginx.conf
upstream fastapi_app {
    server 127.0.0.1:8000;
}

server {
    listen 80;
    server_name midominio.com;

    location / {
        proxy_pass http://fastapi_app;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

---

## 25. Buenas Prácticas

### 1. Usar variables de entorno para configuración

```python
# ✅ Correcto
from app.config import settings

SECRET_KEY = settings.secret_key

# ❌ Incorrecto - nunca hardcodear secretos
SECRET_KEY = "mi-clave-hardcodeada"
```

### 2. Separar modelos de entrada y salida

```python
# ✅ Correcto - Separar schemas
class UsuarioCrear(BaseModel):
    nombre: str
    email: str
    password: str

class UsuarioRespuesta(BaseModel):
    id: int
    nombre: str
    email: str
    # No expone password

# ❌ Incorrecto - Usar un solo modelo para todo
class Usuario(BaseModel):
    id: int
    nombre: str
    email: str
    password: str  # Se expone en la respuesta
```

### 3. Usar Depends para inyección de dependencias

```python
# ✅ Correcto - Usar dependencias
@app.get("/items")
def listar_items(db: Session = Depends(obtener_db)):
    return db.query(Item).all()

# ❌ Incorrecto - Crear sesión manualmente
@app.get("/items")
def listar_items():
    db = SessionLocal()
    items = db.query(Item).all()
    db.close()
    return items
```

### 4. Manejar errores correctamente

```python
# ✅ Correcto - Usar HTTPException
@app.get("/items/{item_id}")
def obtener_item(item_id: int, db: Session = Depends(obtener_db)):
    item = db.query(Item).filter(Item.id == item_id).first()
    if not item:
        raise HTTPException(status_code=404, detail="Item no encontrado")
    return item

# ❌ Incorrecto - Devolver None o ignorar el error
@app.get("/items/{item_id}")
def obtener_item(item_id: int, db: Session = Depends(obtener_db)):
    return db.query(Item).filter(Item.id == item_id).first()
```

### 5. Organizar el código con routers

```python
# ✅ Correcto - Separar por módulos con APIRouter
# routers/usuarios.py, routers/items.py, etc.

# ❌ Incorrecto - Todo en un solo archivo main.py
```

### 6. Usar async cuando corresponda

```python
# ✅ Correcto - Async para operaciones I/O
@app.get("/datos")
async def obtener_datos():
    async with httpx.AsyncClient() as client:
        response = await client.get("https://api.externa.com/datos")
        return response.json()

# ✅ Correcto - Sync para operaciones CPU-bound o librerías síncronas
@app.get("/calcular")
def calcular():
    resultado = operacion_costosa()
    return {"resultado": resultado}
```

### 7. Documentar los endpoints

```python
# ✅ Correcto - Documentar con detalle
@app.post(
    "/items",
    response_model=ItemRespuesta,
    status_code=201,
    summary="Crear item",
    tags=["Items"]
)
def crear_item(item: ItemCrear):
    """
    Crea un nuevo item:

    - **nombre**: nombre del item
    - **precio**: precio (mayor a 0)
    """
    ...
```

### 8. Versionar la API

```python
# Usando prefijos en routers
v1_router = APIRouter(prefix="/api/v1")
v2_router = APIRouter(prefix="/api/v2")

@v1_router.get("/items")
def listar_items_v1():
    return {"version": 1, "items": []}

@v2_router.get("/items")
def listar_items_v2():
    return {"version": 2, "items": [], "total": 0}

app.include_router(v1_router)
app.include_router(v2_router)
```

### 9. Logging adecuado

```python
import logging

# Configurar logging
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

@app.post("/items")
def crear_item(item: ItemCrear, db: Session = Depends(obtener_db)):
    logger.info(f"Creando item: {item.nombre}")
    try:
        nuevo_item = guardar_item(db, item)
        logger.info(f"Item creado con ID: {nuevo_item.id}")
        return nuevo_item
    except Exception as e:
        logger.error(f"Error creando item: {e}")
        raise HTTPException(status_code=500, detail="Error interno")
```

### 10. Migraciones con Alembic

```bash
# Instalar Alembic
pip install alembic

# Inicializar
alembic init alembic

# Crear migración
alembic revision --autogenerate -m "crear tabla usuarios"

# Aplicar migraciones
alembic upgrade head

# Revertir última migración
alembic downgrade -1
```

```python
# alembic/env.py - Configurar para usar los modelos
from app.database import Base
from app.models import usuario, item  # Importar todos los modelos
target_metadata = Base.metadata
```

---

## 🔗 Recursos Adicionales

- **Documentación oficial**: [https://fastapi.tiangolo.com](https://fastapi.tiangolo.com)
- **Repositorio GitHub**: [https://github.com/fastapi/fastapi](https://github.com/fastapi/fastapi)
- **Pydantic docs**: [https://docs.pydantic.dev](https://docs.pydantic.dev)
- **SQLModel docs**: [https://sqlmodel.tiangolo.com](https://sqlmodel.tiangolo.com)
- **Starlette docs**: [https://www.starlette.io](https://www.starlette.io)
- **Uvicorn docs**: [https://www.uvicorn.org](https://www.uvicorn.org)
