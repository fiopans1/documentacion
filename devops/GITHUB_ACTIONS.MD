# GUÍA COMPLETA DE GITHUB ACTIONS

## ¿QUÉ ES GITHUB ACTIONS?

**GitHub Actions** es la plataforma de automatización integrada en GitHub para ejecutar flujos de trabajo directamente dentro del ciclo de vida de un repositorio. Permite automatizar tareas como:

- Compilar proyectos
- Ejecutar tests
- Revisar calidad de código
- Publicar paquetes
- Desplegar aplicaciones
- Etiquetar versiones
- Responder a eventos del repositorio

En definición simple:

- **GitHub** aporta el repositorio, los eventos y la interfaz.
- **Actions** aporta el motor de automatización.
- **Workflow** es el archivo que describe qué se ejecuta.
- **Job** es un bloque de trabajo dentro del workflow.
- **Step** es cada paso individual del job.
- **Runner** es la máquina que ejecuta el job.

GitHub Actions forma parte de la filosofía **CI/CD**:

- **CI (Continuous Integration / Integración Continua)**: validar código automáticamente cada vez que hay cambios.
- **CD (Continuous Delivery / Continuous Deployment)**: preparar o desplegar software automáticamente tras pasar validaciones.

---

## ¿PARA QUÉ SIRVE?

GitHub Actions sirve para convertir tareas manuales repetitivas en procesos automáticos, reproducibles y auditables.

### Casos de uso habituales

1. Ejecutar tests al hacer `push` o abrir una `pull request`.
2. Ejecutar linters y análisis estático.
3. Construir imágenes Docker.
4. Publicar librerías en npm, Maven Central o PyPI.
5. Desplegar aplicaciones a servidores o cloud providers.
6. Crear releases automáticamente.
7. Comentar en issues o PRs.
8. Sincronizar ramas o etiquetas.
9. Generar documentación.
10. Ejecutar tareas programadas con cron.

---

## CONCEPTOS FUNDAMENTALES

## REPOSITORIO DE WORKFLOWS

Los workflows se guardan dentro de:

```text
.github/workflows/
```

Cada archivo YAML dentro de esa carpeta define un workflow.

Ejemplo:

```text
.github/workflows/ci.yml
```

---

## WORKFLOW

Un **workflow** es una definición completa de automatización expresada en YAML.

Un workflow incluye normalmente:

- Nombre
- Eventos que lo disparan
- Uno o varios jobs
- Permisos
- Variables
- Condiciones

Ejemplo mínimo:

```yaml
name: CI básica

on:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: echo "Hola desde GitHub Actions"
```

### Definición de cada parte

- `name`: nombre visible del workflow en la interfaz de GitHub.
- `on`: indica qué evento lo ejecuta.
- `jobs`: agrupa los trabajos del workflow.
- `test`: identificador del job.
- `runs-on`: sistema operativo o runner usado.
- `steps`: lista ordenada de acciones o comandos.
- `uses`: reutiliza una action existente.
- `run`: ejecuta comandos de shell.

### Ejemplo explicado paso a paso

Imagina que tienes una API en Node.js y quieres validar automáticamente el proyecto cada vez que alguien sube cambios a `main`.

```yaml
name: Validar API

on:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm ci
      - run: npm test
```

Qué significa cada línea en un caso real:

- `name: Validar API` → en GitHub verás una ejecución llamada “Validar API”.
- `on: push` → solo arranca cuando alguien hace `git push`.
- `branches: [main]` → no se ejecuta en cualquier rama, solo en `main`.
- `jobs.test` → el workflow tiene un trabajo llamado `test`.
- `runs-on: ubuntu-latest` → GitHub prepara una máquina Linux temporal.
- `actions/checkout@v4` → descarga el código del repositorio dentro de esa máquina.
- `npm ci` → instala dependencias exactamente como están en el lockfile.
- `npm test` → ejecuta la batería de tests.

Si `npm test` falla, GitHub marcará la ejecución en rojo y sabrás que el cambio rompió algo antes de desplegar.

---

## EVENTO

Un **evento** es la señal que dispara un workflow.

GitHub Actions escucha eventos del repositorio y del ecosistema GitHub.

### Eventos comunes

- `push`
- `pull_request`
- `workflow_dispatch`
- `schedule`
- `release`
- `issues`
- `issue_comment`
- `pull_request_review`
- `workflow_call`
- `repository_dispatch`

### Ejemplo con varios eventos

```yaml
on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]
  workflow_dispatch:
```

### Tipos de filtros

Puedes filtrar por:

- Ramas
- Tags
- Paths
- Tipos de acción del evento

Ejemplo:

```yaml
on:
  push:
    branches:
      - main
    paths:
      - "src/**"
      - "package.json"
```

Esto significa que el workflow se ejecuta solo si el `push` afecta a esos archivos o rutas.

### Ejemplo para entenderlo perfectamente

Supón que tienes un monorepo con frontend y backend:

- Si cambias `src/app.js`, el workflow sí se ejecuta.
- Si cambias `README.md` y no está dentro de `paths`, el workflow no se ejecuta.

Eso evita gastar tiempo y minutos de CI cuando el cambio no afecta a la parte que quieres validar.

---

## JOB

Un **job** es una unidad de trabajo dentro del workflow.

Características:

- Se ejecuta en un runner.
- Tiene sus propios steps.
- Puede depender de otros jobs.
- Por defecto, los jobs corren en paralelo si no hay dependencias.

Ejemplo:

```yaml
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Lint"

  test:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Tests"
```

Aquí `lint` y `test` pueden ejecutarse al mismo tiempo.

