# GUÍA COMPLETA DE CADDY SERVER

## ¿QUÉ ES CADDY?

**Caddy** es un servidor web moderno, potente y fácil de usar escrito en Go. Es conocido por ser el primer servidor web que ofrece HTTPS automático por defecto, lo que lo hace extremadamente conveniente para desarrolladores y administradores de sistemas. A diferencia de servidores tradicionales como Apache o Nginx, Caddy está diseñado con la simplicidad y la seguridad como prioridades principales.

### Características principales:

1. **HTTPS Automático**: Caddy obtiene y renueva automáticamente certificados SSL/TLS de Let's Encrypt sin configuración manual.
2. **Configuración Simple**: Usa un archivo de configuración llamado `Caddyfile` con sintaxis intuitiva y legible.
3. **HTTP/2 y HTTP/3**: Soporte nativo para los protocolos más modernos sin configuración adicional.
4. **Servidor de Archivos Estáticos**: Excelente para servir sitios web estáticos con una sola línea de configuración.
5. **Reverse Proxy Potente**: Funciona como proxy inverso con balanceo de carga integrado.
6. **Extensible**: Sistema de plugins y módulos para ampliar funcionalidades.
7. **Escrito en Go**: Compilado en un único binario sin dependencias externas.
8. **Recarga en Caliente**: Actualiza la configuración sin tiempo de inactividad.

---

## ¿POR QUÉ USAR CADDY?

### Ventajas sobre otros servidores web:

**Comparado con Nginx:**
- **Configuración más simple**: El Caddyfile es mucho más legible que los archivos de configuración de Nginx.
- **HTTPS sin esfuerzo**: No necesitas configurar manualmente certificados SSL/TLS.
- **Menos líneas de código**: Lo que tomaría 50+ líneas en Nginx, puede hacerse en 5 líneas con Caddy.

**Comparado con Apache:**
- **Mejor rendimiento**: Caddy es más rápido y usa menos recursos.
- **Configuración moderna**: Sintaxis más clara y menos archivos de configuración.
- **HTTP/2 y HTTP/3 por defecto**: Soporte nativo sin módulos adicionales.

**Casos de uso ideales:**
- Sitios web pequeños y medianos
- Aplicaciones web modernas (SPAs)
- APIs y microservicios
- Reverse proxy para aplicaciones backend
- Entornos de desarrollo
- Proyectos que requieren HTTPS rápidamente

---

## INSTALACIÓN DE CADDY

### En Linux (Ubuntu/Debian)

**Método 1: Desde el repositorio oficial**

```bash
# Instalar dependencias
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https curl

# Agregar el repositorio de Caddy
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list

# Actualizar e instalar Caddy
sudo apt update
sudo apt install caddy
```

**Método 2: Descarga directa del binario**

```bash
# Descargar la última versión
curl -o caddy.tar.gz "https://caddyserver.com/api/download?os=linux&arch=amd64"

# Extraer el binario
tar -xvzf caddy.tar.gz

# Mover a /usr/local/bin
sudo mv caddy /usr/local/bin/

# Dar permisos de ejecución
sudo chmod +x /usr/local/bin/caddy

# Verificar la instalación
caddy version
```

### En macOS

**Usando Homebrew:**

```bash
brew install caddy

# Verificar instalación
caddy version
```

**Descarga directa:**

```bash
curl -o caddy.tar.gz "https://caddyserver.com/api/download?os=darwin&arch=amd64"
tar -xvzf caddy.tar.gz
sudo mv caddy /usr/local/bin/
caddy version
```

### En Windows

**Método 1: Usando Chocolatey**

```powershell
choco install caddy
```

**Método 2: Descarga manual**

1. Ve a https://caddyserver.com/download
2. Descarga el ejecutable para Windows
3. Coloca `caddy.exe` en una carpeta (ej: `C:\caddy`)
4. Agrega la ruta a las variables de entorno PATH
5. Verifica con `caddy version` en CMD o PowerShell

### Instalación con Docker

```bash
# Ejecutar Caddy en Docker
docker run -d \
  --name caddy \
  -p 80:80 \
  -p 443:443 \
  -v caddy_data:/data \
  -v caddy_config:/config \
  -v $PWD/Caddyfile:/etc/caddy/Caddyfile \
  caddy:latest
```

---

## EL CADDYFILE: CONFIGURACIÓN BÁSICA

El **Caddyfile** es el archivo de configuración principal de Caddy. Su sintaxis es extremadamente simple y legible.

### Estructura básica:

```
dirección {
    directiva1
    directiva2
    directiva3
}
```

