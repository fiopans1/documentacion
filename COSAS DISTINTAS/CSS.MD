# CURSO CSS:

## Introducción e Información General sobre CSS

### ¿Qué es CSS?

CSS (Cascading Style Sheets o Hojas de Estilo en Cascada) es un lenguaje de diseño utilizado para controlar la apariencia y el formato de un documento escrito en HTML. Mientras que HTML se encarga de la estructura y el contenido de una página web, CSS se centra en su presentación visual, como colores, fuentes, tamaños y diseño general.

---

### ¿Por qué usar CSS?

1. **Separación de contenido y diseño**:  
   CSS permite separar la estructura (HTML) del estilo visual, facilitando el mantenimiento del código y mejorando la accesibilidad.

2. **Reutilización de estilos**:  
   Los estilos definidos en CSS se pueden aplicar a múltiples páginas web, lo que reduce la redundancia.

3. **Mejor experiencia de usuario (UX)**:  
   CSS permite crear diseños modernos, responsivos y visualmente atractivos que mejoran la experiencia del usuario.

4. **Reducción del tamaño de archivos HTML**:  
   Al delegar el diseño a CSS, el código HTML se mantiene más limpio y ligero.

---

### Características Clave de CSS

- **Lenguaje declarativo**:  
  Los estilos se definen mediante reglas que indican cómo debe mostrarse un elemento.

- **Cascada**:  
  Las reglas se aplican siguiendo un orden de prioridad, basado en la especificidad, el origen del estilo y la importancia de las reglas (`!important`).

- **Herencia**:  
  Algunas propiedades, como `color` o `font-family`, se heredan automáticamente por los elementos hijos.

- **Diseño responsivo**:  
  CSS incluye herramientas como media queries para adaptar los estilos a diferentes dispositivos y tamaños de pantalla.

---

### Historia y Evolución de CSS

1. **CSS1 (1996)**:  
   La primera versión estándar, que introdujo conceptos básicos como fuentes, colores y alineación de texto.

2. **CSS2 (1998)**:  
   Añadió características como hojas de estilo para medios específicos (impresión, pantallas) y soporte para diseños más avanzados.

3. **CSS3 (2001 - Presente)**:  
   Dividido en módulos, incluye características modernas como:
   - Transiciones y animaciones.
   - Gradientes y sombras.
   - Flexbox y Grid para diseño de layouts.

4. **CSS4 (En progreso)**:  
   Aunque no hay una especificación oficial "CSS4", los módulos más recientes (como Selectors Level 4) representan las mejoras actuales.

---

### Cómo Funciona CSS

#### 1. **Reglas CSS**  
Una regla CSS consta de un **selector** y un **bloque de declaraciones**:

```css
selector {
  propiedad: valor;
}
```

Por ejemplo:
```css
h1 {
  color: blue;
  font-size: 24px;
}
```

- **Selector**: Especifica a qué elementos aplicar el estilo (en este caso, todos los `<h1>`).
- **Propiedad**: El aspecto a modificar (color, tamaño, etc.).
- **Valor**: La configuración específica de la propiedad.

---

#### 2. **Métodos de Inclusión de CSS**

- **CSS Inline**  
  Se define directamente en el atributo `style` del elemento HTML.

  ```html
  <p style="color: red;">Texto en rojo</p>
  ```

- **CSS Interno**  
  Se escribe dentro de una etiqueta `<style>` en el archivo HTML.

  ```html
  <style>
    p {
      color: blue;
    }
  </style>
  ```

- **CSS Externo**  
  Se escribe en un archivo `.css` separado y se enlaza con el HTML.

  ```html
  <link rel="stylesheet" href="estilos.css">
  ```

  En el archivo `estilos.css`:
  ```css
  p {
    color: green;
  }
  ```

---

### Ventajas de CSS Externo

1. **Facilidad de mantenimiento**:  
   Puedes cambiar los estilos de toda una web editando un solo archivo.

2. **Reutilización**:  
   Un archivo CSS puede ser usado por múltiples páginas.

3. **Cargado más rápido**:  
   Los navegadores pueden almacenar en caché los archivos CSS, reduciendo tiempos de carga.

---

### ¿Qué puede hacer CSS?

1. **Estilizar texto y elementos**:  
   Colores, fuentes, alineaciones, espaciados, etc.
   ```css
   p {
     font-family: Arial, sans-serif;
     text-align: center;
   }
   ```

