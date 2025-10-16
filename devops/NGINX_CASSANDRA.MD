# 🚀 Guía de Nginx y Cassandra

## 📋 Índice

### Nginx
1. [Introducción a Nginx](#1-introducción-a-nginx)
2. [Instalación y Configuración](#2-instalación-y-configuración)
3. [Servidor Web Básico](#3-servidor-web-básico)
4. [Reverse Proxy](#4-reverse-proxy)
5. [Load Balancing](#5-load-balancing)
6. [SSL/TLS](#6-ssltls)
7. [Caching](#7-caching)
8. [Seguridad](#8-seguridad)
9. [Optimización](#9-optimización)
10. [Logs y Monitoreo](#10-logs-y-monitoreo)

### Cassandra
11. [Introducción a Cassandra](#11-introducción-a-cassandra)
12. [Arquitectura](#12-arquitectura)
13. [Instalación](#13-instalación)
14. [CQL (Cassandra Query Language)](#14-cql-cassandra-query-language)
15. [Data Modeling](#15-data-modeling)
16. [Partitioning y Clustering](#16-partitioning-y-clustering)
17. [Replicación](#17-replicación)
18. [Consistencia](#18-consistencia)
19. [Performance Tuning](#19-performance-tuning)
20. [Backup y Mantenimiento](#20-backup-y-mantenimiento)

---

## PARTE 1: Nginx

## 1. Introducción a Nginx

Nginx es un servidor web y proxy inverso de alto rendimiento, conocido por su eficiencia y bajo consumo de recursos.

### Características principales:
- **Alto rendimiento**: Maneja miles de conexiones concurrentes
- **Proxy inverso**: Redistribuye tráfico a servidores backend
- **Load balancer**: Balancea carga entre múltiples servidores
- **Caching**: Almacena respuestas para mejorar rendimiento
- **SSL/TLS termination**: Gestiona conexiones seguras
- **Servidor de archivos estáticos**: Sirve contenido estático eficientemente

---

## 2. Instalación y Configuración

### Instalación

#### Ubuntu/Debian:
```bash
sudo apt update
sudo apt install nginx

# Verificar instalación
nginx -v

# Ver estado
sudo systemctl status nginx

# Iniciar
sudo systemctl start nginx

# Detener
sudo systemctl stop nginx

# Reiniciar
sudo systemctl restart nginx

# Recargar configuración sin downtime
sudo systemctl reload nginx

# Habilitar inicio automático
sudo systemctl enable nginx
```

#### Docker:
```bash
docker run -d \
  --name nginx \
  -p 80:80 \
  -p 443:443 \
  -v /path/to/content:/usr/share/nginx/html:ro \
  -v /path/to/config:/etc/nginx/conf.d:ro \
  nginx:latest
```

### Estructura de archivos

```plaintext
/etc/nginx/
├── nginx.conf                 # Configuración principal
├── sites-available/           # Sitios disponibles
│   └── default
├── sites-enabled/             # Sitios habilitados (symlinks)
│   └── default -> ../sites-available/default
├── conf.d/                    # Configuraciones adicionales
├── snippets/                  # Snippets reutilizables
└── modules-enabled/           # Módulos habilitados

/var/log/nginx/
├── access.log                 # Log de accesos
└── error.log                  # Log de errores

/usr/share/nginx/html/         # Contenido web por defecto
```

### Configuración principal

```nginx
# /etc/nginx/nginx.conf
user www-data;
worker_processes auto;
pid /run/nginx.pid;
error_log /var/log/nginx/error.log;

events {
    worker_connections 1024;
    use epoll;
    multi_accept on;
}

http {
    # Configuraciones básicas
    include /etc/nginx/mime.types;
    default_type application/octet-stream;
    
    # Logging
    log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                    '$status $body_bytes_sent "$http_referer" '
                    '"$http_user_agent" "$http_x_forwarded_for"';
    
    access_log /var/log/nginx/access.log main;
    
    # Performance
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    types_hash_max_size 2048;
    
    # Compresión
    gzip on;
    gzip_vary on;
    gzip_proxied any;
    gzip_comp_level 6;
    gzip_types text/plain text/css text/xml text/javascript 
               application/json application/javascript application/xml+rss;
    
    # Incluir configuraciones de sitios
    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;
}
```

---

## 3. Servidor Web Básico

### Servidor estático simple

```nginx
# /etc/nginx/sites-available/mysite
server {
    listen 80;
    listen [::]:80;
    
    server_name example.com www.example.com;
    root /var/www/mysite;
    index index.html index.htm;
    
    location / {
        try_files $uri $uri/ =404;
    }
    
    # Logs específicos del sitio
    access_log /var/log/nginx/mysite_access.log;
    error_log /var/log/nginx/mysite_error.log;
}
```

### Servir aplicación SPA (React, Vue, Angular)

```nginx
server {
    listen 80;
    server_name app.example.com;
    root /var/www/spa;
    
    location / {
        try_files $uri $uri/ /index.html;
    }
    
    # Cache de assets estáticos
    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2|ttf|eot)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
    
    # No cachear el index.html
    location = /index.html {
        add_header Cache-Control "no-cache, must-revalidate";
    }
}
```

---

## 4. Reverse Proxy

### Proxy básico

```nginx
server {
    listen 80;
    server_name api.example.com;
    
    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        
        # Headers
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        
        # Timeouts
        proxy_connect_timeout 60s;
        proxy_send_timeout 60s;
        proxy_read_timeout 60s;
    }
}
```

### Proxy con WebSockets

```nginx
server {
    listen 80;
    server_name ws.example.com;
    
    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        
        # WebSocket headers
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        
        # Timeouts para WebSocket
        proxy_read_timeout 86400;
        proxy_send_timeout 86400;
    }
}
```

### Múltiples backends

```nginx
server {
    listen 80;
    server_name app.example.com;
    
    # Frontend
    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
    }
    
    # API
    location /api {
        proxy_pass http://localhost:8080;
        proxy_set_header Host $host;
        rewrite ^/api/(.*) /$1 break;
    }
    
    # Servicio de autenticación
    location /auth {
        proxy_pass http://localhost:9000;
        proxy_set_header Host $host;
    }
}
```

---

## 5. Load Balancing

### Round Robin (por defecto)

```nginx
upstream backend {
    server backend1.example.com;
    server backend2.example.com;
    server backend3.example.com;
}

server {
    listen 80;
    server_name example.com;
    
    location / {
        proxy_pass http://backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

### Least Connections

```nginx
upstream backend {
    least_conn;
    
    server backend1.example.com;
    server backend2.example.com;
    server backend3.example.com;
}
```

### IP Hash (sticky sessions)

```nginx
upstream backend {
    ip_hash;
    
    server backend1.example.com;
    server backend2.example.com;
    server backend3.example.com;
}
```

### Weighted Load Balancing

```nginx
upstream backend {
    server backend1.example.com weight=3;
    server backend2.example.com weight=2;
    server backend3.example.com weight=1;
}
```

### Health Checks

```nginx
upstream backend {
    server backend1.example.com max_fails=3 fail_timeout=30s;
    server backend2.example.com max_fails=3 fail_timeout=30s;
    server backend3.example.com backup;  # Servidor de respaldo
}
```

---

## 6. SSL/TLS

### Configuración SSL básica

```nginx
server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    
    server_name example.com www.example.com;
    
    # Certificados
    ssl_certificate /etc/nginx/ssl/example.com.crt;
    ssl_certificate_key /etc/nginx/ssl/example.com.key;
    
    # Configuración SSL
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers 'ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256';
    ssl_prefer_server_ciphers off;
    
    # Session cache
    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 10m;
    
    # OCSP Stapling
    ssl_stapling on;
    ssl_stapling_verify on;
    
    root /var/www/html;
    index index.html;
    
    location / {
        try_files $uri $uri/ =404;
    }
}

# Redirigir HTTP a HTTPS
server {
    listen 80;
    listen [::]:80;
    server_name example.com www.example.com;
    
    return 301 https://$server_name$request_uri;
}
```

### Let's Encrypt con Certbot

```bash
# Instalar Certbot
sudo apt install certbot python3-certbot-nginx

# Obtener certificado
sudo certbot --nginx -d example.com -d www.example.com

# Renovación automática (cron)
sudo certbot renew --dry-run

# Certificados se renuevan automáticamente
```

---

## 7. Caching

### Proxy Cache

```nginx
# Configurar zona de cache
http {
    proxy_cache_path /var/cache/nginx/data 
                     levels=1:2 
                     keys_zone=my_cache:10m 
                     max_size=10g 
                     inactive=60m 
                     use_temp_path=off;
    
    server {
        listen 80;
        server_name example.com;
        
        location / {
            proxy_cache my_cache;
            proxy_cache_key "$scheme$request_method$host$request_uri";
            proxy_cache_valid 200 60m;
            proxy_cache_valid 404 10m;
            proxy_cache_bypass $http_cache_control;
            
            add_header X-Cache-Status $upstream_cache_status;
            
            proxy_pass http://backend;
        }
    }
}
```

### FastCGI Cache (PHP)

```nginx
http {
    fastcgi_cache_path /var/cache/nginx/fastcgi 
                       levels=1:2 
                       keys_zone=php_cache:10m 
                       max_size=1g 
                       inactive=60m;
    
    server {
        listen 80;
        server_name example.com;
        root /var/www/html;
        
        location ~ \.php$ {
            fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
            fastcgi_cache php_cache;
            fastcgi_cache_valid 200 60m;
            fastcgi_cache_key "$scheme$request_method$host$request_uri";
            
            add_header X-Cache-Status $upstream_cache_status;
            
            include fastcgi_params;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        }
    }
}
```

---

## 8. Seguridad

### Rate Limiting

```nginx
http {
    # Definir zona de rate limit
    limit_req_zone $binary_remote_addr zone=mylimit:10m rate=10r/s;
    
    server {
        listen 80;
        server_name example.com;
        
        location /api/ {
            limit_req zone=mylimit burst=20 nodelay;
            proxy_pass http://backend;
        }
    }
}
```

### Restricción por IP

```nginx
server {
    listen 80;
    server_name admin.example.com;
    
    # Permitir solo IPs específicas
    allow 192.168.1.0/24;
    allow 10.0.0.1;
    deny all;
    
    location / {
        proxy_pass http://backend;
    }
}
```

### Autenticación básica

```bash
# Crear archivo de contraseñas
sudo htpasswd -c /etc/nginx/.htpasswd user1
sudo htpasswd /etc/nginx/.htpasswd user2
```

```nginx
server {
    listen 80;
    server_name admin.example.com;
    
    location / {
        auth_basic "Restricted Area";
        auth_basic_user_file /etc/nginx/.htpasswd;
        
        proxy_pass http://backend;
    }
}
```

### Headers de seguridad

```nginx
server {
    listen 443 ssl http2;
    server_name example.com;
    
    # Security headers
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header Referrer-Policy "no-referrer-when-downgrade" always;
    add_header Content-Security-Policy "default-src 'self' https: data: 'unsafe-inline'" always;
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
    
    location / {
        proxy_pass http://backend;
    }
}
```

---

## 9. Optimización

### Buffer y Timeout Settings

```nginx
http {
    # Buffers
    client_body_buffer_size 10K;
    client_header_buffer_size 1k;
    client_max_body_size 8m;
    large_client_header_buffers 2 1k;
    
    # Timeouts
    client_body_timeout 12;
    client_header_timeout 12;
    keepalive_timeout 15;
    send_timeout 10;
    
    # Workers
    worker_processes auto;
    worker_rlimit_nofile 65535;
    
    events {
        worker_connections 4096;
        use epoll;
        multi_accept on;
    }
}
```

### HTTP/2

```nginx
server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    
    server_name example.com;
    
    # HTTP/2 push
    http2_push_preload on;
    
    location = /index.html {
        http2_push /style.css;
        http2_push /script.js;
    }
}
```

---

## 10. Logs y Monitoreo

### Configuración de logs

```nginx
http {
    # Log format personalizado
    log_format custom '$remote_addr - $remote_user [$time_local] '
                      '"$request" $status $body_bytes_sent '
                      '"$http_referer" "$http_user_agent" '
                      '$request_time $upstream_response_time';
    
    access_log /var/log/nginx/access.log custom;
    error_log /var/log/nginx/error.log warn;
    
    server {
        listen 80;
        server_name example.com;
        
        # Logs específicos
        access_log /var/log/nginx/example_access.log custom;
        error_log /var/log/nginx/example_error.log;
        
        # No logear ciertos endpoints
        location /health {
            access_log off;
            return 200 "healthy\n";
        }
    }
}
```

### Nginx Status

```nginx
server {
    listen 8080;
    server_name localhost;
    
    location /nginx_status {
        stub_status;
        allow 127.0.0.1;
        deny all;
    }
}
```

---

## PARTE 2: Cassandra

## 11. Introducción a Cassandra

Apache Cassandra es una base de datos NoSQL distribuida, diseñada para manejar grandes volúmenes de datos a través de muchos servidores sin un punto único de fallo.

### Características principales:
- **Distribuida**: Diseñada para ejecutarse en múltiples nodos
- **Sin punto único de fallo**: Alta disponibilidad
- **Escalabilidad lineal**: Añadir nodos aumenta capacidad linealmente
- **Replicación**: Datos replicados automáticamente
- **Tunable consistency**: Configurar nivel de consistencia por operación
- **Alto rendimiento**: Escrituras y lecturas muy rápidas

---

## 12. Arquitectura

### Conceptos clave

```plaintext
Cluster
  └── Datacenter 1
       ├── Rack 1
       │   ├── Node 1
       │   └── Node 2
       └── Rack 2
           ├── Node 3
           └── Node 4
```

- **Node**: Instancia individual de Cassandra
- **Cluster**: Colección de nodos
- **Datacenter**: Grupo lógico de nodos
- **Rack**: Grupo lógico dentro de un datacenter
- **Partition Key**: Determina en qué nodo se almacenan los datos
- **Replication Factor**: Número de copias de los datos
- **Consistency Level**: Cuántos nodos deben responder

---

## 13. Instalación

### Docker

```bash
# Ejecutar Cassandra en Docker
docker run -d \
  --name cassandra \
  -p 9042:9042 \
  -v cassandra_data:/var/lib/cassandra \
  cassandra:latest

# Conectar al contenedor
docker exec -it cassandra cqlsh

# Ver logs
docker logs cassandra
```

### Docker Compose (Cluster de 3 nodos)

```yaml
# docker-compose.yml
version: '3'

services:
  cassandra-1:
    image: cassandra:latest
    container_name: cassandra-1
    ports:
      - "9042:9042"
    environment:
      - CASSANDRA_CLUSTER_NAME=TestCluster
      - CASSANDRA_SEEDS=cassandra-1
    volumes:
      - cassandra1_data:/var/lib/cassandra

  cassandra-2:
    image: cassandra:latest
    container_name: cassandra-2
    environment:
      - CASSANDRA_CLUSTER_NAME=TestCluster
      - CASSANDRA_SEEDS=cassandra-1
    depends_on:
      - cassandra-1
    volumes:
      - cassandra2_data:/var/lib/cassandra

  cassandra-3:
    image: cassandra:latest
    container_name: cassandra-3
    environment:
      - CASSANDRA_CLUSTER_NAME=TestCluster
      - CASSANDRA_SEEDS=cassandra-1
    depends_on:
      - cassandra-1
    volumes:
      - cassandra3_data:/var/lib/cassandra

volumes:
  cassandra1_data:
  cassandra2_data:
  cassandra3_data:
```

### Ubuntu/Debian

```bash
# Añadir repositorio
echo "deb https://debian.cassandra.apache.org 41x main" | sudo tee /etc/apt/sources.list.d/cassandra.list
curl https://downloads.apache.org/cassandra/KEYS | sudo apt-key add -

# Instalar
sudo apt update
sudo apt install cassandra

# Iniciar servicio
sudo systemctl start cassandra
sudo systemctl enable cassandra

# Verificar estado
sudo systemctl status cassandra
nodetool status

# Conectar con cqlsh
cqlsh
```

---

## 14. CQL (Cassandra Query Language)

### Crear Keyspace

```sql
-- Keyspace = equivalente a database
CREATE KEYSPACE IF NOT EXISTS myapp
WITH REPLICATION = {
    'class': 'SimpleStrategy',
    'replication_factor': 3
};

-- Usar keyspace
USE myapp;

-- Ver keyspaces
DESCRIBE KEYSPACES;
```

### Crear Tablas

```sql
-- Tabla básica
CREATE TABLE users (
    user_id UUID PRIMARY KEY,
    username TEXT,
    email TEXT,
    created_at TIMESTAMP
);

-- Tabla con partition key y clustering columns
CREATE TABLE posts (
    user_id UUID,
    post_id TIMEUUID,
    title TEXT,
    content TEXT,
    created_at TIMESTAMP,
    PRIMARY KEY (user_id, post_id)
) WITH CLUSTERING ORDER BY (post_id DESC);

-- Tabla con partition key compuesta
CREATE TABLE user_activities (
    user_id UUID,
    activity_date DATE,
    activity_id TIMEUUID,
    activity_type TEXT,
    details MAP<TEXT, TEXT>,
    PRIMARY KEY ((user_id, activity_date), activity_id)
);
```

### INSERT, UPDATE, DELETE

```sql
-- INSERT
INSERT INTO users (user_id, username, email, created_at)
VALUES (uuid(), 'john_doe', 'john@example.com', toTimestamp(now()));

-- Insertar con TTL (Time To Live)
INSERT INTO users (user_id, username, email)
VALUES (uuid(), 'temp_user', 'temp@example.com')
USING TTL 86400;  -- Expira en 24 horas

-- UPDATE
UPDATE users
SET email = 'newemail@example.com'
WHERE user_id = 123e4567-e89b-12d3-a456-426614174000;

-- DELETE
DELETE FROM users
WHERE user_id = 123e4567-e89b-12d3-a456-426614174000;

-- DELETE específica
DELETE email FROM users
WHERE user_id = 123e4567-e89b-12d3-a456-426614174000;
```

### SELECT

```sql
-- SELECT básico
SELECT * FROM users;

-- Con filtros (requiere partition key)
SELECT * FROM users
WHERE user_id = 123e4567-e89b-12d3-a456-426614174000;

-- Con clustering columns
SELECT * FROM posts
WHERE user_id = 123e4567-e89b-12d3-a456-426614174000
AND post_id > minTimeuuid('2024-01-01 00:00:00');

-- LIMIT
SELECT * FROM users LIMIT 10;

-- ALLOW FILTERING (usar con cuidado)
SELECT * FROM users
WHERE email = 'john@example.com'
ALLOW FILTERING;
```

### Tipos de datos

```sql
CREATE TABLE data_types_example (
    id UUID PRIMARY KEY,
    
    -- Numéricos
    int_col INT,
    bigint_col BIGINT,
    float_col FLOAT,
    double_col DOUBLE,
    decimal_col DECIMAL,
    
    -- Texto
    text_col TEXT,
    varchar_col VARCHAR,
    ascii_col ASCII,
    
    -- Temporal
    timestamp_col TIMESTAMP,
    date_col DATE,
    time_col TIME,
    
    -- Booleano
    bool_col BOOLEAN,
    
    -- UUID
    uuid_col UUID,
    timeuuid_col TIMEUUID,
    
    -- Colecciones
    list_col LIST<TEXT>,
    set_col SET<INT>,
    map_col MAP<TEXT, INT>,
    
    -- Otros
    blob_col BLOB,
    inet_col INET,
    counter_col COUNTER
);
```

---

## 15. Data Modeling

### Principios de modelado

1. **Query-first**: Diseñar basándose en las queries necesarias
2. **Denormalización**: Duplicar datos para optimizar lecturas
3. **Una query = una tabla**: Cada query debe acceder a una sola tabla

### Ejemplo: Sistema de blogs

```sql
-- Tabla 1: Posts por usuario (para timeline)
CREATE TABLE posts_by_user (
    user_id UUID,
    post_id TIMEUUID,
    title TEXT,
    content TEXT,
    tags SET<TEXT>,
    created_at TIMESTAMP,
    PRIMARY KEY (user_id, post_id)
) WITH CLUSTERING ORDER BY (post_id DESC);

-- Tabla 2: Posts por tag (para búsqueda por tag)
CREATE TABLE posts_by_tag (
    tag TEXT,
    post_id TIMEUUID,
    user_id UUID,
    title TEXT,
    created_at TIMESTAMP,
    PRIMARY KEY (tag, post_id)
) WITH CLUSTERING ORDER BY (post_id DESC);

-- Tabla 3: Comentarios por post
CREATE TABLE comments_by_post (
    post_id UUID,
    comment_id TIMEUUID,
    user_id UUID,
    comment_text TEXT,
    created_at TIMESTAMP,
    PRIMARY KEY (post_id, comment_id)
) WITH CLUSTERING ORDER BY (comment_id DESC);

-- Tabla 4: Feed del usuario (posts de gente que sigue)
CREATE TABLE user_feed (
    user_id UUID,
    post_id TIMEUUID,
    author_id UUID,
    title TEXT,
    created_at TIMESTAMP,
    PRIMARY KEY (user_id, post_id)
) WITH CLUSTERING ORDER BY (post_id DESC);
```

---

## 16. Partitioning y Clustering

### Partition Key

```sql
-- Partition key simple
CREATE TABLE users (
    user_id UUID,
    name TEXT,
    PRIMARY KEY (user_id)  -- user_id es partition key
);

-- Partition key compuesta
CREATE TABLE time_series (
    sensor_id UUID,
    date DATE,
    timestamp TIMESTAMP,
    value DOUBLE,
    PRIMARY KEY ((sensor_id, date), timestamp)
    -- (sensor_id, date) es partition key compuesta
    -- timestamp es clustering column
);
```

### Clustering Columns

```sql
CREATE TABLE events (
    user_id UUID,
    event_time TIMESTAMP,
    event_type TEXT,
    data MAP<TEXT, TEXT>,
    PRIMARY KEY (user_id, event_time, event_type)
) WITH CLUSTERING ORDER BY (event_time DESC, event_type ASC);

-- Query eficiente
SELECT * FROM events
WHERE user_id = ? AND event_time > ?;

-- Query eficiente con clustering
SELECT * FROM events
WHERE user_id = ? 
AND event_time > ?
AND event_time < ?
AND event_type = 'login';
```

---

## 17. Replicación

### Replication Strategy

```sql
-- SimpleStrategy (desarrollo/testing)
CREATE KEYSPACE dev_keyspace
WITH REPLICATION = {
    'class': 'SimpleStrategy',
    'replication_factor': 3
};

-- NetworkTopologyStrategy (producción)
CREATE KEYSPACE prod_keyspace
WITH REPLICATION = {
    'class': 'NetworkTopologyStrategy',
    'datacenter1': 3,
    'datacenter2': 2
};

-- Ver configuración
DESCRIBE KEYSPACE prod_keyspace;

-- Alterar replicación
ALTER KEYSPACE prod_keyspace
WITH REPLICATION = {
    'class': 'NetworkTopologyStrategy',
    'datacenter1': 5,
    'datacenter2': 3
};
```

---

## 18. Consistencia

### Consistency Levels

```sql
-- Nivel de consistencia en la sesión
CONSISTENCY QUORUM;

-- INSERT con consistencia específica
INSERT INTO users (user_id, name) VALUES (uuid(), 'John')
USING CONSISTENCY LOCAL_QUORUM;

-- SELECT con consistencia
SELECT * FROM users USING CONSISTENCY ONE;
```

### Niveles disponibles:

- **ONE**: Una réplica responde
- **TWO**: Dos réplicas responden
- **THREE**: Tres réplicas responden
- **QUORUM**: Mayoría de réplicas (RF/2 + 1)
- **ALL**: Todas las réplicas
- **LOCAL_QUORUM**: Quorum en datacenter local
- **EACH_QUORUM**: Quorum en cada datacenter
- **ANY**: Al menos un nodo (solo escrituras)

---

## 19. Performance Tuning

### Índices secundarios

```sql
-- Crear índice secundario
CREATE INDEX ON users (email);

-- Query usando índice
SELECT * FROM users WHERE email = 'john@example.com';

-- Eliminar índice
DROP INDEX users_email_idx;
```

### Materialized Views

```sql
CREATE MATERIALIZED VIEW users_by_email AS
SELECT user_id, username, email, created_at
FROM users
WHERE email IS NOT NULL AND user_id IS NOT NULL
PRIMARY KEY (email, user_id);

-- Query usando MV
SELECT * FROM users_by_email
WHERE email = 'john@example.com';
```

### Batch Statements

```sql
BEGIN BATCH
    INSERT INTO users (user_id, name) VALUES (uuid(), 'User1');
    INSERT INTO users (user_id, name) VALUES (uuid(), 'User2');
    INSERT INTO users (user_id, name) VALUES (uuid(), 'User3');
APPLY BATCH;

-- Batch con mismo partition key (más eficiente)
BEGIN BATCH
    INSERT INTO posts_by_user (user_id, post_id, title)
    VALUES (?, uuid(), 'Post 1');
    
    INSERT INTO posts_by_user (user_id, post_id, title)
    VALUES (?, uuid(), 'Post 2');
APPLY BATCH;
```

---

## 20. Backup y Mantenimiento

### Snapshots

```bash
# Crear snapshot
nodetool snapshot myapp -t backup_20240101

# Ver snapshots
nodetool listsnapshots

# Limpiar snapshots
nodetool clearsnapshot myapp -t backup_20240101

# Limpiar todos
nodetool clearsnapshot
```

### Repair

```bash
# Reparar tabla
nodetool repair myapp users

# Reparar keyspace completo
nodetool repair myapp

# Reparar todo el nodo
nodetool repair

# Reparar con parallelismo
nodetool repair -pr myapp
```

### Compaction

```bash
# Forzar compaction
nodetool compact myapp users

# Ver estado de compaction
nodetool compactionstats

# Detener compaction
nodetool stop COMPACTION
```

### Flush

```bash
# Flush memtables a disco
nodetool flush myapp users

# Flush todo
nodetool flush
```

### Monitoreo

```bash
# Estado del cluster
nodetool status

# Info del nodo
nodetool info

# Ver cfstats (tabla stats)
nodetool cfstats myapp.users

# Thread pool stats
nodetool tpstats

# Logs en tiempo real
tail -f /var/log/cassandra/system.log
```

---

## 🎯 Ejemplo Completo: Sistema de Chat

### Cassandra

```sql
-- Keyspace
CREATE KEYSPACE chat_app
WITH REPLICATION = {
    'class': 'NetworkTopologyStrategy',
    'datacenter1': 3
};

USE chat_app;

-- Usuarios
CREATE TABLE users (
    user_id UUID PRIMARY KEY,
    username TEXT,
    email TEXT,
    created_at TIMESTAMP
);

CREATE INDEX ON users (username);

-- Mensajes por conversación
CREATE TABLE messages_by_conversation (
    conversation_id UUID,
    message_id TIMEUUID,
    sender_id UUID,
    content TEXT,
    sent_at TIMESTAMP,
    PRIMARY KEY (conversation_id, message_id)
) WITH CLUSTERING ORDER BY (message_id DESC);

-- Conversaciones por usuario
CREATE TABLE conversations_by_user (
    user_id UUID,
    conversation_id UUID,
    other_user_id UUID,
    last_message TEXT,
    last_message_at TIMESTAMP,
    PRIMARY KEY (user_id, last_message_at, conversation_id)
) WITH CLUSTERING ORDER BY (last_message_at DESC);
```

### Nginx

```nginx
# nginx.conf
upstream chat_backend {
    least_conn;
    server backend1:3000 max_fails=3 fail_timeout=30s;
    server backend2:3000 max_fails=3 fail_timeout=30s;
    server backend3:3000 max_fails=3 fail_timeout=30s;
}

server {
    listen 443 ssl http2;
    server_name chat.example.com;
    
    ssl_certificate /etc/nginx/ssl/cert.pem;
    ssl_certificate_key /etc/nginx/ssl/key.pem;
    
    # Rate limiting
    limit_req_zone $binary_remote_addr zone=chat_limit:10m rate=100r/s;
    
    location / {
        root /var/www/chat-frontend;
        try_files $uri /index.html;
    }
    
    location /api {
        limit_req zone=chat_limit burst=200 nodelay;
        
        proxy_pass http://chat_backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
    
    location /ws {
        proxy_pass http://chat_backend;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_read_timeout 86400;
    }
}
```

---

## 🚀 Mejores Prácticas

### Nginx:
1. Usar HTTP/2
2. Habilitar compresión gzip
3. Configurar SSL correctamente
4. Implementar rate limiting
5. Usar caché cuando sea apropiado
6. Monitorear logs
7. Optimizar buffers y timeouts

### Cassandra:
1. Modelar basándose en queries
2. Usar partition keys apropiadas
3. Evitar hot partitions
4. Configurar replicación correctamente
5. Elegir consistency level apropiado
6. Hacer backups regulares
7. Monitorear métricas
8. Ejecutar repairs periódicamente
9. No usar ALLOW FILTERING en producción
10. Usar batches solo para mismo partition key

---

**¡Domina Nginx y Cassandra para aplicaciones de alta escala! 🚀**