### Ejemplo 1: Servidor web simple (HTTP)

**Caddyfile:**
```
localhost:8080

respond "¡Hola desde Caddy!"
```

**Ejecutar:**
```bash
caddy run
```

Visita `http://localhost:8080` y verás el mensaje.

### Ejemplo 2: Servir archivos estáticos

**Estructura de carpetas:**
```
/var/www/
  └── mi-sitio/
      ├── index.html
      ├── style.css
      └── script.js
```

**Caddyfile:**
```
localhost:8080

root * /var/www/mi-sitio
file_server
```

**Explicación:**
- `root * /var/www/mi-sitio`: Define la raíz del servidor
- `file_server`: Activa el servidor de archivos estáticos

### Ejemplo 3: HTTPS automático

```
midominio.com

root * /var/www/mi-sitio
file_server
```

**¡Eso es todo!** Caddy automáticamente:
1. Obtiene un certificado SSL de Let's Encrypt
2. Configura HTTPS
3. Redirige HTTP a HTTPS
4. Renueva el certificado antes de que expire

---

## REVERSE PROXY CON CADDY

Un **reverse proxy** es un servidor que recibe solicitudes de clientes y las redirige a servidores backend. Caddy es excelente para esto.

### Ejemplo 1: Proxy simple a una aplicación Node.js

Supongamos que tienes una aplicación Node.js corriendo en `localhost:3000`.

**Caddyfile:**
```
localhost:8080

reverse_proxy localhost:3000
```

Ahora puedes acceder a tu aplicación Node.js a través de `http://localhost:8080`.

### Ejemplo 2: Proxy con HTTPS automático

```
api.midominio.com

reverse_proxy localhost:3000
```

Caddy automáticamente obtendrá un certificado SSL para `api.midominio.com`.

### Ejemplo 3: Proxy con múltiples rutas

```
midominio.com

# Ruta para la API
reverse_proxy /api/* localhost:3000

# Ruta para el panel de administración
reverse_proxy /admin/* localhost:4000

# Servir archivos estáticos para todo lo demás
root * /var/www/frontend
file_server
```

**Explicación:**
- Las solicitudes a `/api/*` van al puerto 3000
- Las solicitudes a `/admin/*` van al puerto 4000
- Todo lo demás sirve archivos estáticos

### Ejemplo 4: Headers personalizados

```
midominio.com

reverse_proxy localhost:3000 {
    header_up Host {host}
    header_up X-Real-IP {remote}
    header_up X-Forwarded-For {remote}
    header_up X-Forwarded-Proto {scheme}
}
```

Esto añade headers importantes para que tu aplicación backend sepa información sobre el cliente original.

### Ejemplo 5: Timeout y opciones avanzadas

```
api.midominio.com

reverse_proxy localhost:3000 {
    # Timeout para peticiones
    transport http {
        dial_timeout 5s
        response_header_timeout 30s
    }
    
    # Health checks
    health_uri /health
    health_interval 30s
    health_timeout 5s
}
```

---

## LOAD BALANCING (BALANCEO DE CARGA)

Caddy puede distribuir tráfico entre múltiples servidores backend.

### Ejemplo 1: Balanceo de carga básico

```
api.midominio.com

reverse_proxy localhost:3000 localhost:3001 localhost:3002
```

Caddy distribuirá las peticiones automáticamente entre los tres servidores.

### Ejemplo 2: Estrategias de balanceo

**Round Robin (por defecto):**
```
api.midominio.com

reverse_proxy localhost:3000 localhost:3001 localhost:3002 {
    lb_policy round_robin
}
```

**Least Connections (menos conexiones):**
```
api.midominio.com

reverse_proxy localhost:3000 localhost:3001 localhost:3002 {
    lb_policy least_conn
}
```

**IP Hash (mismo cliente siempre al mismo servidor):**
```
api.midominio.com

reverse_proxy localhost:3000 localhost:3001 localhost:3002 {
    lb_policy ip_hash
}
```

### Ejemplo 3: Health Checks con Load Balancing

```
api.midominio.com

reverse_proxy localhost:3000 localhost:3001 localhost:3002 {
    lb_policy round_robin
    lb_try_duration 2s
    lb_try_interval 500ms
    
    health_uri /health
    health_interval 10s
    health_timeout 2s
    health_status 200
}
```

Si algún servidor no responde con código 200 en `/health`, Caddy lo marcará como no disponible.

---

## CONFIGURACIÓN HTTPS Y CERTIFICADOS SSL/TLS

### HTTPS automático con Let's Encrypt