2. **Diseñar layouts**:  
   Posicionamiento, alineación y distribución de elementos en la página. Ejemplo con **Flexbox**:
   ```css
   .contenedor {
     display: flex;
     justify-content: space-between;
   }
   ```

3. **Añadir animaciones y transiciones**:  
   Efectos visuales para mejorar la experiencia del usuario.
   ```css
   button {
     transition: background-color 0.3s;
   }
   button:hover {
     background-color: lightblue;
   }
   ```

4. **Diseños responsivos**:  
   Usar media queries para adaptar el diseño a diferentes pantallas:
   ```css
   @media (max-width: 768px) {
     body {
       font-size: 14px;
     }
   }
   ```

---

### Herramientas y Frameworks relacionados con CSS

- **Preprocesadores**: Sass, Less.
- **Frameworks**: Bootstrap, Tailwind CSS.
- **Postprocesadores**: PostCSS.

---
---

## Guía Completa sobre Selectores en CSS

CSS (Cascading Style Sheets) utiliza **selectores** para aplicar estilos a elementos HTML. Los selectores son una parte fundamental de CSS, ya que determinan a qué elementos se aplican las reglas de estilo. Esta guía cubre los selectores más importantes, desde básicos hasta avanzados.

---

### ## Selectores Básicos

#### 1. Selector Universal (`*`)
Selecciona **todos los elementos** del documento.

```css
* {
  margin: 0;
  padding: 0;
}
```

---

#### 2. Selector de Tipo (Tag Selector)
Selecciona todos los elementos de un tipo específico (por etiqueta HTML).

```css
p {
  color: blue;
}
```

---

#### 3. Selector de Clase (`.nombre-clase`)
Selecciona todos los elementos que tengan una clase específica.

```css
.introduccion {
  font-size: 18px;
}
```

En HTML:
```html
<p class="introduccion">Texto con estilo de clase.</p>
```

---

#### 4. Selector de ID (`#id`)
Selecciona un único elemento que tenga el atributo `id` especificado.

```css
#principal {
  background-color: lightgray;
}
```

En HTML:
```html
<div id="principal">Contenido principal</div>
```

---

### ## Selectores Combinadores

#### 5. Selector Descendiente (Espacio)
Selecciona los elementos dentro de un contenedor (descendientes).

```css
div p {
  color: green;
}
```

Esto aplicará el estilo a todos los `<p>` dentro de un `<div>`.

---

#### 6. Selector Hijo (`>`)
Selecciona solo los **hijos directos** de un elemento.

```css
div > p {
  font-weight: bold;
}
```

---

#### 7. Selector Hermano Adyacente (`+`)
Selecciona el **primer hermano inmediato** que sigue a un elemento.

```css
h1 + p {
  font-style: italic;
}
```

---

#### 8. Selector General de Hermanos (`~`)
Selecciona todos los hermanos de un elemento que compartan el mismo padre.

```css
h1 ~ p {
  color: red;
}
```

---

### ## Selectores de Atributo

#### 9. Selector Básico de Atributos (`[atributo]`)
Selecciona elementos que tengan un atributo específico.

```css
[input] {
  border: 1px solid black;
}
```

---

#### 10. Selector de Valor Exacto (`[atributo="valor"]`)
Selecciona elementos cuyo atributo tenga un valor exacto.

```css
[type="text"] {
  background-color: yellow;
}
```

---

#### 11. Selector de Subcadena Inicial (`[atributo^="valor"]`)
Selecciona elementos cuyo atributo **empiece** con el valor especificado.

```css
[href^="https"] {
  color: green;
}
```

---

#### 12. Selector de Subcadena Final (`[atributo$="valor"]`)
Selecciona elementos cuyo atributo **termine** con el valor especificado.

```css
[href$=".pdf"] {
  text-decoration: underline;
}
```

---

#### 13. Selector de Subcadena Contenida (`[atributo*="valor"]`)
Selecciona elementos cuyo atributo **contenga** el valor especificado.

```css
[class*="btn"] {
  padding: 10px;
}
```

---

### ## Pseudoclases

#### 14. Estado Hover (`:hover`)
Aplica estilos cuando el usuario pasa el cursor sobre un elemento.

```css
a:hover {
  color: orange;
}
```

---

#### 15. Selector de Primer Hijo (`:first-child`)
Selecciona el primer hijo de un elemento.

