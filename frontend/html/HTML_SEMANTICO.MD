# Guía Completa de HTML Semántico

## ¿Qué es HTML semántico?

HTML semántico es el uso de etiquetas HTML que describen el significado y la función del contenido, no solo su apariencia visual.

No se trata únicamente de “hacer que se vea bien”, sino de estructurar la información de forma que:

- el navegador la entienda mejor
- los lectores de pantalla puedan interpretarla
- los motores de búsqueda comprendan la jerarquía del contenido
- el código sea más mantenible para otros desarrolladores

---

## ¿Por qué es importante?

### 1. Accesibilidad

Las etiquetas semánticas ayudan a tecnologías de asistencia como lectores de pantalla a identificar regiones importantes de la página, encabezados, navegación, formularios, artículos y contenido complementario.

### 2. SEO

Los buscadores entienden mejor una página cuando el contenido está bien estructurado. Una jerarquía correcta de títulos y el uso de elementos como `article`, `section`, `nav` o `main` aportan contexto.

### 3. Mantenimiento

Un HTML semántico hace que el proyecto sea más legible. Es más fácil entender qué representa cada bloque sin depender de clases o estilos.

### 4. Reutilización y escalabilidad

Cuando la estructura es clara, resulta más sencillo integrar CSS, JavaScript, testing y componentes reutilizables.

---

## HTML semántico vs HTML no semántico

### Ejemplo no semántico

```html
<div class="header">Mi sitio</div>
<div class="menu">...</div>
<div class="content">...</div>
<div class="footer">...</div>
```

### Ejemplo semántico

```html
<header>Mi sitio</header>
<nav>...</nav>
<main>...</main>
<footer>...</footer>
```

Ambos pueden verse igual con CSS, pero el segundo comunica claramente la intención del contenido.

---

## Estructura base de un documento HTML semántico

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="Guía de HTML semántico">
  <title>HTML semántico</title>
</head>
<body>
  <header>
    <h1>Mi sitio web</h1>
    <nav aria-label="Navegación principal">
      <ul>
        <li><a href="#inicio">Inicio</a></li>
        <li><a href="#articulos">Artículos</a></li>
        <li><a href="#contacto">Contacto</a></li>
      </ul>
    </nav>
  </header>

  <main>
    <section id="inicio">
      <h2>Bienvenida</h2>
      <p>Contenido principal de la página.</p>
    </section>

    <section id="articulos">
      <h2>Últimos artículos</h2>

      <article>
        <h3>Artículo 1</h3>
        <p>Resumen del artículo.</p>
      </article>
    </section>
  </main>

  <aside>
    <h2>Contenido relacionado</h2>
    <p>Enlaces o información adicional.</p>
  </aside>

  <footer>
    <p>&copy; 2026 Mi sitio</p>
  </footer>
</body>
</html>
```

---

## Elementos semánticos principales

### `header`

Representa la cabecera de una página o de una sección.

Usos comunes:

- logo
- título principal
- navegación principal
- buscador

```html
<header>
  <h1>Tienda Online</h1>
</header>
```

### `nav`

Agrupa enlaces de navegación principales o relevantes.

```html
<nav aria-label="Menú principal">
  <ul>
    <li><a href="/">Inicio</a></li>
    <li><a href="/productos">Productos</a></li>
    <li><a href="/contacto">Contacto</a></li>
  </ul>
</nav>
```

No todos los grupos de enlaces deben ir en `nav`. Úsalo para navegación importante.

### `main`

Contiene el contenido principal del documento. Debe existir solo un `main` por página y no debe repetirse dentro de otras secciones estructurales.

```html
<main>
  <h1>Panel de usuario</h1>
</main>
```

### `section`

Representa una sección temática dentro de la página. Normalmente debe tener un encabezado.

```html
<section>
  <h2>Preguntas frecuentes</h2>
  <p>...</p>
</section>
```

Usa `section` cuando el bloque tenga sentido como agrupación temática.

### `article`

Representa contenido independiente y reutilizable por sí mismo.

Ejemplos:

- noticia
- post de blog
- comentario
- tarjeta de producto
- publicación de foro

```html
<article>
  <h2>Cómo mejorar la accesibilidad web</h2>
  <p>Publicado el 27 de abril de 2026.</p>
</article>
```

### `aside`

Contiene contenido complementario o tangencial al contenido principal.

```html
<aside>
  <h2>Artículos relacionados</h2>
  <ul>
    <li><a href="#">Accesibilidad</a></li>
    <li><a href="#">SEO técnico</a></li>
  </ul>
</aside>
```

### `footer`

Representa el pie de página de un documento o una sección.

```html
<footer>
  <p>© 2026 Mi empresa</p>
</footer>
```

---

## Otros elementos semánticos importantes

### `figure` y `figcaption`

Sirven para agrupar contenido visual y su descripción.

```html
<figure>
  <img src="grafico.png" alt="Evolución de ventas por trimestre">
  <figcaption>Ventas trimestrales de 2025</figcaption>