### Dependencias entre jobs

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Compilar"

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: echo "Desplegar"
```

`deploy` solo empezará cuando `build` termine correctamente.

### Ejemplo práctico

Piensa en este flujo:

1. `build` compila la aplicación.
2. `test` valida que todo funciona.
3. `deploy` publica el resultado.

Si `build` falla, no tendría sentido desplegar algo que ni siquiera compila. Por eso `needs` convierte varios jobs sueltos en una tubería lógica.

---

## STEP

Un **step** es un paso individual dentro de un job.

Puede ser:

- Una action (`uses`)
- Un comando shell (`run`)

Ejemplo:

```yaml
steps:
  - uses: actions/checkout@v4
  - name: Instalar dependencias
    run: npm ci
  - name: Ejecutar tests
    run: npm test
```

### Importante

- Los steps de un mismo job se ejecutan de forma secuencial.
- Si un step falla, el job normalmente falla.
- Los steps comparten el mismo entorno dentro del mismo job.

### Ejemplo muy claro

```yaml
steps:
  - uses: actions/checkout@v4
  - run: npm ci
  - run: npm test
```

Aquí ocurre esto:

1. Primero se descarga el código.
2. Después se instalan las dependencias.
3. Solo cuando la instalación termina bien se ejecutan los tests.

Si `npm ci` falla porque el lockfile está roto, `npm test` ni siquiera arrancará. Eso ayuda a localizar exactamente en qué paso se rompió la ejecución.

---

## RUNNER

Un **runner** es la máquina que ejecuta el job.

GitHub ofrece runners hospedados por GitHub y también puedes tener runners propios.

### GitHub-hosted runners

Ejemplos:

- `ubuntu-latest`
- `windows-latest`
- `macos-latest`

Ventajas:

- Configuración rápida
- Entornos listos para usar
- Mantenimiento a cargo de GitHub

Desventajas:

- Menor control total del entorno
- Tiempo de arranque
- Limitaciones de recursos según plan

### Self-hosted runners

Son máquinas tuyas o de tu infraestructura registradas en GitHub.

Ventajas:

- Control completo del entorno
- Acceso a red privada interna
- Dependencias persistentes
- Hardware especializado

Desventajas:

- Tú administras seguridad, mantenimiento y actualizaciones
- Riesgo mayor si ejecutas workflows de contribuciones no confiables

Ejemplo:

```yaml
runs-on: self-hosted
```

También puedes usar labels:

```yaml
runs-on: [self-hosted, linux, x64]
```

### Ejemplo para decidir qué runner usar

- Si solo necesitas ejecutar `npm test`, normalmente `ubuntu-latest` es suficiente.
- Si necesitas conectarte a una base de datos privada dentro de tu red corporativa, probablemente necesites `self-hosted`.

La idea clave es esta: el runner no es una etiqueta decorativa, es la máquina real donde se ejecutará tu automatización.

---

## ACTION

Una **action** es una pieza reutilizable de automatización.

Puede hacer tareas como:

- Descargar el repositorio
- Configurar un lenguaje
- Subir artefactos
- Desplegar a un proveedor
- Enviar notificaciones

### Formas de usar una action

1. **Action pública del Marketplace**
2. **Action del mismo repositorio**
3. **Action local**
4. **Composite action**
5. **Docker action**
6. **JavaScript action**

Ejemplo:

```yaml
- uses: actions/checkout@v4
```

Eso invoca una action oficial que clona el contenido del repositorio en el runner.

### Ejemplo para verlo claro

Sin `checkout`, el runner arranca vacío respecto a tu proyecto. Eso significa que comandos como:

```yaml
- run: ls
```

no mostrarían los archivos de tu repositorio si antes no hiciste:

```yaml
- uses: actions/checkout@v4
```

Por eso `checkout` suele ser uno de los primeros steps de casi cualquier workflow.

### Recomendación de seguridad

Siempre que sea posible:

- Usa actions oficiales o confiables
- Fija versiones estables
- Mejor aún, fija por SHA cuando la seguridad sea crítica

Ejemplo más seguro:

```yaml
- uses: actions/checkout@8ade135a41bc03ea155e62e844d188df1ea18608
```

---

## ESTRUCTURA GENERAL DE UN WORKFLOW

Ejemplo realista:

```yaml
name: Node CI

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

permissions:
  contents: read

env:
  NODE_VERSION: "20"

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Descargar código
        uses: actions/checkout@v4

      - name: Configurar Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: npm

      - name: Instalar dependencias
        run: npm ci

      - name: Ejecutar tests
        run: npm test
```

### Qué está haciendo este workflow realmente

Este ejemplo representa una CI muy típica:

1. Se dispara al subir cambios o abrir una PR.
2. Usa un token con permiso mínimo de lectura.
3. Define la versión de Node una sola vez en `env`.
4. Descarga el código.
5. Prepara el runtime.
6. Instala dependencias.
7. Ejecuta los tests.

Traducido a lenguaje cotidiano: “cada vez que alguien cambie el proyecto, prepara una máquina limpia, instala todo y comprueba que la aplicación sigue funcionando”.

---

## SINTAXIS YAML EN GITHUB ACTIONS

GitHub Actions usa **YAML**, un formato sensible a la indentación.

### Reglas básicas

- Usa espacios, no tabulaciones
- Mantén la indentación consistente
- Las listas usan `-`
- Los mapas usan `clave: valor`

Ejemplo:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Paso 1
        run: echo "Hola"
```

Un error de indentación puede invalidar el workflow completo.

---

## DIRECTIVAS IMPORTANTES

## `name`

Nombre visual del workflow o del step.

```yaml
name: Deploy Producción
```

## `on`

Define eventos disparadores.

```yaml
on: push
```

o:

```yaml
on:
  push:
    branches: [main]
```

## `jobs`

Contiene todos los jobs del workflow.

## `runs-on`

Define el runner.

## `steps`

Lista de pasos.

## `uses`

Reutiliza una action.

## `run`

Ejecuta comandos shell.

## `with`