```css
li:first-child {
  font-weight: bold;
}
```

---

#### 16. Selector de Último Hijo (`:last-child`)
Selecciona el último hijo de un elemento.

```css
li:last-child {
  text-decoration: underline;
}
```

---

#### 17. Selector de N-ésimo Hijo (`:nth-child(n)`)
Selecciona el hijo número `n` de un elemento.

```css
li:nth-child(2) {
  color: red;
}
```

Usa fórmulas como `odd` (impares) o `even` (pares):
```css
li:nth-child(odd) {
  background-color: lightblue;
}
```

---

#### 18. Selector de Primer Tipo (`:first-of-type`)
Selecciona el primer hijo de un tipo específico.

```css
p:first-of-type {
  font-size: 20px;
}
```

---

### ## Pseudoelementos

#### 19. `::before` y `::after`
Permiten agregar contenido antes o después del contenido de un elemento.

```css
p::before {
  content: "Nota: ";
  font-weight: bold;
}
```

---

#### 20. Selección de la Primera Línea (`::first-line`)
Aplica estilos solo a la primera línea del texto.

```css
p::first-line {
  color: blue;
}
```

---

### ## Selectores Avanzados

#### 21. Selector Negación (`:not(selector)`)
Selecciona elementos que **no coincidan** con el selector especificado.

```css
p:not(.importante) {
  color: gray;
}
```

---

#### 22. Selector de Estado (`:checked`, `:disabled`, `:focus`)
Aplica estilos basados en el estado de los elementos.

```css
input:checked {
  border: 2px solid green;
}
```

---

### ## Combinaciones de Selectores

Puedes combinar selectores para lograr reglas más precisas. Ejemplo:

```css
div > p.highlight:hover {
  background-color: yellow;
}
```

---

### ## Consejos y Mejores Prácticas

1. **Especificidad**: Asegúrate de entender cómo funciona la especificidad para evitar conflictos entre estilos.
2. **Uso de Clases**: Prefiere usar clases en lugar de selectores de tipo para mantener la escalabilidad.
3. **Mantenimiento**: Evita selectores excesivamente complejos; mantén el CSS legible.

### IMPORTANTE, LO QUE ESCRIBAMOS AL FINAL DEL CODIGO DE CSS VA A TENER MAS PRIORIDAD


---
---

## Propiedades Más Importantes y Usadas en CSS

Las propiedades de CSS son los atributos que permiten controlar la apariencia y el diseño de los elementos en una página web. A continuación, se presenta un desglose de las propiedades más importantes y utilizadas en CSS, organizadas por categorías.

---

### ## Propiedades de Texto

1. **`color`**  
   Define el color del texto.  
   ```css
   p {
     color: red;
   }
   ```

2. **`font-size`**  
   Establece el tamaño de la fuente.  
   ```css
   h1 {
     font-size: 24px;
   }
   ```

3. **`font-family`**  
   Define el tipo de fuente a utilizar, con fuentes alternativas como respaldo.  
   ```css
   body {
     font-family: Arial, sans-serif;
   }
   ```

4. **`font-weight`**  
   Controla el grosor del texto.  
   ```css
   h2 {
     font-weight: bold;
   }
   ```

5. **`line-height`**  
   Ajusta el espacio entre líneas de texto.  
   ```css
   p {
     line-height: 1.5;
   }
   ```

6. **`text-align`**  
   Alinea el texto dentro de su contenedor. Valores: `left`, `center`, `right`, `justify`.  
   ```css
   h3 {
     text-align: center;
   }
   ```

7. **`text-decoration`**  
   Añade o elimina decoraciones como subrayados.  
   ```css
   a {
     text-decoration: none;
   }
   ```

8. **`text-transform`**  
   Controla la capitalización del texto. Valores: `uppercase`, `lowercase`, `capitalize`.  
   ```css
   p {
     text-transform: uppercase;
   }
   ```

---

### ## Propiedades de Fondo

1. **`background-color`**  
   Define el color de fondo del elemento.  
   ```css
   div {
     background-color: lightblue;
   }
   ```

2. **`background-image`**  
   Aplica una imagen como fondo.  
   ```css
   body {
     background-image: url('fondo.jpg');
   }
   ```

3. **`background-size`**  
   Controla el tamaño de la imagen de fondo. Valores: `cover`, `contain`, dimensiones específicas.  
   ```css
   div {
     background-size: cover;
   }
   ```