</figure>
```

### `time`

Representa fechas y horas de forma semántica.

```html
<p>Publicado el <time datetime="2026-04-27">27 de abril de 2026</time></p>
```

### `address`

Se utiliza para información de contacto relacionada con el autor, la empresa o la sección más cercana.

```html
<address>
  Escríbenos a <a href="mailto:hola@ejemplo.com">hola@ejemplo.com</a>
</address>
```

### `details` y `summary`

Permiten mostrar contenido desplegable de forma nativa.

```html
<details>
  <summary>¿Qué incluye el curso?</summary>
  <p>Incluye teoría, práctica y ejemplos reales.</p>
</details>
```

### `mark`

Destaca una parte del texto por relevancia.

```html
<p>El tema más importante es <mark>la accesibilidad</mark>.</p>
```

### `abbr`

Indica abreviaturas o siglas.

```html
<p><abbr title="HyperText Markup Language">HTML</abbr> es el lenguaje base de la web.</p>
```

### `blockquote` y `cite`

Para citas extensas y referencia de la fuente.

```html
<blockquote cite="https://example.com">
  <p>La semántica mejora la comprensión del contenido.</p>
</blockquote>
```

---

## Jerarquía correcta de encabezados

Los encabezados (`h1` a `h6`) no son solo estilos visuales; representan la estructura lógica del contenido.

### Buenas prácticas

- usar un `h1` principal por página
- mantener una jerarquía ordenada
- no saltar niveles sin motivo
- no usar encabezados solo para cambiar tamaño visual

### Ejemplo correcto

```html
<h1>Guía de Accesibilidad</h1>
<h2>Introducción</h2>
<h3>Lectores de pantalla</h3>
<h2>Buenas prácticas</h2>
```

### Ejemplo incorrecto

```html
<h1>Guía de Accesibilidad</h1>
<h4>Introducción</h4>
```

---

## Listas semánticas

### Lista no ordenada: `ul`

Se usa cuando el orden no importa.

```html
<ul>
  <li>HTML</li>
  <li>CSS</li>
  <li>JavaScript</li>
</ul>
```

### Lista ordenada: `ol`

Se usa cuando el orden sí importa.

```html
<ol>
  <li>Registrar usuario</li>
  <li>Confirmar email</li>
  <li>Iniciar sesión</li>
</ol>
```

### Lista de definiciones: `dl`

Útil para glosarios, preguntas frecuentes cortas o pares término-valor.

```html
<dl>
  <dt>HTML</dt>
  <dd>Lenguaje de marcado para estructurar contenido web.</dd>
</dl>
```

---

## Formularios semánticos y accesibles

Los formularios son una parte clave del HTML semántico.

### Buenas prácticas en formularios

- asociar cada campo con su `label`
- usar `fieldset` y `legend` para agrupar opciones relacionadas
- elegir el `type` correcto
- usar `button` para acciones
- complementar con atributos como `required`, `autocomplete`, `name` y `aria-describedby` cuando sea necesario

### Ejemplo

```html
<form action="/registro" method="post">
  <fieldset>
    <legend>Crear cuenta</legend>

    <label for="nombre">Nombre</label>
    <input id="nombre" name="nombre" type="text" required>

    <label for="email">Correo electrónico</label>
    <input id="email" name="email" type="email" autocomplete="email" required>

    <label for="password">Contraseña</label>
    <input id="password" name="password" type="password" required>

    <button type="submit">Registrarme</button>
  </fieldset>
</form>
```

### Elementos útiles en formularios

- `label`
- `input`
- `textarea`
- `select`
- `option`
- `button`
- `fieldset`
- `legend`
- `output`

---

## Tablas semánticas

Las tablas deben usarse para datos tabulares, no para maquetación.

### Estructura recomendada

```html
<table>
  <caption>Ventas por trimestre</caption>
  <thead>
    <tr>
      <th scope="col">Trimestre</th>
      <th scope="col">Ventas</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">Q1</th>
      <td>12.000 €</td>
    </tr>
    <tr>
      <th scope="row">Q2</th>
      <td>15.500 €</td>
    </tr>
  </tbody>
</table>
```

### Buenas prácticas

- usar `caption` para describir la tabla
- usar `th` para encabezados
- definir `scope` cuando aplique
- evitar tablas para posicionar contenido

---

## Contenido multimedia semántico

### Imágenes

Toda imagen relevante debe tener un atributo `alt` útil.

```html
<img src="equipo.jpg" alt="Equipo de desarrollo reunido en la oficina">
```

Si la imagen es decorativa, el `alt` puede ser vacío:

```html
<img src="decoracion.svg" alt="">
```

### Audio y video

```html
<video controls>
  <source src="demo.mp4" type="video/mp4">
  Tu navegador no soporta video HTML5.
