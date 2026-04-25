# 🐍🗄️ Python + Bases de Datos + FastAPI

## 📋 Índice

1. [Introducción](#1-introducción)
2. [Conceptos clave antes de empezar](#2-conceptos-clave-antes-de-empezar)
3. [Opciones para conectar Python con bases de datos](#3-opciones-para-conectar-python-con-bases-de-datos)
4. [Preparar el entorno](#4-preparar-el-entorno)
5. [Conexión directa con drivers nativos](#5-conexión-directa-con-drivers-nativos)
6. [Conexión con SQLAlchemy](#6-conexión-con-sqlalchemy)
7. [Modelado de datos con ORM](#7-modelado-de-datos-con-orm)
8. [Integrar la base de datos en FastAPI](#8-integrar-la-base-de-datos-en-fastapi)
9. [CRUD completo con FastAPI y SQLAlchemy](#9-crud-completo-con-fastapi-y-sqlalchemy)
10. [Uso asíncrono con FastAPI](#10-uso-asíncrono-con-fastapi)
11. [Configuración por variables de entorno](#11-configuración-por-variables-de-entorno)
12. [Migraciones con Alembic](#12-migraciones-con-alembic)
13. [Testing con base de datos](#13-testing-con-base-de-datos)
14. [Buenas prácticas](#14-buenas-prácticas)
15. [Errores comunes](#15-errores-comunes)
16. [Estructura de proyecto recomendada](#16-estructura-de-proyecto-recomendada)
17. [Resumen final](#17-resumen-final)

---

## 1. Introducción

Conectar Python con una base de datos es una de las tareas más habituales en backend. Normalmente lo harás para:

- Guardar usuarios, productos, pedidos o logs.
- Leer información para mostrarla en una API.
- Actualizar registros existentes.
- Borrar datos de forma controlada.
- Ejecutar transacciones para mantener consistencia.

Cuando trabajas con **FastAPI**, esta conexión suele formar parte de una API REST: el cliente envía una petición HTTP, FastAPI valida los datos, tu capa de acceso consulta la base de datos y finalmente devuelves una respuesta JSON.

En esta guía veremos el flujo completo:

1. Cómo conectar Python con una base de datos.
2. Qué librerías puedes usar.
3. Cómo modelar tablas.
4. Cómo usar esa conexión dentro de FastAPI.
5. Cómo organizar el código correctamente.

---

## 2. Conceptos clave antes de empezar

Antes de escribir código, conviene tener claros estos conceptos:

### Base de datos relacional

Son motores como:

- **PostgreSQL**
- **MySQL / MariaDB**
- **SQLite**
- **SQL Server**

Trabajan con tablas, filas, columnas, claves primarias y relaciones.

### Driver

Es la librería que permite a Python hablar con una base de datos concreta.

Ejemplos:

- PostgreSQL: `psycopg2` o `asyncpg`
- MySQL: `pymysql` o `mysqlclient`
- SQLite: viene incluido en Python con `sqlite3`

### ORM

Un **ORM (Object Relational Mapper)** te permite trabajar con clases y objetos de Python en lugar de escribir todo el SQL manualmente.

En el ecosistema Python, el más usado es **SQLAlchemy**.

### Sesión

En SQLAlchemy, una **sesión** representa el contexto con el que haces operaciones contra la base de datos:

- leer
- insertar
- actualizar
- borrar
- confirmar cambios con `commit()`

### Migraciones

Son cambios versionados sobre el esquema de la base de datos:

- crear tablas
- añadir columnas
- cambiar restricciones

La herramienta más usada con SQLAlchemy es **Alembic**.

---

## 3. Opciones para conectar Python con bases de datos

En Python tienes varias formas de hacerlo.

### Opción 1: driver nativo

Conectas directamente con la librería oficial o compatible del motor.

**Ventajas:**

- Más simple para scripts pequeños.
- Tienes control total sobre el SQL.
- Ideal para aprender cómo funciona la conexión real.

**Desventajas:**

- Más código repetitivo.
- Debes mapear resultados manualmente.
- Escala peor en proyectos grandes.

### Opción 2: SQLAlchemy Core

Usas SQLAlchemy para gestionar conexión, engine, sesiones y consultas estructuradas, sin depender tanto del ORM.

### Opción 3: SQLAlchemy ORM

Es la opción más habitual en APIs con FastAPI.

**Ventajas:**

- Modelos claros.
- Menos código repetitivo.
- Integración cómoda con FastAPI.
- Mejor organización por capas.

### Opción 4: SQLModel

Es una capa construida sobre FastAPI + Pydantic + SQLAlchemy. Puede ser cómoda, pero si quieres aprender la base sólida del ecosistema, conviene dominar primero **SQLAlchemy**.

---

## 4. Preparar el entorno

### Crear entorno virtual

```bash
python -m venv venv
source venv/bin/activate
```

En Windows:

```bash
venv\Scripts\activate
```

### Instalar dependencias habituales

#### Para SQLite

No necesitas instalar driver adicional si usas `sqlite3`.

#### Para PostgreSQL con SQLAlchemy

```bash
pip install sqlalchemy psycopg2-binary fastapi uvicorn
```

#### Para MySQL con SQLAlchemy

```bash
pip install sqlalchemy pymysql fastapi uvicorn
```

#### Para modo asíncrono con PostgreSQL

```bash
pip install sqlalchemy asyncpg fastapi uvicorn
```

#### Extras recomendados

```bash
pip install pydantic-settings alembic pytest
```

---

## 5. Conexión directa con drivers nativos

Antes de usar FastAPI, es útil entender la conexión básica.

### Ejemplo con SQLite (`sqlite3`)

```python
import sqlite3

conn = sqlite3.connect("app.db")
cursor = conn.cursor()

cursor.execute("""
CREATE TABLE IF NOT EXISTS usuarios (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    nombre TEXT NOT NULL,
    email TEXT UNIQUE NOT NULL
)
""")

cursor.execute(
    "INSERT INTO usuarios (nombre, email) VALUES (?, ?)",
    ("Ana", "ana@email.com")
)

conn.commit()

cursor.execute("SELECT id, nombre, email FROM usuarios")
usuarios = cursor.fetchall()

for usuario in usuarios:
    print(usuario)

conn.close()
```

### Qué está pasando aquí

1. Abres una conexión con `connect()`.
2. Creas un cursor.
3. Ejecutas SQL.
4. Confirmas cambios con `commit()`.
5. Lees resultados.
6. Cierras la conexión.

### Ejemplo con PostgreSQL (`psycopg2`)

```python
import psycopg2

conn = psycopg2.connect(
    host="localhost",
    port=5432,
    dbname="mi_bd",
    user="postgres",
    password="secret"
)

cursor = conn.cursor()

cursor.execute(
    "SELECT id, nombre, email FROM usuarios WHERE activo = %s",
    (True,)
)

for fila in cursor.fetchall():
    print(fila)

cursor.close()
conn.close()
```

### Importante: parametriza siempre las consultas

✅ Correcto:

```python
cursor.execute(
    "SELECT * FROM usuarios WHERE email = %s",
    (email,)
)
```

❌ Incorrecto:

```python
cursor.execute(f"SELECT * FROM usuarios WHERE email = '{email}'")
```

La segunda opción puede abrir la puerta a **SQL Injection**.

---

## 6. Conexión con SQLAlchemy

SQLAlchemy separa muy bien los conceptos de:

- **engine**: maneja la comunicación con la base de datos
- **session**: permite operar contra la base
- **models**: representan tablas

### Crear el engine

#### SQLite

```python
from sqlalchemy import create_engine

DATABASE_URL = "sqlite:///./app.db"

engine = create_engine(
    DATABASE_URL,
    connect_args={"check_same_thread": False}
)
```

#### PostgreSQL

```python
from sqlalchemy import create_engine

DATABASE_URL = "postgresql+psycopg2://postgres:secret@localhost:5432/mi_bd"
engine = create_engine(DATABASE_URL)
```

#### MySQL

```python
from sqlalchemy import create_engine

DATABASE_URL = "mysql+pymysql://root:secret@localhost:3306/mi_bd"
engine = create_engine(DATABASE_URL)
```

### Crear una fábrica de sesiones

```python
from sqlalchemy.orm import sessionmaker

SessionLocal = sessionmaker(
    autocommit=False,
    autoflush=False,
    bind=engine
)
```

### Crear la clase base del ORM

```python
from sqlalchemy.orm import declarative_base

Base = declarative_base()
```

### Flujo habitual

1. Creas el `engine`.
2. Creas `SessionLocal`.
3. Defines modelos heredando de `Base`.
4. Abres una sesión.
5. Ejecutas operaciones.
6. Haces `commit()` o `rollback()`.
7. Cierras la sesión.

---

## 7. Modelado de datos con ORM

### Ejemplo de modelo `Usuario`

```python
from sqlalchemy import Boolean, Column, Integer, String
from sqlalchemy.orm import declarative_base

Base = declarative_base()

class Usuario(Base):
    __tablename__ = "usuarios"

    id = Column(Integer, primary_key=True, index=True)
    nombre = Column(String(100), nullable=False)
    email = Column(String(150), unique=True, index=True, nullable=False)
    activo = Column(Boolean, default=True)
```

### Crear las tablas

```python
Base.metadata.create_all(bind=engine)
```

Esto sirve para ejemplos pequeños o pruebas rápidas. En proyectos reales, es mejor usar **migraciones con Alembic**.

---

## 8. Integrar la base de datos en FastAPI

La forma más limpia de usar la base de datos en FastAPI es mediante una **dependencia** que abra y cierre la sesión por cada petición.

### Archivo `database.py`

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import declarative_base, sessionmaker

DATABASE_URL = "sqlite:///./app.db"

engine = create_engine(
    DATABASE_URL,
    connect_args={"check_same_thread": False}
)

SessionLocal = sessionmaker(
    autocommit=False,
    autoflush=False,
    bind=engine
)

Base = declarative_base()
```

### Dependencia `get_db`

```python
from collections.abc import Generator
from sqlalchemy.orm import Session
from .database import SessionLocal

def get_db() -> Generator[Session, None, None]:
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

### Usarla en FastAPI con `Depends`

```python
from fastapi import Depends, FastAPI
from sqlalchemy import text
from sqlalchemy.orm import Session

app = FastAPI()

@app.get("/health-db")
def health_db(db: Session = Depends(get_db)):
    db.execute(text("SELECT 1"))
    return {"ok": True}
```

### ¿Por qué así?

Porque FastAPI:

- crea la sesión al entrar en la petición
- la inyecta en el endpoint
- la cierra correctamente al terminar

Así evitas fugas de conexiones y código repetido.

---

## 9. CRUD completo con FastAPI y SQLAlchemy

Aquí tienes un ejemplo clásico y bien organizado.

### Modelo ORM

```python
from sqlalchemy import Boolean, Column, Integer, String
from .database import Base

class Usuario(Base):
    __tablename__ = "usuarios"

    id = Column(Integer, primary_key=True, index=True)
    nombre = Column(String(100), nullable=False)
    email = Column(String(150), unique=True, index=True, nullable=False)
    activo = Column(Boolean, default=True)
```

### Schemas de Pydantic

```python
from pydantic import BaseModel, EmailStr

class UsuarioCreate(BaseModel):
    nombre: str
    email: EmailStr

class UsuarioUpdate(BaseModel):
    nombre: str | None = None
    email: EmailStr | None = None
    activo: bool | None = None

class UsuarioResponse(BaseModel):
    id: int
    nombre: str
    email: EmailStr
    activo: bool

    model_config = {"from_attributes": True}
```

### Operaciones CRUD

```python
from sqlalchemy.orm import Session
from .models import Usuario
from .schemas import UsuarioCreate, UsuarioUpdate

def crear_usuario(db: Session, data: UsuarioCreate) -> Usuario:
    usuario = Usuario(nombre=data.nombre, email=data.email)
    db.add(usuario)
    db.commit()
    db.refresh(usuario)
    return usuario

def obtener_usuario(db: Session, usuario_id: int) -> Usuario | None:
    return db.query(Usuario).filter(Usuario.id == usuario_id).first()

def listar_usuarios(db: Session) -> list[Usuario]:
    return db.query(Usuario).order_by(Usuario.id.desc()).all()

def actualizar_usuario(
    db: Session,
    usuario: Usuario,
    data: UsuarioUpdate
) -> Usuario:
    if data.nombre is not None:
        usuario.nombre = data.nombre
    if data.email is not None:
        usuario.email = data.email
    if data.activo is not None:
        usuario.activo = data.activo

    db.commit()
    db.refresh(usuario)
    return usuario

def eliminar_usuario(db: Session, usuario: Usuario) -> None:
    db.delete(usuario)
    db.commit()
```

### Endpoints en FastAPI

```python
from fastapi import Depends, FastAPI, HTTPException, status
from sqlalchemy.orm import Session

from .crud import (
    actualizar_usuario,
    crear_usuario,
    eliminar_usuario,
    listar_usuarios,
    obtener_usuario,
)
from .database import Base, engine
from .dependencies import get_db
from .schemas import UsuarioCreate, UsuarioResponse, UsuarioUpdate

Base.metadata.create_all(bind=engine)

app = FastAPI()

@app.post("/usuarios", response_model=UsuarioResponse, status_code=status.HTTP_201_CREATED)
def create_user(data: UsuarioCreate, db: Session = Depends(get_db)):
    return crear_usuario(db, data)

@app.get("/usuarios", response_model=list[UsuarioResponse])
def get_users(db: Session = Depends(get_db)):
    return listar_usuarios(db)

@app.get("/usuarios/{usuario_id}", response_model=UsuarioResponse)
def get_user(usuario_id: int, db: Session = Depends(get_db)):
    usuario = obtener_usuario(db, usuario_id)
    if not usuario:
        raise HTTPException(status_code=404, detail="Usuario no encontrado")
    return usuario

@app.put("/usuarios/{usuario_id}", response_model=UsuarioResponse)
def update_user(usuario_id: int, data: UsuarioUpdate, db: Session = Depends(get_db)):
    usuario = obtener_usuario(db, usuario_id)
    if not usuario:
        raise HTTPException(status_code=404, detail="Usuario no encontrado")
    return actualizar_usuario(db, usuario, data)

@app.delete("/usuarios/{usuario_id}", status_code=status.HTTP_204_NO_CONTENT)
def delete_user(usuario_id: int, db: Session = Depends(get_db)):
    usuario = obtener_usuario(db, usuario_id)
    if not usuario:
        raise HTTPException(status_code=404, detail="Usuario no encontrado")
    eliminar_usuario(db, usuario)
```

### Flujo completo de una petición

Cuando llamas a `POST /usuarios`:

1. FastAPI recibe JSON.
2. Pydantic valida el body.
3. `Depends(get_db)` crea una sesión.
4. La función CRUD inserta el usuario.
5. `commit()` guarda el cambio.
6. FastAPI serializa la respuesta.
7. La sesión se cierra al terminar.

---

## 10. Uso asíncrono con FastAPI

FastAPI soporta `async`, pero eso **no significa** que todo deba ser asíncrono.

### Cuándo usar sync

Usa enfoque síncrono cuando:

- estás empezando
- usas SQLAlchemy clásico con `Session`
- tu aplicación no necesita alta concurrencia todavía

### Cuándo usar async

Tiene sentido cuando:

- usas drivers asíncronos como `asyncpg`
- tu aplicación soporta muchas conexiones concurrentes
- quieres coherencia con servicios externos asíncronos

### Configuración async con SQLAlchemy

```python
from sqlalchemy.ext.asyncio import AsyncSession, async_sessionmaker, create_async_engine
from sqlalchemy.orm import declarative_base

DATABASE_URL = "postgresql+asyncpg://postgres:secret@localhost:5432/mi_bd"

engine = create_async_engine(DATABASE_URL, echo=False)
AsyncSessionLocal = async_sessionmaker(engine, expire_on_commit=False)
Base = declarative_base()
```

### Dependencia async

```python
from collections.abc import AsyncGenerator

async def get_db() -> AsyncGenerator[AsyncSession, None]:
    async with AsyncSessionLocal() as session:
        yield session
```

### Endpoint async

```python
from fastapi import Depends, FastAPI
from sqlalchemy import select
from sqlalchemy.ext.asyncio import AsyncSession

app = FastAPI()

@app.get("/usuarios")
async def listar(db: AsyncSession = Depends(get_db)):
    result = await db.execute(select(Usuario))
    usuarios = result.scalars().all()
    return usuarios
```

### Regla práctica

Si tu proyecto es mediano y no tienes un motivo claro para usar async en la base de datos, empieza con el enfoque **sync**. Es más simple de depurar y mantener.

---

## 11. Configuración por variables de entorno

No hardcodees credenciales en el código.

### Ejemplo con `pydantic-settings`

```python
from pydantic_settings import BaseSettings, SettingsConfigDict

class Settings(BaseSettings):
    database_url: str

    model_config = SettingsConfigDict(env_file=".env", extra="ignore")

settings = Settings()
```

### Archivo `.env`

```env
DATABASE_URL=postgresql+psycopg2://postgres:secret@localhost:5432/mi_bd
```

### Uso

```python
from sqlalchemy import create_engine

engine = create_engine(settings.database_url)
```

### Beneficios

- separas configuración y código
- cambias entre desarrollo, test y producción fácilmente
- reduces riesgo de exponer credenciales

---

## 12. Migraciones con Alembic

Crear tablas con `Base.metadata.create_all()` está bien para demos, pero en aplicaciones reales lo habitual es usar migraciones.

### Instalar Alembic

```bash
pip install alembic
```

### Inicializar

```bash
alembic init alembic
```

### Crear una migración

```bash
alembic revision --autogenerate -m "crear tabla usuarios"
```

### Aplicarla

```bash
alembic upgrade head
```

### Cuándo usar migraciones

Siempre que el esquema pueda cambiar entre versiones del proyecto:

- nuevas tablas
- nuevas columnas
- índices
- claves foráneas
- restricciones

---

## 13. Testing con base de datos

En FastAPI es muy común probar endpoints con una base temporal o aislada.

### Idea general

1. Creas una base de datos de prueba.
2. Sobrescribes la dependencia `get_db`.
3. Ejecutas tests contra esa base.
4. Limpias al terminar.

### Ejemplo simplificado

```python
import pytest
from fastapi.testclient import TestClient
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

from app.database import Base
from app.dependencies import get_db
from app.main import app

SQLALCHEMY_DATABASE_URL = "sqlite:///./test.db"

engine = create_engine(
    SQLALCHEMY_DATABASE_URL,
    connect_args={"check_same_thread": False}
)
TestingSessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

def override_get_db():
    db = TestingSessionLocal()
    try:
        yield db
    finally:
        db.close()

app.dependency_overrides[get_db] = override_get_db

@pytest.fixture(scope="function", autouse=True)
def setup_database():
    Base.metadata.create_all(bind=engine)
    yield
    Base.metadata.drop_all(bind=engine)

client = TestClient(app)

def test_crear_usuario():
    response = client.post(
        "/usuarios",
        json={"nombre": "Ana", "email": "ana@email.com"}
    )

    assert response.status_code == 201
```

### Recomendación

Para tests más serios:

- usa una base separada
- no compartas la base de desarrollo
- limpia datos entre tests
- prueba errores, validaciones y casos límite

---

## 14. Buenas prácticas

### 1. Separa responsabilidades

No pongas todo en `main.py`.

Divide al menos en:

- `database.py`
- `models.py`
- `schemas.py`
- `crud.py` o `repositories.py`
- `routers/`

### 2. Usa modelos distintos para entrada y salida

No devuelvas directamente datos sensibles del ORM.

Por ejemplo:

- el modelo de entrada puede aceptar `password`
- el modelo de salida nunca debería devolverla

### 3. Maneja transacciones correctamente

Si una operación falla:

- haz `rollback()`
- registra el error si corresponde
- devuelve un error adecuado

### 4. Usa índices donde tenga sentido

Campos como:

- `email`
- `username`
- claves foráneas
- columnas muy filtradas

pueden necesitar índices para mejorar rendimiento.

### 5. No abras conexiones manualmente en cada endpoint

Usa dependencias de FastAPI y una fábrica de sesiones.

### 6. Evita lógica de negocio dentro de los endpoints

El endpoint debería coordinar:

- validación
- autorización
- llamada a servicio o repositorio
- respuesta

La lógica de negocio debería vivir en otra capa.

### 7. Usa migraciones en proyectos reales

`create_all()` es cómodo, pero Alembic es la opción mantenible.

### 8. Elige sync o async con criterio

No uses async solo “porque sí”. Úsalo cuando toda la cadena realmente lo aproveche.

---

## 15. Errores comunes

### Error 1: olvidar cerrar sesiones

Esto puede dejar conexiones abiertas y agotar el pool.

### Error 2: usar el mismo modelo para todo

Mezclar ORM + request body + response body suele acabar mal. Separa:

- modelo de tabla
- schema de entrada
- schema de salida

### Error 3: no hacer `refresh()` tras crear un registro

Después de `commit()`, `refresh()` te ayuda a recuperar campos generados por la base como:

- `id`
- timestamps
- valores por defecto

### Error 4: mezclar código sync con drivers async sin cuidado

Si eliges `AsyncSession`, tu código debe ser coherente con esa decisión.

### Error 5: no controlar duplicados

Si `email` es único, debes contemplar el caso en que ya exista.

### Error 6: construir SQL con f-strings

Eso puede provocar vulnerabilidades de **SQL Injection**.

### Error 7: usar SQLite en producción sin evaluar limitaciones

SQLite es excelente para:

- demos
- scripts
- herramientas pequeñas
- tests

Pero para sistemas concurrentes o multiusuario normalmente conviene PostgreSQL o MySQL.

---

## 16. Estructura de proyecto recomendada

```text
app/
├── main.py
├── core/
│   └── config.py
├── db/
│   ├── database.py
│   ├── models.py
│   └── migrations/
├── schemas/
│   └── usuario.py
├── crud/
│   └── usuario.py
├── api/
│   └── routes/
│       └── usuarios.py
└── tests/
    └── test_usuarios.py
```

### Qué va en cada parte

- **main.py**: crea la app y registra routers
- **config.py**: lee variables de entorno
- **database.py**: engine, sesión y base ORM
- **models.py**: tablas
- **schemas/**: modelos Pydantic
- **crud/**: acceso a datos
- **routes/**: endpoints
- **tests/**: pruebas

---

## 17. Resumen final

La forma más práctica y profesional de conectar **Python con bases de datos** en proyectos modernos es:

1. usar un motor como PostgreSQL, MySQL o SQLite
2. conectarte mediante un driver compatible
3. apoyarte en **SQLAlchemy** para organizar el acceso a datos
4. integrar la sesión en **FastAPI** con `Depends`
5. separar modelos ORM, schemas Pydantic y lógica CRUD
6. usar migraciones y variables de entorno en proyectos reales

Si estás empezando, este camino suele ser el más recomendable:

1. aprende primero la conexión básica con `sqlite3` o `psycopg2`
2. después pasa a SQLAlchemy
3. finalmente integra todo con FastAPI

Así entiendes tanto la base técnica como la arquitectura correcta para una API mantenible.

---

## 📚 Guías relacionadas de este repositorio

- [FastAPI](FASTAPI.MD)
- [Python Completo](PYTHON.MD)
- [SQL Avanzado](../../databases/SQL_AVANZADO.MD)
- [Relaciones entre Tablas SQL](../../databases/RELACIONES%20TABLAS%20CON%20SQL.MD)