4. **`background-position`**  
   Establece la posición de la imagen de fondo.  
   ```css
   div {
     background-position: center;
   }
   ```

5. **`background-repeat`**  
   Controla si la imagen de fondo se repite.  
   ```css
   div {
     background-repeat: no-repeat;
   }
   ```

---

### ## Propiedades de Espaciado

1. **`margin`**  
   Define el espacio exterior de un elemento (margen).  
   ```css
   h1 {
     margin: 20px;
   }
   ```

2. **`padding`**  
   Define el espacio interior entre el contenido y el borde del elemento.  
   ```css
   p {
     padding: 10px;
   }
   ```

3. **`border`**  
   Establece un borde alrededor del elemento.  
   ```css
   div {
     border: 2px solid black;
   }
   ```

4. **`box-sizing`**  
   Controla cómo se calculan los tamaños de ancho y alto del elemento.  
   ```css
   div {
     box-sizing: border-box;
   }
   ```

---

### ## Propiedades de Diseño y Posicionamiento

1. **`width` y `height`**  
   Definen el ancho y alto de un elemento.  
   ```css
   img {
     width: 300px;
     height: auto;
   }
   ```

2. **`max-width` y `max-height`**  
   Establecen límites máximos para el tamaño del elemento.  
   ```css
   img {
     max-width: 100%;
   }
   ```

3. **`position`**  
   Controla la posición de un elemento. Valores: `static`, `relative`, `absolute`, `fixed`, `sticky`.  
   ```css
   div {
     position: absolute;
     top: 50px;
     left: 100px;
   }
   ```

4. **`z-index`**  
   Define el orden en la pila de elementos (profundidad).  
   ```css
   div {
     z-index: 10;
   }
   ```

5. **`display`**  
   Especifica cómo se presenta un elemento. Valores comunes: `block`, `inline`, `flex`, `grid`, `none`.  
   ```css
   div {
     display: flex;
   }
   ```

6. **`flex` y `grid`**  
   Utilizados para diseños modernos.  
   ```css
   .contenedor {
     display: flex;
     justify-content: space-between;
   }
   ```

---

### ## Propiedades de Bordes y Sombras

1. **`border-radius`**  
   Crea bordes redondeados.  
   ```css
   img {
     border-radius: 50%;
   }
   ```

2. **`box-shadow`**  
   Añade una sombra al elemento.  
   ```css
   div {
     box-shadow: 2px 4px 10px rgba(0, 0, 0, 0.5);
   }
   ```

3. **`outline`**  
   Añade un contorno que no afecta al tamaño del elemento.  
   ```css
   button {
     outline: 2px solid blue;
   }
   ```

---

### ## Propiedades de Transiciones y Animaciones

1. **`transition`**  
   Controla las transiciones entre estados.  
   ```css
   button {
     transition: background-color 0.3s ease;
   }
   ```

2. **`animation`**  
   Define animaciones avanzadas.  
   ```css
   @keyframes mover {
     from {
       transform: translateX(0);
     }
     to {
       transform: translateX(100px);
     }
   }
   div {
     animation: mover 2s infinite;
   }
   ```

---

### ## Propiedades Responsivas

1. **`media queries`**  
   Aplica estilos basados en el tamaño del dispositivo.  
   ```css
   @media (max-width: 768px) {
     body {
       font-size: 14px;
     }
   }
   ```

2. **`object-fit`**  
   Ajusta cómo una imagen o vídeo se ajusta a su contenedor.  
   ```css
   img {
     object-fit: cover;
   }
   ```

---



## Guía Básica de CSS: Medidas y Colores

### Medidas en CSS

En CSS, las medidas se utilizan para definir el tamaño de elementos, como anchos, altos, márgenes, paddings, fuentes, entre otros. Estas se dividen en dos grandes categorías: **absolutas** y **relativas**.

#### 1. **Medidas Absolutas**
Son valores fijos que no dependen del tamaño del contenedor o de la pantalla.

- **px (píxeles):** Unidad básica en pantallas. Ejemplo: `width: 200px;`
- **cm (centímetros):** Útil para documentos impresos. Ejemplo: `width: 10cm;`
- **mm (milímetros):** Similar al anterior pero más pequeño. Ejemplo: `width: 15mm;`
- **in (pulgadas):** 1 pulgada equivale a 2.54 cm. Ejemplo: `width: 2in;`
- **pt (puntos):** Usado en tipografía. 1 pt = 1/72 de pulgada. Ejemplo: `font-size: 12pt;`
- **pc (picas):** 1 pc = 12 pt. Ejemplo: `width: 2pc;`

