# Guía Completa de HTML Normativo y Etiquetas

## 1. ¿Qué es HTML?

**HTML (HyperText Markup Language)** es el lenguaje de marcado estándar para definir la **estructura** de una página web.

HTML no se encarga del diseño visual ni de la lógica de negocio:

- **HTML** estructura el contenido
- **CSS** lo presenta visualmente
- **JavaScript** añade comportamiento

### Ejemplo básico

```html
<h1>Bienvenido</h1>
<p>Esta página explica los fundamentos de HTML.</p>
```

En este ejemplo:

- `<h1>` representa un título principal
- `<p>` representa un párrafo

---

## 2. ¿Qué significa HTML normativo?

Cuando hablamos de **HTML normativo** nos referimos a escribir HTML de forma **correcta, válida, semántica y alineada con el estándar**.

Eso implica:

- usar etiquetas según su propósito real
- respetar una estructura válida del documento
- anidar elementos correctamente
- usar atributos adecuados
- favorecer accesibilidad y semántica
- evitar usar HTML solo “porque visualmente funciona”

### Ejemplo correcto

```html
<article>
  <h2>Noticias del día</h2>
  <p>Hoy se publicó una nueva actualización.</p>
</article>
```

### Ejemplo incorrecto

```html
<div>
  <div>Noticias del día</div>
  <div>Hoy se publicó una nueva actualización.</div>
</div>
```

El segundo ejemplo puede mostrarse en pantalla, pero pierde semántica:

- no define que el texto principal es un encabezado
- no indica que el contenido es un artículo
- dificulta accesibilidad, SEO y mantenimiento

---

## 3. Estructura mínima de un documento HTML válido

Una página HTML moderna suele comenzar así:

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Mi página</title>
</head>
<body>
  <h1>Hola mundo</h1>
</body>
</html>
```

### Qué hace cada parte

#### `<!DOCTYPE html>`

Indica al navegador que el documento usa **HTML5**.

```html
<!DOCTYPE html>
```

Si no se declara, el navegador puede entrar en un modo de compatibilidad no deseado.

#### `<html>`

Es el elemento raíz de todo el documento.

```html
<html lang="es">
```

El atributo `lang` es importante para:

- lectores de pantalla
- motores de búsqueda
- correctores y herramientas de accesibilidad

#### `<head>`

Contiene metadatos y configuraciones del documento.

```html
<head>
  <meta charset="UTF-8">
  <title>Mi página</title>
</head>
```

#### `<body>`

Contiene el contenido visible de la página.

```html
<body>
  <h1>Contenido visible</h1>
</body>
```

---

## 4. Anatomía de una etiqueta HTML

Una etiqueta suele tener:

- apertura
- contenido
- cierre

```html
<p>Este es un párrafo</p>
```

### Partes

- `<p>`: etiqueta de apertura
- `Este es un párrafo`: contenido
- `</p>`: etiqueta de cierre

### Etiquetas con atributos

```html
<a href="https://example.com" target="_blank">Ir al sitio</a>
```

Aquí:

- `href` indica el destino
- `target="_blank"` abre en una pestaña nueva

---

## 5. Etiquetas vacías o `void elements`

Algunas etiquetas **no llevan cierre** porque no contienen contenido interno.

Ejemplos frecuentes:

- `<img>`
- `<br>`
- `<hr>`
- `<meta>`
- `<link>`
- `<input>`

### Ejemplo

```html
<img src="foto.jpg" alt="Paisaje de montaña">
<br>
<hr>
```

Aunque a veces verás `<br />`, en HTML5 se acepta simplemente `<br>`.

---

## 6. El `head`: metadatos esenciales

El `head` no suele mostrarse directamente en pantalla, pero es crítico.

```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="Guía completa de HTML">
  <title>Guía HTML</title>
  <link rel="stylesheet" href="styles.css">