Pasa inputs a una action.

```yaml
- uses: actions/setup-node@v4
  with:
    node-version: 20
```

## `env`

Define variables de entorno.

```yaml
env:
  APP_ENV: production
```

## `needs`

Crea dependencias entre jobs.

## `if`

Ejecuta condicionalmente workflows, jobs o steps.

## `permissions`

Controla permisos del `GITHUB_TOKEN`.

## `defaults`

Define opciones por defecto para `run`.

Ejemplo:

```yaml
defaults:
  run:
    shell: bash
```

## `timeout-minutes`

Establece tiempo máximo de ejecución.

## `continue-on-error`

Permite que un step o job falle sin romper el workflow.

### Mini ejemplo conjunto

```yaml
jobs:
  lint:
    runs-on: ubuntu-latest
    timeout-minutes: 10
    steps:
      - uses: actions/checkout@v4
      - run: npm ci
      - run: npm run lint

  experimento:
    if: github.ref == 'refs/heads/develop'
    runs-on: ubuntu-latest
    continue-on-error: true
    steps:
      - run: echo "Prueba no bloqueante"
```

Cómo leerlo:

- `timeout-minutes: 10` evita que `lint` se quede colgado indefinidamente.
- `if:` hace que `experimento` solo corra en `develop`.
- `continue-on-error: true` permite que ese job falle sin romper toda la pipeline.

---

## CONTEXTOS Y EXPRESIONES

Los **contextos** son objetos con información del workflow, evento, runner, jobs y pasos. Se usan con la sintaxis:

```yaml
${{ ... }}
```

### Contextos más usados

- `github`
- `env`
- `vars`
- `secrets`
- `job`
- `steps`
- `runner`
- `matrix`
- `needs`
- `inputs`

### Ejemplos

```yaml
run: echo "${{ github.ref }}"
```

```yaml
run: echo "${{ github.actor }}"
```

```yaml
if: github.ref == 'refs/heads/main'
```

### Datos útiles del contexto `github`

- `github.repository`
- `github.actor`
- `github.ref`
- `github.sha`
- `github.event_name`
- `github.event`
- `github.workspace`

### Operadores frecuentes

- `==`
- `!=`
- `&&`
- `||`
- `!`

### Funciones frecuentes

- `success()`
- `failure()`
- `cancelled()`
- `always()`
- `contains()`
- `startsWith()`
- `endsWith()`
- `format()`
- `join()`
- `toJson()`
- `fromJson()`
- `hashFiles()`

Ejemplo:

```yaml
if: contains(github.event.pull_request.title, 'release')
```

### Ejemplo explicado en lenguaje simple

Supón que una PR se llama:

```text
release: versión 2.0.0
```

Entonces esta condición:

```yaml
if: contains(github.event.pull_request.title, 'release')
```

devuelve `true`, y GitHub ejecuta el job o step correspondiente.

Si la PR se llamara `fix: corregir login`, devolvería `false` y esa parte del workflow se saltaría.

---

## VARIABLES, VARS Y SECRETS

## `env`

`env` define variables de entorno.

Se puede usar a nivel:

- workflow
- job
- step

Ejemplo:

```yaml
env:
  APP_NAME: mi-app
```

Ejemplo real:

```yaml
env:
  APP_NAME: mi-app

steps:
  - run: echo "Construyendo $APP_NAME"
```

Eso evita repetir el mismo valor muchas veces.

## `vars`

`vars` son variables configurables en GitHub a nivel de repositorio, organización o entorno.

Ejemplo:

```yaml
run: echo "${{ vars.APP_REGION }}"
```

Ejemplo real:

Si en GitHub defines una variable de repositorio llamada `APP_REGION=eu-west-1`, puedes reutilizarla así:

```yaml
- run: echo "La región es ${{ vars.APP_REGION }}"
```

Es útil para valores compartidos que no son secretos.

Se usan para valores no sensibles como:

- regiones
- nombres de entorno
- flags de configuración

## `secrets`

`secrets` almacena información sensible:

- tokens
- contraseñas
- claves privadas
- certificados

Ejemplo:

```yaml
env:
  API_TOKEN: ${{ secrets.API_TOKEN }}
```

Ejemplo real:

```yaml
steps:
  - name: Llamar a una API privada
    env:
      API_TOKEN: ${{ secrets.API_TOKEN }}
    run: curl -H "Authorization: ******" https://api.example.com/data
```

Aquí el token nunca se escribe en el YAML en texto plano; GitHub lo inyecta en tiempo de ejecución.

### Buenas prácticas

- Nunca escribas secretos en texto plano en YAML.
- No hagas `echo` directo de secretos.
- Usa secretos por entorno cuando sea posible.
- Da el mínimo alcance necesario.

---

## GITHUB_TOKEN

GitHub crea automáticamente un token temporal para cada ejecución:

```yaml
${{ secrets.GITHUB_TOKEN }}
```

Hoy se suele usar mediante:

```yaml
github.token
```

o directamente por actions compatibles.

### ¿Para qué sirve?

- Leer contenido del repositorio
- Crear comentarios
- Subir estados
- Crear deployments
- Publicar paquetes según permisos

### Muy importante: permisos

Debes limitar sus permisos con `permissions`.

Ejemplo seguro:

```yaml
permissions:
  contents: read
```

Ejemplo para comentar en PR:

```yaml
permissions:
  contents: read
  pull-requests: write
```

Principio recomendado: **mínimo privilegio**.

### Ejemplo práctico de uso

Si un workflow solo ejecuta tests, `contents: read` suele ser suficiente.

Si además quieres que publique un comentario automático en la PR cuando terminan los tests, necesitarás algo como:

```yaml
permissions:
  contents: read
  pull-requests: write
```

La lógica es: das permiso extra solo a la capacidad concreta que realmente vas a usar.

---