</video>
```

Cuando sea posible, añade subtítulos y transcripciones.

---

## Landmarks y accesibilidad

Muchos elementos semánticos modernos actúan como **landmarks**, es decir, regiones reconocibles para tecnologías de asistencia.

Landmarks frecuentes:

- `header`
- `nav`
- `main`
- `aside`
- `footer`
- `form`

Esto facilita que un usuario navegue rápidamente entre zonas clave de la página.

### Cuándo usar ARIA

Primero usa HTML semántico nativo. Recurre a ARIA solo cuando el HTML no exprese completamente la intención.

Ejemplo correcto:

```html
<nav aria-label="Navegación secundaria">
  ...
</nav>
```

Evita reemplazar etiquetas semánticas por `div` con roles innecesarios si ya existe un elemento HTML adecuado.

---

## Cuándo usar `section`, `article` o `div`

Esta es una de las dudas más comunes.

### Usa `article` cuando:

- el contenido puede entenderse por sí solo
- podría compartirse o reutilizarse fuera del contexto actual

### Usa `section` cuando:

- agrupas contenido relacionado dentro de un tema
- normalmente existe un encabezado para esa agrupación

### Usa `div` cuando:

- no hay un elemento semántico que describa correctamente el bloque
- solo necesitas un contenedor para layout, estilos o scripting

### Regla práctica

Si puedes nombrar claramente la función del bloque con una etiqueta HTML existente, úsala. Si no, probablemente `div` sea suficiente.

---

## Errores comunes

### 1. Usar `div` para todo

```html
<div class="nav">...</div>
```

Mejor:

```html
<nav>...</nav>
```

### 2. Usar encabezados por estilo visual

Los encabezados deben indicar estructura, no tamaño de letra.

### 3. Saltar niveles de títulos

Puede romper la comprensión del documento para accesibilidad y SEO.

### 4. No usar `label` en formularios

Esto perjudica la usabilidad y la accesibilidad.

### 5. Usar tablas para maquetar

Es una práctica antigua y poco mantenible.

### 6. No definir `lang` en el documento

```html
<html lang="es">
```

Es importante para pronunciación en lectores de pantalla y para buscadores.

---

## Ejemplo completo de página semántica

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Blog de desarrollo web</title>
</head>
<body>
  <header>
    <h1>Blog de desarrollo web</h1>
    <nav aria-label="Principal">
      <ul>
        <li><a href="#articulos">Artículos</a></li>
        <li><a href="#tutoriales">Tutoriales</a></li>
        <li><a href="#contacto">Contacto</a></li>
      </ul>
    </nav>
  </header>

  <main>
    <section id="articulos">
      <h2>Artículos destacados</h2>

      <article>
        <header>
          <h3>Qué es HTML semántico</h3>
          <p>Publicado por Ana el <time datetime="2026-04-27">27 de abril de 2026</time></p>
        </header>

        <p>HTML semántico consiste en usar etiquetas con significado real.</p>

        <footer>
          <p><a href="/html-semantico">Leer más</a></p>
        </footer>
      </article>
    </section>

    <aside>
      <h2>Recursos recomendados</h2>
      <ul>
        <li><a href="#">Accesibilidad web</a></li>
        <li><a href="#">SEO on-page</a></li>
      </ul>
    </aside>
  </main>

  <footer id="contacto">
    <address>
      Contacto: <a href="mailto:hola@blog.com">hola@blog.com</a>
    </address>
  </footer>
</body>
</html>
```

---

## Buenas prácticas generales

- usa etiquetas por significado, no por apariencia
- mantén una jerarquía lógica de encabezados
- utiliza `main` una sola vez por documento
- añade `alt` útil a imágenes informativas
- usa formularios correctamente etiquetados
- evita anidar etiquetas semánticas sin sentido
- apóyate en ARIA solo cuando haga falta
- piensa siempre en accesibilidad, SEO y mantenimiento

---

## Checklist rápido de HTML semántico

Antes de dar una página por terminada, revisa:

- ¿hay un `header`, `main` y `footer` cuando corresponde?
- ¿la navegación principal está dentro de `nav`?
- ¿los títulos siguen un orden lógico?
- ¿las secciones tienen sentido temático?
- ¿los artículos son realmente contenido independiente?
- ¿las imágenes tienen `alt` adecuado?
- ¿los formularios tienen `label`?
- ¿las tablas representan datos reales?
- ¿se ha definido `lang` en `<html>`?

---

## Resumen final

HTML semántico consiste en describir correctamente la función del contenido mediante etiquetas apropiadas. No solo mejora la legibilidad del código, sino también la accesibilidad, el SEO y la mantenibilidad del proyecto.

Cuanto mejor estructurado esté el HTML, más sólida será la base sobre la que se construyen CSS, JavaScript, componentes y funcionalidades futuras.
