# Guía Completa de jQuery

## ¿Qué es jQuery?

**jQuery** es una librería de JavaScript diseñada para simplificar tareas comunes del frontend como:

- Selección y manipulación del DOM
- Manejo de eventos
- Animaciones y efectos
- Peticiones AJAX
- Compatibilidad entre navegadores

Durante muchos años fue la forma más rápida de añadir interactividad a una web. Aunque hoy JavaScript moderno cubre gran parte de sus funciones de forma nativa, **jQuery sigue siendo útil** en proyectos legacy, sitios con mucho código heredado, integraciones con plugins antiguos o equipos que necesitan una capa simple sobre el DOM.

---

## ¿Cuándo conviene usar jQuery hoy?

### Buenos escenarios

- Mantenimiento de proyectos antiguos
- Landing pages o sitios pequeños sin framework
- Integración con plugins ya existentes
- Proyectos donde se necesita compatibilidad con código histórico

### Cuándo no es la mejor opción

- Aplicaciones SPA modernas con React, Vue o Angular
- Proyectos nuevos donde JavaScript nativo ya cubre lo necesario
- Aplicaciones con arquitectura basada en componentes

---

## Instalación

### 1. Usando CDN

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Mi proyecto con jQuery</title>
</head>
<body>
  <script src="https://code.jquery.com/jquery-3.7.1.min.js"></script>
</body>
</html>
```

### 2. Usando npm

```bash
npm install jquery
```

Y luego:

```js
import $ from 'jquery';
```

### 3. Versión slim

Existe una versión **slim** de jQuery que excluye algunas funcionalidades como **AJAX** y ciertos efectos.

```html
<script src="https://code.jquery.com/jquery-3.7.1.slim.min.js"></script>
```

Úsala solo si sabes que no necesitas esas características.

---

## La idea central de jQuery

La base de jQuery es la función **`$()`**, que permite:

- Seleccionar elementos
- Encadenar acciones
- Ejecutar métodos utilitarios

```js
$('.boton').addClass('activo').text('Procesado');
```

Eso significa:

1. Buscar elementos con clase `boton`
2. Añadir la clase `activo`
3. Cambiar su texto

---

## Esperar a que el DOM esté listo

Una práctica clásica en jQuery es esperar a que el documento cargue antes de manipular elementos:

```js
$(document).ready(function () {
  console.log('DOM listo');
});
```

Forma corta:

```js
$(function () {
  console.log('DOM listo');
});
```

Esto evita intentar acceder a elementos que todavía no existen en el DOM.

---

## Selectores básicos

jQuery usa selectores muy parecidos a CSS.

### Selector por etiqueta

```js
$('p');
```

### Selector por clase

```js
$('.card');
```

### Selector por id

```js
$('#menu');
```

### Selector combinado

```js
$('ul li.activo');
```

### Selectores de atributos

```js
$('input[type="email"]');
$('a[target="_blank"]');
```

### Pseudoselectores útiles

```js
$(':checked');
$(':selected');
$(':disabled');
$(':visible');
$(':hidden');
```

---

## Recorrer el DOM

jQuery no solo selecciona, también facilita moverse entre nodos.

### Hijos y descendientes

```js
$('#contenedor').children();
$('#contenedor').find('.item');
```

### Padres y ancestros

```js
$('.item').parent();
$('.item').closest('.card');
```

### Hermanos

```js
$('.activo').siblings();
$('.activo').next();
$('.activo').prev();
```

### Filtrar selecciones

```js
$('.item').first();
$('.item').last();
$('.item').eq(2);
$('.item').not('.oculto');
```

---

## Manipulación de contenido

### Obtener o cambiar texto

```js
$('#titulo').text();
$('#titulo').text('Nuevo título');
```

### Obtener o cambiar HTML

```js
$('#contenido').html();
$('#contenido').html('<strong>Hola</strong>');
```

> Usa `.html()` con cuidado. Si insertas contenido no confiable puedes introducir vulnerabilidades XSS. Si solo quieres texto plano, usa `.text()`.

### Obtener o cambiar valores de formularios

```js
$('#nombre').val();
$('#nombre').val('Juan');
```

---

## Manipulación de atributos, propiedades y estilos

### Atributos

```js
$('img').attr('src');
$('img').attr('alt', 'Imagen de ejemplo');
```

### Propiedades

Útil especialmente con formularios:

```js
$('#acepto').prop('checked', true);
$('#boton').prop('disabled', false);
```

### Clases CSS

```js
$('#caja').addClass('activo');
$('#caja').removeClass('activo');
$('#caja').toggleClass('activo');
$('#caja').hasClass('activo');
```

### Estilos inline

```js
$('#caja').css('color', 'red');
$('#caja').css({
  backgroundColor: '#111',
  color: '#fff',
  padding: '1rem'
});
```

---

## Insertar, mover y eliminar elementos

### Insertar contenido

```js
$('#lista').append('<li>Nuevo al final</li>');
$('#lista').prepend('<li>Nuevo al inicio</li>');
$('#bloque').before('<p>Antes del bloque</p>');
$('#bloque').after('<p>Después del bloque</p>');
```

### Mover elementos

```js
$('#elemento').appendTo('#destino');
```

### Eliminar elementos

```js
$('#aviso').remove();
$('#contenedor').empty();
```

- **`remove()`** elimina el elemento seleccionado
- **`empty()`** elimina solo su contenido interno

---

## Iteración con `.each()`

```js
$('.item').each(function (index, element) {
  console.log(index, $(element).text());
});
```

También puedes usar `this` dentro del callback:

```js
$('.item').each(function () {
  $(this).addClass('procesado');
});
```

---

## Eventos

jQuery simplifica mucho el manejo de eventos.

### Click

```js
$('#boton').on('click', function () {
  alert('Has hecho click');
});
```

### Eventos comunes

```js
$('#input').on('focus', function () {});
$('#input').on('blur', function () {});
$('#formulario').on('submit', function (event) {
  event.preventDefault();
});
$('#menu').on('mouseenter', function () {});
$('#menu').on('mouseleave', function () {});
```

### Atajo con métodos cortos

```js
$('#boton').click(function () {
  console.log('click');
});
```

Aunque funcionan, hoy suele preferirse **`.on()`** por consistencia y flexibilidad.

---

## Delegación de eventos

La delegación permite escuchar eventos en elementos creados dinámicamente.

```js
$('#lista').on('click', 'li', function () {
  $(this).toggleClass('seleccionado');
});
```

Esto registra el evento en `#lista`, pero lo aplica a los `li` que coincidan, incluso si se agregan después.