La configuración más simple:

```
midominio.com

respond "¡HTTPS automático!"
```

Caddy automáticamente:
1. Valida que tienes control del dominio
2. Solicita certificado a Let's Encrypt
3. Instala el certificado
4. Configura HTTPS
5. Renueva antes de expirar

### Configuración de email para Let's Encrypt

```
{
    email tu@email.com
}

midominio.com

file_server
```

El email es importante para recibir notificaciones sobre certificados.

### Usar certificados personalizados

Si ya tienes tus propios certificados:

```
midominio.com

tls /ruta/al/certificado.crt /ruta/a/la/clave.key

file_server
```

### Configuración TLS avanzada

```
midominio.com

tls {
    # Protocolos permitidos
    protocols tls1.2 tls1.3
    
    # Ciphers específicos
    ciphers TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384 TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
    
    # Curves
    curves x25519 secp384r1 secp521r1
}

file_server
```

### Staging de Let's Encrypt (para pruebas)

```
{
    acme_ca https://acme-staging-v02.api.letsencrypt.org/directory
}

midominio.com

file_server
```

Usa esto cuando estés probando para no alcanzar límites de rate de Let's Encrypt.

### Certificados wildcard

```
*.midominio.com

tls {
    dns cloudflare {env.CLOUDFLARE_API_TOKEN}
}

reverse_proxy localhost:3000
```

Para wildcards necesitas validación DNS (requiere plugin).

---

## CONFIGURACIÓN DE MÚLTIPLES SITIOS

### Ejemplo 1: Múltiples dominios en un Caddyfile

```
# Sitio 1
sitio1.com {
    root * /var/www/sitio1
    file_server
}

# Sitio 2
sitio2.com {
    root * /var/www/sitio2
    file_server
}

# Sitio 3 - API
api.midominio.com {
    reverse_proxy localhost:3000
}
```

### Ejemplo 2: Subdominios

```
# Sitio principal
midominio.com {
    root * /var/www/main
    file_server
}

# Blog
blog.midominio.com {
    root * /var/www/blog
    file_server
}

# API
api.midominio.com {
    reverse_proxy localhost:3000
}

# Panel admin
admin.midominio.com {
    reverse_proxy localhost:4000
}
```

### Ejemplo 3: Configuración compartida con snippets

```
(common_settings) {
    encode gzip
    header {
        X-Content-Type-Options "nosniff"
        X-Frame-Options "DENY"
        X-XSS-Protection "1; mode=block"
    }
}

sitio1.com {
    import common_settings
    root * /var/www/sitio1
    file_server
}

sitio2.com {
    import common_settings
    root * /var/www/sitio2
    file_server
}
```

---

## COMPRESIÓN Y OPTIMIZACIÓN

### Habilitar compresión Gzip/Brotli

```
midominio.com

encode gzip zstd

root * /var/www/html
file_server
```

**Compresión selectiva:**
```
midominio.com

encode {
    gzip 6
    zstd
    match {
        header Content-Type text/*
        header Content-Type application/json*
        header Content-Type application/javascript*
    }
}

file_server
```

### Cache headers

```
midominio.com

header {
    # Cache para archivos estáticos por 1 año
    Cache-Control "public, max-age=31536000, immutable"
}

root * /var/www/html
file_server
```

**Cache selectivo por tipo de archivo:**
```
midominio.com

@static {
    path *.css *.js *.png *.jpg *.jpeg *.gif *.svg *.woff *.woff2
}

header @static {
    Cache-Control "public, max-age=31536000, immutable"
}

root * /var/www/html
file_server
```

---

## REDIRECCIONES

### Redirección simple

```
www.midominio.com {
    redir https://midominio.com{uri}
}

midominio.com {
    root * /var/www/html
    file_server
}
```

### Redirecciones específicas

```
midominio.com

# Redirigir una ruta específica
redir /old-page /new-page

# Redirigir con código de estado específico
redir /temporary /new-location 302
redir /permanent /new-location 301

root * /var/www/html
file_server
```

### Redirecciones con patrones

```
midominio.com

# Redirigir todo del blog antiguo al nuevo
redir /blog/* https://blog.midominio.com{uri}

# Redirigir HTTP a HTTPS (aunque Caddy lo hace automáticamente)
redir http://midominio.com https://midominio.com{uri}

file_server
```

---

## MANEJO DE ERRORES Y PÁGINAS PERSONALIZADAS

### Página de error personalizada

```
midominio.com

handle_errors {
    @404 {
        expression {http.error.status_code} == 404
    }
    rewrite @404 /404.html
    file_server
}

root * /var/www/html
file_server
```

