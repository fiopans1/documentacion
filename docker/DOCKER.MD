# GUÍA RÁPIDA DE DOCKER

## QUE ES UN CONTENEDOR:
Un contenedor en Docker es una unidad ligera y portátil que empaqueta una aplicación junto con todas sus dependencias, configuraciones y librerías necesarias para ejecutarla. Funciona como un entorno aislado que se ejecuta sobre el sistema operativo del host, asegurando que la aplicación sea consistente y reproducible en cualquier entorno, ya sea desarrollo, pruebas o producción. Es como una "caja" que contiene todo lo necesario para que la aplicación funcione.

Lo importante es que al encontrarse completamente empaquetado es fácil de compartir entre los desarrolladores.

Los contenedores es almacenan en un repositorio de contenedores, existen dos tipos, privados y públicos (docker Hub)

## QUE ES UNA IMAGEN DOCKER:
Una **imagen en Docker** es una plantilla estática e inmutable que define todo lo necesario para ejecutar una aplicación dentro de un contenedor. Contiene el sistema de archivos de la aplicación, las librerías, las dependencias, las configuraciones y los binarios necesarios para garantizar que la aplicación funcione de manera consistente, sin importar el entorno en el que se ejecute.

Las imágenes se crean a partir de un **Dockerfile**, que es un archivo de texto que define paso a paso cómo construir la imagen, incluyendo las instrucciones para instalar dependencias, copiar archivos, configurar variables de entorno y especificar el comando principal que se ejecutará cuando se inicie un contenedor basado en la imagen.

### Características principales:
1. **Inmutabilidad**: Una vez creada, la imagen no cambia. Cualquier modificación requiere la creación de una nueva versión de la imagen.
2. **Capas**: Las imágenes están construidas en capas, donde cada instrucción en el Dockerfile genera una nueva capa. Esto permite reutilizar capas entre imágenes y reduce el espacio ocupado en el sistema.
3. **Compatibilidad**: Las imágenes son portables y pueden ejecutarse en cualquier máquina que tenga Docker instalado, asegurando consistencia entre entornos de desarrollo, pruebas y producción.

### Ejemplo de flujo:
1. Se escribe un `Dockerfile` que define cómo construir la imagen.
2. Docker usa este archivo para crear una imagen mediante el comando `docker build`.
3. La imagen resultante puede almacenarse en un registro (como Docker Hub) para ser compartida o distribuida.
4. Los contenedores se inician a partir de esta imagen usando el comando `docker run`.

En resumen, una imagen es la base sobre la que se ejecutan los contenedores. Actúa como una "receta" que define el entorno exacto para una aplicación, asegurando que sea replicable y ejecutable en cualquier lugar con Docker.

Un contenedor en resumen es una imagen corriendo

## QUE ES UN CONTAINER:

Un container son capas y capas de imágenes

## DIFERENCIA DOCKER Y MAQUINA VIRTUAL
### Diferencia entre Docker y una Máquina Virtual (VM)

Aunque Docker y las máquinas virtuales permiten ejecutar aplicaciones de manera aislada, funcionan de forma diferente debido a cómo manejan los recursos del sistema y el nivel de virtualización que utilizan.

---

### 1. **Estructura y Arquitectura**
- **Docker (Contenedores)**:
  - Usa la virtualización a nivel de sistema operativo.
  - Los contenedores comparten el kernel del sistema operativo del host.
  - Cada contenedor contiene solo la aplicación, sus dependencias y un sistema de archivos ligero.
  - No incluye un sistema operativo completo, lo que los hace más ligeros.

  **Diagrama básico de Docker**:
  ```
  Host OS
  ├── Docker Engine
  │   ├── Container A (App + Libs)
  │   ├── Container B (App + Libs)
  ```

- **Máquina Virtual (VM)**:
  - Usa la virtualización a nivel de hardware.
  - Cada VM incluye un sistema operativo completo (Guest OS) además de la aplicación y sus dependencias.
  - Dependen de un hipervisor (como VMware, VirtualBox o Hyper-V) para gestionar y emular el hardware.

  **Diagrama básico de una Máquina Virtual**:
  ```
  Host OS
  ├── Hypervisor
  │   ├── VM A (Guest OS + App + Libs)
  │   ├── VM B (Guest OS + App + Libs)
  ```

---

### 2. **Uso de Recursos**
- **Docker**:
  - Ligero: Como comparte el kernel del host, los contenedores consumen menos memoria y CPU.
  - Arranque rápido: Los contenedores se inician en segundos debido a la ausencia de un sistema operativo completo.

- **Máquina Virtual**:
  - Pesada: Cada VM ejecuta un sistema operativo completo, lo que consume más recursos.
  - Arranque más lento: Las VMs necesitan tiempo para iniciar el sistema operativo invitado.

---

### 3. **Portabilidad**
- **Docker**:
  - Altamente portátil: Los contenedores pueden ejecutarse en cualquier máquina que tenga Docker instalado, independientemente del sistema operativo subyacente.
  - Ideal para despliegues consistentes en diferentes entornos (desarrollo, pruebas, producción).

- **Máquina Virtual**:
  - Menos portátil: Las VMs dependen del hipervisor y pueden requerir ajustes para migrarse entre entornos con diferentes configuraciones.

---

### 4. **Aislamiento**
- **Docker**:
  - Aislamiento parcial: Los contenedores están aislados en términos de procesos, red y sistema de archivos, pero comparten el kernel del host.
  - Puede haber riesgos de seguridad si el kernel del host tiene vulnerabilidades.

- **Máquina Virtual**:
  - Aislamiento completo: Cada VM tiene su propio sistema operativo, proporcionando un mayor nivel de separación y seguridad entre entornos.

---

### 5. **Casos de Uso**
- **Docker**:
  - Desarrollo ágil y pruebas: Ideal para aplicaciones modernas basadas en microservicios.
  - Escalabilidad: Perfecto para sistemas distribuidos y despliegues en la nube.
  - Ambientes ligeros y replicables.

- **Máquina Virtual**:
  - Sistemas heredados: Útil para ejecutar aplicaciones antiguas que necesitan un sistema operativo completo.
  - Aislamiento extremo: Para entornos donde la seguridad y la separación son críticas.
  - Uso de múltiples sistemas operativos en una misma máquina (Windows, Linux, etc.).

---

### 6. **Velocidad y Tamaño**
- **Docker**:
  - Tamaño más pequeño (MB) y arranque rápido (segundos).
  - Ejemplo: Un contenedor de Nginx puede pesar 30 MB y arrancar en menos de un segundo.

- **Máquina Virtual**:
  - Tamaño mayor (GB) y arranque lento (minutos).
  - Ejemplo: Una VM con Ubuntu puede pesar varios GB y tardar minutos en arrancar.

---

### Comparación Rápida

