# 🧠 SQLAlchemy para FastAPI

## 📋 Índice

1. [Qué aporta SQLAlchemy dentro de FastAPI](#1-qué-aporta-sqlalchemy-dentro-de-fastapi)
2. [Cuándo usar SQLAlchemy ORM y cuándo usar Core](#2-cuándo-usar-sqlalchemy-orm-y-cuándo-usar-core)
3. [Versiones, drivers y dependencias](#3-versiones-drivers-y-dependencias)
4. [Arquitectura recomendada en un proyecto FastAPI](#4-arquitectura-recomendada-en-un-proyecto-fastapi)
5. [Configurar engine, sesión y base declarativa](#5-configurar-engine-sesión-y-base-declarativa)
6. [Definir modelos con SQLAlchemy 2.0](#6-definir-modelos-con-sqlalchemy-20)
7. [Relaciones entre modelos](#7-relaciones-entre-modelos)
8. [Separar ORM, schemas y lógica de aplicación](#8-separar-orm-schemas-y-lógica-de-aplicación)
9. [Dependencias de FastAPI para abrir y cerrar sesión](#9-dependencias-de-fastapi-para-abrir-y-cerrar-sesión)
10. [CRUD con estilo SQLAlchemy 2.0](#10-crud-con-estilo-sqlalchemy-20)
11. [Consultas habituales en APIs](#11-consultas-habituales-en-apis)
12. [Transacciones, commit, rollback y refresh](#12-transacciones-commit-rollback-y-refresh)
13. [Manejo de errores de base de datos](#13-manejo-de-errores-de-base-de-datos)
14. [Carga eficiente de relaciones](#14-carga-eficiente-de-relaciones)
15. [Paginación, filtros y ordenación](#15-paginación-filtros-y-ordenación)
16. [Uso asíncrono con AsyncSession](#16-uso-asíncrono-con-asyncsession)
17. [Migraciones con Alembic](#17-migraciones-con-alembic)
18. [Testing con base de datos](#18-testing-con-base-de-datos)
19. [Buenas prácticas específicas para FastAPI + SQLAlchemy](#19-buenas-prácticas-específicas-para-fastapi--sqlalchemy)
20. [Errores comunes](#20-errores-comunes)
21. [Estructura de proyecto recomendada](#21-estructura-de-proyecto-recomendada)
22. [Resumen final](#22-resumen-final)

---

## 1. Qué aporta SQLAlchemy dentro de FastAPI

Si **FastAPI** ya resuelve rutas, validación, inyección de dependencias y documentación automática, **SQLAlchemy** cubre la parte de acceso a datos relacionales:

- conexión a PostgreSQL, MySQL, SQLite y otros motores
- definición de tablas como clases Python
- consultas expresivas sin escribir todo el SQL manualmente
- manejo de sesiones y transacciones
- carga de relaciones entre entidades
- integración natural con Alembic para migraciones

En una API real, FastAPI y SQLAlchemy suelen quedar repartidos así:

- **FastAPI**: HTTP, validación, auth, dependencias, serialización
- **SQLAlchemy**: persistencia, consultas, transacciones, mapeo ORM
- **Pydantic**: contratos de entrada y salida

La idea clave es esta: **FastAPI no reemplaza a SQLAlchemy, y SQLAlchemy no reemplaza a FastAPI**. Se complementan.

---

## 2. Cuándo usar SQLAlchemy ORM y cuándo usar Core

SQLAlchemy tiene dos formas principales de trabajar:

### SQLAlchemy ORM

Es la opción más usada en aplicaciones FastAPI porque te permite modelar tablas como clases.

Úsalo cuando:

- quieres código más expresivo a nivel de dominio
- trabajas con entidades relacionadas (`Usuario`, `Pedido`, `Item`)
- necesitas una arquitectura mantenible por capas
- quieres aprovechar `relationship`, cascadas y carga diferida/anticipada

### SQLAlchemy Core

Trabaja más cerca de SQL, pero con una API Python muy potente.

Úsalo cuando:

- necesitas consultas complejas o muy optimizadas
- haces reporting o agregaciones avanzadas
- quieres construir SQL de forma explícita sin usar objetos ORM completos
- tienes procesos batch o scripts de datos

### Regla práctica

En FastAPI es muy común combinar ambos:

- **ORM** para CRUD y lógica habitual
- **Core / expresiones SQLAlchemy** para consultas complejas, joins o métricas

---

## 3. Versiones, drivers y dependencias

Hoy conviene pensar en **SQLAlchemy 2.x**. Su estilo moderno usa `select()`, `Session.execute()` y tipos como `Mapped[...]`.

### Dependencias mínimas

#### PostgreSQL síncrono

```bash
pip install fastapi uvicorn sqlalchemy psycopg2-binary
```

#### PostgreSQL asíncrono

```bash
pip install fastapi uvicorn sqlalchemy asyncpg
```

#### MySQL

```bash
pip install fastapi uvicorn sqlalchemy pymysql
```

#### SQLite

```bash
pip install fastapi uvicorn sqlalchemy
```

#### Extras recomendados

```bash
pip install alembic pydantic-settings pytest
```

### URLs típicas de conexión

```python
# SQLite
sqlite_url = "sqlite:///./app.db"

# PostgreSQL sync
postgres_url = "postgresql+psycopg2://<usuario>:<password>@<host>:5432/<db_name>"

# PostgreSQL async
postgres_async_url = "postgresql+asyncpg://<usuario>:<password>@<host>:5432/<db_name>"

# MySQL
mysql_url = "mysql+pymysql://<usuario>:<password>@<host>:3306/<db_name>"
```

### Recomendación

Si estás empezando con FastAPI + SQLAlchemy, usa primero:

- **PostgreSQL** como motor realista de producción
- **modo sync** para aprender el flujo sin complejidad extra

Después ya decides si `AsyncSession` te aporta valor.

---

## 4. Arquitectura recomendada en un proyecto FastAPI

Una integración limpia evita meter consultas SQLAlchemy directamente dentro de cada endpoint.

```text
app/
├── main.py
├── core/
│   └── config.py
├── db/
│   ├── base.py
│   ├── session.py
│   ├── models/
│   │   ├── usuario.py
│   │   └── pedido.py
│   └── migrations/
├── schemas/
│   ├── usuario.py
│   └── pedido.py
├── repositories/
│   └── usuario_repository.py
├── services/
│   └── usuario_service.py
├── api/
│   └── routes/
│       └── usuarios.py
└── tests/
    └── test_usuarios.py
```

### Responsabilidades

- **db/session.py**: engine y fábrica de sesiones
- **db/base.py**: base declarativa común
- **db/models/**: entidades ORM
- **schemas/**: modelos de entrada/salida con Pydantic
- **repositories/**: consultas y persistencia
- **services/**: reglas de negocio
- **routes/**: capa HTTP y dependencias FastAPI

Esto hace que SQLAlchemy quede encapsulado y no se mezcle con la capa web.

---

## 5. Configurar engine, sesión y base declarativa

### `core/config.py`

```python
from pydantic_settings import BaseSettings, SettingsConfigDict

class Settings(BaseSettings):
    database_url: str

    model_config = SettingsConfigDict(env_file=".env", extra="ignore")

settings = Settings()
```

### `db/base.py`

```python
from sqlalchemy.orm import DeclarativeBase

class Base(DeclarativeBase):
    pass
```

### `db/session.py`

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

from app.core.config import settings

engine = create_engine(
    settings.database_url,
    pool_pre_ping=True,
    future=True,
)

SessionLocal = sessionmaker(
    bind=engine,
    autoflush=False,
    autocommit=False,
    expire_on_commit=False,
)
```

### Por qué importa cada pieza

- **engine**: representa el acceso al motor y al pool de conexiones
- **SessionLocal**: crea sesiones independientes por petición
- **`autoflush=False`**: evita escrituras implícitas inesperadas
- **`expire_on_commit=False`**: práctico en APIs para devolver objetos recién guardados sin recargarlos siempre
- **`pool_pre_ping=True`**: ayuda a detectar conexiones muertas en entornos reales

### Nota sobre SQLite

Si usas SQLite en desarrollo:

```python
engine = create_engine(
    "sqlite:///./app.db",
    connect_args={"check_same_thread": False},
)
```

Ese `check_same_thread=False` es específico de SQLite.

---

## 6. Definir modelos con SQLAlchemy 2.0

El estilo moderno usa `Mapped[...]` y `mapped_column()`.

### `db/models/usuario.py`

```python
from datetime import datetime

from sqlalchemy import Boolean, DateTime, String, func
from sqlalchemy.orm import Mapped, mapped_column

from app.db.base import Base

class Usuario(Base):
    __tablename__ = "usuarios"

    id: Mapped[int] = mapped_column(primary_key=True, index=True)
    nombre: Mapped[str] = mapped_column(String(100), nullable=False)
    email: Mapped[str] = mapped_column(String(150), unique=True, index=True, nullable=False)
    hashed_password: Mapped[str] = mapped_column(String(255), nullable=False)
    activo: Mapped[bool] = mapped_column(Boolean, default=True, nullable=False)
    creado_en: Mapped[datetime] = mapped_column(
        DateTime(timezone=True),
        server_default=func.now(),
        nullable=False,
    )
```

### Ventajas de este estilo

- mejor tipado estático
- más claridad sobre qué atributos están mapeados
- alineado con SQLAlchemy 2.x
- mejor autocompletado en editores

### Cuándo seguir usando `Column`

Todavía verás mucho código con `Column(...)`. Sigue siendo válido, pero si empiezas desde cero conviene adoptar el estilo moderno.

---

## 7. Relaciones entre modelos

### Ejemplo `Usuario` → `Pedido`

```python
from datetime import datetime

from sqlalchemy import DateTime, ForeignKey, Numeric, String, func
from sqlalchemy.orm import Mapped, mapped_column, relationship

from app.db.base import Base

class Usuario(Base):
    __tablename__ = "usuarios"

    id: Mapped[int] = mapped_column(primary_key=True)
    nombre: Mapped[str] = mapped_column(String(100), nullable=False)

    pedidos: Mapped[list["Pedido"]] = relationship(
        back_populates="usuario",
        cascade="all, delete-orphan",
    )

class Pedido(Base):
    __tablename__ = "pedidos"

    id: Mapped[int] = mapped_column(primary_key=True)
    total: Mapped[float] = mapped_column(Numeric(10, 2), nullable=False)
    estado: Mapped[str] = mapped_column(String(50), nullable=False)
    creado_en: Mapped[datetime] = mapped_column(
        DateTime(timezone=True),
        server_default=func.now(),
        nullable=False,
    )
    usuario_id: Mapped[int] = mapped_column(ForeignKey("usuarios.id"), index=True)

    usuario: Mapped[Usuario] = relationship(back_populates="pedidos")
```

### Ideas importantes

- **`ForeignKey`** representa la clave foránea en base de datos
- **`relationship`** crea navegación entre objetos Python
- **`back_populates`** enlaza ambos lados de la relación
- **`cascade="all, delete-orphan"`** elimina hijos huérfanos cuando corresponde

### Cuidado

No expongas relaciones profundas sin control en la respuesta de la API, porque puedes:

- serializar más datos de los necesarios
- provocar consultas extra no deseadas
- crear estructuras difíciles de mantener

---

## 8. Separar ORM, schemas y lógica de aplicación

Un error muy habitual es usar el mismo modelo para todo.

Debes separar:

1. **modelo ORM**: representa la tabla
2. **schema de entrada**: define lo que acepta la API
3. **schema de salida**: define lo que devuelve la API
4. **servicio/repositorio**: ejecuta consultas y reglas de negocio

### `schemas/usuario.py`

```python
from datetime import datetime

from pydantic import BaseModel, EmailStr

class UsuarioCreate(BaseModel):
    nombre: str
    email: EmailStr
    password: str

class UsuarioUpdate(BaseModel):
    nombre: str | None = None
    email: EmailStr | None = None
    activo: bool | None = None

class UsuarioResponse(BaseModel):
    id: int
    nombre: str
    email: EmailStr
    activo: bool
    creado_en: datetime

    model_config = {"from_attributes": True}
```

### Por qué `from_attributes`

Permite que Pydantic convierta un objeto ORM a la respuesta JSON usando atributos del objeto, no solo diccionarios.

---

## 9. Dependencias de FastAPI para abrir y cerrar sesión

FastAPI encaja muy bien con SQLAlchemy cuando cada petición usa su propia sesión.

### `api/dependencies.py`

```python
from collections.abc import Generator
from sqlalchemy.orm import Session

from app.db.session import SessionLocal

def get_db() -> Generator[Session, None, None]:
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

### Uso en un endpoint

```python
from fastapi import APIRouter, Depends
from sqlalchemy.orm import Session

from app.api.dependencies import get_db

router = APIRouter(prefix="/usuarios", tags=["Usuarios"])

@router.get("/")
def listar_usuarios(db: Session = Depends(get_db)):
    ...
```

### Ventaja del patrón

- la sesión dura lo mismo que la petición
- no dejas conexiones abiertas
- puedes reutilizar la dependencia en toda la app
- el testing es más sencillo porque puedes sobrescribirla

---

## 10. CRUD con estilo SQLAlchemy 2.0

### Repositorio básico

```python
from sqlalchemy import select
from sqlalchemy.orm import Session

from app.db.models.usuario import Usuario
from app.schemas.usuario import UsuarioCreate, UsuarioUpdate

class UsuarioRepository:
    def __init__(self, db: Session) -> None:
        self.db = db

    def get_by_id(self, usuario_id: int) -> Usuario | None:
        stmt = select(Usuario).where(Usuario.id == usuario_id)
        return self.db.execute(stmt).scalar_one_or_none()

    def get_by_email(self, email: str) -> Usuario | None:
        stmt = select(Usuario).where(Usuario.email == email)
        return self.db.execute(stmt).scalar_one_or_none()

    def list(self, skip: int = 0, limit: int = 20) -> list[Usuario]:
        stmt = select(Usuario).offset(skip).limit(limit).order_by(Usuario.id.desc())
        return list(self.db.execute(stmt).scalars().all())

    def create(self, data: UsuarioCreate, hashed_password: str) -> Usuario:
        usuario = Usuario(
            nombre=data.nombre,
            email=data.email,
            hashed_password=hashed_password,
        )
        self.db.add(usuario)
        self.db.commit()
        self.db.refresh(usuario)
        return usuario

    def update(self, usuario: Usuario, data: UsuarioUpdate) -> Usuario:
        cambios = data.model_dump(exclude_unset=True)
        for campo, valor in cambios.items():
            setattr(usuario, campo, valor)

        self.db.commit()
        self.db.refresh(usuario)
        return usuario

    def delete(self, usuario: Usuario) -> None:
        self.db.delete(usuario)
        self.db.commit()
```

### Endpoint apoyado en repositorio

```python
from fastapi import APIRouter, Depends, HTTPException, status
from sqlalchemy.orm import Session

from app.api.dependencies import get_db
from app.repositories.usuario_repository import UsuarioRepository
from app.schemas.usuario import UsuarioCreate, UsuarioResponse

router = APIRouter(prefix="/usuarios", tags=["Usuarios"])

@router.post("/", response_model=UsuarioResponse, status_code=status.HTTP_201_CREATED)
def crear_usuario(data: UsuarioCreate, db: Session = Depends(get_db)):
    repo = UsuarioRepository(db)

    existente = repo.get_by_email(data.email)
    if existente:
        raise HTTPException(status_code=409, detail="El email ya existe")

    usuario = repo.create(data, hashed_password="hash-ejemplo")
    return usuario
```

### Idea importante

El endpoint coordina HTTP; SQLAlchemy vive en el repositorio o servicio.

---

## 11. Consultas habituales en APIs

### Buscar uno por ID

```python
stmt = select(Usuario).where(Usuario.id == usuario_id)
usuario = db.execute(stmt).scalar_one_or_none()
```

### Filtrar por varios campos

```python
stmt = select(Usuario).where(
    Usuario.activo.is_(True),
    Usuario.email.ilike("%empresa.com"),
)
usuarios = db.execute(stmt).scalars().all()
```

### Ordenar

```python
stmt = select(Usuario).order_by(Usuario.creado_en.desc())
```

### Obtener solo algunas columnas

```python
stmt = select(Usuario.id, Usuario.nombre, Usuario.email)
filas = db.execute(stmt).all()
```

### Contar registros

```python
from sqlalchemy import func, select

stmt = select(func.count()).select_from(Usuario)
total = db.execute(stmt).scalar_one()
```

### Join explícito

```python
stmt = (
    select(Pedido)
    .join(Pedido.usuario)
    .where(Usuario.activo.is_(True))
    .order_by(Pedido.creado_en.desc())
)
pedidos = db.execute(stmt).scalars().all()
```

### Cuándo usar `scalar_one_or_none()` y `scalars()`

- **`scalar_one_or_none()`**: para esperar como máximo un registro
- **`scalar_one()`**: para exigir exactamente un registro
- **`scalars().all()`**: para obtener una lista de entidades ORM

---

## 12. Transacciones, commit, rollback y refresh

SQLAlchemy no guarda cambios en base hasta que haces `commit()`.

### Flujo típico

```python
usuario = Usuario(nombre="Ana", email="ana@empresa.com", hashed_password="hash")
db.add(usuario)
db.commit()
db.refresh(usuario)
```

### Qué hace cada operación

- **`add()`**: marca el objeto para inserción
- **`commit()`**: confirma la transacción
- **`refresh()`**: vuelve a cargar el objeto desde base
- **`rollback()`**: deshace la transacción actual si algo falla

### Patrón seguro

```python
from sqlalchemy.exc import SQLAlchemyError

try:
    db.add(usuario)
    db.commit()
    db.refresh(usuario)
except SQLAlchemyError:
    db.rollback()
    raise
```

### Cuándo es importante `refresh()`

Especialmente cuando quieres devolver:

- `id` autogenerado
- timestamps generados por la base
- valores por defecto calculados en el motor

---

## 13. Manejo de errores de base de datos

En FastAPI no conviene dejar escapar excepciones SQLAlchemy crudas hasta el cliente.

### Capturar errores de integridad

```python
from fastapi import HTTPException
from sqlalchemy.exc import IntegrityError

try:
    db.add(usuario)
    db.commit()
except IntegrityError:
    db.rollback()
    raise HTTPException(status_code=409, detail="Conflicto de integridad en base de datos")
```

### Errores frecuentes

- **`IntegrityError`**: claves únicas duplicadas, claves foráneas inválidas
- **`OperationalError`**: problemas de conexión o del motor
- **`ProgrammingError`**: SQL inválido o esquema inconsistente

### Recomendación

- traduce errores de persistencia a errores HTTP claros
- haz `rollback()` siempre tras un fallo en transacción
- registra el error internamente, pero no expongas detalles sensibles

---

## 14. Carga eficiente de relaciones

Uno de los temas más importantes en SQLAlchemy es **cómo se cargan las relaciones**.

### Problema clásico: N+1

Pides una lista de usuarios y luego, para cada usuario, accedes a `usuario.pedidos`. Eso puede disparar muchas consultas pequeñas.

### Solución con `selectinload`

```python
from sqlalchemy import select
from sqlalchemy.orm import selectinload

stmt = (
    select(Usuario)
    .options(selectinload(Usuario.pedidos))
    .order_by(Usuario.id)
)
usuarios = db.execute(stmt).scalars().all()
```

### Cuándo usar cada estrategia

- **`selectinload`**: muy buena opción general para colecciones
- **`joinedload`**: útil cuando quieres traer la relación en un join
- **lazy loading**: cómodo, pero peligroso si no controlas cuántas consultas dispara

### En APIs FastAPI

Si sabes que el response necesita relaciones, cárgalas de forma explícita en la consulta. No dependas del acceso perezoso por accidente.

---

## 15. Paginación, filtros y ordenación

Casi cualquier API termina necesitando esto.

### Ejemplo de listado paginado

```python
from sqlalchemy import select

stmt = (
    select(Usuario)
    .where(Usuario.activo.is_(True))
    .order_by(Usuario.id.desc())
    .offset(skip)
    .limit(limit)
)
usuarios = db.execute(stmt).scalars().all()
```

### Recomendaciones

- valida `skip` y `limit` en FastAPI
- pon límites máximos razonables
- define orden consistente
- si el volumen crece mucho, considera paginación por cursor

### Ejemplo de endpoint

```python
from fastapi import Depends, Query
from sqlalchemy.orm import Session

@router.get("/", response_model=list[UsuarioResponse])
def listar_usuarios(
    skip: int = Query(0, ge=0),
    limit: int = Query(20, ge=1, le=100),
    db: Session = Depends(get_db),
):
    repo = UsuarioRepository(db)
    return repo.list(skip=skip, limit=limit)
```

---

## 16. Uso asíncrono con AsyncSession

SQLAlchemy soporta integración async, pero no deberías usarla solo por moda.

### Cuándo merece la pena

- usas drivers async reales como `asyncpg`
- tu aplicación maneja mucha concurrencia
- toda tu capa de datos será coherente en async

### `db/session.py` en async

```python
from sqlalchemy.ext.asyncio import AsyncSession, async_sessionmaker, create_async_engine

from app.core.config import settings

engine = create_async_engine(settings.database_url, pool_pre_ping=True)
AsyncSessionLocal = async_sessionmaker(
    bind=engine,
    autoflush=False,
    expire_on_commit=False,
)
```

### Dependencia async

```python
from collections.abc import AsyncGenerator

async def get_db() -> AsyncGenerator[AsyncSession, None]:
    async with AsyncSessionLocal() as db:
        yield db
```

### Consulta async

```python
from sqlalchemy import select
from sqlalchemy.ext.asyncio import AsyncSession

async def get_by_email(db: AsyncSession, email: str) -> Usuario | None:
    stmt = select(Usuario).where(Usuario.email == email)
    result = await db.execute(stmt)
    return result.scalar_one_or_none()
```

### Endpoint async

```python
from fastapi import Depends
from sqlalchemy.ext.asyncio import AsyncSession

@router.get("/{usuario_id}", response_model=UsuarioResponse)
async def obtener_usuario(usuario_id: int, db: AsyncSession = Depends(get_db)):
    stmt = select(Usuario).where(Usuario.id == usuario_id)
    result = await db.execute(stmt)
    usuario = result.scalar_one_or_none()
    if not usuario:
        raise HTTPException(status_code=404, detail="Usuario no encontrado")
    return usuario
```

### Regla práctica

Si no tienes una razón clara para usar async en la base de datos, empieza en sync. Es más simple y suficiente en muchísimas APIs.

---

## 17. Migraciones con Alembic

En proyectos reales no deberías depender de `Base.metadata.create_all()` como mecanismo principal para evolucionar el esquema.

### Instalar e inicializar

```bash
pip install alembic
alembic init alembic
```

### Configurar `alembic/env.py`

```python
from app.db.base import Base
from app.db.models import usuario, pedido

target_metadata = Base.metadata
```

### Crear migración

```bash
alembic revision --autogenerate -m "crear tablas iniciales"
```

### Aplicar migraciones

```bash
alembic upgrade head
```

### Buenas prácticas con Alembic

- revisa siempre la migración generada
- no confíes ciegamente en `--autogenerate`
- mantén importados todos los modelos antes de construir `target_metadata`
- separa claramente desarrollo, test y producción con URLs distintas

---

## 18. Testing con base de datos

FastAPI facilita mucho el testing gracias a `dependency_overrides`.

### Objetivo

Sustituir `get_db` por una sesión de test aislada.

```python
import pytest
from fastapi.testclient import TestClient
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker
from sqlalchemy.pool import StaticPool

from app.api.dependencies import get_db
from app.db.base import Base
from app.main import app

SQLALCHEMY_DATABASE_URL = "sqlite://"

engine = create_engine(
    SQLALCHEMY_DATABASE_URL,
    connect_args={"check_same_thread": False},
    poolclass=StaticPool,
)
TestingSessionLocal = sessionmaker(bind=engine, autoflush=False, autocommit=False)

@pytest.fixture(scope="function")
def client():
    Base.metadata.create_all(bind=engine)

    def override_get_db():
        db = TestingSessionLocal()
        try:
            yield db
        finally:
            db.close()

    app.dependency_overrides[get_db] = override_get_db

    with TestClient(app) as test_client:
        yield test_client

    app.dependency_overrides.clear()
    Base.metadata.drop_all(bind=engine)
```

### Qué valida este patrón

- que tus endpoints funcionan de verdad
- que el wiring entre FastAPI y SQLAlchemy es correcto
- que puedes probar CRUD sin tocar tu base real

### Para proyectos grandes

Considera usar una base de test más parecida a producción, por ejemplo PostgreSQL en contenedor.

---

## 19. Buenas prácticas específicas para FastAPI + SQLAlchemy

### 1. Una sesión por petición

No reutilices la misma sesión globalmente entre requests.

### 2. No mezcles ORM y response model indiscriminadamente

Devuelve schemas Pydantic, no modelos ORM sin control.

### 3. Haz explícitas las relaciones necesarias

Si la respuesta necesita datos relacionados, usa `selectinload` o `joinedload`.

### 4. Usa `select()` en código nuevo

Es el estilo actual de SQLAlchemy 2.x.

### 5. Encapsula acceso a datos

Evita meter consultas complejas dentro del endpoint.

### 6. Haz rollback tras errores de escritura

Si falla un `commit()`, la sesión queda en estado problemático hasta hacer `rollback()`.

### 7. No hardcodees la URL de conexión

Usa variables de entorno y configuración centralizada.

### 8. Usa índices donde haga falta

Especialmente en:

- emails
- usernames
- claves foráneas
- columnas de búsqueda frecuente

### 9. No abuses del modo async

Solo úsalo si toda la cadena lo justifica.

### 10. Usa Alembic en cuanto el proyecto deje de ser un ejemplo pequeño

Es la forma mantenible de evolucionar el esquema.

---

## 20. Errores comunes

### Error 1: abrir sesiones manualmente en cada endpoint

Acaba generando código repetido y fugas de conexión.

### Error 2: olvidar `rollback()` tras un fallo

Después de una excepción en escritura, la sesión no queda lista mágicamente.

### Error 3: usar el modelo ORM como body y response a la vez

Eso mezcla persistencia con contrato HTTP.

### Error 4: dejar relaciones lazy en respuestas grandes

Puede disparar muchas consultas inesperadas.

### Error 5: usar `create_all()` como estrategia de producción

Sirve para demos, no para evolución seria del esquema.

### Error 6: no controlar duplicados con restricciones únicas

Debes esperar conflictos y traducirlos a respuestas HTTP correctas.

### Error 7: mezclar sync y async sin criterio

`Session` y `AsyncSession` no son intercambiables.

### Error 8: construir SQL inseguro manualmente

Si sales del ORM o del Core expresivo, evita concatenar strings con datos del usuario.

---

## 21. Estructura de proyecto recomendada

```text
app/
├── main.py
├── core/
│   └── config.py
├── db/
│   ├── base.py
│   ├── session.py
│   └── models/
│       ├── __init__.py
│       ├── usuario.py
│       └── pedido.py
├── schemas/
│   ├── usuario.py
│   └── pedido.py
├── repositories/
│   ├── usuario_repository.py
│   └── pedido_repository.py
├── services/
│   └── usuario_service.py
├── api/
│   ├── dependencies.py
│   └── routes/
│       └── usuarios.py
└── tests/
    └── test_usuarios.py
```

### Lectura rápida de la arquitectura

- **FastAPI** vive en `api/` y `main.py`
- **SQLAlchemy** vive sobre todo en `db/` y `repositories/`
- **Pydantic** vive en `schemas/`
- **la lógica de negocio** vive en `services/`

Esto facilita mantenimiento, testing y evolución del proyecto.

---

## 22. Resumen final

Si ya conoces FastAPI, lo más importante de SQLAlchemy para integrarlo bien es dominar estas ideas:

1. crear un `engine` y una fábrica de sesiones correctos
2. usar una sesión por petición mediante `Depends`
3. separar modelos ORM, schemas y lógica de acceso a datos
4. hacer consultas con el estilo moderno de SQLAlchemy 2.x
5. controlar transacciones, errores y relaciones explícitamente
6. usar Alembic para migraciones reales
7. elegir sync o async con criterio, no por moda

La combinación **FastAPI + SQLAlchemy + Pydantic + Alembic** sigue siendo una de las bases más sólidas para construir APIs Python mantenibles.

---

## 📚 Guías relacionadas de este repositorio

- [FastAPI](FASTAPI.MD)
- [Python + Bases de Datos + FastAPI](PYTHON_BASES_DE_DATOS_FASTAPI.MD)
- [Python Completo](PYTHON.MD)
- [SQL Avanzado](../../databases/SQL_AVANZADO.MD)
- [Relaciones entre Tablas SQL](../../databases/RELACIONES%20TABLAS%20CON%20SQL.MD)