### Múltiples páginas de error

```
midominio.com

handle_errors {
    @404 {
        expression {http.error.status_code} == 404
    }
    @5xx {
        expression {http.error.status_code} >= 500
    }
    
    rewrite @404 /404.html
    rewrite @5xx /500.html
    file_server
}

root * /var/www/html
file_server
```

---

## LOGGING (REGISTROS)

### Activar logs básicos

```
midominio.com

log {
    output file /var/log/caddy/access.log
}

file_server
```

### Logs detallados

```
{
    log {
        output file /var/log/caddy/caddy.log
        level DEBUG
    }
}

midominio.com

log {
    output file /var/log/caddy/access.log {
        roll_size 100mb
        roll_keep 10
        roll_keep_for 720h
    }
    format json
}

file_server
```

### Logs selectivos

```
midominio.com

log {
    output file /var/log/caddy/access.log
    format filter {
        wrap json
        fields {
            request>headers>Authorization delete
            request>headers>Cookie delete
        }
    }
}

file_server
```

---

## AUTENTICACIÓN BÁSICA

### HTTP Basic Auth

```
midominio.com

basicauth {
    usuario $2a$14$password_hash_aqui
}

file_server
```

**Generar hash de contraseña:**

```bash
caddy hash-password
# Ingresa tu contraseña y obtendrás el hash
```

### Proteger rutas específicas

```
midominio.com

# Área pública
root * /var/www/html
file_server

# Área protegida
route /admin/* {
    basicauth {
        admin $2a$14$hash_de_la_contraseña
    }
    reverse_proxy localhost:4000
}
```

---

## REWRITE Y TRY FILES

### Rewrite de URLs

```
midominio.com

# Reescribir /api/v1/* a /v1/*
rewrite /api/v1/* /v1{path}

reverse_proxy localhost:3000
```

### Try Files (para SPAs)

```
midominio.com

root * /var/www/my-spa

# Intenta servir el archivo, si no existe, sirve index.html
try_files {path} /index.html

file_server
```

**Ejemplo para React/Vue/Angular:**
```
midominio.com

root * /var/www/dist

encode gzip

# Configuración perfecta para SPAs
try_files {path} {path}/ /index.html

file_server
```

---

## TEMPLATES

Caddy puede procesar templates en archivos HTML.

### Ejemplo básico

**Caddyfile:**
```
localhost:8080

root * /var/www/html
templates
file_server
```

**index.html:**
```html
<!DOCTYPE html>
<html>
<head>
    <title>{{.Req.Host}}</title>
</head>
<body>
    <h1>Bienvenido a {{.Req.Host}}</h1>
    <p>Tu IP es: {{.RemoteIP}}</p>
    <p>Hora del servidor: {{now | date "2006-01-02 15:04:05"}}</p>
</body>
</html>
```

### Includes

**layout.html:**
```html
<!DOCTYPE html>
<html>
<head>
    <title>{{block "title" .}}Mi Sitio{{end}}</title>
</head>
<body>
    {{block "content" .}}{{end}}
</body>
</html>
```

**index.html:**
```html
{{define "title"}}Inicio{{end}}
{{define "content"}}
    <h1>Página de inicio</h1>
{{end}}
{{template "layout.html" .}}
```

---

## CADDY CON DOCKER

### Docker Compose básico

**docker-compose.yml:**
```yaml
version: '3.8'

services:
  caddy:
    image: caddy:latest
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - ./site:/srv
      - caddy_data:/data
      - caddy_config:/config
    networks:
      - web

volumes:
  caddy_data:
  caddy_config:

networks:
  web:
```

### Con aplicación Node.js

**docker-compose.yml:**
```yaml
version: '3.8'

services:
  caddy:
    image: caddy:latest
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - caddy_data:/data
      - caddy_config:/config
    networks:
      - web
    depends_on:
      - app

  app:
    build: ./app
    restart: unless-stopped
    environment:
      - NODE_ENV=production
    networks:
      - web

volumes:
  caddy_data:
  caddy_config:

networks:
  web:
```

**Caddyfile:**
```
midominio.com

reverse_proxy app:3000
```

### Stack completo (Frontend + Backend + DB)

**docker-compose.yml:**
```yaml
version: '3.8'

services:
  caddy:
    image: caddy:latest
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - ./frontend/dist:/srv
      - caddy_data:/data
      - caddy_config:/config
    networks:
      - web
    depends_on:
      - api

  api:
    build: ./backend
    restart: unless-stopped
    environment:
      - DATABASE_URL=postgresql://user:password@db:5432/mydb
    networks:
      - web
    depends_on:
      - db

  db:
    image: postgres:15-alpine
    restart: unless-stopped
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=mydb
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - web

volumes:
  caddy_data:
  caddy_config:
  postgres_data:

networks:
  web:
```

