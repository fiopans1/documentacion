# Guía Completa de CSS

## 1. ¿Qué es CSS?

**CSS (Cascading Style Sheets)** es el lenguaje que define la presentación visual de un documento HTML.

Con CSS puedes controlar:

- colores
- tipografía
- espaciado
- tamaños
- layouts
- animaciones
- diseño responsive

HTML define la estructura y el contenido; CSS define cómo se ve.

---

## 2. Cómo funciona CSS

Una regla CSS está formada por:

- **selector**: a qué elemento afecta
- **declaraciones**: qué propiedades se aplican

```css
selector {
  propiedad: valor;
}
```

Ejemplo:

```css
h1 {
  color: #1d4ed8;
  font-size: 2rem;
}
```

---

## 3. Formas de usar CSS

## CSS inline

```html
<p style="color: red;">Texto</p>
```

Útil para pruebas rápidas, pero no para mantener una aplicación.

## CSS interno

```html
<style>
  p {
    color: blue;
  }
</style>
```

Suele usarse poco en proyectos modernos.

## CSS externo

```html
<link rel="stylesheet" href="styles.css" />
```

Es la forma recomendada en casi todos los proyectos.

---

## 4. Cascada, herencia y especificidad

CSS no aplica reglas al azar. Decide qué estilo gana según varios criterios.

## Cascada

Influye el orden de aparición y el origen de la regla.

Si dos reglas tienen la misma prioridad, suele ganar la última.

```css
p {
  color: blue;
}

p {
  color: green;
}
```

Resultado: el texto será verde.

## Herencia

Algunas propiedades se heredan automáticamente, por ejemplo:

- `color`
- `font-family`
- `line-height`

Otras no, por ejemplo:

- `margin`
- `padding`
- `border`
- `width`

## Especificidad

Cuanto más específico es un selector, más fuerza tiene.

Orden simplificado de menor a mayor:

- selector universal `*`
- selectores de elemento (`p`, `div`)
- clases, atributos y pseudoclases (`.card`, `[type="text"]`, `:hover`)
- IDs (`#menu`)
- estilos inline
- `!important` como último recurso

Evita depender de `!important` salvo casos puntuales y muy justificados.

---

## 5. Selectores esenciales

## Selector universal

```css
* {
  box-sizing: border-box;
}
```

## Selector por etiqueta

```css
p {
  color: #334155;
}
```

## Selector por clase

```css
.alerta {
  color: #b91c1c;
}
```

## Selector por ID

```css
#header {
  background: #0f172a;
}
```

## Selector descendiente

```css
article p {
  line-height: 1.7;
}
```

## Hijo directo

```css
nav > ul {
  display: flex;
}
```

## Hermano adyacente

```css
h2 + p {
  margin-top: 0;
}
```

## Hermanos generales

```css
h2 ~ p {
  color: #475569;
}
```

## Atributos

```css
input[type="email"] {
  border-color: #60a5fa;
}
```

## Pseudoclases

```css
a:hover {
  color: #2563eb;
}

input:focus {
  outline: 2px solid #2563eb;
}

li:first-child {
  font-weight: 700;
}
```

## Pseudoelementos

```css
p::first-line {
  font-weight: 600;
}

.badge::before {
  content: "Nuevo: ";
}
```

## Selectores modernos útiles

```css
.card:not(.destacada) {
  opacity: 0.85;
}

section:has(img) {
  padding-bottom: 2rem;
}
```

> `:has()` es potente, pero conviene revisar compatibilidad antes de usarlo en proyectos críticos.

---

## 6. Unidades más importantes

## Absolutas

- `px`
- `cm`
- `mm`
- `in`
- `pt`

Se usan menos para layouts modernos, salvo casos concretos.

## Relativas

- `%`
- `em`
- `rem`
- `vw`
- `vh`
- `vmin`
- `vmax`
- `ch`

Recomendación práctica:

- usa `rem` para tipografía y espaciado escalable
- usa `%`, `fr`, `vw`, `clamp()` y `max-width` para responsive
- usa `px` cuando necesites precisión puntual

Ejemplo:

```css
html {
  font-size: 16px;
}

h1 {
  font-size: 2.5rem;
}

.container {
  width: min(90%, 1200px);
}
```

---

## 7. Colores en CSS