| Aspecto                  | Docker (Contenedores)      | Máquina Virtual (VM)      |
|--------------------------|----------------------------|---------------------------|
| **Virtualización**       | A nivel de sistema operativo | A nivel de hardware        |
| **Kernel**               | Comparte el kernel del host | Cada VM tiene su propio kernel |
| **Recursos**             | Ligero, usa pocos recursos | Pesado, consume más memoria y CPU |
| **Aislamiento**          | Parcial                   | Completo                  |
| **Velocidad de arranque**| Segundos                  | Minutos                   |
| **Portabilidad**         | Altamente portátil        | Menos portátil            |
| **Tamaño**               | Pequeño (MB)             | Grande (GB)               |
| **Uso típico**           | Microservicios, nube      | Sistemas heredados, alta seguridad |

---

En resumen, Docker es ideal para entornos ligeros, ágiles y portátiles, mientras que las máquinas virtuales son más adecuadas para aplicaciones que requieren aislamiento total o sistemas operativos completos.

## COMANDOS DE IMÁGENES:


```
docker images
```

Devuelve un listado con todas las imágenes que tenemos en el ordenador

---

```
docker pull <direccion>:<version>
```
Te descargas una imagen de un repositorio indicando la versión

---

```
docker pull --platform linux/x86_64 <app>
```
Importante si tenemos docker con mac y procesador m1, con esto descargamos indicando que queremos la versión de x86

---

```
docker images rm <app>:<version>
```
Eliminas dicha imagen de tu repositorio local

---

```
docker build <file>
```

Con esto construyes una imagen a partir de un dockerfile

## COMANDOS CONTENEDORES:

```
docker create --name <name> <imagen> -e USER=username
```
Este comando crea un contenedor a partir de una imagen

Con la opción de --name le podemos poner un nombre al contenedor

Si nuestra imagen usa variables de entorno podemos asignarles un valor, como por ejemplo tenemos la variable de entorno USER podemos poner `-e USER = <valor> `, tenemos que indicarlo con el -e

---
```
docker start <idcontenedor>
```
Inicia el contenedor que creamos anteriormente

---
```
docker ps -a
```

Este comando lo que hace es mostrar una tabla con los contenedores que tenemos en la máquina y si están corriendo o no
Con docker ps -a va a mostrar TODOS los contenedores (esté parado o no)

---

```
docker stop <idcontenedor>
```
Para el contenedor

---

```
docker rm <idcontenedor>
```
Elimina el contenedor

---
```
docker logs --follow <contenedor>
```
Con este comando nos permite ver los logs de la aplicación que está corriendo en el contenedor.

El --follow es para estar escuchando continuamente por si hay nuevos logs

---
```
docker run <image>
```
Hace 3 cosas, primero busca la imagen, sino la encuentra la descarga, luego crea el contenedor a partir de la imagen, y por último inicia el contenedor.

Podemos usar comandos como --name o -p

Si no pones nada se queda escuchando los logs, entonces si quieres que te devuelva la terminar y el docker se quede en segundo plano ponle -d

---
 ## PORT MAPPING:
Mapeamos el puerto de nuestro contenedor los puertos de nuestra máquina.
El **port mapping** en Docker es una técnica que permite conectar un puerto del contenedor con un puerto del host, facilitando el acceso a los servicios que se ejecutan dentro del contenedor desde fuera de este. Como los contenedores están aislados por defecto, el mapeo de puertos es fundamental para exponer servicios (como aplicaciones web o APIs) al mundo exterior o al host local.

---

### ¿Cómo funciona?
Cuando inicias un contenedor, puedes usar la opción `-p` (o `--publish`) para especificar qué puertos del contenedor estarán disponibles en el host. El formato básico es:

```
docker run -p <PUERTO_HOST>:<PUERTO_CONTENEDOR> <IMAGEN>
```

- **PUERTO_HOST**: Es el puerto en el sistema anfitrión (host) que será accesible desde fuera.
- **PUERTO_CONTENEDOR**: Es el puerto interno dentro del contenedor en el que la aplicación está escuchando.

---

### Ejemplo 1: Servidor web básico

Supongamos que tienes una imagen Docker que ejecuta un servidor web (como Nginx) y que escucha en el puerto **80** dentro del contenedor.

Si deseas acceder a este servidor desde tu máquina local, puedes mapearlo a un puerto del host:

```bash
docker run -p 8080:80 nginx
```

- **80**: Puerto dentro del contenedor donde Nginx está sirviendo las solicitudes.
- **8080**: Puerto en el host que redirige las solicitudes al puerto 80 del contenedor.

Ahora puedes abrir un navegador en tu máquina local y acceder al servidor web escribiendo:

```
http://localhost:8080
```

---

### Ejemplo 2: API en un puerto diferente

Si tienes un servicio como una API que escucha en el puerto **5000** dentro del contenedor, pero deseas exponerlo al puerto **3000** en el host, usarías:

```bash
docker run -p 3000:5000 my-api-image
```

Aquí, cualquier solicitud que hagas a `http://localhost:3000` en el host será redirigida al puerto **5000** del contenedor.

---

### Mapeo de múltiples puertos

También puedes mapear varios puertos de un contenedor a diferentes puertos del host:

```bash
docker run -p 8080:80 -p 8443:443 nginx
```

- **80 (contenedor) -> 8080 (host)**: Para el tráfico HTTP.
- **443 (contenedor) -> 8443 (host)**: Para el tráfico HTTPS.

---

### Mapeo a cualquier puerto disponible (modo aleatorio)

Si no especificas un puerto del host y solo usas `-p :<PUERTO_CONTENEDOR>`, Docker asignará automáticamente un puerto disponible en el host:

```bash
docker run -p :80 nginx
```

Puedes verificar qué puerto se asignó utilizando el comando `docker ps`, que mostrará algo como:

```
0.0.0.0:32768->80/tcp
```

Esto significa que el puerto **80** del contenedor fue asignado al puerto **32768** del host.

---

### Consideraciones:
1. **Colisiones de puertos**: No puedes mapear el mismo puerto del host para múltiples contenedores al mismo tiempo. Por ejemplo, si un contenedor ya usa el puerto **8080**, obtendrás un error al intentar asignarlo a otro.
2. **Seguridad**: Al mapear un puerto, estás exponiendo ese servicio al mundo exterior (si tu máquina está en una red pública). Usa cortafuegos o configura reglas específicas si no deseas accesos no autorizados.

---

En resumen, el **port mapping** es una funcionalidad clave en Docker que conecta servicios dentro de un contenedor con el mundo exterior. Esto permite desarrollar, probar y desplegar aplicaciones de forma consistente y accesible en cualquier entorno.


## DOCKERFILE:

Un **Dockerfile** es un archivo de texto que contiene una serie de instrucciones para construir una imagen de Docker. Estas instrucciones definen qué sistema operativo base usar, qué dependencias instalar, cómo copiar archivos al contenedor, y cómo configurar y ejecutar una aplicación dentro de un contenedor.

---

### ¿Cómo funciona?
El Dockerfile sirve como una receta para crear una imagen. Docker lee este archivo línea por línea, ejecuta cada instrucción, y genera una nueva capa de la imagen en cada paso. El resultado final es una imagen que puede ser usada para crear contenedores consistentes y reproducibles.

---

### Estructura básica de un Dockerfile:
A continuación, se describen las principales instrucciones que puedes incluir:

1. **`FROM`**:
   - Especifica la imagen base que se usará.
   - Ejemplo:
     ```dockerfile
     FROM ubuntu:20.04
     ```

