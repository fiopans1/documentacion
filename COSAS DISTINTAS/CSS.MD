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