</head>
```

### Etiquetas importantes del `head`

#### `<meta charset="UTF-8">`

Define la codificación de caracteres.

```html
<meta charset="UTF-8">
```

Permite mostrar correctamente tildes, eñes y símbolos.

#### `<meta name="viewport">`

Hace que la página se adapte mejor a móviles.

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

#### `<title>`

Define el título de la pestaña del navegador.

```html
<title>Curso de HTML</title>
```

#### `<meta name="description">`

Sirve para describir la página.

```html
<meta name="description" content="Aprende HTML desde cero con ejemplos">
```

#### `<link>`

Se usa normalmente para enlazar CSS u otros recursos.

```html
<link rel="stylesheet" href="styles.css">
```

#### `<script>`

Permite cargar JavaScript.

```html
<script src="app.js"></script>
```

Si no necesitas ejecutar el script antes de pintar el contenido, suele ser mejor usar:

```html
<script src="app.js" defer></script>
```

---

## 7. Etiquetas de texto más usadas

## Encabezados: `<h1>` a `<h6>`

Representan títulos jerárquicos.

```html
<h1>Guía de HTML</h1>
<h2>Introducción</h2>
<h3>Etiquetas básicas</h3>
```

### Buena práctica

- usa un `<h1>` principal por página como referencia semántica
- no elijas encabezados por tamaño visual, sino por jerarquía

### Mal uso

```html
<h1>Título principal</h1>
<h4>Subsección inmediata</h4>
```

Si realmente es subsección directa, normalmente sería más coherente usar `<h2>`.

## Párrafo: `<p>`

```html
<p>HTML define la estructura del contenido.</p>
```

Se usa para bloques de texto normales.

## Saltos y separadores: `<br>` y `<hr>`

```html
<p>Línea 1<br>Línea 2</p>
<hr>
```

- `<br>` fuerza un salto de línea
- `<hr>` representa un cambio temático o separador

No conviene abusar de `<br>` para maquetar espacios.

## Resaltado semántico

### `<strong>`

Da importancia fuerte al contenido.

```html
<p><strong>Importante:</strong> guarda una copia de seguridad.</p>
```

### `<em>`

Da énfasis al contenido.

```html
<p>Debes leer <em>atentamente</em> las instrucciones.</p>
```

### `<b>` y `<i>`

Sirven para resaltar visualmente sin añadir tanto significado semántico.

```html
<p><b>Oferta</b> disponible hoy.</p>
<p><i>Término en otro idioma</i>.</p>
```

Cuando quieras transmitir importancia o énfasis real, normalmente es mejor usar `strong` o `em`.

## Texto pequeño y complementario

### `<small>`

```html
<small>Aplican términos y condiciones.</small>
```

### `<mark>`

```html
<p>La palabra clave es <mark>HTML</mark>.</p>
```

### `<sup>` y `<sub>`

```html
<p>m<sup>2</sup></p>
<p>H<sub>2</sub>O</p>
```

---

## 8. Listas

## Lista no ordenada: `<ul>`

```html
<ul>
  <li>HTML</li>
  <li>CSS</li>
  <li>JavaScript</li>
</ul>
```

Se usa cuando el orden no importa.

## Lista ordenada: `<ol>`

```html
<ol>
  <li>Abrir el editor</li>
  <li>Crear el archivo</li>
  <li>Guardar los cambios</li>
</ol>
```

Se usa cuando el orden sí importa.

## Elemento de lista: `<li>`

`<li>` debe estar dentro de una lista válida, normalmente `ul` u `ol`.

### Incorrecto

```html
<li>Elemento suelto</li>
```

### Correcto

```html
<ul>
  <li>Elemento dentro de la lista</li>
</ul>
```

## Lista de definiciones: `<dl>`, `<dt>`, `<dd>`

```html
<dl>
  <dt>HTML</dt>
  <dd>Lenguaje de marcado para estructurar contenido web.</dd>
</dl>
```

Muy útil para glosarios, preguntas frecuentes o pares concepto-definición.

---

## 9. Enlaces

La etiqueta principal es `<a>`.

```html
<a href="https://developer.mozilla.org/">MDN</a>
```

### Enlace interno

```html
<a href="/contacto">Ir a contacto</a>
```

### Enlace externo

```html
<a href="https://example.com" target="_blank" rel="noopener noreferrer">
  Abrir sitio externo
</a>
```

Si usas `target="_blank"`, conviene añadir `rel="noopener noreferrer"` por seguridad y rendimiento.

### Enlace a una sección de la misma página

```html
<a href="#faq">Ir a preguntas frecuentes</a>