2. **`RUN`**:
   - Ejecuta comandos en la imagen, como instalar paquetes o configurar el entorno. Ejecuta comandos del sistemo operativo
   - Ejemplo:
     ```dockerfile
     RUN apt-get update && apt-get install -y python3
     ```

3. **`COPY` o `ADD`**:
   - Copian archivos desde el sistema local al contenedor.
   - Ejemplo:
     ```dockerfile
     COPY app.py /app/
     ```

4. **`WORKDIR`**:
   - Establece el directorio de trabajo dentro del contenedor.
   - Ejemplo:
     ```dockerfile
     WORKDIR /app
     ```

5. **`CMD`**:
   - Especifica el comando que se ejecutará cuando se inicie el contenedor.
   - Ejemplo:
     ```dockerfile
     CMD ["python3", "app.py"]
     ```

6. **`EXPOSE`**:
   - Indica qué puertos usará la aplicación dentro del contenedor (esto no publica el puerto, pero lo documenta).
   - Ejemplo:
     ```dockerfile
     EXPOSE 8080
     ```

---

### Ejemplo de Dockerfile completo:
Aquí un ejemplo de un Dockerfile para una aplicación simple en Python:

```dockerfile
# Imagen base
FROM python:3.9-slim

# Establecer el directorio de trabajo
WORKDIR /app

# Copiar los archivos de la aplicación al contenedor
COPY . .

# Instalar las dependencias
RUN pip install -r requirements.txt

# Exponer el puerto de la aplicación
EXPOSE 5000

# Comando para ejecutar la aplicación
CMD ["python", "app.py"]
```

---

### Flujo para usar un Dockerfile:
1. **Crear el Dockerfile**: Escribe las instrucciones necesarias en un archivo llamado `Dockerfile`.
2. **Construir la imagen**:
   - Usa el comando `docker build` para generar una imagen.
   - Ejemplo:
     ```bash
     docker build -t my-app .
     ```
     Aquí `my-app` es el nombre de la imagen, y `.` indica que el Dockerfile está en el directorio actual.
3. **Crear y ejecutar un contenedor**:
   - Usa la imagen para iniciar un contenedor:
     ```bash
     docker run -p 5000:5000 my-app
     ```

---

### Beneficios de un Dockerfile:
1. **Reproducibilidad**: Permite generar imágenes consistentes en diferentes entornos.
2. **Automatización**: Facilita la automatización del proceso de creación de imágenes.
3. **Portabilidad**: Las imágenes creadas con Dockerfiles funcionan en cualquier máquina con Docker instalado.

En resumen, un **Dockerfile** es una herramienta esencial para crear imágenes personalizadas, simplificando el desarrollo, la implementación y la distribución de aplicaciones.

NOTA:  Las imágenes siempre se van a basar en otra imagen 

### Diferencias clave entre **`RUN`** y **`CMD`**:

1. **`RUN`**:
   - Se ejecuta **durante la construcción de la imagen**.
   - Realiza tareas como instalar paquetes o configurar el sistema.
   - Sus efectos son permanentes en la imagen.

2. **`CMD`**:
   - Define el comando que se ejecutará cuando el contenedor **se inicie**.
   - Es el punto de entrada predeterminado de la aplicación.
   - No afecta la imagen durante su construcción. 

**Ejemplo resumen**:
```dockerfile
# RUN instala dependencias durante la construcción
RUN apt-get install -y curl

# CMD ejecuta un comando cuando arranca el contenedor
CMD ["echo", "¡Hola, mundo!"]
```



## INTERFACES DE RED:

```
docker network ls
```
Lista todas las redes que tengo en mi equipo configuradas en docker

En Docker, puedes crear una red personalizada, asignar IPs específicas a los contenedores, conectarlos entre ellos, y permitir que tu equipo local interactúe con ellos. Aquí te explico paso a paso:

---

### 1. **Crear una red en Docker**
Usamos el comando `docker network create` para crear una red personalizada.

#### Ejemplo:
```bash
docker network create \
    --driver bridge \
    --subnet=192.168.1.0/24 \
    mi_red_personalizada
```

**Explicación**:
(Son parámetros opcionales)
- **`--driver bridge`**: Crea una red de tipo puente (la más común para contenedores locales).
- **`--subnet`**: Especifica el rango de IPs de la red (en este caso, `192.168.1.0/24`).

---

### 2. **Asignar una IP fija a un contenedor**
Al iniciar un contenedor, puedes asignarle una IP fija en una red personalizada con `--network` y `--ip`.

#### Ejemplo:
```bash
docker run --rm -dit \
    --name contenedor1 \
    --network mi_red_personalizada \
    --ip 192.168.1.100 \
    nginx
```

**Explicación**:
- **`--network mi_red_personalizada`**: Conecta el contenedor a la red `mi_red_personalizada`.
- **`--ip 192.168.1.100`**: Asigna al contenedor la IP fija `192.168.1.100`.

---

### 3. **Conectar varios contenedores entre sí**
Puedes conectar múltiples contenedores a la misma red personalizada para que se comuniquen entre ellos mediante sus IPs o nombres.

#### Ejemplo:
```bash
docker run --rm -dit \
    --name contenedor2 \
    --network mi_red_personalizada \
    --ip 192.168.1.101 \
    alpine sleep 3600
```

- Ahora, los contenedores `contenedor1` y `contenedor2` pueden comunicarse.
- Desde `contenedor2`, puedes hacer ping a `contenedor1`:
  ```bash
  docker exec -it contenedor2 ping 192.168.1.100
  ```

---

### 4. **Conectar los contenedores con tu equipo local**
Para acceder a los contenedores desde tu máquina, debes mapear los puertos del contenedor al host con `-p`.

#### Ejemplo:
```bash
docker run --rm -dit \
    --name contenedor3 \
    --network mi_red_personalizada \
    --ip 192.168.1.102 \
    -p 8080:80 \
    nginx
```

**Explicación**:
- **`-p 8080:80`**: Mapea el puerto `80` del contenedor al puerto `8080` de tu máquina.
- Ahora puedes acceder al servicio de `nginx` desde tu navegador local usando `http://localhost:8080`.

---

### 5. **Conectar un contenedor a múltiples redes**
Un contenedor puede conectarse a varias redes si es necesario.

#### Ejemplo:
```bash
docker network create \
    --subnet=192.168.2.0/24 \
    otra_red

docker network connect \
    otra_red \
    contenedor1
```

- Ahora, `contenedor1` está conectado a ambas redes: `mi_red_personalizada` y `otra_red`.

---

### Resumen del flujo:
1. Crear una red con `docker network create`.
2. Asignar IPs fijas a los contenedores usando `--ip`.
3. Conectar los contenedores a la misma red para que puedan comunicarse.
4. Usar mapeo de puertos (`-p`) para interactuar desde tu equipo local.
5. Opcional: conectar un contenedor a múltiples redes si es necesario.

Este enfoque te da control sobre la configuración de red de tus contenedores y facilita su comunicación tanto entre ellos como con tu equipo.

---
```
docker network rm <red>
```
Elimina la red