#### 2. **Medidas Relativas**
Se adaptan al contexto del elemento o pantalla, siendo más flexibles.

- **em:** Relativa al tamaño de la fuente del elemento padre. Ejemplo: `font-size: 2em;` (el doble del tamaño base).
- **rem:** Relativa al tamaño de la fuente del elemento raíz (normalmente `<html>`). Ejemplo: `font-size: 1.5rem;`
- **% (porcentaje):** Relativa al tamaño del contenedor padre. Ejemplo: `width: 50%;`
- **vw (viewport width):** Relativa al ancho del viewport. 1vw = 1% del ancho. Ejemplo: `width: 80vw;`
- **vh (viewport height):** Relativa a la altura del viewport. Ejemplo: `height: 100vh;`
- **vmin/vmax:** Relativa al menor o mayor tamaño entre el ancho y alto del viewport. Ejemplo: `font-size: 10vmin;`
- **ch:** Relativa al ancho del carácter "0" de la fuente actual. Ejemplo: `width: 20ch;`

---

### Colores en CSS

Los colores se definen en CSS usando distintos formatos. Estos pueden aplicarse a fondos, textos, bordes, sombras, etc.

#### 1. **Colores por Nombre**
CSS admite 140 nombres de colores predefinidos. Ejemplo:  
```css
color: red;
background-color: lightblue;
```

#### 2. **Hexadecimal**
Formato común que usa códigos hexadecimales para definir colores.  
- Sintaxis: `#RRGGBB` (rojo, verde, azul).  
  Ejemplo:  
  ```css
  color: #ff0000; /* Rojo */
  background-color: #00ff00; /* Verde */
  ```

#### 3. **RGB**
Permite definir colores especificando valores de rojo, verde y azul en un rango de 0 a 255.  
- Sintaxis: `rgb(r, g, b)`  
  Ejemplo:  
  ```css
  color: rgb(255, 0, 0); /* Rojo */
  background-color: rgb(0, 255, 0); /* Verde */
  ```

#### 4. **RGBA**
Es similar a `rgb` pero incluye un canal alfa para la transparencia (valor de 0 a 1).  
- Sintaxis: `rgba(r, g, b, a)`  
  Ejemplo:  
  ```css
  background-color: rgba(0, 0, 255, 0.5); /* Azul con 50% de opacidad */
  ```

#### 5. **HSL**
Representa el color según tono (hue), saturación (saturation) y luminosidad (lightness).  
- Sintaxis: `hsl(h, s%, l%)`  
  Ejemplo:  
  ```css
  color: hsl(0, 100%, 50%); /* Rojo */
  background-color: hsl(120, 100%, 25%); /* Verde oscuro */
  ```

#### 6. **HSLA**
Extiende HSL con un canal alfa para la transparencia.  
- Sintaxis: `hsla(h, s%, l%, a)`  
  Ejemplo:  
  ```css
  background-color: hsla(240, 100%, 50%, 0.3); /* Azul con 30% de opacidad */
  ```

---

### Ejemplo Combinado de Medidas y Colores

```css
div {
  width: 50%; /* Medida relativa */
  height: 200px; /* Medida absoluta */
  background-color: rgba(255, 0, 0, 0.5); /* Rojo semitransparente */
  border: 2px solid #000; /* Negro */
  font-size: 1.5rem; /* Relativa al tamaño base */
  color: hsl(240, 100%, 90%); /* Azul claro */
}
```
---

## Miniguía de CSS: Box Model y Overflow

### Box Model en CSS

El **box model** es el modelo de caja en CSS, que define cómo se estructura y se calcula el espacio ocupado por un elemento HTML. Cada elemento es una "caja" compuesta por cuatro áreas principales:

1. **Content (Contenido):**  
   El área donde se muestra el contenido, como texto, imágenes, etc.  
   - Se define con propiedades como `width` y `height`.  
   Ejemplo:  
   ```css
   div {
     width: 200px;
     height: 100px;
   }
   ```

2. **Padding (Relleno):**  
   Espacio entre el contenido y el borde de la caja.  
   - Propiedades: `padding`, `padding-top`, `padding-right`, `padding-bottom`, `padding-left`.  
   Ejemplo:  
   ```css
   div {
     padding: 20px; /* Relleno uniforme */
   }
   ```