<h2 id="faq">Preguntas frecuentes</h2>
```

### Enlace de email o teléfono

```html
<a href="mailto:soporte@example.com">Enviar correo</a>
<a href="tel:+34123456789">Llamar</a>
```

---

## 10. Imágenes y contenido incrustado

## Imagen: `<img>`

```html
<img src="perfil.jpg" alt="Foto de perfil de Ana" width="200">
```

### Atributos importantes

- `src`: ruta de la imagen
- `alt`: texto alternativo
- `width` y `height`: dimensiones recomendables

### Por qué `alt` es obligatorio en HTML bien hecho

Porque ayuda a:

- accesibilidad
- lectores de pantalla
- contextos donde la imagen no carga

### Mal ejemplo

```html
<img src="grafica.png">
```

### Mejor ejemplo

```html
<img src="grafica.png" alt="Gráfica de ventas mensuales">
```

## Figura y pie de imagen: `<figure>` y `<figcaption>`

```html
<figure>
  <img src="paisaje.jpg" alt="Montañas al atardecer">
  <figcaption>Paisaje tomado durante una excursión en otoño.</figcaption>
</figure>
```

Esto agrupa contenido visual con su descripción.

## Iframe: `<iframe>`

```html
<iframe
  src="https://www.youtube.com/embed/VIDEO_ID"
  title="Video introductorio de HTML">
</iframe>
```

Debe usarse con cuidado por seguridad, rendimiento y accesibilidad.

---

## 11. Estructura semántica de HTML5

Una de las bases del HTML normativo moderno es usar etiquetas semánticas.

## `<header>`

Representa la cabecera de una página o sección.

```html
<header>
  <h1>Mi blog</h1>
</header>
```

## `<nav>`

Representa navegación principal o secundaria importante.

```html
<nav>
  <a href="/">Inicio</a>
  <a href="/articulos">Artículos</a>
</nav>
```

## `<main>`

Representa el contenido principal del documento.

```html
<main>
  <h2>Contenido principal</h2>
  <p>Este bloque contiene el tema central de la página.</p>
</main>
```

Normalmente debe existir un solo `<main>` principal por página.

## `<section>`

Agrupa contenido relacionado dentro de un tema.

```html
<section>
  <h2>Servicios</h2>
  <p>Desarrollo web y consultoría.</p>
</section>
```

## `<article>`

Representa una unidad de contenido independiente.

```html
<article>
  <h2>Cómo aprender HTML</h2>
  <p>Empieza por la estructura semántica.</p>
</article>
```

## `<aside>`

Representa contenido complementario.

```html
<aside>
  <h2>Recursos relacionados</h2>
  <p>Puedes leer la guía de CSS después.</p>
</aside>
```

## `<footer>`

Representa pie de página o de sección.

```html
<footer>
  <p>© 2026 Mi sitio</p>
</footer>
```

### Ejemplo de layout semántico completo

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Portal</title>
</head>
<body>
  <header>
    <h1>Portal de aprendizaje</h1>
    <nav>
      <a href="/">Inicio</a>
      <a href="/cursos">Cursos</a>
      <a href="/contacto">Contacto</a>
    </nav>
  </header>

  <main>
    <section>
      <h2>Cursos destacados</h2>
      <article>
        <h3>Curso de HTML</h3>
        <p>Aprende estructura, etiquetas y semántica.</p>
      </article>
    </section>

    <aside>
      <h2>Consejo</h2>
      <p>Practica cada etiqueta con ejemplos pequeños.</p>
    </aside>
  </main>

  <footer>
    <p>Todos los derechos reservados.</p>
  </footer>
</body>
</html>
```

---

## 12. Contenedores genéricos: `<div>` y `<span>`

## `<div>`

Es un contenedor genérico de bloque.

```html
<div class="tarjeta">
  <h2>Título</h2>
  <p>Contenido</p>
</div>
```

## `<span>`

Es un contenedor genérico en línea.

```html
<p>Estado: <span class="activo">activo</span></p>
```

### Cuándo usarlos

Úsalos cuando:

- no exista una etiqueta semántica mejor
- necesites agrupar contenido para estilos o scripts

### Mala práctica

Usar `div` para todo:

```html
<div>
  <div>Menú</div>
  <div>Contenido</div>
</div>
```

### Mejor práctica

```html
<header>
  <nav>Menú</nav>
</header>
<main>Contenido</main>
```

---

## 13. Tablas

Las tablas deben usarse para **datos tabulares**, no para maquetar toda la página.

## Estructura básica

```html
<table>
  <thead>
    <tr>
      <th>Nombre</th>
      <th>Edad</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Ana</td>
      <td>28</td>
    </tr>
    <tr>
      <td>Luis</td>
      <td>31</td>
    </tr>
  </tbody>
</table>
```