**IMPORTANTE**: El nombre del dominio de un contenedor va a ser el mismo que nosotros le asignamos cuando creamos el contenedor

## DOCKER COMPOSE:

**Docker Compose** es una herramienta que facilita la gestión de aplicaciones Docker de múltiples contenedores mediante un archivo de configuración YAML. Con Docker Compose, puedes definir, configurar y ejecutar todos los servicios necesarios para tu aplicación en un solo archivo, lo que simplifica la configuración, la escalabilidad y la reutilización.

---

### **¿Cómo funciona Docker Compose?**

1. **Definición del entorno**:
   - Usas un archivo llamado `docker-compose.yml` para describir:
     - Contenedores que necesitas (servicios).
     - Redes entre contenedores.
     - Volúmenes para persistencia de datos.
     - Configuración de variables de entorno.

2. **Ejecución del entorno**:
   - Con un solo comando (`docker-compose up`), Docker Compose:
     - Crea redes personalizadas.
     - Construye las imágenes (si es necesario).
     - Inicia los contenedores definidos en el archivo.
     - Conecta los contenedores entre sí automáticamente según la configuración.

3. **Escalabilidad y limpieza**:
   - Puedes escalar servicios fácilmente con `docker-compose up --scale`.
   - Detener y eliminar los contenedores creados es simple con `docker-compose down`.

---

### **Estructura de un archivo `docker-compose.yml`**

El archivo `docker-compose.yml` se estructura en varias secciones principales:

1. **Version**:
   - Define la versión del esquema de Compose (por ejemplo, `3.9` es común para versiones recientes).

2. **Services**:
   - Lista los contenedores o servicios que quieres desplegar.
   - Cada servicio puede incluir:
     - Imagen base.
     - Construcción personalizada desde un `Dockerfile`.
     - Puertos mapeados.
     - Volúmenes montados.
     - Variables de entorno.
     - Dependencias de otros servicios.

3. **Networks**:
   - Define redes personalizadas que conectan los servicios.

4. **Volumes**:
   - Declara volúmenes para persistencia de datos.

---

### **Ejemplo completo: `docker-compose.yml`**

Supongamos que quieres configurar una aplicación con una base de datos PostgreSQL y una aplicación web en Python con Flask.

```yaml
version: '3.9'

services:
  web:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "5000:5000"
    environment:
      - DATABASE_URL=postgres://user:password@db:5432/mydb
    depends_on:
      - db

  db:
    image: postgres:14
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: mydb
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:
```

**Explicación**:
- **`web`**:
  - Construye un contenedor desde el `Dockerfile` en el directorio actual.
  - Expone el puerto `5000` para que la aplicación sea accesible.
  - Usa la variable de entorno `DATABASE_URL` para conectarse al servicio `db`.
  - Declara una dependencia en el servicio `db` con `depends_on`.
  
- **`db`**:
  - Usa una imagen oficial de PostgreSQL.
  - Configura credenciales mediante variables de entorno.
  - Monta un volumen para persistir los datos de la base de datos.

- **`volumes`**:
  - Declara un volumen llamado `pgdata` para que los datos de PostgreSQL persistan incluso si el contenedor es eliminado.

---

### **Comandos básicos de Docker Compose**

1. **Iniciar los servicios**:
   ```bash
   docker-compose up
   ```
   - Inicia todos los servicios definidos en el archivo `docker-compose.yml`.
   - Añade el flag `-d` para ejecutarlos en segundo plano:
     ```bash
     docker-compose up -d
     ```

2. **Detener los servicios**:
   ```bash
   docker-compose down
   ```
   - Detiene y elimina los contenedores, redes y volúmenes anónimos creados.

3. **Ver logs de los servicios**:
   ```bash
   docker-compose logs
   ```

4. **Escalar servicios**:
   ```bash
   docker-compose up --scale web=3
   ```
   - Lanza 3 instancias del servicio `web`.

5. **Reconstruir servicios**:
   ```bash
   docker-compose up --build
   ```
   - Vuelve a construir las imágenes antes de iniciar los servicios.

---

### **Ventajas de Docker Compose**

1. **Simplicidad**: Un solo archivo gestiona múltiples contenedores, redes y volúmenes.
2. **Reusabilidad**: Puedes compartir el archivo con otros desarrolladores o equipos.
3. **Automatización**: Configura dependencias entre servicios fácilmente.
4. **Escalabilidad**: Escalar servicios es tan simple como usar un comando.
5. **Portabilidad**: La configuración es consistente en cualquier entorno que soporte Docker.

---

### **Ejemplo en la práctica**

Imagina que tu proyecto tiene la siguiente estructura:
```
project/
├── Dockerfile
├── app.py
├── requirements.txt
├── docker-compose.yml
```

1. **Contenido del `Dockerfile`**:
   ```dockerfile
   FROM python:3.9-slim
   WORKDIR /app
   COPY requirements.txt .
   RUN pip install -r requirements.txt
   COPY . .
   CMD ["python", "app.py"]
   ```

2. **Ejecutar la aplicación**:
   ```bash
   docker-compose up
   ```

Esto levantará tu aplicación Flask (`web`) y la base de datos PostgreSQL (`db`), conectándolos automáticamente.

---

Docker Compose es ideal para gestionar entornos de desarrollo complejos, permitiendo orquestar múltiples contenedores con facilidad.

---
Aquí tienes una plantilla genérica para un archivo `docker-compose.yml` que puedes personalizar según las necesidades de tu proyecto:

```yaml
version: '3.9'  # Esquema de versión, ajusta según sea necesario.

services:
  app:  # Nombre del servicio principal
    image: app_image_name  # O usa "build" para construir desde un Dockerfile
    build:
      context: ./app  # Directorio donde está el Dockerfile
      dockerfile: Dockerfile
    ports:
      - "8080:80"  # Mapea el puerto 80 del contenedor al 8080 del host
    environment:  # Variables de entorno
      - ENV_VAR1=value1
      - ENV_VAR2=value2
    volumes:
      - ./app_data:/app/data  # Montaje de volumen: persistencia o uso local
    depends_on:
      - db  # Declara dependencias de otros servicios (opcional)

  db:  # Servicio de base de datos
    image: mysql:8.0  # Imagen de base de datos, cambia según el caso
    ports:
      - "3306:3306"  # Puerto para conectarse desde el host
    environment:
      MYSQL_ROOT_PASSWORD: root_password
      MYSQL_DATABASE: mydatabase
      MYSQL_USER: user
      MYSQL_PASSWORD: password
    volumes:
      - db_data:/var/lib/mysql  # Persistencia para los datos de la base de datos

  redis:  # Servicio adicional, como Redis para caching
    image: redis:alpine
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data  # Volumen persistente para Redis

volumes:  # Volúmenes para persistencia
  db_data:  # Volumen para la base de datos
  redis_data:  # Volumen para Redis
```

---

### **Cómo usar esta plantilla:**

1. **Personaliza los servicios:**
   - Cambia los nombres (`app`, `db`, `redis`) y las configuraciones según tus necesidades.
   - Si tienes un `Dockerfile` para tu aplicación, especifica la ruta en `build`.