**Caddyfile:**
```
midominio.com {
    # API endpoints
    reverse_proxy /api/* api:3000
    
    # Frontend estático
    root * /srv
    encode gzip
    try_files {path} /index.html
    file_server
}
```

---

## CONFIGURACIÓN EN PRODUCCIÓN

### Ejemplo completo de producción

**Caddyfile:**
```
{
    # Email para Let's Encrypt
    email admin@midominio.com
    
    # Configuración global de logs
    log {
        output file /var/log/caddy/caddy.log {
            roll_size 100mb
            roll_keep 10
        }
        level INFO
    }
}

# Redirigir www a non-www
www.midominio.com {
    redir https://midominio.com{uri} permanent
}

# Sitio principal
midominio.com {
    # Logs específicos del sitio
    log {
        output file /var/log/caddy/midominio-access.log {
            roll_size 50mb
            roll_keep 20
        }
        format json
    }
    
    # Security headers
    header {
        X-Content-Type-Options "nosniff"
        X-Frame-Options "SAMEORIGIN"
        X-XSS-Protection "1; mode=block"
        Referrer-Policy "strict-origin-when-cross-origin"
        Strict-Transport-Security "max-age=31536000; includeSubDomains; preload"
        Content-Security-Policy "default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline';"
        Permissions-Policy "geolocation=(), microphone=(), camera=()"
        -Server
    }
    
    # Compresión
    encode gzip zstd
    
    # API backend
    reverse_proxy /api/* localhost:3000 {
        header_up X-Real-IP {remote}
        header_up X-Forwarded-For {remote}
        header_up X-Forwarded-Proto {scheme}
        
        health_uri /api/health
        health_interval 30s
        health_timeout 5s
    }
    
    # Archivos estáticos con cache
    @static {
        path *.css *.js *.png *.jpg *.jpeg *.gif *.svg *.woff *.woff2 *.ttf *.eot *.ico
    }
    
    header @static {
        Cache-Control "public, max-age=31536000, immutable"
    }
    
    # Frontend SPA
    root * /var/www/html
    try_files {path} /index.html
    file_server
    
    # Página de error personalizada
    handle_errors {
        @404 {
            expression {http.error.status_code} == 404
        }
        @5xx {
            expression {http.error.status_code} >= 500
        }
        
        rewrite @404 /404.html
        rewrite @5xx /500.html
        file_server
    }
}
```

### Systemd service (Linux)

**Verificar servicio:**
```bash
sudo systemctl status caddy
```

**Iniciar servicio:**
```bash
sudo systemctl start caddy
```

**Detener servicio:**
```bash
sudo systemctl stop caddy
```

**Reiniciar servicio:**
```bash
sudo systemctl restart caddy
```

**Recargar configuración sin downtime:**
```bash
sudo systemctl reload caddy
```

**Habilitar inicio automático:**
```bash
sudo systemctl enable caddy
```

---

## COMANDOS ÚTILES DE CADDY

### Ejecutar Caddy

```bash
# Ejecutar con Caddyfile en directorio actual
caddy run

# Ejecutar con archivo específico
caddy run --config /path/to/Caddyfile

# Ejecutar en segundo plano
caddy start

# Detener Caddy
caddy stop

# Recargar configuración sin downtime
caddy reload

# Verificar sintaxis del Caddyfile
caddy validate

# Adaptar Caddyfile a JSON
caddy adapt

# Ver la configuración en formato JSON
caddy adapt --pretty
```

### Comandos de certificados

```bash
# Ver certificados instalados
caddy list-modules

# Limpiar caché de certificados
caddy untrust

# Generar hash de contraseña
caddy hash-password

# Ver versión
caddy version

# Ver ayuda
caddy help
```

### File server rápido

```bash
# Servidor de archivos en el directorio actual
caddy file-server

# En un puerto específico
caddy file-server --listen :8080

# Con un directorio específico
caddy file-server --root /var/www/html

# Con browse habilitado
caddy file-server --browse
```

### Reverse proxy rápido

```bash
# Proxy rápido
caddy reverse-proxy --from :80 --to localhost:3000

# Con HTTPS automático
caddy reverse-proxy --from midominio.com --to localhost:3000
```

---

## CONFIGURACIÓN JSON (ALTERNATIVA AL CADDYFILE)