### Etiquetas clave

- `<table>`: tabla
- `<thead>`: cabecera
- `<tbody>`: cuerpo
- `<tr>`: fila
- `<th>`: celda de encabezado
- `<td>`: celda normal

## Título de tabla con `<caption>`

```html
<table>
  <caption>Usuarios registrados</caption>
  <tr>
    <th>Nombre</th>
    <th>Email</th>
  </tr>
  <tr>
    <td>Ana</td>
    <td>ana@example.com</td>
  </tr>
</table>
```

Esto mejora comprensión y accesibilidad.

---

## 14. Formularios y controles

Los formularios son una parte esencial de HTML.

## `<form>`

```html
<form action="/registro" method="post">
  <!-- campos -->
</form>
```

### Atributos comunes

- `action`: a dónde se envían los datos
- `method`: cómo se envían (`get` o `post`)

## `<label>` e `<input>`

```html
<form action="/login" method="post">
  <label for="email">Correo electrónico</label>
  <input id="email" name="email" type="email" required>

  <label for="password">Contraseña</label>
  <input id="password" name="password" type="password" required>

  <button type="submit">Entrar</button>
</form>
```

### Por qué `label` es importante

Porque:

- mejora accesibilidad
- amplía el área clicable
- deja claro qué campo corresponde a qué texto

## Tipos comunes de `input`

```html
<input type="text" name="nombre">
<input type="email" name="correo">
<input type="password" name="clave">
<input type="number" name="edad">
<input type="date" name="fecha">
<input type="checkbox" name="acepta">
<input type="radio" name="plan" value="pro">
<input type="file" name="archivo">
```

## Área de texto: `<textarea>`

```html
<label for="mensaje">Mensaje</label>
<textarea id="mensaje" name="mensaje" rows="4" cols="30"></textarea>
```

## Lista desplegable: `<select>` y `<option>`

```html
<label for="pais">País</label>
<select id="pais" name="pais">
  <option value="es">España</option>
  <option value="mx">México</option>
  <option value="ar">Argentina</option>
</select>
```

## Agrupar campos: `<fieldset>` y `<legend>`

```html
<fieldset>
  <legend>Preferencias</legend>

  <label>
    <input type="checkbox" name="newsletter">
    Recibir newsletter
  </label>
</fieldset>
```

Esto da más contexto semántico al formulario.

## Botones: `<button>`

```html
<button type="submit">Guardar</button>
<button type="reset">Limpiar</button>
<button type="button">Abrir modal</button>
```

Es mejor indicar el `type` explícitamente.

---

## 15. Etiquetas multimedia

## Audio

```html
<audio controls>
  <source src="audio.mp3" type="audio/mpeg">
  Tu navegador no soporta audio HTML5.
</audio>
```

## Video

```html
<video controls width="640">
  <source src="video.mp4" type="video/mp4">
  Tu navegador no soporta video HTML5.
</video>
```

## Seguimiento de subtítulos con `<track>`

```html
<video controls>
  <source src="clase.mp4" type="video/mp4">
  <track src="subtitulos-es.vtt" kind="subtitles" srclang="es" label="Español">
</video>
```

Esto es especialmente útil para accesibilidad.

---

## 16. Etiquetas semánticas de texto y código

## Citas

### `<blockquote>`

```html
<blockquote cite="https://example.com">
  La simplicidad es la máxima sofisticación.
</blockquote>
```

### `<q>`

```html
<p>Ella dijo: <q>HTML es la base de la web</q>.</p>
```

## Código

### `<code>`

```html
<p>Usa la etiqueta <code>&lt;section&gt;</code> para agrupar contenido relacionado.</p>
```

### `<pre>`

```html
<pre>
Línea 1
  Línea 2 con indentación
</pre>
```

`pre` conserva espacios y saltos de línea.

### Combinación `pre` + `code`

```html
<pre><code>const mensaje = 'Hola mundo';</code></pre>
```

## Abreviaturas y datos de tiempo

### `<abbr>`

```html
<p><abbr title="HyperText Markup Language">HTML</abbr> es esencial en frontend.</p>
```

### `<time>`

```html
<time datetime="2026-06-08">8 de junio de 2026</time>
```

---

## 17. Atributos globales importantes

Hay atributos que pueden usarse en muchos elementos.

## `id`

Identifica de forma única un elemento.

```html
<section id="contacto">
  <h2>Contacto</h2>
</section>
```