Es una técnica muy importante cuando:

- Insertas elementos con AJAX
- Renderizas listas dinámicas
- Quieres evitar muchos listeners individuales

---

## Efectos y animaciones

jQuery incluye efectos sencillos listos para usar.

### Mostrar y ocultar

```js
$('#panel').hide();
$('#panel').show();
$('#panel').toggle();
```

### Con animación

```js
$('#panel').fadeIn();
$('#panel').fadeOut();
$('#panel').slideDown();
$('#panel').slideUp();
```

### Animación personalizada

```js
$('#caja').animate({
  left: '200px',
  opacity: 0.5
}, 400);
```

### Duraciones

```js
$('#panel').fadeIn(200);
$('#panel').fadeOut('slow');
```

Para interfaces modernas complejas suele ser mejor usar CSS transitions, CSS animations o librerías específicas, pero estos métodos siguen siendo útiles en soluciones rápidas.

---

## AJAX con jQuery

Una de las razones por las que jQuery fue tan popular fue su API sencilla para AJAX.

### Método `$.ajax()`

```js
$.ajax({
  url: '/api/usuarios',
  method: 'GET',
  dataType: 'json',
  success: function (respuesta) {
    console.log(respuesta);
  },
  error: function (xhr, status, error) {
    console.error(error);
  }
});
```

### Métodos abreviados

```js
$.get('/api/usuarios', function (data) {
  console.log(data);
});

$.post('/api/login', {
  email: 'test@test.com',
  password: '123456'
}, function (data) {
  console.log(data);
});
```