3. **Border (Borde):**  
   Contorno que rodea el padding.  
   - Propiedades: `border`, `border-width`, `border-style`, `border-color`.  
   Ejemplo:  
   ```css
   div {
     border: 2px solid black; /* Borde sólido negro */
   }
   ```

4. **Margin (Margen):**  
   Espacio exterior que separa la caja de otros elementos.  
   - Propiedades: `margin`, `margin-top`, `margin-right`, `margin-bottom`, `margin-left`.  
   Ejemplo:  
   ```css
   div {
     margin: 10px; /* Margen uniforme */
   }
   ```

#### Visualización del Box Model

```css
div {
  width: 200px;
  height: 100px;
  padding: 10px;
  border: 5px solid blue;
  margin: 20px;
  background-color: lightgray;
}
```

Este ejemplo genera un cuadro con un tamaño total de:  
- **Ancho total:** `width + padding + border + margin` → `200 + 10*2 + 5*2 + 20*2 = 270px`.  
- **Alto total:** `height + padding + border + margin` → `100 + 10*2 + 5*2 + 20*2 = 170px`.

#### Propiedad `box-sizing`
Controla si `width` y `height` incluyen el padding y el borde.  
- **`content-box` (por defecto):** Solo incluye el contenido.  
- **`border-box`:** Incluye contenido, padding y borde.  

Ejemplo:  
```css
div {
  box-sizing: border-box;
}
```

---

### Propiedad `overflow`

La propiedad `overflow` controla qué ocurre cuando el contenido de un elemento se desborda del área asignada.

#### Valores principales:

1. **`visible` (por defecto):**  
   El contenido que se desborda es visible fuera de la caja.  
   Ejemplo:  
   ```css
   div {
     width: 200px;
     height: 100px;
     overflow: visible;
   }
   ```

2. **`hidden`:**  
   El contenido que se desborda es recortado.  
   Ejemplo:  
   ```css
   div {
     width: 200px;
     height: 100px;
     overflow: hidden;
   }
   ```

3. **`scroll`:**  
   Aparecen barras de desplazamiento, incluso si no es necesario.  
   Ejemplo:  
   ```css
   div {
     width: 200px;
     height: 100px;
     overflow: scroll;
   }
   ```

4. **`auto`:**  
   Las barras de desplazamiento aparecen solo si el contenido se desborda.  
   Ejemplo:  
   ```css
   div {
     width: 200px;
     height: 100px;
     overflow: auto;
   }
   ```

---

#### Ejemplo Práctico

```html
<div style="
  width: 300px; 
  height: 150px; 
  border: 2px solid black; 
  overflow: auto;">
  <p>
    Este texto es demasiado largo para caber en el área asignada, por lo que deberías ver barras de desplazamiento si el contenido se desborda.
  </p>
</div>
```
---

## Miniguía de CSS: Propiedad `display`

La propiedad `display` define cómo un elemento se comporta visualmente en el flujo del documento, es decir, cómo se posiciona y cómo interactúan sus hijos.

### Valores Principales de `display`

1. **`block`:**  
   - El elemento se comporta como un bloque, ocupando toda la anchura disponible.  
   - Comienza en una nueva línea.  
   Ejemplo:  
   ```css
   div {
     display: block;
   }
   ```

2. **`inline`:**  
   - El elemento ocupa solo el espacio necesario.  
   - No inicia en una nueva línea.  
   Ejemplo:  
   ```css
   span {
     display: inline;
   }
   ```

3. **`inline-block`:**  
   - Similar a `inline`, pero permite establecer dimensiones (`width`, `height`).  
   Ejemplo:  
   ```css
   img {
     display: inline-block;
   }
   ```

4. **`none`:**  
   - Oculta completamente el elemento; no ocupa espacio en el diseño.  
   Ejemplo:  
   ```css
   div {
     display: none;
   }
   ```

5. **`flex`:**  
   - Activa un contenedor **flexible** para gestionar el diseño de los elementos hijos.  
   - Detallado más adelante.  

6. **`grid`:**  
   - Activa un contenedor de **rejilla bidimensional** para distribuir elementos en filas y columnas.  
   - Detallado más adelante.  

---

### Modelo Flexbox (`display: flex`)

El modelo **Flexbox** es ideal para diseños unidimensionales, ya sea en **fila** o en **columna**.