## `class`

Se usa para agrupar elementos con fines de estilo o comportamiento.

```html
<p class="destacado">Texto importante</p>
```

## `title`

Añade información adicional.

```html
<abbr title="Cascading Style Sheets">CSS</abbr>
```

## `hidden`

Oculta un elemento.

```html
<div hidden>Contenido oculto</div>
```

## `lang`

Indica el idioma.

```html
<p lang="en">Hello world</p>
```

## `data-*`

Permite guardar datos personalizados.

```html
<button data-id="42" data-role="delete">Eliminar</button>
```

---

## 18. Reglas de anidación y validez

Un HTML normativo no solo usa etiquetas correctas: también las **anida bien**.

### Ejemplo incorrecto

```html
<p>
  Texto inicial
  <div>Este div no debería ir dentro de un párrafo</div>
</p>
```

Un `<p>` no debe contener ciertos elementos de bloque como un `div`.

### Forma correcta

```html
<p>Texto inicial</p>
<div>Bloque separado</div>
```

### Otro ejemplo incorrecto

```html
<a href="/inicio">
  <button>Ir al inicio</button>
</a>
```

Normalmente no conviene anidar elementos interactivos dentro de otros elementos interactivos.

### Mejor solución

```html
<a href="/inicio" class="boton">Ir al inicio</a>
```

o:

```html
<button type="button" onclick="location.href='/inicio'">Ir al inicio</button>
```

según el comportamiento real que necesites.

---

## 19. HTML semántico vs HTML no semántico

## No semántico

```html
<div class="header">Cabecera</div>
<div class="menu">Menú</div>
<div class="content">Contenido</div>
<div class="footer">Pie</div>
```

## Semántico

```html
<header>Cabecera</header>
<nav>Menú</nav>
<main>Contenido</main>
<footer>Pie</footer>
```

### Por qué el segundo es mejor

- comunica mejor la intención del contenido
- facilita accesibilidad
- mejora mantenibilidad
- ayuda al SEO
- hace el código más fácil de leer

---

## 20. Accesibilidad básica en HTML

HTML bien escrito mejora muchísimo la accesibilidad incluso antes de añadir ARIA.

## Buenas prácticas clave

### 1. Usar encabezados en orden lógico

```html
<h1>Tienda online</h1>
<h2>Productos</h2>
<h3>Oferta destacada</h3>
```

### 2. Añadir `alt` a imágenes

```html
<img src="producto.jpg" alt="Zapatilla deportiva color azul">
```

### 3. Asociar `label` con campos

```html
<label for="nombre">Nombre</label>
<input id="nombre" name="nombre" type="text">
```

### 4. Usar botones para acciones y enlaces para navegación

#### Navegación

```html
<a href="/perfil">Ver perfil</a>
```

#### Acción

```html
<button type="button">Guardar cambios</button>
```

### 5. No depender solo del color

Si marcas errores solo con rojo, algunas personas no lo percibirán bien. Acompaña el color con texto o iconos.

```html
<p class="error">Error: el correo es obligatorio.</p>
```

---

## 21. ARIA: cuándo usarlo y cuándo no

La primera regla suele ser:

**si una etiqueta HTML nativa ya resuelve el problema, úsala antes que ARIA**.

### Mejor

```html
<button type="button">Abrir menú</button>
```

### Peor

```html
<div role="button" tabindex="0">Abrir menú</div>
```

El segundo caso obliga a recrear comportamiento accesible que el botón ya ofrece por defecto.

ARIA se usa cuando HTML nativo no basta, no como sustituto indiscriminado.

---

## 22. Etiquetas obsoletas o desaconsejadas

Algunas etiquetas existieron en el pasado pero hoy no deberían usarse en HTML moderno.

Ejemplos:

- `<font>`
- `<center>`
- `<big>`
- `<strike>`

### Ejemplo antiguo

```html
<center><font color="red">Texto</font></center>
```

### Forma moderna

```html
<p class="texto-destacado">Texto</p>
```

Y luego el estilo se controla con CSS.

---

## 23. Errores comunes al escribir HTML

## 1. Usar `div` para todo

Esto empobrece la semántica.

## 2. Omitir `alt` en imágenes

Perjudica accesibilidad.

## 3. Saltarse la jerarquía de encabezados

Complica la lectura estructural.

## 4. Usar tablas para maquetar layouts

Las tablas son para datos tabulares.

