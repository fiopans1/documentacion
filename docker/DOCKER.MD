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