## FILTROS DE RAMAS, TAGS Y PATHS

Puedes controlar exactamente cuándo corre un workflow.

### Por ramas

```yaml
on:
  push:
    branches:
      - main
      - develop
```

### Excluir ramas

```yaml
on:
  pull_request:
    branches-ignore:
      - experimental/**
```

### Por tags

```yaml
on:
  push:
    tags:
      - "v*"
```

### Por rutas

```yaml
on:
  push:
    paths:
      - "backend/**"
      - ".github/workflows/**"
```

### Ignorar rutas

```yaml
on:
  push:
    paths-ignore:
      - "docs/**"
```

### Ejemplo muy fácil de visualizar

- Un commit que solo cambia `docs/guia.md` no lanzará el workflow.
- Un commit que cambia `src/server.js` sí lo lanzará.

Esto es muy útil cuando separas workflows de documentación, frontend, backend o infraestructura.

---

## WORKFLOW_DISPATCH

`workflow_dispatch` permite lanzar workflows manualmente desde la interfaz de GitHub.

Ejemplo:

```yaml
on:
  workflow_dispatch:
    inputs:
      environment:
        description: "Entorno de despliegue"
        required: true
        default: "staging"
```

Uso:

- despliegues manuales
- tareas operativas
- ejecuciones bajo demanda
- mantenimiento

Luego accedes al input así:

```yaml
${{ inputs.environment }}
```

### Ejemplo completo de uso manual

Imagina un botón en GitHub que te deja elegir `staging` o `production`.

```yaml
- run: echo "Voy a desplegar en ${{ inputs.environment }}"
```

Si el usuario lanza manualmente el workflow y elige `production`, ese comando imprimirá exactamente ese valor. Es una forma muy cómoda de crear operaciones controladas bajo demanda.

---

## SCHEDULE

Permite ejecutar workflows por tiempo usando cron.

Ejemplo:

```yaml
on:
  schedule:
    - cron: "0 3 * * *"
```

Esto significa todos los días a las 03:00 UTC.

### Ejemplo práctico

Un caso típico es regenerar cada noche un reporte:

```yaml
on:
  schedule:
    - cron: "0 3 * * *"
```

En lenguaje normal: “todos los días, a las 03:00 UTC, ejecuta este workflow aunque nadie haya hecho push”.

### Usos comunes

- backups
- sincronizaciones
- limpieza
- rotación de artefactos
- tests nocturnos

### Ojo

- Se usa horario UTC
- No garantiza precisión al segundo
- En repositorios inactivos puede haber particularidades de ejecución

---

## MATRICES

La estrategia **matrix** permite ejecutar el mismo job con múltiples combinaciones.

Ejemplo:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node: [18, 20, 22]
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node }}
      - run: npm ci
      - run: npm test
```

Esto ejecuta el job tres veces, una por cada versión de Node.

### Cómo imaginarlo

No pienses en un solo job mágico, sino en tres jobs casi idénticos:

- uno con Node 18
- otro con Node 20
- otro con Node 22

Eso te permite detectar si tu proyecto funciona en unas versiones y falla en otras.

### Matriz multidimensional

```yaml
strategy:
  matrix:
    os: [ubuntu-latest, windows-latest]
    python: ["3.10", "3.11", "3.12"]
```

### Opciones útiles

- `include`
- `exclude`
- `fail-fast`
- `max-parallel`

Ejemplo:

```yaml
strategy:
  fail-fast: false
  matrix:
    os: [ubuntu-latest, windows-latest]
    node: [18, 20]
    exclude:
      - os: windows-latest
        node: 18
```

---

## DEPENDENCIAS ENTRE JOBS CON `needs`

Sirve para construir pipelines.

Ejemplo:

```yaml
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - run: echo "lint"

  test:
    needs: lint
    runs-on: ubuntu-latest
    steps:
      - run: echo "test"

  deploy:
    needs: [lint, test]
    runs-on: ubuntu-latest
    steps:
      - run: echo "deploy"
```

Aquí `deploy` espera a `lint` y `test`.

También puedes leer outputs de jobs previos.

### Ejemplo mental

Es como decir:

- “Primero revisa estilo”
- “Luego ejecuta tests”
- “Solo si ambas cosas salen bien, despliega”

Sin `needs`, esos trabajos podrían correr en paralelo y el despliegue no tendría por qué esperar a que terminen las validaciones.

---

## OUTPUTS

Los **outputs** permiten pasar datos entre steps o jobs.

### Output de step

```yaml
- name: Generar valor
  id: generar
  run: echo "version=1.2.3" >> "$GITHUB_OUTPUT"
```

Uso:

```yaml
${{ steps.generar.outputs.version }}
```

Ejemplo explicado:

Ese output sirve cuando un step calcula un dato y el siguiente lo necesita. Por ejemplo, si un step genera la versión `1.2.3`, otro puede reutilizarla para etiquetar una imagen Docker o nombrar un artefacto.

### Output de job

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    outputs:
      app_version: ${{ steps.meta.outputs.version }}
    steps:
      - id: meta
        run: echo "version=1.2.3" >> "$GITHUB_OUTPUT"
```

En otro job:

```yaml
${{ needs.build.outputs.app_version }}
```

Ejemplo real:

El job `build` puede calcular la versión final y el job `deploy` usar exactamente ese mismo valor, evitando recalcularlo o cometer errores por duplicación.

---

## CONDICIONES CON `if`

Puedes controlar ejecución condicional.

### En jobs

```yaml
if: github.ref == 'refs/heads/main'
```

### En steps

```yaml
- name: Ejecutar solo si falló algo antes
  if: failure()
  run: echo "Hubo un error"
```

### Funciones importantes

- `success()`: todo fue bien
- `failure()`: hubo fallo
- `always()`: ejecutar siempre
- `cancelled()`: workflow cancelado

