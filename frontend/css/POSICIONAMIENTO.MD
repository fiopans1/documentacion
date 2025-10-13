# 🎨 Guía de Posicionamiento CSS - Z-Index, Grid y Flexbox

## 📋 Índice

1. [Introducción al Posicionamiento](#1-introducción-al-posicionamiento)
2. [Position Property](#2-position-property)
3. [Z-Index y Stacking Context](#3-z-index-y-stacking-context)
4. [Flexbox](#4-flexbox)
5. [CSS Grid](#5-css-grid)
6. [Flexbox vs Grid](#6-flexbox-vs-grid)
7. [Layouts Comunes](#7-layouts-comunes)
8. [Responsive Design](#8-responsive-design)
9. [Mejores Prácticas](#9-mejores-prácticas)

---

## 1. Introducción al Posicionamiento

El posicionamiento CSS es fundamental para controlar cómo se ubican los elementos en una página web.

### Conceptos clave:
- **Document Flow**: Flujo natural del documento
- **Position**: Control de posicionamiento de elementos
- **Z-Index**: Profundidad y orden de apilamiento
- **Flexbox**: Layout unidimensional (fila o columna)
- **Grid**: Layout bidimensional (filas y columnas)

---

## 2. Position Property

### position: static (por defecto)

```css
.elemento {
    position: static;
    /* No se puede usar top, right, bottom, left, z-index */
}
```

### position: relative

```html
<div class="container">
    <div class="box relative">Relativo</div>
</div>
```

```css
.relative {
    position: relative;
    top: 20px;
    left: 30px;
    /* Se desplaza desde su posición original pero mantiene su espacio */
}
```

### position: absolute

```html
<div class="container">
    <div class="box absolute">Absoluto</div>
</div>
```

```css
.container {
    position: relative; /* Punto de referencia para el absolute */
    height: 300px;
    border: 2px solid #333;
}

.absolute {
    position: absolute;
    top: 10px;
    right: 10px;
    /* Posicionado relativo al ancestro positioned más cercano */
    /* Sale del flujo del documento */
}
```

### position: fixed

```css
.navbar {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    background: #333;
    /* Fijo respecto al viewport, no se mueve al hacer scroll */
}

.back-to-top {
    position: fixed;
    bottom: 20px;
    right: 20px;
}
```

### position: sticky

```css
.sidebar {
    position: sticky;
    top: 20px;
    /* Se comporta como relative hasta que alcanza el scroll threshold,
       luego se comporta como fixed */
}

/* Ejemplo práctico: encabezado de tabla pegajoso */
thead {
    position: sticky;
    top: 0;
    background: white;
    z-index: 10;
}
```

### Ejemplo completo de positions

```html
<div class="demo-container">
    <div class="box static">Static</div>
    <div class="box relative">Relative (top: 20px)</div>
    <div class="box absolute">Absolute (top: 0, right: 0)</div>
    <div class="box fixed">Fixed (bottom: 20px, right: 20px)</div>
</div>
```

```css
.demo-container {
    position: relative;
    height: 500px;
    border: 2px dashed #999;
    padding: 20px;
}

.box {
    padding: 15px;
    margin: 10px 0;
    background: #3498db;
    color: white;
}

.static {
    position: static;
    /* Posición normal */
}

.relative {
    position: relative;
    top: 20px;
    left: 20px;
    background: #2ecc71;
}

.absolute {
    position: absolute;
    top: 0;
    right: 0;
    background: #e74c3c;
}

.fixed {
    position: fixed;
    bottom: 20px;
    right: 20px;
    background: #f39c12;
}
```

---

## 3. Z-Index y Stacking Context

### Entendiendo z-index

```css
/* z-index solo funciona con position != static */

.layer-1 {
    position: relative;
    z-index: 1;
}

.layer-2 {
    position: relative;
    z-index: 2; /* Aparece por encima de layer-1 */
}

.layer-negative {
    position: relative;
    z-index: -1; /* Aparece debajo del contenido normal */
}
```

### Stacking Context

Un stacking context es un grupo 3D de elementos HTML.

**Crean stacking context:**
- `position: absolute/relative` con `z-index != auto`
- `position: fixed/sticky`
- Elementos hijos de flex/grid con `z-index != auto`
- `opacity` < 1
- `transform != none`
- `filter != none`
- `will-change`

```html
<div class="parent">
    <div class="child-1">
        <div class="grandchild">z-index: 999</div>
    </div>
    <div class="child-2">z-index: 2</div>
</div>
```

```css
.parent {
    position: relative;
}

.child-1 {
    position: relative;
    z-index: 1; /* Crea nuevo stacking context */
}

.grandchild {
    position: absolute;
    z-index: 999; /* Solo compite dentro de child-1 */
    /* NO aparecerá sobre child-2 aunque tenga z-index mayor */
}

.child-2 {
    position: relative;
    z-index: 2;
    /* Aparece sobre child-1 completo */
}
```

### Ejemplo práctico: Modal con overlay

```html
<div class="page-content">
    <h1>Contenido de la página</h1>
</div>

<div class="modal-overlay">
    <div class="modal">
        <h2>Modal Dialog</h2>
        <button class="close-btn">×</button>
    </div>
</div>
```

```css
.modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    z-index: 1000; /* Por encima del contenido */
    display: flex;
    justify-content: center;
    align-items: center;
}

.modal {
    position: relative;
    background: white;
    padding: 30px;
    border-radius: 8px;
    max-width: 500px;
    z-index: 1001; /* Por encima del overlay */
}

.close-btn {
    position: absolute;
    top: 10px;
    right: 10px;
    z-index: 1002; /* Por encima del modal */
}
```

### Sistema de capas organizado

```css
:root {
    --z-dropdown: 1000;
    --z-sticky: 1020;
    --z-fixed: 1030;
    --z-modal-backdrop: 1040;
    --z-modal: 1050;
    --z-popover: 1060;
    --z-tooltip: 1070;
}

.dropdown {
    z-index: var(--z-dropdown);
}

.modal {
    z-index: var(--z-modal);
}

.tooltip {
    z-index: var(--z-tooltip);
}
```

---

## 4. Flexbox

Flexbox es ideal para layouts unidimensionales (una fila o una columna).

### Conceptos básicos

```html
<div class="flex-container">
    <div class="flex-item">1</div>
    <div class="flex-item">2</div>
    <div class="flex-item">3</div>
</div>
```

```css
.flex-container {
    display: flex;
    /* Los hijos se convierten en flex items */
}
```

### Propiedades del contenedor (flex container)

#### flex-direction

```css
.container {
    display: flex;
    flex-direction: row; /* Por defecto, horizontal →  */
    /* flex-direction: row-reverse; ← */
    /* flex-direction: column; vertical ↓ */
    /* flex-direction: column-reverse; ↑ */
}
```

#### justify-content (eje principal)

```css
.container {
    display: flex;
    justify-content: flex-start; /* Por defecto, al inicio */
    /* justify-content: flex-end; al final */
    /* justify-content: center; centrado */
    /* justify-content: space-between; espacio entre items */
    /* justify-content: space-around; espacio alrededor */
    /* justify-content: space-evenly; espacio uniforme */
}
```

#### align-items (eje transversal)

```css
.container {
    display: flex;
    height: 300px;
    align-items: stretch; /* Por defecto, estira items */
    /* align-items: flex-start; al inicio */
    /* align-items: flex-end; al final */
    /* align-items: center; centrado */
    /* align-items: baseline; alineado por línea base */
}
```

#### flex-wrap

```css
.container {
    display: flex;
    flex-wrap: nowrap; /* Por defecto, no envuelve */
    /* flex-wrap: wrap; envuelve a nueva línea */
    /* flex-wrap: wrap-reverse; envuelve en reversa */
}
```

#### gap

```css
.container {
    display: flex;
    gap: 20px; /* Espacio entre items */
    /* gap: 20px 10px; fila columna */
    row-gap: 20px;
    column-gap: 10px;
}
```

### Propiedades de los items (flex items)

#### flex-grow

```css
.item {
    flex-grow: 0; /* Por defecto, no crece */
    /* flex-grow: 1; crece proporcionalmente */
}

.item-1 {
    flex-grow: 1; /* Toma 1 parte */
}

.item-2 {
    flex-grow: 2; /* Toma 2 partes (el doble) */
}
```

#### flex-shrink

```css
.item {
    flex-shrink: 1; /* Por defecto, puede encogerse */
    /* flex-shrink: 0; no se encoge */
}
```

#### flex-basis

```css
.item {
    flex-basis: auto; /* Por defecto */
    /* flex-basis: 200px; tamaño base */
    /* flex-basis: 30%; porcentaje */
}
```

#### flex (shorthand)

```css
.item {
    flex: 0 1 auto; /* grow shrink basis (por defecto) */
    /* flex: 1; equivale a flex: 1 1 0% */
    /* flex: auto; equivale a flex: 1 1 auto */
    /* flex: none; equivale a flex: 0 0 auto */
}
```

#### align-self

```css
.item {
    align-self: auto; /* Hereda de align-items */
    /* align-self: flex-start; */
    /* align-self: center; */
    /* align-self: flex-end; */
}
```

#### order

```css
.item-1 {
    order: 0; /* Por defecto */
}

.item-2 {
    order: -1; /* Aparece primero */
}

.item-3 {
    order: 1; /* Aparece último */
}
```

### Ejemplos prácticos con Flexbox

#### Centrar elemento perfectamente

```css
.container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}
```

#### Navbar responsive

```html
<nav class="navbar">
    <div class="logo">Logo</div>
    <ul class="nav-links">
        <li><a href="#">Home</a></li>
        <li><a href="#">About</a></li>
        <li><a href="#">Contact</a></li>
    </ul>
</nav>
```

```css
.navbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1rem 2rem;
    background: #333;
}

.nav-links {
    display: flex;
    gap: 2rem;
    list-style: none;
}
```

#### Card layout

```html
<div class="cards-container">
    <div class="card">Card 1</div>
    <div class="card">Card 2</div>
    <div class="card">Card 3</div>
</div>
```

```css
.cards-container {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
}

.card {
    flex: 1 1 300px; /* Crece, encoge, mínimo 300px */
    padding: 20px;
    background: #f0f0f0;
}
```

#### Holy Grail Layout con Flexbox

```html
<div class="holy-grail">
    <header>Header</header>
    <div class="content">
        <aside class="sidebar">Sidebar</aside>
        <main>Main Content</main>
        <aside class="sidebar">Sidebar</aside>
    </div>
    <footer>Footer</footer>
</div>
```

```css
.holy-grail {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
}

header, footer {
    padding: 20px;
    background: #333;
    color: white;
}

.content {
    display: flex;
    flex: 1;
}

.sidebar {
    flex: 0 0 200px;
    background: #f0f0f0;
    padding: 20px;
}

main {
    flex: 1;
    padding: 20px;
}
```

---

## 5. CSS Grid

CSS Grid es ideal para layouts bidimensionales (filas y columnas simultáneamente).

### Conceptos básicos

```html
<div class="grid-container">
    <div class="grid-item">1</div>
    <div class="grid-item">2</div>
    <div class="grid-item">3</div>
</div>
```

```css
.grid-container {
    display: grid;
    /* Los hijos se convierten en grid items */
}
```

### Definir columnas y filas

```css
.grid {
    display: grid;
    
    /* 3 columnas de tamaño fijo */
    grid-template-columns: 200px 200px 200px;
    
    /* 3 filas de tamaño fijo */
    grid-template-rows: 100px 100px 100px;
    
    /* Usando repeat */
    grid-template-columns: repeat(3, 200px);
    
    /* Mezcla de unidades */
    grid-template-columns: 200px 1fr 2fr;
    /* 1fr = fracción del espacio disponible */
    
    /* auto-fill y auto-fit */
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    /* Crea tantas columnas como quepan */
}
```

### gap

```css
.grid {
    display: grid;
    gap: 20px; /* Espacio entre celdas */
    /* gap: 20px 10px; fila columna */
    row-gap: 20px;
    column-gap: 10px;
}
```

### Posicionar items

```css
.item {
    /* Por número de línea */
    grid-column-start: 1;
    grid-column-end: 3; /* Ocupa columnas 1-2 */
    
    /* Shorthand */
    grid-column: 1 / 3;
    grid-row: 1 / 3;
    
    /* Usando span */
    grid-column: span 2; /* Ocupa 2 columnas */
    grid-row: span 3; /* Ocupa 3 filas */
    
    /* Shorthand completo */
    grid-area: 1 / 1 / 3 / 3;
    /* row-start / col-start / row-end / col-end */
}
```

### Grid Template Areas

```html
<div class="layout">
    <header>Header</header>
    <aside>Sidebar</aside>
    <main>Main</main>
    <footer>Footer</footer>
</div>
```

```css
.layout {
    display: grid;
    grid-template-columns: 200px 1fr;
    grid-template-rows: auto 1fr auto;
    grid-template-areas:
        "header header"
        "sidebar main"
        "footer footer";
    min-height: 100vh;
    gap: 10px;
}

header {
    grid-area: header;
}

aside {
    grid-area: sidebar;
}

main {
    grid-area: main;
}

footer {
    grid-area: footer;
}
```

### Alineación en Grid

```css
.grid {
    display: grid;
    
    /* Alinear items dentro de sus celdas */
    justify-items: start; /* stretch (default), start, end, center */
    align-items: start;
    
    /* Alinear el grid completo dentro del contenedor */
    justify-content: center;
    align-content: center;
}

.item {
    /* Alinear item individual */
    justify-self: end;
    align-self: center;
}
```

### Ejemplos prácticos con Grid

#### Gallery responsive

```html
<div class="gallery">
    <img src="1.jpg" alt="">
    <img src="2.jpg" alt="">
    <img src="3.jpg" alt="">
    <!-- más imágenes -->
</div>
```

```css
.gallery {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 20px;
}

.gallery img {
    width: 100%;
    height: 200px;
    object-fit: cover;
}
```

#### Dashboard layout

```html
<div class="dashboard">
    <header class="header">Header</header>
    <aside class="sidebar">Sidebar</aside>
    <main class="main">Main Content</main>
    <aside class="widgets">Widgets</aside>
    <footer class="footer">Footer</footer>
</div>
```

```css
.dashboard {
    display: grid;
    grid-template-columns: 200px 1fr 250px;
    grid-template-rows: 80px 1fr 60px;
    grid-template-areas:
        "header header header"
        "sidebar main widgets"
        "footer footer footer";
    min-height: 100vh;
    gap: 10px;
}

.header { grid-area: header; }
.sidebar { grid-area: sidebar; }
.main { grid-area: main; }
.widgets { grid-area: widgets; }
.footer { grid-area: footer; }
```

#### Masonry layout (tipo Pinterest)

```css
.masonry {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    grid-auto-rows: 10px; /* Filas pequeñas */
    gap: 20px;
}

.masonry-item {
    /* Cada item ocupa el espacio que necesita */
    grid-row-end: span 20; /* Ajustar según altura del contenido */
}

/* Para contenido dinámico, usar JavaScript para calcular el span */
```

#### Card grid con featured item

```html
<div class="card-grid">
    <div class="card featured">Featured</div>
    <div class="card">Card 1</div>
    <div class="card">Card 2</div>
    <div class="card">Card 3</div>
    <div class="card">Card 4</div>
</div>
```

```css
.card-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 20px;
}

.card {
    padding: 20px;
    background: #f0f0f0;
}

.featured {
    grid-column: span 2;
    grid-row: span 2;
    background: #3498db;
    color: white;
}
```

---

## 6. Flexbox vs Grid

### Cuándo usar Flexbox

✅ Layouts unidimensionales (fila o columna)
✅ Navegación
✅ Alineación de items
✅ Componentes pequeños
✅ Cuando el tamaño del contenido determina el layout

```css
/* Ejemplo: barra de navegación */
.navbar {
    display: flex;
    justify-content: space-between;
}
```

### Cuándo usar Grid

✅ Layouts bidimensionales (filas y columnas)
✅ Layouts de página completa
✅ Grids complejas
✅ Cuando el layout determina el tamaño del contenido

```css
/* Ejemplo: layout de página */
.page {
    display: grid;
    grid-template-areas:
        "header header"
        "sidebar main"
        "footer footer";
}
```

### Combinando Flexbox y Grid

```html
<div class="page-grid">
    <header class="header">
        <nav class="navbar-flex">
            <div class="logo">Logo</div>
            <ul class="nav-links">
                <li>Home</li>
                <li>About</li>
            </ul>
        </nav>
    </header>
    <main class="main-grid">
        <!-- contenido -->
    </main>
</div>
```

```css
/* Grid para el layout general */
.page-grid {
    display: grid;
    grid-template-rows: auto 1fr;
    min-height: 100vh;
}

/* Flexbox para el navbar */
.navbar-flex {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.nav-links {
    display: flex;
    gap: 2rem;
}

/* Grid para el contenido principal */
.main-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 20px;
}
```

---

## 7. Layouts Comunes

### Layout clásico de blog

```html
<div class="blog-layout">
    <header>Header</header>
    <div class="content">
        <main>
            <article>Post 1</article>
            <article>Post 2</article>
        </main>
        <aside>Sidebar</aside>
    </div>
    <footer>Footer</footer>
</div>
```

```css
.blog-layout {
    display: grid;
    grid-template-rows: auto 1fr auto;
    min-height: 100vh;
}

.content {
    display: grid;
    grid-template-columns: 1fr 300px;
    gap: 20px;
    padding: 20px;
}

@media (max-width: 768px) {
    .content {
        grid-template-columns: 1fr;
    }
}
```

### Layout de app móvil

```html
<div class="app-layout">
    <header>Top Bar</header>
    <main>Content</main>
    <nav class="bottom-nav">
        <button>Home</button>
        <button>Search</button>
        <button>Profile</button>
    </nav>
</div>
```

```css
.app-layout {
    display: flex;
    flex-direction: column;
    height: 100vh;
}

header {
    flex: 0 0 60px;
}

main {
    flex: 1;
    overflow-y: auto;
}

.bottom-nav {
    flex: 0 0 60px;
    display: flex;
    justify-content: space-around;
}
```

### Layout de e-commerce

```html
<div class="shop-layout">
    <header>Header</header>
    <div class="shop-content">
        <aside class="filters">Filtros</aside>
        <main class="products">
            <div class="product-grid">
                <div class="product">Product 1</div>
                <div class="product">Product 2</div>
                <!-- más productos -->
            </div>
        </main>
    </div>
</div>
```

```css
.shop-layout {
    display: grid;
    grid-template-rows: auto 1fr;
    min-height: 100vh;
}

.shop-content {
    display: grid;
    grid-template-columns: 250px 1fr;
    gap: 20px;
}

.product-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 20px;
}
```

---

## 8. Responsive Design

### Media Queries con Flexbox

```css
.container {
    display: flex;
    flex-wrap: wrap;
}

.item {
    flex: 1 1 300px;
}

@media (max-width: 768px) {
    .container {
        flex-direction: column;
    }
    
    .item {
        flex: 1 1 100%;
    }
}
```

### Media Queries con Grid

```css
.grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 20px;
}

@media (max-width: 1200px) {
    .grid {
        grid-template-columns: repeat(3, 1fr);
    }
}

@media (max-width: 768px) {
    .grid {
        grid-template-columns: repeat(2, 1fr);
    }
}

@media (max-width: 480px) {
    .grid {
        grid-template-columns: 1fr;
    }
}
```

### Auto-responsive sin media queries

```css
/* Flexbox responsive */
.flex-container {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
}

.flex-item {
    flex: 1 1 calc(33.333% - 20px);
    min-width: 250px;
}

/* Grid responsive */
.grid-container {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 20px;
}
```

---

## 9. Mejores Prácticas

### 1. Usa las herramientas correctas

```css
/* ✅ Usar Flexbox para componentes unidimensionales */
.navbar {
    display: flex;
    justify-content: space-between;
}

/* ✅ Usar Grid para layouts bidimensionales */
.page-layout {
    display: grid;
    grid-template-areas: "header" "main" "footer";
}
```

### 2. Mobile-first approach

```css
/* Comienza con mobile */
.container {
    display: flex;
    flex-direction: column;
}

/* Luego adapta para pantallas más grandes */
@media (min-width: 768px) {
    .container {
        flex-direction: row;
    }
}
```

### 3. Usa variables CSS

```css
:root {
    --gap-small: 10px;
    --gap-medium: 20px;
    --gap-large: 40px;
    --sidebar-width: 250px;
    --content-max-width: 1200px;
}

.grid {
    display: grid;
    gap: var(--gap-medium);
}
```

### 4. Evita valores mágicos

```css
/* ❌ Malo */
.item {
    width: 23.5%;
    margin-right: 2%;
}

/* ✅ Bueno */
.container {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 20px;
}
```

### 5. Considera la accesibilidad

```css
/* Mantén el orden lógico del HTML */
.container {
    display: flex;
}

/* Usa order solo cuando sea necesario */
.item-importante {
    order: -1; /* Aparece primero visualmente */
}

/* Asegúrate de que el orden visual coincida con el orden del DOM
   para lectores de pantalla */
```

### 6. Usa Grid Template Areas para claridad

```css
/* ✅ Más legible */
.layout {
    grid-template-areas:
        "header header header"
        "sidebar main aside"
        "footer footer footer";
}

/* vs múltiples grid-column/grid-row */
```

### 7. Performance

```css
/* Evita layouts innecesariamente complejos */
/* Usa min-content, max-content, fit-content cuando sea apropiado */
.item {
    width: fit-content;
}

/* Usa will-change con cuidado */
.animated-item {
    will-change: transform;
}
```

---

## 🎨 Recursos y Referencias

### DevTools para debugging

**Firefox DevTools** - Excelente inspector de Grid
**Chrome DevTools** - Buen inspector de Flexbox y Grid

### Generadores y Herramientas

- [Flexbox Froggy](https://flexboxfroggy.com/) - Juego para aprender Flexbox
- [Grid Garden](https://cssgridgarden.com/) - Juego para aprender Grid
- [CSS Grid Generator](https://cssgrid-generator.netlify.app/)
- [Flexbox Generator](https://loading.io/flexbox/)

### Patrones comunes

```css
/* Centrar perfectamente */
.center {
    display: grid;
    place-items: center;
}

/* Stack vertical con espacio */
.stack {
    display: flex;
    flex-direction: column;
    gap: 1rem;
}

/* Layout de 12 columnas */
.twelve-col {
    display: grid;
    grid-template-columns: repeat(12, 1fr);
}

/* Full-bleed layout */
.full-bleed {
    width: 100vw;
    margin-left: calc(50% - 50vw);
}
```

---

**¡Domina el posicionamiento CSS y crea layouts profesionales! 🎨**