### Cargar HTML en un elemento

```js
$('#contenedor').load('/fragmento.html');
```

### Enviar formularios con serialización

```js
$('#formulario').on('submit', function (event) {
  event.preventDefault();

  $.ajax({
    url: '/api/contacto',
    method: 'POST',
    data: $(this).serialize(),
    success: function () {
      alert('Formulario enviado');
    }
  });
});
```

> En proyectos modernos, `fetch` suele ser la alternativa nativa. Aun así, jQuery AJAX sigue apareciendo mucho en sistemas existentes.

---

## Manejo de formularios

### Leer valores

```js
const nombre = $('#nombre').val();
const aceptado = $('#acepto').is(':checked');
```

### Escuchar cambios

```js
$('#pais').on('change', function () {
  console.log($(this).val());
});
```

### Validación simple

```js
$('#registro').on('submit', function (event) {
  const email = $('#email').val();

  if (!email) {
    event.preventDefault();
    $('#error').text('El email es obligatorio');
  }
});
```

---

## Métodos utilitarios frecuentes

### `$.trim()`

```js
const nombre = $.trim($('#nombre').val());
```

### `$.each()`

```js
$.each([1, 2, 3], function (index, valor) {
  console.log(index, valor);
});
```

### `$.map()`

```js
const dobles = $.map([1, 2, 3], function (n) {
  return n * 2;
});
```

### `$.extend()`

```js
const configuracion = $.extend({}, defaults, opcionesUsuario);
```

Sirve para combinar objetos, algo muy usado en plugins y configuraciones.

---

## Encadenamiento

Una de las fortalezas de jQuery es el **chaining**:

```js
$('#mensaje')
  .text('Guardado correctamente')
  .addClass('ok')
  .fadeIn(200);
```

Esto mejora la legibilidad cuando aplicas varias transformaciones al mismo elemento.

---

## Uso de `this` en callbacks

Dentro de muchos callbacks de jQuery, `this` hace referencia al elemento actual del DOM.

```js
$('.boton-eliminar').on('click', function () {
  $(this).closest('.fila').remove();
});
```

Si quieres usar métodos jQuery sobre `this`, normalmente lo envuelves con `$(this)`.

---

## Crear plugins en jQuery

jQuery fue muy adoptado por su ecosistema de plugins.

### Ejemplo básico

```js
$.fn.resaltar = function (color) {
  return this.css('backgroundColor', color || 'yellow');
};

$('.nota').resaltar('#ffe08a');
```

Puntos importantes:

- Los plugins suelen colgarse de `$.fn`
- Deben devolver `this` para permitir encadenamiento
- Normalmente aceptan opciones configurables

---

## Diferencias entre `.attr()`, `.prop()` y `.data()`

### `.attr()`

Trabaja con atributos HTML.

```js
$('#logo').attr('alt', 'Logo principal');
```

### `.prop()`

Trabaja con propiedades del DOM, muy útil en inputs y checkboxes.

```js
$('#acepto').prop('checked', true);
```

### `.data()`

Permite leer o almacenar datos asociados al elemento.

```html
<button id="boton" data-id="15">Editar</button>
```

```js
$('#boton').data('id'); // 15
$('#boton').data('estado', 'activo');
```

---

## Buenas prácticas con jQuery

1. **Usa selectores específicos pero simples**
   - Evita búsquedas excesivamente complejas si puedes cachear referencias.

2. **Cachea selecciones reutilizadas**

   ```js
   const $modal = $('#modal');
   $modal.addClass('visible');
   $modal.find('.titulo').text('Hola');
   ```

3. **Prefiere delegación de eventos en listas dinámicas**

4. **No abuses de `.html()`**
   - Usa `.text()` cuando el contenido provenga del usuario.

5. **Separa la lógica**
   - Aunque jQuery facilita scripts rápidos, conviene organizar funciones y módulos.

6. **Evita mezclar demasiada lógica de negocio con el DOM**

7. **Comprueba si realmente necesitas jQuery**
   - En proyectos nuevos, muchas tareas ya se resuelven con JavaScript moderno.