Muy útil para:

- notificaciones
- limpieza
- publicación selectiva
- despliegues restringidos

### Ejemplo muy común

```yaml
- name: Subir logs aunque falle el test
  if: failure()
  uses: actions/upload-artifact@v4
  with:
    name: logs
    path: logs/
```

Si los tests fallan, este step todavía se ejecuta y guarda los logs para analizarlos después.

---

## CACHÉ

El caché acelera workflows reutilizando dependencias descargadas previamente.

Ejemplo con Node:

```yaml
- uses: actions/setup-node@v4
  with:
    node-version: 20
    cache: npm
```

Ejemplo genérico:

```yaml
- uses: actions/cache@v4
  with:
    path: ~/.m2/repository
    key: ${{ runner.os }}-maven-${{ hashFiles('**/pom.xml') }}
    restore-keys: |
      ${{ runner.os }}-maven-
```

### Conceptos clave

- `path`: qué carpeta cachear
- `key`: identificador único del caché
- `restore-keys`: claves parciales para reutilizar cachés similares

### Buenas prácticas

- Usa claves estables pero vinculadas a lockfiles o manifests
- No caches archivos sensibles
- No abuses del caché si el coste de restaurarlo no compensa

### Ejemplo para entender cuándo ayuda

Si tu proyecto Node tarda 4 minutos en instalar dependencias desde cero y con caché tarda 40 segundos, el ahorro es clarísimo.

En cambio, si el directorio cacheado pesa mucho y recuperarlo tarda casi lo mismo que reinstalar, el caché deja de aportar valor.

---

## ARTEFACTOS

Los **artifacts** son archivos generados durante una ejecución y guardados temporalmente en GitHub.

Ejemplos:

- binarios
- reportes de test
- cobertura
- logs
- paquetes generados

Subida:

```yaml
- uses: actions/upload-artifact@v4
  with:
    name: reporte-tests
    path: coverage/
```

Descarga:

```yaml
- uses: actions/download-artifact@v4
  with:
    name: reporte-tests
```

### Diferencia entre artefacto y caché

- **Artefacto**: pensado para conservar resultados de una ejecución.
- **Caché**: pensado para acelerar ejecuciones futuras.

### Ejemplo comparativo

- Si generas `coverage/` y quieres descargar ese reporte al terminar la ejecución, eso es un **artefacto**.
- Si guardas `~/.m2/repository` para que el siguiente build de Maven sea más rápido, eso es **caché**.

---

## SERVICIOS

Puedes levantar servicios auxiliares para tests dentro de un job.

Ejemplo con PostgreSQL:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:16
        env:
          POSTGRES_USER: app
          POSTGRES_PASSWORD: secret
          POSTGRES_DB: app_db
        ports:
          - 5432:5432
        options: >-
          --health-cmd "pg_isready -U app"
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
```

Esto es muy útil para:

- bases de datos
- Redis
- colas
- brokers
- dependencias de integración

### Ejemplo práctico

Si tu aplicación necesita PostgreSQL para ejecutar tests de integración, GitHub levanta ese contenedor al arrancar el job. Así puedes probar consultas reales sin instalar manualmente PostgreSQL dentro del runner.

---

## WORKING DIRECTORY Y SHELL

Puedes configurar el directorio de trabajo y la shell por defecto.

```yaml
defaults:
  run:
    shell: bash
    working-directory: ./backend
```

También puedes hacerlo por step:

```yaml
- run: npm test
  working-directory: frontend
```

---

## REUSABLE WORKFLOWS

Un **reusable workflow** permite reutilizar un workflow completo desde otro workflow.

Se define con:

```yaml
on:
  workflow_call:
```

Ejemplo de workflow reutilizable:

```yaml
name: Reusable Node Test

on:
  workflow_call:
    inputs:
      node_version:
        required: true
        type: string

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ inputs.node_version }}
      - run: npm ci
      - run: npm test
```

Y para llamarlo:

```yaml
jobs:
  call-tests:
    uses: ./.github/workflows/reusable-node-test.yml
    with:
      node_version: "20"
```

### ¿Cuándo usarlo?

- Cuando varios repositorios comparten la misma automatización
- Cuando quieres centralizar lógica de CI/CD
- Cuando quieres reducir duplicación

### Ejemplo claro

Si tienes 5 repositorios Node y todos hacen exactamente `checkout + setup-node + npm ci + npm test`, repetir ese YAML en 5 sitios complica el mantenimiento. Un reusable workflow te deja definirlo una vez y llamarlo desde todos.

---

## COMPOSITE ACTIONS

Una **composite action** agrupa varios steps reutilizables como si fuera una action única.

Se define normalmente con un archivo `action.yml`.

Ejemplo conceptual:

```yaml
name: Setup Project
description: Configura dependencias del proyecto

runs:
  using: composite
  steps:
    - run: npm ci
      shell: bash
    - run: npm run build
      shell: bash
```

### Diferencia frente a reusable workflows

- **Composite action**: reutiliza steps.
- **Reusable workflow**: reutiliza jobs completos y configuración de workflow.

### Ejemplo para no confundirlos

- Si siempre repites “instalar dependencias + compilar assets”, una composite action encaja muy bien.
- Si quieres reutilizar un pipeline entero con permisos, inputs, runners y varios jobs, entonces necesitas un reusable workflow.

---

## ENTORNOS (`environments`)

Los **environments** sirven para representar destinos como:

- `dev`
- `staging`
- `production`

Permiten aplicar:

- secretos específicos
- reglas de protección
- aprobaciones manuales
- restricciones de despliegue

Ejemplo:

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: production
    steps:
      - run: echo "Deploy a producción"
```

### Muy útil para producción

Puedes exigir aprobación manual antes de que el job use secretos sensibles o haga un despliegue real.

### Ejemplo real