Formas comunes de definir color:

## Nombre

```css
color: red;
```

## Hexadecimal

```css
color: #ef4444;
```

## RGB y RGBA

```css
color: rgb(239, 68, 68);
background-color: rgba(15, 23, 42, 0.8);
```

## HSL y HSLA

```css
color: hsl(221 83% 53%);
background-color: hsla(221, 83%, 53%, 0.15);
```

Buenas prácticas:

- usa una paleta consistente
- evita contrastes pobres
- comprueba accesibilidad de color
- centraliza colores con variables CSS

---

## 8. Modelo de caja (Box Model)

Todo elemento se representa como una caja compuesta por:

- contenido
- padding
- borde
- margen

```css
.card {
  width: 320px;
  padding: 1rem;
  border: 1px solid #cbd5e1;
  margin: 1.5rem;
}
```

## `box-sizing`

Muy recomendable usar:

```css
*,
*::before,
*::after {
  box-sizing: border-box;
}
```

Con `border-box`, el ancho y alto incluyen padding y borde, lo que facilita el layout.

---

## 9. Propiedades de texto y tipografía

Las más usadas:

- `color`
- `font-family`
- `font-size`
- `font-weight`
- `font-style`
- `line-height`
- `letter-spacing`
- `text-align`
- `text-transform`
- `text-decoration`
- `text-wrap`

Ejemplo:

```css
body {
  font-family: Inter, Arial, sans-serif;
  color: #1e293b;
  line-height: 1.6;
}

h1,
h2,
h3 {
  line-height: 1.2;
}
```

Consejos:

- evita tamaños demasiado pequeños
- usa `line-height` entre `1.4` y `1.7` para texto largo
- limita anchos de lectura con `max-width`

---

## 10. Fondos, bordes y sombras

## Fondos

```css
.hero {
  background-color: #eff6ff;
  background-image: linear-gradient(to right, #dbeafe, #eff6ff);
  background-position: center;
  background-size: cover;
}
```

## Bordes

```css
.box {
  border: 1px solid #cbd5e1;
  border-radius: 12px;
}
```

## Sombras

```css
.card {
  box-shadow: 0 10px 30px rgba(15, 23, 42, 0.08);
}
```

Úsalas con moderación; demasiadas sombras hacen la interfaz más pesada.

---

## 11. `display` y flujo normal

Valores comunes de `display`:

- `block`
- `inline`
- `inline-block`
- `none`
- `flex`
- `grid`

```css
span {
  display: inline;
}

button {
  display: inline-block;
}

.modal {
  display: none;
}
```

`display: none` elimina el elemento del flujo visual. Si solo quieres ocultarlo visualmente en algunos casos, quizá necesites otra estrategia.

---

## 12. Tamaños y overflow

Propiedades frecuentes:

- `width`
- `height`
- `min-width`
- `min-height`
- `max-width`
- `max-height`
- `overflow`
- `overflow-x`
- `overflow-y`

```css
.container {
  width: 100%;
  max-width: 1200px;
  margin-inline: auto;
}

.code-block {
  overflow-x: auto;
}
```

Para imágenes:

```css
img {
  max-width: 100%;
  height: auto;
}
```

---

## 13. Posicionamiento

Valores de `position`:

- `static`
- `relative`
- `absolute`
- `fixed`
- `sticky`

```css
.badge-wrapper {
  position: relative;
}

.badge {
  position: absolute;
  top: -8px;
  right: -8px;
}
```

## `z-index`

Sirve para controlar superposición, pero solo funciona como esperas cuando el elemento participa en un contexto de apilamiento adecuado.

Usa `z-index` con una escala clara, por ejemplo:

- base: `1`
- header fijo: `10`
- dropdown: `100`
- modal: `1000`

---

## 14. Flexbox

Flexbox es ideal para layouts de **una dimensión**: fila o columna.

## Contenedor flex

```css
.toolbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
}
```

Propiedades principales del contenedor:

- `display: flex`
- `flex-direction`
- `justify-content`
- `align-items`
- `align-content`
- `gap`
- `flex-wrap`

Propiedades de los hijos:

- `flex-grow`
- `flex-shrink`
- `flex-basis`
- `flex`
- `align-self`
- `order`

Ejemplo útil:

```css
.layout {
  display: flex;
  flex-wrap: wrap;
  gap: 1.5rem;
}

.sidebar {
  flex: 1 1 240px;
}

.content {
  flex: 3 1 600px;
}
```

---

## 15. CSS Grid

Grid es ideal para layouts de **dos dimensiones**: filas y columnas.

```css
.grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 1rem;
}
```

Propiedades más útiles:

- `grid-template-columns`
- `grid-template-rows`
- `grid-template-areas`
- `gap`
- `column-gap`
- `row-gap`
- `justify-items`
- `align-items`
- `place-items`

En ítems:

- `grid-column`
- `grid-row`
- `grid-area`
- `justify-self`
- `align-self`

Ejemplo responsive:

```css
.cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 1rem;
}
```

---

## 16. Responsive design

CSS moderno debe adaptarse a distintos tamaños de pantalla.

## Media queries

```css
@media (max-width: 768px) {
  .sidebar {
    display: none;
  }
}
```

## Estrategia recomendada

- diseña mobile-first
- usa breakpoints por necesidad real, no por costumbre
- evita fijar demasiados anchos rígidos
- combina `max-width`, `%`, `rem`, `grid` y `flex`

## Funciones modernas útiles

```css
.title {
  font-size: clamp(1.8rem, 4vw, 3rem);
}

.wrapper {
  width: min(92%, 1200px);
}
```

---

## 17. Variables CSS (custom properties)

Sirven para centralizar valores reutilizables.

```css
:root {
  --color-primary: #2563eb;
  --color-text: #1e293b;
  --radius-md: 12px;
  --shadow-sm: 0 4px 16px rgba(15, 23, 42, 0.08);
}

.button {
  background-color: var(--color-primary);
  border-radius: var(--radius-md);
  box-shadow: var(--shadow-sm);
}
```

Ventajas:

- consistencia visual
- theming más sencillo
- menos duplicación
- mantenimiento más fácil

---

## 18. Transiciones, transforms y animaciones

## Transition

```css
.button {
  transition: background-color 0.2s ease, transform 0.2s ease;
}

.button:hover {
  transform: translateY(-2px);
}
```

## Transform

```css
.card:hover {
  transform: scale(1.02);
}
```

## Animation

```css
@keyframes fade-in {
  from {
    opacity: 0;
    transform: translateY(8px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.panel {
  animation: fade-in 0.3s ease;
}
```

Buenas prácticas:

- no abuses de animaciones largas
- prioriza propiedades eficientes como `transform` y `opacity`
- respeta `prefers-reduced-motion` si la interfaz usa mucho movimiento

```css
@media (prefers-reduced-motion: reduce) {
  * {
    animation: none !important;
    transition: none !important;
    scroll-behavior: auto !important;
  }
}
```

---

## 19. Formularios

Propiedades y patrones frecuentes:

```css
input,
select,
textarea,
button {
  font: inherit;
}

input,
textarea,
select {
  width: 100%;
  padding: 0.75rem 1rem;
  border: 1px solid #cbd5e1;
  border-radius: 0.75rem;
}

input:focus,
textarea:focus,
select:focus {
  outline: 2px solid #93c5fd;
  outline-offset: 2px;
  border-color: #2563eb;
}
```

Recomendaciones:

- mantén estados claros de `hover`, `focus`, `disabled` y error
- no elimines el focus visible sin sustituirlo por otro accesible
- usa labels siempre que sea posible

---

## 20. Tablas, imágenes y media

## Tablas

```css
table {
  width: 100%;
  border-collapse: collapse;
}

th,
td {
  padding: 0.75rem;
  border-bottom: 1px solid #e2e8f0;
  text-align: left;
}
```

## Imágenes y vídeo

```css
img,
picture,
video,
canvas,
svg {
  display: block;
  max-width: 100%;
}
```

Esto evita muchos problemas de desbordamiento.

---

## 21. Funciones CSS modernas muy útiles

## `calc()`

```css
.sidebar {
  height: calc(100vh - 64px);
}
```

## `min()`

```css
.container {
  width: min(100%, 1100px);
}
```

## `max()`

```css
.card {
  padding: max(1rem, 2vw);
}
```

## `clamp()`

```css
h1 {
  font-size: clamp(2rem, 5vw, 4rem);
}
```