---

## Errores comunes

### 1. Ejecutar scripts antes de que el DOM exista

```js
$(function () {
  // correcto
});
```

### 2. Usar listeners directos en elementos dinámicos

Incorrecto:

```js
$('.item').click(function () {});
```

Si esos elementos se crean después, no funcionará.

Correcto:

```js
$('#lista').on('click', '.item', function () {});
```

### 3. Insertar HTML no sanitizado

```js
$('#salida').html(textoDelUsuario);
```

Si `textoDelUsuario` no es seguro, esto puede abrir la puerta a **XSS**.

### 4. Encadenar demasiadas manipulaciones sin estructura

Aunque jQuery facilita el chaining, sigue siendo importante mantener el código claro y modular.

---

## jQuery vs JavaScript moderno

### Selección de elementos

Con jQuery:

```js
$('.item');
```

Con JavaScript moderno:

```js
document.querySelectorAll('.item');
```

### Evento click

Con jQuery:

```js
$('#boton').on('click', function () {});
```

Con JavaScript moderno:

```js
document.getElementById('boton').addEventListener('click', function () {});
```

### AJAX

Con jQuery:

```js
$.get('/api/usuarios', function (data) {
  console.log(data);
});
```

Con JavaScript moderno:

```js
fetch('/api/usuarios')
  .then(response => response.json())
  .then(data => console.log(data));
```

Esto muestra por qué jQuery ha perdido protagonismo en proyectos nuevos, pero no elimina su valor en sistemas heredados.

---

## Ejemplo práctico completo

### HTML

```html
<input type="text" id="tarea" placeholder="Nueva tarea">
<button id="agregar">Agregar</button>
<ul id="lista-tareas"></ul>
```

### jQuery

```js
$(function () {
  $('#agregar').on('click', function () {
    const tarea = $.trim($('#tarea').val());

    if (!tarea) {
      return;
    }

    $('#lista-tareas').append(`
      <li>
        <span>${tarea}</span>
        <button class="eliminar">Eliminar</button>
      </li>
    `);

    $('#tarea').val('');
  });

  $('#lista-tareas').on('click', '.eliminar', function () {
    $(this).closest('li').remove();
  });
});
```

### Qué aplica este ejemplo

- Espera al DOM listo
- Lee valores de un input
- Inserta contenido dinámicamente
- Usa delegación de eventos
- Manipula el DOM con poco código

---

## Ecosistema relacionado

- **jQuery UI**: componentes visuales, interacciones y widgets
- **DataTables**: tablas avanzadas con paginación y búsqueda
- **Select2**: selects mejorados
- **Slick / Owl Carousel**: sliders y carruseles

Muchos proyectos heredados aún dependen de este ecosistema.

---

## Resumen

jQuery fue una pieza clave en la evolución del frontend porque simplificó el trabajo con el DOM, eventos y AJAX cuando los navegadores eran mucho menos consistentes. Hoy:

- **Sigue siendo útil** en proyectos legacy y scripts rápidos
- **No suele ser la primera opción** para aplicaciones modernas grandes
- **Sigue valiendo la pena entenderlo** porque aún aparece en muchas bases de código reales

Si trabajas con mantenimiento web, administración de paneles antiguos o plantillas heredadas, dominar jQuery sigue siendo una habilidad práctica.

---

## Conceptos que conviene recordar

- `$(selector)` para seleccionar elementos
- `$(document).ready()` o `$(function(){})` para esperar al DOM
- `.on()` para eventos
- `.append()`, `.remove()`, `.text()`, `.html()`, `.val()` para manipular contenido
- `.addClass()`, `.removeClass()`, `.toggleClass()` para estilos
- `$.ajax()`, `$.get()`, `$.post()` para peticiones
- Delegación de eventos para contenido dinámico
- Evitar XSS usando `.text()` cuando el contenido no sea confiable

---

## Recursos recomendados

- Documentación oficial: https://api.jquery.com/
- Descarga oficial: https://jquery.com/
- jQuery UI: https://jqueryui.com/