Caddy también acepta configuración en formato JSON.

**Ejemplo básico en JSON:**

```json
{
  "apps": {
    "http": {
      "servers": {
        "srv0": {
          "listen": [":443"],
          "routes": [
            {
              "match": [
                {
                  "host": ["midominio.com"]
                }
              ],
              "handle": [
                {
                  "handler": "file_server",
                  "root": "/var/www/html"
                }
              ]
            }
          ]
        }
      }
    }
  }
}
```

**Convertir Caddyfile a JSON:**

```bash
caddy adapt --config Caddyfile --pretty > caddy.json
```

**Ejecutar con JSON:**

```bash
caddy run --config caddy.json
```

---

## CASOS DE USO REALES

### Caso 1: Blog estático con Hugo/Jekyll

**Estructura:**
```
/var/www/
  └── mi-blog/
      ├── index.html
      ├── posts/
      ├── css/
      └── js/
```

**Caddyfile:**
```
blog.midominio.com {
    root * /var/www/mi-blog
    encode gzip
    
    @static {
        path *.css *.js *.png *.jpg *.svg
    }
    
    header @static Cache-Control "public, max-age=31536000"
    
    file_server
}
```

### Caso 2: API REST con Node.js/Express

**Caddyfile:**
```
api.midominio.com {
    reverse_proxy localhost:3000 {
        header_up X-Real-IP {remote}
        header_up X-Forwarded-For {remote}
        
        health_uri /health
        health_interval 30s
    }
    
    # Rate limiting (requiere plugin)
    # rate_limit {
    #     zone api_limit {
    #         key {remote}
    #         events 100
    #         window 1m
    #     }
    # }
}
```

### Caso 3: Aplicación React/Vue con API

**Caddyfile:**
```
midominio.com {
    # Logs
    log {
        output file /var/log/caddy/app.log
    }
    
    # API backend
    reverse_proxy /api/* localhost:3000
    
    # Frontend React
    root * /var/www/react-app/build
    encode gzip
    
    # SPA routing
    try_files {path} /index.html
    
    file_server
}
```

### Caso 4: Múltiples aplicaciones con subpaths

**Caddyfile:**
```
midominio.com {
    # App 1 en /app1
    route /app1/* {
        uri strip_prefix /app1
        reverse_proxy localhost:3001
    }
    
    # App 2 en /app2
    route /app2/* {
        uri strip_prefix /app2
        reverse_proxy localhost:3002
    }
    
    # Landing page principal
    root * /var/www/landing
    file_server
}
```

### Caso 5: WebSocket support

**Caddyfile:**
```
app.midominio.com {
    # WebSocket y HTTP al mismo backend
    reverse_proxy localhost:3000 {
        # Headers para WebSocket
        header_up Upgrade {http.request.header.Upgrade}
        header_up Connection {http.request.header.Connection}
    }
}
```

### Caso 6: Servir múltiples SPAs

**Caddyfile:**
```
# Dashboard admin
admin.midominio.com {
    root * /var/www/admin-spa
    try_files {path} /index.html
    encode gzip
    file_server
}

# Panel de usuario
app.midominio.com {
    root * /var/www/user-spa
    try_files {path} /index.html
    encode gzip
    file_server
}

# Landing page
midominio.com {
    root * /var/www/landing
    file_server
}
```

---

## SEGURIDAD BEST PRACTICES

### Headers de seguridad completos

```
midominio.com {
    header {
        # Prevenir MIME sniffing
        X-Content-Type-Options "nosniff"
        
        # Prevenir clickjacking
        X-Frame-Options "SAMEORIGIN"
        
        # XSS Protection
        X-XSS-Protection "1; mode=block"
        
        # Referrer Policy
        Referrer-Policy "strict-origin-when-cross-origin"
        
        # HSTS (HTTP Strict Transport Security)
        Strict-Transport-Security "max-age=31536000; includeSubDomains; preload"
        
        # Content Security Policy
        Content-Security-Policy "default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline'; img-src 'self' data: https:; font-src 'self'; connect-src 'self'; frame-ancestors 'self';"
        
        # Permissions Policy
        Permissions-Policy "geolocation=(), microphone=(), camera=(), payment=(), usb=(), magnetometer=(), gyroscope=(), accelerometer=()"
        
        # Ocultar versión del servidor
        -Server
    }
    
    file_server
}
```

### Restringir acceso por IP

```
midominio.com {
    @blocked {
        not remote_ip 192.168.1.0/24 10.0.0.0/8
    }
    
    respond @blocked "Acceso denegado" 403
    
    file_server
}
```