2. **Ajusta los puertos:**
   - Mapea los puertos que necesita cada servicio entre el contenedor y el host.

3. **Define volúmenes:**
   - Usa volúmenes para persistir datos, como bases de datos o cachés.

4. **Ejecuta el entorno con Docker Compose:**
   ```bash
   docker-compose up
   ```

5. **Detén y elimina el entorno:**
   ```bash
   docker-compose down
   ```

---

Esta plantilla es un punto de partida genérico. Puedes adaptarla para incluir redes personalizadas, configuraciones específicas para orquestadores como Kubernetes, o integrar otros servicios según tu proyecto.

---
## VOLUMENES:

### **¿Qué son los volúmenes en Docker?**

Los **volúmenes** en Docker son una forma de **persistir datos** generados y utilizados por los contenedores. Se usan para almacenar información fuera del sistema de archivos temporal de los contenedores, asegurando que los datos no se pierdan cuando un contenedor se detiene o elimina.

---

### **Características clave de los volúmenes**
1. **Persistencia**:
   - Los datos almacenados en un volumen permanecen incluso si el contenedor asociado se elimina.

2. **Separación de datos**:
   - Los volúmenes están desacoplados del sistema de archivos del contenedor, facilitando la gestión de datos y contenedores por separado.

3. **Compatibilidad y rendimiento**:
   - Están optimizados para trabajar con Docker, ofreciendo mejor rendimiento en comparación con el uso de bind mounts (montajes directos de carpetas del host).

4. **Portabilidad**:
   - Los volúmenes pueden moverse y ser utilizados por diferentes contenedores.

---

### **¿Por qué usar volúmenes?**
1. **Persistir datos críticos**:
   - Bases de datos, registros, archivos generados por la aplicación, etc.
2. **Compartir datos entre contenedores**:
   - Cuando varios contenedores necesitan acceder al mismo conjunto de datos.
3. **Separar configuración del código**:
   - Mantener configuraciones o datos de usuario fuera de la imagen del contenedor.

---

### **Tipos de volúmenes**

1. **Volúmenes administrados por Docker**:
   - Docker crea y administra el almacenamiento en su sistema interno.
   - Son ideales para mantener datos sin preocuparse de la ubicación en el host.
   - **Ejemplo de uso**:
     ```bash
     docker run -v my_volume:/data nginx
     ```
     Aquí, `my_volume` es un volumen gestionado por Docker, y `nginx` puede acceder a él desde `/data`.

     Cuando ponemos volume:/ruta, ruta va a ser la ruta desde donde va a acceder el contenedor al volumen

2. **Bind Mounts** (Montajes de directorios del host):
   - Se vincula una carpeta específica del sistema operativo anfitrión con el contenedor.
   - Son útiles cuando necesitas usar o sincronizar archivos desde tu máquina local.
   - **Ejemplo de uso**:
     ```bash
     docker run -v /mi/carpeta/host:/data nginx
     ```
     Aquí, `/mi/carpeta/host` es una carpeta en el host que se monta en `/data` dentro del contenedor.

3. **Volúmenes temporales (`tmpfs`)**:
   - Se crean en la RAM del host, desapareciendo al detener el contenedor.
   - Ideales para datos sensibles o temporales.
   - **Ejemplo de uso**:
     ```bash
     docker run --tmpfs /data nginx
     ```

---

### **Cómo trabajar con volúmenes**

#### 1. Crear un volumen manualmente:
```bash
docker volume create mi_volumen
```

#### 2. Usar el volumen al iniciar un contenedor:
```bash
docker run -v mi_volumen:/data nginx
```

#### 3. Ver volúmenes existentes:
```bash
docker volume ls
```

#### 4. Inspeccionar detalles de un volumen:
```bash
docker volume inspect mi_volumen
```

#### 5. Eliminar un volumen:
```bash
docker volume rm mi_volumen
```

---

### **Volúmenes en Docker Compose**

En **Docker Compose**, puedes declarar volúmenes en la sección `volumes` y asignarlos a servicios. Ejemplo:

```yaml
version: '3.9'

services:
  app:
    image: nginx
    volumes:
      - mi_volumen:/data

volumes:
  mi_volumen:  # Declaración del volumen
```

---

### **Ventajas de los volúmenes sobre bind mounts**
1. Más seguros: Docker administra su ciclo de vida y acceso.
2. Más portables: Pueden usarse independientemente del sistema operativo anfitrión.
3. Optimizados para Docker: Mejoran el rendimiento en aplicaciones críticas.

---

### **Resumen**
Los **volúmenes** son esenciales para aplicaciones Docker que necesitan persistir datos o compartirlos entre contenedores. Usarlos correctamente mejora la robustez y flexibilidad de tu entorno.

---
En Docker, **puedes asignar un volumen a varios contenedores** para que compartan datos. Esto es útil en casos como:

- Compartir archivos de configuración entre servicios.
- Acceder a los mismos datos desde diferentes aplicaciones.
- Crear un flujo de trabajo donde un contenedor genera datos y otro los procesa.

---

### **Cómo asignar un volumen a varios contenedores**

#### 1. **Definir y compartir un volumen en Docker CLI**

Un volumen puede ser creado manualmente o automáticamente cuando lo asignas a un contenedor. Luego, puedes usar el mismo volumen en múltiples contenedores.

#### Ejemplo:
```bash
# Crear un volumen (opcional, Docker lo crea automáticamente si no existe)
docker volume create mi_volumen

# Iniciar el primer contenedor con el volumen
docker run -d --name contenedor1 -v mi_volumen:/app/data nginx

# Iniciar el segundo contenedor usando el mismo volumen
docker run -d --name contenedor2 -v mi_volumen:/app/data alpine sleep 3600
```

En este caso:
- Ambos contenedores (`contenedor1` y `contenedor2`) comparten el volumen `mi_volumen`, que está montado en `/app/data`.
- Los cambios realizados por uno en `/app/data` serán visibles para el otro.

---

#### 2. **Definir un volumen compartido en Docker Compose**

Con Docker Compose, puedes asignar un volumen compartido a múltiples servicios.

#### Ejemplo `docker-compose.yml`:
```yaml
version: '3.9'

services:
  app1:
    image: nginx
    volumes:
      - datos_compartidos:/app/data

  app2:
    image: alpine
    command: sleep 3600
    volumes:
      - datos_compartidos:/app/data

volumes:
  datos_compartidos:
```

En este caso:
- El volumen `datos_compartidos` es usado por ambos servicios (`app1` y `app2`).
- Los datos creados/modificados en `/app/data` por uno estarán disponibles para el otro.

Ejecuta:
```bash
docker-compose up
```

---

### **Consideraciones al compartir volúmenes**

1. **Sincronización de datos**:
   - Los contenedores tienen acceso concurrente al volumen, por lo que pueden leer y escribir datos simultáneamente.
   - Asegúrate de que tu aplicación pueda manejar este acceso concurrente.

2. **Diferentes rutas de montaje**:
   - Puedes montar el mismo volumen en diferentes rutas dentro de los contenedores.
   ```bash
   docker run -d --name contenedor1 -v mi_volumen:/app/data nginx
   docker run -d --name contenedor2 -v mi_volumen:/config alpine
   ```