Un desarrollador puede lanzar un deploy, pero el job quedará esperando hasta que una persona autorizada apruebe el entorno `production`. Así evitas que cualquier ejecución despliegue directamente en producción sin control.

---

## CONCURRENCIA

La concurrencia evita ejecuciones duplicadas innecesarias.

Ejemplo:

```yaml
concurrency:
  group: deploy-${{ github.ref }}
  cancel-in-progress: true
```

Esto hace que si llega un nuevo workflow del mismo grupo:

- el anterior se cancele
- quede activo solo el más reciente

Muy útil para:

- despliegues
- tests largos
- ramas con pushes frecuentes

### Ejemplo muy típico

Si haces tres pushes seguidos a la misma rama en dos minutos, no tiene sentido mantener tres despliegues activos a la vez. Con `cancel-in-progress: true`, GitHub cancela los anteriores y deja vivo solo el más reciente.

---

## TIMEOUTS Y CONTROL DE ERRORES

### Timeout

```yaml
timeout-minutes: 30
```

Evita ejecuciones eternas.

### Continue on error

```yaml
continue-on-error: true
```

Útil cuando:

- una parte es informativa
- pruebas experimentales no deben bloquear
- quieres recopilar resultados aunque un paso falle

### Fail-fast en matrices

```yaml
strategy:
  fail-fast: false
```

Permite que las demás combinaciones sigan ejecutándose aunque una falle.

---

## PERMISOS

La clave `permissions` controla qué puede hacer el token automático del workflow.

Ejemplo minimalista:

```yaml
permissions:
  contents: read
```

Permisos comunes:

- `actions`
- `checks`
- `contents`
- `deployments`
- `id-token`
- `issues`
- `packages`
- `pull-requests`
- `statuses`

Ejemplo para publicar paquete:

```yaml
permissions:
  contents: read
  packages: write
```

Ejemplo para OIDC:

```yaml
permissions:
  id-token: write
  contents: read
```

### Regla de oro

Nunca des permisos globales si el job no los necesita.

---

## OIDC Y AUTENTICACIÓN MODERNA

**OIDC (OpenID Connect)** permite autenticarse ante proveedores cloud sin guardar credenciales permanentes en secretos.

Flujo simplificado:

1. GitHub emite un token de identidad temporal.
2. El proveedor cloud confía en GitHub bajo condiciones definidas.
3. El workflow intercambia ese token por credenciales temporales.

### Ventajas

- Evita secretos estáticos
- Reduce riesgo de filtración
- Mejor trazabilidad
- Credenciales temporales y limitadas

### Muy usado con

- AWS
- Azure
- GCP
- Vault

Para usarlo suele hacer falta:

```yaml
permissions:
  id-token: write
  contents: read
```

### Ejemplo conceptual

Antes muchas pipelines guardaban una clave cloud fija en `secrets`. Con OIDC, el workflow pide una identidad temporal, el proveedor la valida y devuelve credenciales de corta duración. El resultado es más seguro porque no dependes de un secreto permanente.

---

## EVENTOS `pull_request` VS `pull_request_target`

Esta diferencia es crítica para seguridad.

### `pull_request`

- Se ejecuta en el contexto del código de la PR
- Más seguro para código no confiable
- Normalmente no debe exponer secretos sensibles

### `pull_request_target`

- Se ejecuta en el contexto del repositorio base
- Tiene acceso a más privilegios en muchos casos
- Puede ser peligroso si se usa mal con código de forks

### Regla práctica

Usa `pull_request_target` solo cuando entiendas perfectamente su impacto y necesites permisos elevados para automatizaciones específicas como etiquetado o comentarios controlados.

No mezcles ejecución de código no confiable con secretos o tokens potentes.

### Ejemplo para verlo claro

- Para ejecutar tests sobre el código enviado por una PR externa, normalmente usarías `pull_request`.
- Para poner automáticamente una etiqueta o dejar un comentario usando permisos del repo base, podrías usar `pull_request_target`, pero evitando ejecutar scripts no confiables de la PR.

---

## SEGURIDAD EN GITHUB ACTIONS

La seguridad es uno de los temas más importantes.

## Riesgos comunes

1. Exposición de secretos
2. Uso de actions de terceros comprometidas
3. Inyección de comandos
4. Permisos excesivos del token
5. Ejecución de código no confiable en PRs de forks
6. Self-hosted runners inseguros

## Buenas prácticas

### 1. Principio de mínimo privilegio

Limita `permissions` al mínimo.

### 2. Usa secretos solo cuando sea imprescindible

Y preferiblemente por entorno.

### 3. Fija versiones de actions

Idealmente por SHA.

### 4. No interpolar datos no confiables sin validar

Por ejemplo, cuidado con:

```yaml
run: echo "${{ github.event.pull_request.title }}"
```

Si luego ese valor se usa en shell de forma insegura, puede generar problemas.

### 5. Separa validación de despliegue

No mezcles jobs que prueban código externo con jobs que usan secretos críticos.

### 6. Protege entornos

Usa `environment` con aprobaciones.

### 7. Revisa self-hosted runners

- Aísla red
- Limpia el entorno entre ejecuciones
- Restringe quién puede usarlos

### 8. Evita imprimir información sensible

GitHub enmascara secretos conocidos, pero no debes depender de ello.

### Ejemplo de error típico

Esto es mala idea:

```yaml
- run: echo "Token: ${{ secrets.API_TOKEN }}"
```

Aunque GitHub intente enmascararlo, exponer secretos en logs sigue siendo una práctica peligrosa y evitable.

---

## DEBUGGING Y DIAGNÓSTICO

Cuando un workflow falla debes revisar:

1. Logs de cada step
2. Variables calculadas
3. Condiciones `if`
4. Outputs
5. Artefactos generados

### Activar logs detallados