### Proteger archivos sensibles

```
midominio.com {
    @sensitive {
        path *.env* *.config.js *.key *.pem /config/* /.git/*
    }
    
    respond @sensitive "No encontrado" 404
    
    root * /var/www/html
    file_server
}
```

---

## MONITOREO Y DEBUGGING

### Logs detallados para debugging

```
{
    debug
}

midominio.com {
    log {
        output file /var/log/caddy/debug.log
        level DEBUG
    }
    
    file_server
}
```

### Ver logs en tiempo real

```bash
# Logs del sistema (si Caddy es servicio)
sudo journalctl -u caddy -f

# Logs de archivo
tail -f /var/log/caddy/access.log

# Logs con formato JSON
tail -f /var/log/caddy/access.log | jq
```

### Validar configuración

```bash
# Validar sintaxis
caddy validate --config Caddyfile

# Ver configuración adaptada a JSON
caddy adapt --config Caddyfile --pretty

# Ejecutar en modo debug
caddy run --watch
```

---

## TROUBLESHOOTING (SOLUCIÓN DE PROBLEMAS)

### Problema: Caddy no obtiene certificado SSL

**Causas comunes:**
1. Puerto 80 o 443 bloqueado en firewall
2. DNS no apunta correctamente al servidor
3. El dominio no es públicamente accesible

**Soluciones:**
```bash
# Verificar puertos
sudo netstat -tlnp | grep caddy

# Verificar DNS
dig +short midominio.com

# Verificar conectividad
curl -I http://midominio.com

# Ver logs de certificados
sudo journalctl -u caddy | grep -i certificate

# Probar con staging de Let's Encrypt
```

**Caddyfile con staging:**
```
{
    acme_ca https://acme-staging-v02.api.letsencrypt.org/directory
}

midominio.com {
    file_server
}
```

### Problema: Error "bind: address already in use"

**Causa:** Otro servicio está usando el puerto 80 o 443.

**Solución:**
```bash
# Ver qué está usando el puerto
sudo lsof -i :80
sudo lsof -i :443

# Detener servicios conflictivos (Apache, Nginx, etc.)
sudo systemctl stop apache2
sudo systemctl stop nginx

# Deshabilitar inicio automático
sudo systemctl disable apache2
sudo systemctl disable nginx
```

### Problema: Reverse proxy no funciona

**Verificaciones:**
```bash
# Verificar que el backend esté corriendo
curl http://localhost:3000

# Verificar conectividad desde Caddy
docker exec caddy curl http://app:3000  # si usas Docker

# Ver logs de Caddy
sudo journalctl -u caddy -n 50

# Verificar configuración
caddy validate
```

**Debugging en Caddyfile:**
```
midominio.com {
    log {
        level DEBUG
    }
    
    reverse_proxy localhost:3000 {
        # Ver headers enviados
        header_up X-Debug true
        
        # Timeout más largo para debug
        transport http {
            dial_timeout 30s
        }
    }
}
```

### Problema: Página 404 en SPA (React/Vue)

**Causa:** Caddy intenta buscar archivos físicos para rutas de React Router.

**Solución:**
```
midominio.com {
    root * /var/www/react-app/build
    
    # Esto es clave para SPAs
    try_files {path} /index.html
    
    file_server
}
```

### Problema: WebSocket no funciona

**Solución:** Asegurar que los headers correctos se pasen:

```
midominio.com {
    reverse_proxy localhost:3000 {
        header_up Upgrade {http.request.header.Upgrade}
        header_up Connection {http.request.header.Connection}
        header_up Host {host}
    }
}
```

### Problema: Caddy se reinicia constantemente

**Verificar:**
```bash
# Ver status
sudo systemctl status caddy

# Ver logs completos
sudo journalctl -u caddy -xe

# Validar Caddyfile
caddy validate --config /etc/caddy/Caddyfile
```

### Problema: Rate limit de Let's Encrypt

**Síntomas:** Error al obtener certificados después de varios intentos.

**Solución:**
```bash
# Limpiar certificados fallidos
sudo rm -rf /var/lib/caddy/.local/share/caddy/certificates

# Usar staging mientras pruebas
```

```
{
    acme_ca https://acme-staging-v02.api.letsencrypt.org/directory
}
```

**Límites de Let's Encrypt:**
- 50 certificados por dominio registrado por semana
- 5 reintentos por hora
- Usar staging para desarrollo

---

## MIGRACIÓN DESDE OTROS SERVIDORES

### Desde Nginx