---

## 22. Arquitectura y organización

A medida que crece un proyecto, organizar CSS importa mucho.

Estrategias comunes:

- CSS tradicional por archivos
- BEM (`.card__title`, `.card--active`)
- CSS Modules
- Sass/SCSS
- utility classes
- CSS-in-JS

## Recomendaciones prácticas

- usa nombres semánticos y consistentes
- evita selectores excesivamente largos
- agrupa estilos por componente o por dominio funcional
- centraliza tokens globales: colores, spacing, tipografía, sombras
- define una convención y mantenla

Ejemplo BEM:

```css
.card {}
.card__title {}
.card__content {}
.card--featured {}
```

---

## 23. Buenas prácticas

- usa `box-sizing: border-box`
- prioriza clases sobre IDs para estilos reutilizables
- evita `!important` salvo necesidad real
- diseña mobile-first
- usa variables CSS para valores repetidos
- comprueba contraste y accesibilidad
- evita profundidades de selector innecesarias
- no mezcles demasiadas responsabilidades en un solo archivo
- revisa consistencia de spacing y tipografía
- intenta que el layout dependa más de flex/grid que de hacks antiguos

---

## 24. Errores comunes

## 1. Abusar de `!important`

Genera hojas de estilo difíciles de mantener.

## 2. Usar tamaños fijos para todo

Rompe la adaptabilidad en pantallas distintas.

## 3. Ignorar el focus

Perjudica accesibilidad y navegación por teclado.

## 4. Selectores demasiado específicos

Luego cuesta sobrescribir estilos sin entrar en guerras de especificidad.

## 5. No separar estructura de presentación

Terminas con CSS repetido y difícil de escalar.

---

## 25. Accesibilidad en CSS

CSS también influye en accesibilidad.

Puntos clave:

- asegura contraste suficiente
- no bases toda la información solo en color
- conserva estados `focus` visibles
- respeta `prefers-reduced-motion`
- evita tamaños de letra muy pequeños
- mantén suficiente espacio clicable en botones y enlaces

Ejemplo de clase útil para ocultar visualmente pero mantener accesible:

```css
.visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}
```

---

## 26. Mini hoja de referencia

## Layout

- `display`
- `position`
- `top/right/bottom/left`
- `z-index`
- `overflow`
- `width/height`
- `max-width`
- `min-height`

## Espaciado

- `margin`
- `padding`
- `gap`

## Texto

- `font-size`
- `font-weight`
- `line-height`
- `text-align`
- `letter-spacing`

## Visual

- `background`
- `border`
- `border-radius`
- `box-shadow`
- `opacity`

## Movimiento

- `transition`
- `transform`
- `animation`

---

## 27. Ejemplo práctico pequeño

```css
:root {
  --color-bg: #f8fafc;
  --color-surface: #ffffff;
  --color-text: #0f172a;
  --color-primary: #2563eb;
  --radius: 16px;
  --shadow: 0 10px 30px rgba(15, 23, 42, 0.08);
}

* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: Inter, Arial, sans-serif;
  background: var(--color-bg);
  color: var(--color-text);
}

.container {
  width: min(92%, 1100px);
  margin-inline: auto;
}

.hero {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 2rem;
  align-items: center;
  min-height: 100vh;
}

.card {
  background: var(--color-surface);
  border-radius: var(--radius);
  box-shadow: var(--shadow);
  padding: 1.5rem;
}

.button {
  display: inline-block;
  padding: 0.85rem 1.2rem;
  border-radius: 999px;
  background: var(--color-primary);
  color: white;
  text-decoration: none;
  transition: transform 0.2s ease, opacity 0.2s ease;
}

.button:hover {
  transform: translateY(-2px);
}

.button:focus-visible {
  outline: 3px solid #93c5fd;
  outline-offset: 3px;
}
```

---

## 28. Conclusión

CSS no es solo "poner colores". Es la base del layout, la legibilidad, la adaptación responsive, la accesibilidad y la experiencia visual de una aplicación.

Si dominas bien estos bloques:

- cascada y especificidad
- selectores
- box model
- flexbox y grid
- responsive design
- variables y funciones modernas
- accesibilidad

podrás construir interfaces mucho más limpias, escalables y mantenibles.