3. **Restricciones de acceso**:
   - Si necesitas que un contenedor solo pueda **leer** los datos, puedes usar la opción `:ro` (read-only).
   ```bash
   docker run -d --name contenedor2 -v mi_volumen:/app/data:ro alpine
   ```

   En este caso, `contenedor2` solo puede leer los datos, no escribirlos.

---

### **Uso típico: Procesamiento colaborativo**

Un ejemplo práctico:
1. **Un contenedor genera datos**:
   - Un servidor de logs escribe registros en el volumen.
2. **Otro contenedor los procesa**:
   - Una aplicación de análisis lee esos registros desde el mismo volumen.

```bash
docker run -d --name log_server -v logs:/var/log nginx
docker run -d --name log_processor -v logs:/logs alpine
```

El contenedor `log_processor` puede procesar los registros generados por `log_server` porque ambos comparten el volumen `logs`.

---

### **Resumen**

- **Sí, un volumen puede ser asignado a múltiples contenedores.**
- **Cómo hacerlo**:
  - Usando la opción `-v` en CLI o `volumes` en Docker Compose.
- Los datos se comparten y sincronizan automáticamente entre los contenedores.
- Puedes controlar el acceso con configuraciones como `:ro` para lectura únicamente.

Esto simplifica la colaboración entre contenedores y permite arquitecturas modulares.

---

Y si te lo estás preguntando, sí, puedes usar el mismo volumen y montarlo en rutas diferentes dentro de distintos contenedores. Esto es completamente válido en Docker. Cada contenedor verá el contenido del volumen en la ruta que hayas especificado, pero internamente estarán compartiendo los mismos datos.

---

### **Ejemplo en Docker CLI**

Supongamos que tienes un volumen llamado `mi_volumen`:

1. **Crear el volumen** (opcional, Docker lo crea automáticamente si no existe):
   ```bash
   docker volume create mi_volumen
   ```

2. **Montar el volumen en diferentes rutas en distintos contenedores**:

   - Contenedor 1: monta `mi_volumen` en `/ruta1`.
   ```bash
   docker run -d --name contenedor1 -v mi_volumen:/ruta1 nginx
   ```

   - Contenedor 2: monta `mi_volumen` en `/ruta2`.
   ```bash
   docker run -d --name contenedor2 -v mi_volumen:/ruta2 alpine sleep 3600
   ```

   Ahora:
   - Los datos que se escriban en `/ruta1` en `contenedor1` estarán disponibles en `/ruta2` en `contenedor2` (y viceversa).
   - La ruta de montaje dentro del contenedor no afecta el contenido del volumen compartido.

---

### **Ejemplo con Docker Compose**

En Docker Compose, puedes lograr lo mismo definiendo un volumen compartido y especificando rutas de montaje diferentes para cada servicio.

```yaml
version: '3.9'

services:
  servicio1:
    image: nginx
    volumes:
      - mi_volumen:/ruta1

  servicio2:
    image: alpine
    command: sleep 3600
    volumes:
      - mi_volumen:/ruta2

volumes:
  mi_volumen:
```

1. Crea y ejecuta el entorno:
   ```bash
   docker-compose up
   ```

2. Los servicios `servicio1` y `servicio2` comparten el volumen `mi_volumen`, pero tienen diferentes rutas internas (`/ruta1` y `/ruta2`, respectivamente).

---

### **¿Qué pasa con los datos?**

- Si `servicio1` escribe un archivo en `/ruta1/archivo.txt`, `servicio2` podrá acceder a ese archivo en `/ruta2/archivo.txt`.
- La ruta de montaje solo define dónde dentro del contenedor estará disponible el volumen, pero el contenido del volumen es el mismo para ambos contenedores.

---

### **Casos de uso comunes**

1. **Organización por roles**:
   - Un contenedor almacena datos en una carpeta estructurada (`/ruta1`), mientras que otro accede a una parte de los datos desde una carpeta diferente (`/ruta2`).

2. **Separación lógica de tareas**:
   - Por ejemplo, un contenedor podría generar logs en `/ruta1`, mientras otro contenedor los procesa desde `/ruta2`.

---

### **Resumen**

- Puedes usar un mismo volumen con rutas de montaje diferentes en distintos contenedores.
- La ruta de montaje afecta únicamente al contenedor, pero los datos subyacentes son los mismos.
- Esto es útil para organizar tareas y compartir datos de manera flexible.

---
**Y puedo enlazarlo y acceder desde mi host a dichos archivos?**

Sí, puedes enlazar un volumen Docker a una carpeta en tu host utilizando **bind mounts**, lo que permite que los archivos sean accesibles tanto desde los contenedores como directamente desde tu sistema host. Esto es especialmente útil para desarrollo, depuración o casos donde necesitas procesar los datos fuera del contenedor.

---

### **Cómo hacerlo**

#### 1. **Usando Docker CLI**
Puedes vincular una carpeta del host al volumen montado dentro del contenedor utilizando la opción `-v` o `--mount`.

```bash
docker run -d \
  --name contenedor1 \
  -v /mi/carpeta/host:/ruta1 \
  nginx
```

#### Explicación:
- `/mi/carpeta/host`: Es la carpeta en el host donde estarán los archivos.
- `/ruta1`: Es la ruta en el contenedor donde se monta la carpeta del host.
- Cualquier archivo creado/modificado en `/ruta1` dentro del contenedor estará visible en `/mi/carpeta/host` en tu máquina local, y viceversa.

---

#### 2. **Usando Docker Compose**

En Docker Compose, puedes especificar una carpeta del host directamente en la sección `volumes`.

```yaml
version: '3.9'

services:
  servicio1:
    image: nginx
    volumes:
      - /mi/carpeta/host:/ruta1

  servicio2:
    image: alpine
    command: sleep 3600
    volumes:
      - /mi/carpeta/host:/ruta2
```

1. Ejecuta:
   ```bash
   docker-compose up
   ```

2. Ahora:
   - La carpeta `/mi/carpeta/host` de tu sistema estará accesible como `/ruta1` en `servicio1` y como `/ruta2` en `servicio2`.
   - Los cambios hechos en los archivos desde cualquier contenedor o el host estarán sincronizados.

---

### **Acceso desde el host**

1. Ve directamente a la carpeta del host vinculada, por ejemplo:
   ```bash
   cd /mi/carpeta/host
   ls
   ```

2. Cualquier archivo creado dentro del contenedor aparecerá aquí, y viceversa.

---

### **Precauciones**

1. **Permisos**:
   - Asegúrate de que los permisos de la carpeta en el host permiten que Docker y los contenedores lean y escriban.
   - Usa `chmod` o `chown` si es necesario:
     ```bash
     chmod 777 /mi/carpeta/host
     ```

2. **Consistencia de datos**:
   - Si varios contenedores acceden al volumen y lo editan simultáneamente, podría haber conflictos. Considera usar aplicaciones diseñadas para manejo concurrente.