## 5. No cerrar correctamente la estructura del documento

Aunque el navegador a veces “corrige”, el código queda menos predecible.

## 6. Mezclar navegación y acciones

Usa:

- `<a>` para ir a otro recurso
- `<button>` para ejecutar acciones

## 7. Abusar de atributos inline

### Menos recomendable

```html
<p style="color: red;">Texto</p>
```

### Más mantenible

```html
<p class="error">Texto</p>
```

---

## 24. Ejemplo práctico de página HTML bien estructurada

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="Página de ejemplo con HTML semántico">
  <title>Ejemplo de HTML normativo</title>
</head>
<body>
  <header>
    <h1>Academia Web</h1>
    <nav>
      <a href="#cursos">Cursos</a>
      <a href="#contacto">Contacto</a>
    </nav>
  </header>

  <main>
    <section id="cursos">
      <h2>Cursos disponibles</h2>

      <article>
        <h3>HTML desde cero</h3>
        <p>Aprende estructura, etiquetas y semántica.</p>
      </article>

      <article>
        <h3>CSS moderno</h3>
        <p>Aprende diseño visual y responsive.</p>
      </article>
    </section>

    <section id="contacto">
      <h2>Contacto</h2>
      <form action="/contacto" method="post">
        <label for="nombre">Nombre</label>
        <input id="nombre" name="nombre" type="text" required>

        <label for="correo">Correo</label>
        <input id="correo" name="correo" type="email" required>

        <label for="mensaje">Mensaje</label>
        <textarea id="mensaje" name="mensaje" rows="5"></textarea>

        <button type="submit">Enviar</button>
      </form>
    </section>
  </main>

  <footer>
    <p>© 2026 Academia Web</p>
  </footer>
</body>
</html>
```

### Qué tiene bien este ejemplo

- `doctype` correcto
- idioma definido
- metadatos básicos
- uso de `header`, `nav`, `main`, `section`, `article`, `footer`
- formulario con `label`
- jerarquía clara de encabezados

---

## 25. Cómo pensar qué etiqueta usar

Hazte estas preguntas:

1. ¿Este contenido es un título?
2. ¿Es un párrafo?
3. ¿Es navegación?
4. ¿Es una acción o un enlace?
5. ¿Es contenido principal o complementario?
6. ¿Existe una etiqueta semántica específica antes de usar `div` o `span`?

### Ejemplo

Si quieres crear un menú principal:

- no uses solo un `div`
- piensa en `nav`

```html
<nav>
  <a href="/">Inicio</a>
  <a href="/blog">Blog</a>
</nav>
```

Si quieres una acción de envío:

```html
<button type="submit">Enviar</button>
```

Si quieres ir a otra página:

```html
<a href="/registro">Ir al registro</a>
```

---

## 26. Resumen rápido de etiquetas importantes

| Categoría | Etiquetas |
|---|---|
| Estructura base | `html`, `head`, `body`, `title`, `meta`, `link`, `script` |
| Texto | `h1-h6`, `p`, `strong`, `em`, `small`, `mark`, `sup`, `sub` |
| Agrupación | `div`, `span`, `section`, `article`, `aside` |
| Semántica layout | `header`, `nav`, `main`, `footer` |
| Listas | `ul`, `ol`, `li`, `dl`, `dt`, `dd` |
| Enlaces y medios | `a`, `img`, `figure`, `figcaption`, `iframe` |
| Tablas | `table`, `thead`, `tbody`, `tr`, `th`, `td`, `caption` |
| Formularios | `form`, `label`, `input`, `textarea`, `select`, `option`, `button`, `fieldset`, `legend` |
| Multimedia | `audio`, `video`, `source`, `track` |
| Código y citas | `code`, `pre`, `blockquote`, `q`, `abbr`, `time` |

---

## 27. Conclusión

Escribir **HTML normativo** significa crear documentos:

- válidos
- semánticos
- accesibles
- mantenibles
- fáciles de entender

No se trata solo de que “el navegador lo renderice”, sino de que el marcado exprese correctamente el significado del contenido.

Si dominas bien las etiquetas HTML y su uso semántico:

- tu código será más limpio
- tu CSS será más fácil de mantener
- tu JavaScript trabajará sobre una estructura más clara
- la accesibilidad y el SEO mejorarán desde la base

La mejor práctica general es esta:

**primero piensa en el significado del contenido y luego elige la etiqueta**.