**Nginx:**
```nginx
server {
    listen 80;
    server_name midominio.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name midominio.com;
    
    ssl_certificate /etc/ssl/cert.pem;
    ssl_certificate_key /etc/ssl/key.pem;
    
    location / {
        root /var/www/html;
        index index.html;
        try_files $uri $uri/ /index.html;
    }
    
    location /api {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

**Equivalente en Caddy:**
```
midominio.com {
    # HTTPS automático, no necesita configuración SSL
    
    # API reverse proxy
    reverse_proxy /api/* localhost:3000
    
    # Frontend
    root * /var/www/html
    try_files {path} /index.html
    file_server
}
```

### Desde Apache

**Apache (.htaccess):**
```apache
RewriteEngine On
RewriteCond %{HTTPS} off
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]

<Directory /var/www/html>
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>
```

**Equivalente en Caddy:**
```
midominio.com {
    root * /var/www/html
    file_server browse
}
```

---

## PLUGINS Y EXTENSIONES

Caddy tiene un sistema de módulos extensible.

### Plugins populares:

1. **caddy-dns** - Proveedores DNS para validación de certificados wildcard
2. **caddy-ratelimit** - Limitar velocidad de peticiones
3. **caddy-git** - Sincronizar archivos desde Git
4. **caddy-docker** - Proxy automático para contenedores Docker

### Instalar Caddy con plugins

**Usando xcaddy:**
```bash
# Instalar xcaddy
go install github.com/caddyserver/xcaddy/cmd/xcaddy@latest

# Compilar Caddy con plugins
xcaddy build \
    --with github.com/caddy-dns/cloudflare \
    --with github.com/mholt/caddy-ratelimit

# Reemplazar el binario
sudo mv caddy /usr/local/bin/
```

**Usando Docker con plugins:**
```dockerfile
FROM caddy:builder AS builder

RUN xcaddy build \
    --with github.com/caddy-dns/cloudflare

FROM caddy:latest

COPY --from=builder /usr/bin/caddy /usr/bin/caddy
```

---

## COMPARACIÓN DE RENDIMIENTO

### Benchmark simple

**Test con Apache Bench:**
```bash
# 10000 peticiones, 100 concurrentes
ab -n 10000 -c 100 https://midominio.com/

# Resultado típico de Caddy:
# Requests per second: 15000-20000
# Time per request: 5-7ms
```

### Uso de recursos

**Memoria:**
- Caddy: ~10-50 MB en reposo
- Nginx: ~5-20 MB en reposo
- Apache: ~50-200 MB en reposo

**CPU:**
- Caddy es comparable a Nginx en eficiencia
- Mejor que Apache bajo carga alta

---

## RECURSOS ADICIONALES

### Documentación oficial:
- **Web oficial**: https://caddyserver.com/
- **Documentación**: https://caddyserver.com/docs/
- **Foro comunitario**: https://caddy.community/
- **GitHub**: https://github.com/caddyserver/caddy

### Tutoriales recomendados:
1. Caddy Quick Start Guide
2. Automatic HTTPS with Caddy
3. Building a Reverse Proxy with Caddy
4. Deploying SPAs with Caddy

### Herramientas útiles:
- **Caddy JSON Config Generator**: Convierte Caddyfile a JSON
- **SSL Labs**: Prueba la configuración SSL de tu sitio
- **Let's Encrypt Rate Limits**: Monitorea tus límites

---

## CONCLUSIÓN

Caddy es una herramienta excepcional para servir sitios web modernos. Sus ventajas principales son:

✅ **HTTPS automático sin configuración**
✅ **Sintaxis simple y legible**
✅ **Excelente para desarrollo y producción**
✅ **HTTP/2 y HTTP/3 por defecto**
✅ **Recarga en caliente de configuración**
✅ **Extensible con plugins**
✅ **Excelente documentación**

**Cuándo usar Caddy:**
- Necesitas HTTPS rápidamente
- Quieres configuración simple
- Sirves SPAs (React, Vue, Angular)
- Necesitas reverse proxy moderno
- Proyectos pequeños y medianos
- Desarrollo local

**Cuándo considerar alternativas:**
- Infraestructura muy grande y compleja
- Necesitas funcionalidades muy específicas de Nginx/Apache
- Ya tienes experiencia profunda con otro servidor

**Recomendación final:** Si estás empezando un nuevo proyecto o quieres simplificar tu infraestructura, Caddy es una excelente elección. La facilidad de uso y el HTTPS automático lo hacen ideal para la mayoría de casos de uso modernos.

---

**¡Disfruta usando Caddy! 🚀**