#### Propiedades del Contenedor Flex

1. **`flex-direction`:** Define la dirección principal de los elementos.  
   Valores:  
   - `row` (por defecto): De izquierda a derecha.  
   - `row-reverse`: De derecha a izquierda.  
   - `column`: De arriba a abajo.  
   - `column-reverse`: De abajo a arriba.  
   Ejemplo:  
   ```css
   .flex-container {
     display: flex;
     flex-direction: row;
   }
   ```

2. **`justify-content`:** Alinea los elementos a lo largo del eje principal.  
   Valores:  
   - `flex-start` (por defecto): Alineados al inicio.  
   - `flex-end`: Alineados al final.  
   - `center`: Centra los elementos.  
   - `space-between`: Espacio uniforme entre los elementos.  
   - `space-around`: Espacio uniforme alrededor de cada elemento.  
   Ejemplo:  
   ```css
   .flex-container {
     display: flex;
     justify-content: center;
   }
   ```

3. **`align-items`:** Alinea los elementos en el eje perpendicular.  
   Valores:  
   - `stretch` (por defecto): Ocupan todo el espacio disponible.  
   - `flex-start`: Alineados al inicio del eje cruzado.  
   - `flex-end`: Alineados al final del eje cruzado.  
   - `center`: Centra los elementos.  
   Ejemplo:  
   ```css
   .flex-container {
     display: flex;
     align-items: center;
   }
   ```

4. **`flex-wrap`:** Controla si los elementos se ajustan en varias líneas cuando no caben en una sola.  
   Valores:  
   - `nowrap` (por defecto): Todo en una sola línea.  
   - `wrap`: Los elementos se ajustan en varias líneas.  
   Ejemplo:  
   ```css
   .flex-container {
     display: flex;
     flex-wrap: wrap;
   }
   ```

#### Ejemplo Práctico con Flexbox

```css
.flex-container {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  gap: 10px; /* Espacio entre elementos */
}

.flex-item {
  background-color: lightblue;
  padding: 10px;
}
```

---

### Modelo Grid (`display: grid`)

El modelo **Grid** es ideal para diseños bidimensionales, distribuyendo elementos en **filas** y **columnas**.

#### Propiedades del Contenedor Grid

1. **`grid-template-columns` y `grid-template-rows`:** Define el tamaño de las columnas y filas.  
   Ejemplo:  
   ```css
   .grid-container {
     display: grid;
     grid-template-columns: 1fr 2fr; /* Dos columnas: 1 parte y 2 partes */
     grid-template-rows: 100px auto; /* Una fila fija y una adaptable */
   }
   ```

2. **`gap`:** Define el espacio entre filas y columnas.  
   Ejemplo:  
   ```css
   .grid-container {
     gap: 20px; /* Espacio uniforme */
   }
   ```

3. **`justify-items`:** Alinea los elementos en el eje horizontal.  
   Valores:  
   - `start`, `end`, `center`, `stretch`.  
   Ejemplo:  
   ```css
   .grid-container {
     justify-items: center;
   }
   ```

4. **`align-items`:** Alinea los elementos en el eje vertical.  
   Ejemplo:  
   ```css
   .grid-container {
     align-items: stretch;
   }
   ```

5. **`grid-area`:** Define cómo un elemento ocupa un espacio en el grid.  
   Ejemplo:  
   ```css
   .grid-item {
     grid-area: 1 / 2 / 3 / 4; /* Fila 1 a 3, columna 2 a 4 */
   }
   ```

#### Ejemplo Práctico con Grid

```css
.grid-container {
  display: grid;
  grid-template-columns: repeat(3, 1fr); /* Tres columnas iguales */
  grid-template-rows: 100px auto; /* Fila fija y una adaptable */
  gap: 10px;
}

.grid-item {
  background-color: lightcoral;
  padding: 10px;
  text-align: center;
}
```

---

### Comparación Flexbox vs Grid

| **Propiedad**       | **Flexbox**                         | **Grid**                              |
|---------------------|-------------------------------------|---------------------------------------|
| **Uso principal**   | Ejes unidimensionales (fila o columna). | Ejes bidimensionales (filas y columnas). |
| **Flexibilidad**    | Ideal para elementos dinámicos.     | Más estructura para diseños complejos. |
| **Facilidad**       | Más sencillo para alineaciones básicas. | Ideal para cuadrículas organizadas.    |