Puedes habilitar mayor nivel de detalle mediante secretos especiales de GitHub para debugging, cuando sea necesario.

### Técnicas útiles

- Usar `echo` para variables no sensibles
- Imprimir contexto serializado:

```yaml
- run: echo '${{ toJson(github) }}'
```

Hazlo con cuidado, porque el contexto puede contener datos que no convenga exponer.

### Guardar logs como artefacto

Muy útil cuando una aplicación genera reportes o salidas de test complejas.

### Ejemplo práctico

Si tus tests E2E generan capturas, vídeos o reportes HTML, puedes subirlos como artefactos para revisarlos desde la interfaz de GitHub sin volver a ejecutar localmente.

---

## MARKETPLACE DE ACTIONS

GitHub tiene un **Marketplace** de actions reutilizables.

Allí puedes encontrar acciones para:

- lenguajes
- cloud
- Docker
- testing
- seguridad
- notificaciones
- releases

### Recomendaciones al elegir una action

- Revisar mantenedor
- Revisar popularidad y uso
- Revisar frecuencia de actualización
- Revisar código fuente
- Revisar permisos que requiere
- Revisar si tiene documentación clara

---

## EJEMPLO: CI PARA NODE.JS

```yaml
name: Node CI

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

permissions:
  contents: read

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [18, 20, 22]

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: npm

      - name: Install
        run: npm ci

      - name: Lint
        run: npm run lint

      - name: Test
        run: npm test

      - name: Build
        run: npm run build
```

### Explicación línea por línea

- El workflow se ejecuta en `push` y `pull_request`, así cubres tanto integración continua como revisión de cambios.
- `permissions: contents: read` limita el token a lectura del repositorio.
- La `matrix` prueba varias versiones de Node para asegurar compatibilidad.
- `checkout` descarga el proyecto.
- `setup-node` instala la versión concreta de Node de cada combinación.
- `cache: npm` acelera la instalación.
- `npm ci` instala exactamente lo definido en el lockfile.
- `npm run lint` revisa calidad.
- `npm test` valida comportamiento.
- `npm run build` comprueba que el proyecto se puede construir.

En conjunto, esta pipeline responde a la pregunta: “¿mi proyecto compila, pasa tests y mantiene compatibilidad en varias versiones de Node?”.

---

## EJEMPLO: CI PARA PYTHON

```yaml
name: Python CI

on:
  push:
    branches: [main]
  pull_request:

permissions:
  contents: read

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: ["3.10", "3.11", "3.12"]

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python-version }}

      - name: Instalar dependencias
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Ejecutar tests
        run: pytest
```

### Explicación línea por línea

- La matriz recorre varias versiones de Python para verificar compatibilidad.
- `setup-python` instala cada runtime.
- `python -m pip install --upgrade pip` actualiza el instalador para evitar problemas de resolución o TLS.
- `pip install -r requirements.txt` instala dependencias del proyecto.
- `pytest` ejecuta la suite de tests.

Este workflow te dice rápidamente si tu código Python funciona de forma consistente en diferentes versiones del intérprete.

---

## EJEMPLO: BUILD Y PUSH DE IMAGEN DOCKER

```yaml
name: Docker Publish

on:
  push:
    tags:
      - "v*"

permissions:
  contents: read
  packages: write

jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Login en GHCR
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Buildx
        uses: docker/setup-buildx-action@v3

      - name: Build y push
        uses: docker/build-push-action@v6
        with:
          context: .
          push: true
          tags: ghcr.io/owner/app:${{ github.ref_name }}
```

### Explicación línea por línea

- El workflow solo arranca cuando haces push de un tag como `v1.0.0`.
- `packages: write` permite publicar en el registro.
- `docker/login-action` autentica contra GHCR.
- `setup-buildx-action` prepara el builder moderno de Docker.
- `build-push-action` construye y publica la imagen.
- `github.ref_name` reutiliza el nombre del tag para versionar la imagen.

Traducido: “cuando publiques una versión, GitHub construirá una imagen Docker y la subirá al registro con ese mismo número de versión”.

---

## EJEMPLO: DESPLIEGUE CON APROBACIÓN A PRODUCCIÓN

```yaml
name: Deploy Production

on:
  workflow_dispatch:

permissions:
  contents: read
  id-token: write

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: production
    concurrency:
      group: production-deploy
      cancel-in-progress: false

    steps:
      - uses: actions/checkout@v4
      - run: echo "Desplegando a producción"
```

Si el entorno `production` tiene revisores requeridos, el job esperará aprobación.

### Explicación línea por línea

- `workflow_dispatch` hace que el despliegue sea manual, no automático.
- `id-token: write` prepara el workflow para autenticación moderna por OIDC.
- `environment: production` aplica reglas y secretos del entorno de producción.
- `concurrency` evita solapamientos peligrosos entre despliegues.
- El step final representa el comando real de despliegue.

Este patrón es muy bueno para producción porque combina control manual, aislamiento del entorno y prevención de despliegues duplicados.

---

## EJEMPLO: WORKFLOW REUTILIZABLE

### Archivo reutilizable

```yaml
name: Reusable Build

on:
  workflow_call:
    inputs:
      java_version:
        required: true
        type: string

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: ${{ inputs.java_version }}
      - run: ./mvnw verify
```

### Workflow consumidor

```yaml
jobs:
  java-build:
    uses: ./.github/workflows/reusable-build.yml
    with:
      java_version: "21"
```

### Qué ventaja aporta este ejemplo

El workflow consumidor no necesita repetir toda la lógica de Java. Solo pasa el input `java_version: "21"` y reutiliza la automatización centralizada. Si mañana cambias la lógica del build, modificas un único archivo reutilizable en vez de muchos workflows duplicados.

---

## ARCHIVOS IMPORTANTES DEL ENTORNO DE EJECUCIÓN