3. **Portabilidad**:
   - Si mapeas una carpeta del host directamente, tu configuración podría no ser portátil a otras máquinas (ya que las rutas en el host podrían diferir). Usa volúmenes de Docker si necesitas más portabilidad.

---

### **Ejemplo práctico**

Supongamos que tienes un servidor Nginx y quieres que sus archivos de configuración sean editables desde el host:

1. **Crea la carpeta en el host**:
   ```bash
   mkdir -p /mi/nginx/conf
   echo "Configuración personalizada" > /mi/nginx/conf/default.conf
   ```

2. **Ejecuta el contenedor**:
   ```bash
   docker run -d \
     --name mi_nginx \
     -v /mi/nginx/conf:/etc/nginx/conf.d \
     -p 8080:80 \
     nginx
   ```

3. **Accede y modifica desde el host**:
   - Edita `/mi/nginx/conf/default.conf` y el cambio será reflejado inmediatamente en el contenedor.

4. **Prueba desde un navegador**:
   - Visita `http://localhost:8080` y verifica que tu configuración funciona.

---

### **Resumen**

- Puedes acceder a los archivos de un volumen Docker desde tu host usando bind mounts.
- Define una carpeta del host para sincronizarla con una ruta en el contenedor.
- Los cambios son visibles y editables tanto desde el contenedor como desde el host.
- Es ideal para desarrollo y debugging, pero asegúrate de manejar permisos y evitar conflictos en accesos concurrentes.

---
### PLANTILLA DOCKER COMPOSE CON VOLUMES:

Aquí tienes una plantilla genérica de un archivo `docker-compose.yml` que incluye la configuración de **volúmenes**:

```yaml
version: '3.9'  # Esquema de versión, ajusta según la compatibilidad de tu entorno.

services:
  app:
    image: nginx  # Imagen del servicio principal
    ports:
      - "8080:80"  # Mapea el puerto 80 del contenedor al 8080 del host
    volumes:
      - app_data:/usr/share/nginx/html  # Volumen gestionado por Docker
      - ./config/nginx:/etc/nginx/conf.d  # Bind mount para compartir archivos desde el host
    depends_on:
      - db  # Espera que el servicio db esté listo antes de arrancar

  db:
    image: mysql:8.0  # Servicio de base de datos
    environment:
      MYSQL_ROOT_PASSWORD: root_password
      MYSQL_DATABASE: my_database
      MYSQL_USER: user
      MYSQL_PASSWORD: password
    volumes:
      - db_data:/var/lib/mysql  # Volumen persistente para almacenar los datos

volumes:
  app_data:  # Volumen gestionado por Docker para la aplicación
  db_data:   # Volumen gestionado por Docker para la base de datos
```

---

### **Explicación de la plantilla**

#### **Servicios**
1. **`app`:**
   - Es un servicio basado en la imagen `nginx`.
   - Los volúmenes:
     - `app_data` es un volumen gestionado por Docker donde se almacenan los archivos web.
     - `./config/nginx` es un **bind mount** para compartir configuraciones de Nginx desde el host.

2. **`db`:**
   - Servicio basado en MySQL con configuración de variables de entorno.
   - El volumen `db_data` asegura que los datos de la base de datos se mantengan persistentes incluso si el contenedor se elimina.

#### **Volúmenes**
- **`app_data`** y **`db_data`** son volúmenes gestionados por Docker que se declaran en la sección `volumes`.
- Docker se encarga de crear y gestionar estos volúmenes automáticamente.

---

### **Comandos útiles**

1. **Iniciar los servicios**:
   ```bash
   docker-compose up -d
   ```

2. **Listar los volúmenes creados por Docker Compose**:
   ```bash
   docker volume ls
   ```

3. **Ver detalles de un volumen**:
   ```bash
   docker volume inspect <nombre_del_volumen>
   ```

4. **Detener y eliminar los servicios (sin eliminar volúmenes)**:
   ```bash
   docker-compose down
   ```

5. **Eliminar volúmenes junto con los servicios**:
   ```bash
   docker-compose down --volumes
   ```

---

### **Ampliaciones opcionales**

1. **Montar múltiples carpetas desde el host**:
   ```yaml
   volumes:
      - ./host_files:/container_files
      - ./logs:/var/log/app
   ```

2. **Volumen de solo lectura**:
   ```yaml
   volumes:
      - ./read_only_folder:/container_read_only:ro
   ```

3. **Personalizar las redes para conectar contenedores y host**:
   ```yaml
   networks:
     my_network:
       driver: bridge
   ```

Esta plantilla es personalizable y puede ampliarse según tus necesidades específicas.


## PASAR ARGUMENTOS A UN DOCKER RUN

Para **pasar argumentos a un contenedor Docker** al ejecutarlo con `docker run`, existen varias formas dependiendo de lo que desees hacer:

### **1. Pasar argumentos al comando que se ejecuta dentro del contenedor**
Puedes pasar argumentos directamente después de la imagen, y serán procesados por el comando que ejecuta el contenedor (ya sea el que defines en `ENTRYPOINT` o `CMD` del Dockerfile).

```bash
docker run <options> <image_name> <command> <arguments>
```

#### Ejemplo:
```bash
docker run ubuntu bash -c "echo $1" --soyunargumento
```
- **`--soyunargumento`** es el argumento que se pasa al contenedor.

---

### **2. Sobrescribir `ENTRYPOINT` o `CMD`**
Si tu Dockerfile tiene un `ENTRYPOINT` o `CMD` definido, puedes sobrescribirlos al pasar argumentos adicionales al ejecutar el contenedor.

#### Ejemplo Dockerfile:
```Dockerfile
ENTRYPOINT ["echo"]
CMD ["Hello, world!"]
```

Al ejecutar:

```bash
docker run ubuntu --soyunargumento
```
- Sobrescribe el valor por defecto de `CMD` y pasa el argumento `--soyunargumento` al `echo`.

---

### **3. Pasar argumentos como variables de entorno**
Puedes usar la opción `-e` para definir variables de entorno dentro del contenedor.

```bash
docker run -e MI_ARG="--soyunargumento" ubuntu bash -c "echo $MI_ARG"
```

- Aquí, `MI_ARG` es una variable de entorno que pasas al contenedor, y el contenedor imprime su valor.

---

### **Resumen rápido**:
- **Argumentos al comando del contenedor**: `docker run <image_name> <command> <arguments>`
- **Sobrescribir `ENTRYPOINT`/`CMD`**: Pasando nuevos argumentos directamente al ejecutar.
- **Variables de entorno**: Usar `-e` para pasar valores al contenedor.

¡Eso es todo! Así puedes pasar argumentos a tu contenedor Docker según tus necesidades.

**En resumen si tú defines un comando en el entrypoint en el dockerfile, más adelante le puedes mandar argumentos en el docker run para complementar el comando del entrypoint, por ejemplo si en el entrypoint se ejecuta un script, tú luego en el docker run puedes mandarle argumentos que procesará ese script**

`Si tengo un entrypoint en el dockerfile se sobreescribe si mando un comando en el docker run o solo si añado argumentos?`

Si tienes un `ENTRYPOINT` definido en el **Dockerfile**, el **comando** que pasas en `docker run` **no lo sobrescribe** directamente. Sin embargo, el comportamiento depende de lo que pases al ejecutar el contenedor:

### **1. Si solo pasas argumentos**:
Cuando ejecutas `docker run` y solo pasas **argumentos**, el **`ENTRYPOINT`** del Dockerfile **se mantiene** y esos argumentos se agregan al comando definido en el `ENTRYPOINT`.

#### Ejemplo:

**Dockerfile**:
```Dockerfile
ENTRYPOINT ["echo"]
```

**Comando `docker run`**:
```bash
docker run ubuntu --soyunargumento
```

**Resultado**:
El contenedor ejecutará el comando `echo` con el argumento `--soyunargumento`. El `ENTRYPOINT` no se sobrescribe, simplemente recibe los argumentos proporcionados.

**Salida**:
```
--soyunargumento
```

---

### **2. Si pasas un nuevo comando**:
Si proporcionas un **comando** en el `docker run`, este **sobrescribirá el `ENTRYPOINT`** definido en el Dockerfile. El comando pasado reemplaza al `ENTRYPOINT`, y los argumentos se pasan a este nuevo comando.

#### Ejemplo:

**Dockerfile**:
```Dockerfile
ENTRYPOINT ["echo"]
```

**Comando `docker run`**:
```bash
docker run ubuntu bash -c "echo Hola Mundo"
```

**Resultado**:
En este caso, el `ENTRYPOINT` que es `echo` se sobrescribe por el comando `bash -c "echo Hola Mundo"`. Esto ejecutará `bash`, no `echo`.

**Salida**:
```
Hola Mundo
```

---

### **3. Uso combinado de `ENTRYPOINT` y `CMD`**:
- Si defines un **`ENTRYPOINT`** y **`CMD`** en el Dockerfile, **`CMD`** actúa como un argumento por defecto que se pasa al `ENTRYPOINT`.
- Si pasas un comando o argumento en `docker run`, este sobrescribirá a `CMD`, pero no a `ENTRYPOINT`.

#### Ejemplo Dockerfile:

```Dockerfile
ENTRYPOINT ["echo"]
CMD ["Hola"]
```

**Comando `docker run`**:
```bash
docker run ubuntu --soyunargumento
```

**Resultado**:
El `ENTRYPOINT` es `echo`, y el argumento por defecto es `Hola`, pero cuando pasas `--soyunargumento`, lo sobrescribes:

**Salida**:
```
--soyunargumento
```

---

### Resumen:

1. **Si solo pasas argumentos**: Se añaden al `ENTRYPOINT`, pero **no sobrescriben** el comando.
2. **Si pasas un comando** (por ejemplo, `bash`): El **`ENTRYPOINT` se sobrescribe** con el nuevo comando.
3. **`CMD` es un valor por defecto**: Solo se usa si no pasas nada al ejecutar el contenedor.

¡Espero que esto aclare cómo funciona el `ENTRYPOINT` y cómo se maneja en `docker run`! Si tienes más dudas, ¡aquí estoy para ayudarte!

## CONECTARSE A UN CONTENEDOR:
Para conectarte a un contenedor en Docker y poder interactuar con él, puedes usar el comando `docker exec` o `docker attach`, dependiendo de lo que necesites hacer.

### **1. Conectarte a un contenedor en ejecución usando `docker exec`**

Si deseas **ejecutar comandos** dentro de un contenedor en ejecución (por ejemplo, acceder a una terminal interactiva), puedes usar el comando `docker exec`.

#### **Ejemplo:**

```bash
docker exec -it <container_id_or_name> bash
```

- **`-it`**: Esta opción permite que la terminal sea interactiva (`-i` para interactiva y `-t` para asignar una pseudo-terminal).
- **`<container_id_or_name>`**: El ID o nombre del contenedor al que deseas conectarte.
- **`bash`**: El comando que ejecutarás dentro del contenedor. Usualmente `bash` para acceder a la shell, pero puede ser `sh` si la imagen no tiene `bash`.

#### **Ejemplo de uso:**

```bash
docker exec -it my_container bash
```

Esto abrirá una sesión interactiva en el contenedor con nombre `my_container`, permitiéndote ejecutar comandos dentro del mismo.

### **2. Conectarte a un contenedor en ejecución usando `docker attach`**

Si prefieres **adjuntarte** a la sesión principal del contenedor (en lugar de ejecutar nuevos comandos), puedes usar `docker attach`. Esto te conecta directamente al flujo de salida y entrada estándar del contenedor.

#### **Ejemplo:**

```bash
docker attach <container_id_or_name>
```

- **`<container_id_or_name>`**: El ID o nombre del contenedor al que deseas conectarte.

**Nota**: `docker attach` te lleva a la sesión principal del contenedor, lo que puede ser útil si el contenedor está ejecutando un programa interactivo.

#### **Ejemplo de uso:**

```bash
docker attach my_container
```

Esto conectará tu terminal a la salida estándar del contenedor `my_container`. 

### **3. Desconectarte de un contenedor sin detenerlo**

Si usas `docker attach`, para salir de la sesión interactiva sin detener el contenedor, debes presionar `Ctrl + C` o usar el comando de salida de la aplicación que está ejecutándose.

Si prefieres desconectarte sin detener el contenedor, puedes hacerlo con la siguiente combinación de teclas:

```bash
Ctrl + P + Q
```

Esto te permitirá salir del contenedor y dejarlo en ejecución.

---

### **Resumen:**

- **`docker exec -it <container_id_or_name> bash`**: Accede a una terminal interactiva dentro del contenedor.
- **`docker attach <container_id_or_name>`**: Conéctate al proceso principal del contenedor.

Ambos métodos te permiten interactuar con el contenedor, pero `docker exec` es generalmente más flexible para ejecutar comandos dentro del contenedor, mientras que `docker attach` te conecta directamente a la sesión en curso del contenedor.

---
---
---

## NOTAS Y MÁS COMANDOS:

**NOTA:** Si queremos pasar variables de entorno en docker build no es con -e, es con --build-arg (no persisten durate toda la imagen, solo durante la construcción)

---
**NOTA:** Un `ENTRYPOINT` en un DOCKERFILE es una instrucción que define el comando principal que se ejecutará cuando se inciie un contenedor basado en la imagen creada.

Las diferencias con CMD son las siguientes:


Un **ENTRYPOINT** en un Dockerfile es una instrucción que define el comando principal que se ejecutará cuando se inicie un contenedor basado en la imagen creada. Este comando es el punto de entrada del contenedor, y generalmente se utiliza para especificar un proceso principal que siempre debe ejecutarse.


Diferencias entre CMD y ENTRYPOINT
| **Característica**          | **ENTRYPOINT**                             | **CMD**                                   |
|------------------------------|--------------------------------------------|-------------------------------------------|
| **Propósito**                | Define el comando principal fijo.          | Proporciona valores por defecto al contenedor. |
| **Sobreescritura**           | Más rígido; no se sobreescribe fácilmente. | Puede ser reemplazado al ejecutar el contenedor. |
| **Compatibilidad**           | Puede combinarse con `CMD` para agregar argumentos. | Se sobrescribe si `docker run` incluye un comando. |

---