GitHub expone archivos especiales para comunicar datos:

- `GITHUB_ENV`: exportar variables de entorno para pasos siguientes
- `GITHUB_OUTPUT`: definir outputs
- `GITHUB_PATH`: añadir rutas al PATH
- `GITHUB_STEP_SUMMARY`: crear resumen enriquecido del job

### Ejemplo `GITHUB_ENV`

```yaml
- run: echo "APP_VERSION=1.2.3" >> "$GITHUB_ENV"
```

### Ejemplo `GITHUB_STEP_SUMMARY`

```yaml
- run: |
    echo "## Resultado" >> "$GITHUB_STEP_SUMMARY"
    echo "- Build correcto" >> "$GITHUB_STEP_SUMMARY"
```

Esto genera un resumen visible en la interfaz del workflow.

---

## MATRIZ DE DECISIONES: CUÁNDO USAR CADA COSA

### `run`

Úsalo cuando necesitas ejecutar comandos simples de shell.

Ejemplo:

```yaml
- run: npm test
```

### `uses`

Úsalo cuando ya existe una action probada para esa tarea.

Ejemplo:

```yaml
- uses: actions/setup-node@v4
```

### Composite action

Úsala cuando repites varios steps concretos.

Ejemplo:

“Instalar dependencias + compilar” repetido en varios workflows del mismo repo.

### Reusable workflow

Úsalo cuando quieres reutilizar pipelines enteros entre repositorios o proyectos.

Ejemplo:

Una CI estándar compartida por todos los microservicios de una organización.

### Self-hosted runner

Úsalo cuando necesitas:

- acceso a red privada
- hardware especial
- software muy específico
- alto control del entorno

Ejemplo:

Un despliegue que solo puede conectarse a servidores internos no accesibles desde runners públicos de GitHub.

---

## GITHUB ACTIONS Y MONOREPOS

En monorepos suele interesar:

- filtrar por `paths`
- usar matrices por proyecto
- usar workflows reutilizables
- evitar ejecutar todo si solo cambió una parte

Ejemplo:

```yaml
on:
  pull_request:
    paths:
      - "frontend/**"
      - ".github/workflows/frontend-ci.yml"
```

Esto reduce tiempo y coste.

---

## COSTE Y MINUTOS DE EJECUCIÓN

GitHub Actions consume minutos y almacenamiento según el plan y el tipo de runner.

Factores que influyen:

- sistema operativo
- duración del workflow
- frecuencia de ejecuciones
- artefactos almacenados

### Cómo optimizar coste

- usar filtros por ramas y paths
- cancelar ejecuciones obsoletas con `concurrency`
- cachear dependencias
- evitar pasos innecesarios
- dividir CI y CD cuando convenga

---

## ERRORES COMUNES

1. Mala indentación YAML
2. Usar secretos en PRs desde forks sin entender restricciones
3. No limitar `permissions`
4. No usar `checkout`
5. Cachés mal diseñados
6. Rutas equivocadas
7. Condiciones `if` mal escritas
8. Confundir `pull_request` con `pull_request_target`
9. No fijar versiones de actions
10. Acoplar demasiado un workflow a un entorno concreto

---

## BUENAS PRÁCTICAS GENERALES

1. Mantén workflows pequeños y claros.
2. Separa CI, release y deploy cuando tenga sentido.
3. Usa nombres descriptivos en jobs y steps.
4. Limita permisos del token.
5. Usa caché con criterio.
6. Reutiliza lógica con reusable workflows o composite actions.
7. Protege producción con environments y approvals.
8. Evita secretos estáticos si puedes usar OIDC.
9. Revisa la seguridad de actions de terceros.
10. Documenta el propósito de cada workflow.

---

## ESTRUCTURA RECOMENDADA DE WORKFLOWS

Ejemplo de organización:

```text
.github/
  workflows/
    ci.yml
    release.yml
    deploy-staging.yml
    deploy-production.yml
    reusable-test.yml
  actions/
    setup-project/
      action.yml
```

### Idea general

- `ci.yml`: lint, test, build
- `release.yml`: versionado y publicación
- `deploy-staging.yml`: despliegue automático
- `deploy-production.yml`: despliegue controlado
- `reusable-test.yml`: workflow reutilizable
- `.github/actions/`: actions compuestas locales

---

## CÓMO LEER UN WORKFLOW PASO A PASO

Cuando veas un workflow, intenta responder en este orden:

1. **¿Qué lo dispara?** → `on`
2. **¿Qué permisos tiene?** → `permissions`
3. **¿Qué variables usa?** → `env`, `vars`, `secrets`
4. **¿Cuántos jobs tiene?** → `jobs`
5. **¿Qué dependencias hay?** → `needs`
6. **¿En qué runners corre?** → `runs-on`
7. **¿Qué hace realmente cada job?** → `steps`
8. **¿Publica artefactos o despliega?**
9. **¿Tiene riesgos de seguridad?**

Si sigues ese orden, entenderás casi cualquier workflow más rápido.

---

## RESUMEN FINAL

GitHub Actions es un sistema de automatización orientado a eventos dentro de GitHub. Su fuerza está en que une:

- repositorio
- automatización
- CI/CD
- seguridad
- despliegue
- reutilización

Las piezas clave que debes dominar son:

- **Workflow**: automatización completa
- **Evento**: disparador
- **Job**: unidad de trabajo
- **Step**: acción individual
- **Runner**: máquina de ejecución
- **Action**: componente reutilizable
- **Secrets / Vars / Env**: configuración
- **Permissions**: control de acceso
- **Artifacts / Cache**: persistencia y optimización
- **Matrix / Needs / If**: orquestación
- **Environments / OIDC**: despliegue y seguridad

Si entiendes bien esos conceptos, ya puedes leer, mantener y diseñar pipelines de GitHub Actions de forma profesional.
