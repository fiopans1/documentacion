La estructura de archivos de un proyecto de **React** puede variar según el tamaño del proyecto, las preferencias del equipo y las herramientas utilizadas. Sin embargo, aquí te presento una estructura común y bien organizada basada en las mejores prácticas:

## Estructura de Archivos Común para un Proyecto de React

```plaintext
my-react-app/
├── public/                 # Archivos públicos
│   ├── index.html          # Archivo HTML raíz (punto de entrada)
│   ├── favicon.ico         # Ícono del sitio
│   ├── manifest.json       # Configuración de la PWA (si se usa)
│   └── assets/             # Recursos estáticos como imágenes o fuentes
│
├── src/                    # Código fuente del proyecto
│   ├── components/         # Componentes reutilizables
│   │   ├── Header.js
│   │   ├── Footer.js
│   │   └── Button.js
│   │
│   ├── pages/              # Páginas principales de la aplicación
│   │   ├── HomePage.js
│   │   ├── AboutPage.js
│   │   └── ContactPage.js
│   │
│   ├── layouts/            # Diseños generales de la aplicación
│   │   ├── MainLayout.js
│   │   └── AdminLayout.js
│   │
│   ├── hooks/              # Custom hooks
│   │   └── useAuth.js
│   │
│   ├── context/            # Contextos para React Context API
│   │   └── AuthContext.js
│   │
│   ├── services/           # Lógica de acceso a APIs o servicios externos
│   │   ├── api.js
│   │   └── authService.js
│   │
│   ├── utils/              # Funciones utilitarias
│   │   ├── formatDate.js
│   │   └── validations.js
│   │
│   ├── styles/             # Archivos de estilos globales y temáticos
│   │   ├── global.css
│   │   ├── variables.css
│   │   └── Button.module.css
│   │
│   ├── App.js              # Componente principal
│   ├── index.js            # Punto de entrada de React
│   ├── reportWebVitals.js  # Métricas de rendimiento (opcional)
│   └── setupTests.js       # Configuración para pruebas (opcional)
│
├── node_modules/           # Dependencias instaladas
│
├── .env                    # Variables de entorno
├── .gitignore              # Archivos y carpetas a ignorar en Git
├── package.json            # Configuración del proyecto y dependencias
├── package-lock.json       # Versiones bloqueadas de dependencias
├── README.md               # Documentación del proyecto
└── tsconfig.json           # Configuración de TypeScript (si se usa)
```

## Descripción de Carpetas Clave

1. **`public/`**:

   - Contiene recursos que no pasan por el empaquetador de JavaScript.
   - `index.html` es el archivo donde React inyecta el código de la aplicación.

2. **`src/`**:

   - Es el núcleo del proyecto donde se encuentran todos los archivos de desarrollo.
   - Puedes dividirla en subcarpetas como `components`, `pages`, y `hooks` para organizar mejor el código.

3. **`components/`**:

   - Almacena componentes reutilizables.
   - Puedes organizarla aún más (por ejemplo, subcarpetas para formularios o navegación).

4. **`pages/`**:

   - Contiene las vistas principales de la aplicación, como "Inicio" o "Contacto".

5. **`services/`**:

   - Aquí colocas las funciones para interactuar con APIs o servicios externos.

6. **`styles/`**:

   - Centraliza los archivos CSS. Puedes incluir estilos globales, módulos de CSS y temas.

7. **`utils/`**:
   - Contiene funciones auxiliares que se usan en diferentes partes del proyecto, como validaciones o formateo de datos.

## Personalización de la Estructura

- **Proyectos pequeños**: Puedes combinar carpetas para simplificar la estructura (por ejemplo, unir `pages/` y `components/`).
- **Proyectos grandes**: Es útil dividir más las carpetas, usar TypeScript y adoptar una arquitectura más avanzada (como Redux para el manejo de estado).

---

# Guía Completa sobre React

## 1. Introducción a React

- ¿Qué es React?
- Características principales
- Ventajas de usar React
- Casos de uso de React
- Instalación y configuración inicial

## 2. Estructura de un Proyecto React

- Explicación de la estructura típica
- Archivos principales (`index.html`, `App.js`, etc.)
- Configuración de entorno (`.env`)

## 3. JSX (JavaScript XML)

- ¿Qué es JSX?
- Sintaxis básica
- Reglas y buenas prácticas

## 4. Componentes

- Tipos de componentes (funcionales y de clase)
- Creación y uso de componentes
- Props: qué son y cómo se usan
- Propiedades predeterminadas y validación con `prop-types`

## 5. Estado y Ciclo de Vida

- Introducción al estado (`state`)
- Manejo del estado en componentes de clase
- El Hook `useState` en componentes funcionales
- Ciclo de vida de los componentes de clase
- Uso de `useEffect` para manejar efectos secundarios

## 6. Eventos en React

- Manejo de eventos
- Diferencias entre eventos en React y en JavaScript estándar
- Pasar parámetros a los manejadores de eventos

## 7. Comunicación entre Componentes

- Comunicación padre-hijo mediante props
- Comunicación hijo-padre mediante funciones
- Context API para compartir estado global

## 8. Estilizado en React

- CSS en línea y clases
- CSS Modules
- Styled-components y CSS-in-JS
- Integración con librerías como TailwindCSS o Bootstrap

## 9. Manejo del Estado Global

- Introducción a Redux
- Uso de Context API como alternativa
- Librerías modernas como Zustand o Jotai

## 10. Enrutamiento en React

- Introducción a React Router
- Configuración de rutas
- Rutas protegidas y redirecciones
- Navegación entre páginas

## 11. Formularios en React

- Manejo de formularios controlados y no controlados
- Validaciones básicas y avanzadas
- Librerías para formularios como `Formik` y `React Hook Form`

## 12. Llamadas a APIs y Manejo de Datos

- Uso de `fetch` y `axios`
- Uso del Hook `useEffect` para llamadas a APIs
- Gestión de estado de datos con `react-query` o `SWR`

## 13. Pruebas en React

- Introducción a pruebas unitarias con Jest
- Testing de componentes con React Testing Library
- Pruebas de integración y end-to-end con Cypress

## 14. Optimización de Rendimiento

- React.memo y React.PureComponent
- Lazy loading de componentes con `React.lazy` y `Suspense`
- Uso de `useMemo` y `useCallback` para optimización

## 15. React y TypeScript

- Configuración de un proyecto con TypeScript
- Tipado de props, estado y componentes
- Beneficios y mejores prácticas

## 16. Buenas Prácticas en React

- Estructura limpia de componentes
- Dividir componentes en funcionales y contenedores
- Documentación y uso consistente de estilos
- Uso de linters y formateadores como ESLint y Prettier

## 17. Despliegue de una Aplicación React

- Empaquetado con Create React App o Vite
- Configuración para producción
- Despliegue en plataformas como Netlify, Vercel o AWS Amplify

## 18. React Avanzado

- React Server Components (RSC)
- Suspense para datos asíncronos
- Manejo de WebSockets y tiempo real
- Renderizado del lado del servidor (SSR) con Next.js

## 19. Herramientas y Librerías Complementarias

- Storybook para documentación de componentes
- Recoil para gestión de estado global
- Librerías de animación como Framer Motion

## 20. Proyectos Prácticos

- Crear un contador simple
- To-do list con manejo de estado
- Blog con integración de APIs
- Tablero Kanban con drag and drop
- E-commerce con carrito de compras y autenticación

---

# 1. Introducción a React

## ¿Qué es React?

**React** es una biblioteca de JavaScript de código abierto desarrollada por **Meta (anteriormente Facebook)**. Su propósito principal es facilitar la construcción de interfaces de usuario interactivas y eficientes para aplicaciones web y móviles.

React se basa en el paradigma de **componentes**, lo que permite dividir una aplicación en piezas reutilizables y modulares. Cada componente administra su propia lógica y estado, haciendo que las aplicaciones sean más predecibles y fáciles de mantener.

---

## Características principales

1. **Componentes**:

   - React organiza la interfaz de usuario en componentes reutilizables.
   - Cada componente es una pieza independiente que gestiona su propio estado y lógica.

2. **Declarativo**:

   - En lugar de manipular el DOM directamente, describes el "qué" debería suceder, y React se encarga del "cómo".
   - Usa una sintaxis declarativa llamada **JSX** para diseñar las vistas.

3. **Virtual DOM**:

   - React utiliza un árbol virtual de DOM en memoria para mejorar el rendimiento.
   - Solo actualiza las partes necesarias del DOM real, lo que hace que las aplicaciones sean rápidas y eficientes.

4. **Unidireccionalidad del flujo de datos**:

   - Los datos fluyen de manera unidireccional (de padre a hijo).
   - Esto facilita la depuración y el seguimiento del estado de la aplicación.

5. **React Hooks**:

   - Introducidos en React 16.8, permiten manejar el estado y el ciclo de vida en componentes funcionales.
   - Ejemplos: `useState`, `useEffect`, `useContext`.

6. **Compatible con otras tecnologías**:

   - React puede usarse con otras bibliotecas o frameworks como Redux, Next.js, y más.

7. **Ecosistema amplio**:
   - Una comunidad activa y un extenso ecosistema de herramientas y librerías complementarias.

---

## Ventajas de usar React

1. **Reutilización de componentes**:

   - Mejora la productividad al permitir usar los mismos componentes en diferentes partes del proyecto.

2. **Rendimiento**:

   - Gracias al **Virtual DOM**, React optimiza la actualización de la interfaz.

3. **Desarrollo modular y escalable**:

   - Las aplicaciones se construyen como un conjunto de bloques (componentes), lo que facilita el mantenimiento y la escalabilidad.

4. **Ecosistema rico**:

   - Amplia cantidad de herramientas, extensiones, y librerías de terceros.

5. **Aprendizaje y adopción**:

   - Documentación oficial detallada y una comunidad global que ayuda a resolver problemas.

6. **Popularidad y respaldo**:

   - React está ampliamente adoptado en la industria, con empresas como Meta, Netflix, y Airbnb usándolo.

7. **Renderizado dinámico**:
   - React puede manejar cambios en tiempo real, lo que lo hace ideal para aplicaciones interactivas.

---

## Casos de uso de React

1. **Aplicaciones de una sola página (SPA)**:

   - Donde el contenido cambia dinámicamente sin recargar toda la página (e.g., dashboards).

2. **Aplicaciones web complejas**:

   - Como plataformas de e-commerce, redes sociales, o sistemas de gestión.

3. **Sistemas de interacción en tiempo real**:

   - Chats, aplicaciones de mensajería o colaboración en línea.

4. **Sitios con alto rendimiento**:

   - React es ideal para aplicaciones que requieren optimización, como Netflix o Airbnb.

5. **Aplicaciones móviles**:
   - A través de **React Native**, puedes construir aplicaciones móviles con la misma filosofía.

---

## Instalación y Configuración Inicial

React puede configurarse de varias maneras, dependiendo de tus necesidades. A continuación, te muestro cómo comenzar:

### 1. **Crear un proyecto con Create React App**

**Create React App (CRA)** es la forma más rápida de configurar un proyecto React con todas las herramientas necesarias (como Webpack, Babel, etc.) ya incluidas.

#### Pasos:

1. Asegúrate de tener **Node.js** y **npm** o **yarn** instalados.

   - Verifica con:
     ```bash
     node -v
     npm -v
     ```

2. Ejecuta el siguiente comando para crear un proyecto:
   ```bash
   npx create-react-app my-app
   cd my-app
   npm start
   ```
   Esto inicializa un proyecto React y arranca un servidor de desarrollo.

#### Estructura generada:

```plaintext
my-app/
├── node_modules/         # Dependencias del proyecto
├── public/               # Archivos públicos (HTML, imágenes)
├── src/                  # Código fuente de React
│   ├── App.css           # Estilos globales para App.js
│   ├── App.js            # Componente principal
│   ├── index.css         # Estilos generales
│   └── index.js          # Punto de entrada de React
├── .gitignore            # Archivos a ignorar en Git
├── package.json          # Configuración del proyecto y dependencias
└── README.md             # Información del proyecto
```

3. Inicia el servidor:
   ```bash
   npm start
   ```
   Esto abre la aplicación en `http://localhost:3000`.

---

### 2. **Crear un proyecto React con Vite**

**Vite** es una alternativa moderna a Create React App, con tiempos de carga más rápidos y una configuración más liviana.

#### Pasos:

1. Instala Vite:
   ```bash
   npm create vite@latest my-app
   ```
2. Selecciona el template `React` o `React + TypeScript`.
3. Instala las dependencias:
   ```bash
   cd my-app
   npm install
   ```
4. Inicia el servidor de desarrollo:
   ```bash
   npm run dev
   ```

---

# 2. Estructura de un Proyecto React

React organiza los archivos de un proyecto de forma estructurada para facilitar el desarrollo, mantenimiento y escalabilidad de las aplicaciones. A continuación, se detalla la estructura típica y el propósito de los archivos principales.

---

## Explicación de la Estructura Típica

Un proyecto React generalmente incluye las siguientes carpetas y archivos clave:

```plaintext
my-react-app/
├── public/                # Archivos públicos
│   ├── index.html         # Archivo HTML raíz
│   ├── favicon.ico        # Ícono del sitio
│   └── assets/            # Recursos estáticos como imágenes
│
├── src/                   # Código fuente del proyecto
│   ├── components/        # Componentes reutilizables
│   ├── pages/             # Páginas principales
│   ├── styles/            # Archivos CSS y temas
│   ├── App.js             # Componente principal
│   ├── index.js           # Punto de entrada de React
│   └── reportWebVitals.js # Métricas de rendimiento (opcional)
│
├── .env                   # Variables de entorno
├── package.json           # Configuración del proyecto y dependencias
├── package-lock.json      # Versiones bloqueadas de dependencias
├── README.md              # Documentación del proyecto
└── node_modules/          # Dependencias instaladas
```

---

## Archivos Principales

### **1. `public/index.html`**

- **Ubicación**: Carpeta `public/`.
- **Propósito**: Es el archivo HTML base de la aplicación. React utiliza este archivo para inyectar dinámicamente los componentes.
- **Contenido principal**:
  ```html
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <title>React App</title>
    </head>
    <body>
      <div id="root"></div>
    </body>
  </html>
  ```
  - **`<div id="root"></div>`**: Es el contenedor donde React renderiza toda la aplicación. Este es el punto de entrada del DOM.

---

### **2. `src/index.js`**

- **Ubicación**: Carpeta `src/`.
- **Propósito**: Es el punto de entrada de la aplicación React.
- **Responsabilidades**:
  - Importa las bibliotecas necesarias como React y ReactDOM.
  - Renderiza el componente principal (`App.js`) dentro del contenedor raíz (`<div id="root">`).
- **Ejemplo de código**:

  ```javascript
  import React from "react";
  import ReactDOM from "react-dom/client";
  import "./index.css"; // Estilos globales
  import App from "./App"; // Componente principal

  const root = ReactDOM.createRoot(document.getElementById("root"));
  root.render(
    <React.StrictMode>
      <App />
    </React.StrictMode>
  );
  ```

---

### **3. `src/App.js`**

- **Ubicación**: Carpeta `src/`.
- **Propósito**: Es el componente principal de la aplicación. Sirve como punto de partida para organizar la lógica y otros componentes.
- **Responsabilidades**:
  - Importa y organiza otros componentes.
  - Contiene la estructura básica de la aplicación.
- **Ejemplo de código**:

  ```javascript
  import React from "react";

  function App() {
    return (
      <div>
        <h1>¡Bienvenido a React!</h1>
      </div>
    );
  }

  export default App;
  ```

---

### **4. `public/favicon.ico`**

- **Propósito**: Es el ícono que aparece en la pestaña del navegador.

---

### **5. `public/manifest.json`**

- **Propósito**: Configura la Progressive Web App (PWA). Es opcional, pero útil para aplicaciones que se instalarán como apps móviles o de escritorio.
- **Ejemplo de contenido**:
  ```json
  {
    "short_name": "ReactApp",
    "name": "React Application",
    "icons": [
      {
        "src": "favicon.ico",
        "sizes": "64x64 32x32 24x24 16x16",
        "type": "image/x-icon"
      }
    ],
    "start_url": ".",
    "display": "standalone",
    "theme_color": "#000000",
    "background_color": "#ffffff"
  }
  ```

---

### **6. `src/reportWebVitals.js`**

- **Propósito**: Mide y reporta métricas de rendimiento como el tiempo de carga. Es opcional y no siempre se utiliza.
- **Ejemplo de uso**:

  ```javascript
  const reportWebVitals = (onPerfEntry) => {
    if (onPerfEntry && onPerfEntry instanceof Function) {
      import("web-vitals").then(
        ({ getCLS, getFID, getFCP, getLCP, getTTFB }) => {
          getCLS(onPerfEntry);
          getFID(onPerfEntry);
          getFCP(onPerfEntry);
          getLCP(onPerfEntry);
          getTTFB(onPerfEntry);
        }
      );
    }
  };

  export default reportWebVitals;
  ```

---

## Configuración de Entorno (`.env`)

React permite usar archivos `.env` para almacenar variables de entorno. Estas variables ayudan a configurar valores sensibles o específicos del entorno (desarrollo, prueba, producción).

### **Características clave de `.env` en React**

1. Todas las variables deben comenzar con `REACT_APP_`.
   - Ejemplo: `REACT_APP_API_URL=http://api.example.com`.
2. Estas variables estarán disponibles dentro de la aplicación mediante `process.env`.

### **Creación y Uso**

1. **Archivo `.env`**:
   - Ubicación: en la raíz del proyecto.
   - Ejemplo:
     ```plaintext
     REACT_APP_API_URL=https://api.misitio.com
     REACT_APP_ENVIRONMENT=development
     ```
2. **Acceso en el código**:
   - Usar `process.env` para acceder a las variables:
     ```javascript
     const apiUrl = process.env.REACT_APP_API_URL;
     console.log("API URL:", apiUrl);
     ```

### **Variables por Entorno**

Puedes usar diferentes archivos `.env` para distintos entornos:

- `.env` (variables generales)
- `.env.development` (para desarrollo)
- `.env.production` (para producción)

---

# 3. JSX (JavaScript XML)

## ¿Qué es JSX?

**JSX (JavaScript XML)** es una extensión de la sintaxis de JavaScript que permite escribir estructuras similares a HTML directamente en el código JavaScript. Es una de las características principales de React, ya que facilita la creación de interfaces de usuario declarativas.

Aunque parece HTML, **JSX es transformado a código JavaScript puro** en tiempo de compilación usando herramientas como Babel. Por ejemplo, el siguiente código JSX:

```jsx
const element = <h1>Hello, World!</h1>;
```

Se transforma en:

```javascript
const element = React.createElement("h1", null, "Hello, World!");
```

### Ventajas de usar JSX

1. **Legibilidad**: Combina la estructura de la interfaz con la lógica, haciendo el código más fácil de entender.
2. **Integración con JavaScript**: Puedes usar funciones, variables, y expresiones de JavaScript directamente dentro de JSX.
3. **Declarativo**: Permite describir la UI de manera clara, lo que mejora el flujo de desarrollo.

---

## Sintaxis Básica

### **1. Elementos JSX básicos**

Un elemento JSX típico se ve como un elemento HTML:

```jsx
const element = <h1>¡Hola, Mundo!</h1>;
```

### **2. Expresiones en JSX**

Puedes insertar cualquier expresión de JavaScript dentro de `{}`:

```jsx
const name = "Juan";
const element = <h1>Hola, {name}!</h1>;
```

### **3. Atributos en JSX**

Los atributos en JSX son similares a HTML, pero algunos cambian ligeramente para ajustarse a JavaScript:

- **class** se convierte en `className`:
  ```jsx
  const element = <div className="container">Contenido</div>;
  ```
- **for** se convierte en `htmlFor`:
  ```jsx
  const label = <label htmlFor="input">Etiqueta</label>;
  ```

### **4. Fragments (<> ... </>)**

React requiere que todo el contenido devuelto esté dentro de un único nodo. Si no deseas usar un contenedor extra como `<div>`, puedes usar un **Fragment**:

```jsx
return (
  <>
    <h1>Título</h1>
    <p>Este es un párrafo.</p>
  </>
);
```

### **5. Renderizado condicional**

Puedes usar expresiones condicionales dentro de JSX:

- **Operador ternario**:
  ```jsx
  const isLoggedIn = true;
  return (
    <div>
      {isLoggedIn ? <h1>Bienvenido</h1> : <h1>Por favor, inicia sesión</h1>}
    </div>
  );
  ```
- **Operador lógico &&**:
  ```jsx
  const isVisible = true;
  return <div>{isVisible && <p>Este mensaje es visible.</p>}</div>;
  ```

### **6. Listas y bucles**

Usa el método `map()` para renderizar listas:

```jsx
const items = ["Manzana", "Banana", "Cereza"];
return (
  <ul>
    {items.map((item, index) => (
      <li key={index}>{item}</li>
    ))}
  </ul>
);
```

---

## Reglas y Buenas Prácticas

### **1. JSX siempre debe estar envuelto en un contenedor**

React requiere que el contenido esté dentro de un único elemento raíz. Usa un contenedor como `<div>` o `<Fragment>`:

```jsx
return (
  <div>
    <h1>Hola</h1>
    <p>Bienvenido a React</p>
  </div>
);
```

### **2. Usa la propiedad `key` al renderizar listas**

Cuando renderizas una lista de elementos, React necesita una propiedad `key` única para cada elemento. Esto ayuda a React a identificar qué elementos han cambiado:

```jsx
const items = ["A", "B", "C"];
return (
  <ul>
    {items.map((item, index) => (
      <li key={index}>{item}</li>
    ))}
  </ul>
);
```

### **3. Nombres de componentes en mayúscula**

Cuando creas componentes personalizados, sus nombres deben comenzar con una letra mayúscula. React los diferencia de los elementos HTML nativos de esta forma:

```jsx
function MiComponente() {
  return <h1>Hola desde un componente</h1>;
}
```

### **4. Atributos y estilos en camelCase**

Atributos y propiedades de estilo usan camelCase:

- Atributos:
  ```jsx
  const input = <input autoFocus />;
  ```
- Estilos:
  ```jsx
  const style = { backgroundColor: "blue", fontSize: "16px" };
  return <div style={style}>Texto con estilo</div>;
  ```

### **5. Evita usar `bind` en el renderizado**

En lugar de usar `bind` o funciones inline, define funciones fuera del renderizado para evitar problemas de rendimiento:

- Malo:
  ```jsx
  <button onClick={this.handleClick.bind(this)}>Click</button>
  ```
- Bueno:
  ```jsx
  <button onClick={this.handleClick}>Click</button>
  ```

### **6. Evita comentarios dentro de JSX**

Los comentarios deben estar dentro de `{}`:

```jsx
return (
  <div>
    {/* Este es un comentario */}
    <h1>Hola, React</h1>
  </div>
);
```

---

## Conclusión

JSX combina lo mejor de HTML y JavaScript, haciendo que la creación de interfaces de usuario sea más intuitiva y declarativa. Al seguir las reglas y buenas prácticas mencionadas, puedes escribir código limpio, legible y eficiente.

---

# 4. Componentes en React

Los componentes son la base de React. Son piezas reutilizables que permiten dividir la interfaz de usuario en partes independientes, manejables y probadas individualmente.

---

## Tipos de Componentes

En React, existen dos tipos principales de componentes:

### **1. Componentes Funcionales**

Son funciones de JavaScript que devuelven elementos JSX. Son la forma más moderna y recomendada de escribir componentes.

#### **Características:**

- Simples y fáciles de entender.
- Usan _hooks_ como `useState` y `useEffect` para manejar estado y ciclo de vida.
- Preferidos en proyectos modernos debido a su menor complejidad.

#### **Ejemplo:**

```jsx
function Saludo(props) {
  return <h1>Hola, {props.nombre}!</h1>;
}

export default Saludo;
```

#### **Ventajas:**

- Menor cantidad de código.
- Mejor rendimiento debido a su naturaleza más simple.
- Totalmente compatibles con las funcionalidades modernas de React.

---

### **2. Componentes de Clase**

Son clases de JavaScript que extienden `React.Component`. Eran la forma estándar de manejar estado y ciclo de vida antes de la introducción de los _hooks_.

#### **Características:**

- Tienen acceso al estado (`this.state`) y métodos de ciclo de vida (`componentDidMount`, `componentDidUpdate`, etc.).
- Más verbosos y gradualmente menos comunes en proyectos nuevos.

#### **Ejemplo:**

```jsx
import React, { Component } from "react";

class Saludo extends Component {
  render() {
    return <h1>Hola, {this.props.nombre}!</h1>;
  }
}

export default Saludo;
```

#### **Cuándo usarlos:**

- En proyectos más antiguos o cuando el equipo de desarrollo ya está acostumbrado a ellos.

---

## Creación y Uso de Componentes

### **Creación de un Componente**

Un componente puede ser tan simple como esto:

```jsx
function ComponenteSimple() {
  return <h1>Este es un componente simple</h1>;
}
```

### **Uso de un Componente**

Para usar un componente, simplemente inclúyelo como una etiqueta JSX:

```jsx
import ComponenteSimple from "./ComponenteSimple";

function App() {
  return (
    <div>
      <ComponenteSimple />
    </div>
  );
}
```

---

## Props: Qué Son y Cómo se Usan

### **¿Qué son las Props?**

- Las **props** (abreviatura de "properties") son argumentos que se pasan a los componentes para personalizarlos.
- Son **inmutables**, lo que significa que no se pueden modificar dentro del componente.

### **Cómo Usar Props**

1. **Pasar props a un componente:**

   ```jsx
   function Saludo(props) {
     return <h1>Hola, {props.nombre}!</h1>;
   }

   <Saludo nombre="Juan" />;
   ```

2. **Acceder a las props en componentes de clase:**

   ```jsx
   class Saludo extends React.Component {
     render() {
       return <h1>Hola, {this.props.nombre}!</h1>;
     }
   }
   ```

3. **Props con valores dinámicos:**
   Puedes pasar variables o resultados de funciones como props:
   ```jsx
   const nombre = "María";
   <Saludo nombre={nombre} />;
   ```

---

## Propiedades Predeterminadas y Validación con PropTypes

### **Propiedades Predeterminadas**

Puedes definir valores predeterminados para las props en caso de que no se proporcionen. Esto se hace usando la propiedad estática `defaultProps`.

#### **Ejemplo:**

```jsx
function Saludo(props) {
  return <h1>Hola, {props.nombre}!</h1>;
}

Saludo.defaultProps = {
  nombre: "Visitante",
};
```

En este caso, si `nombre` no se pasa como prop, el valor predeterminado será "Visitante".

---

### **Validación de Props con `prop-types`**

Para garantizar que las props sean del tipo correcto, puedes usar la biblioteca `prop-types`. Esta validación es especialmente útil en proyectos grandes para prevenir errores.

#### **Instalación de `prop-types`:**

```bash
npm install prop-types
```

#### **Uso de PropTypes:**

```jsx
import PropTypes from "prop-types";

function Saludo(props) {
  return <h1>Hola, {props.nombre}!</h1>;
}

Saludo.propTypes = {
  nombre: PropTypes.string.isRequired, // Debe ser una cadena y obligatorio
};
```

#### **Tipos de PropTypes Comunes:**

- **`PropTypes.string`**: Cadena de texto.
- **`PropTypes.number`**: Número.
- **`PropTypes.bool`**: Booleano.
- **`PropTypes.array`**: Arreglo.
- **`PropTypes.object`**: Objeto.
- **`PropTypes.func`**: Función.
- **`PropTypes.node`**: Elemento renderizable (JSX, texto, etc.).
- **`PropTypes.oneOf`**: Valor entre varias opciones:
  ```jsx
  color: PropTypes.oneOf(['rojo', 'verde', 'azul']),
  ```
- **`PropTypes.shape`**: Objeto con una estructura específica:
  ```jsx
  usuario: PropTypes.shape({
    nombre: PropTypes.string,
    edad: PropTypes.number,
  }),
  ```

---

## Buenas Prácticas con Componentes

1. **Divide y vencerás:** Los componentes deben ser pequeños y responsables de una única tarea.
2. **Reutilización:** Evita duplicar código creando componentes reutilizables.
3. **Composición:** Usa componentes pequeños para construir componentes más complejos.
4. **Mantén el estado fuera de los componentes si no es necesario:** Utiliza props para pasar datos.
5. **Usa nombres descriptivos:** Nombra tus componentes y props de manera que su propósito sea claro.

---

# 5. Estado y Ciclo de Vida en React

El estado (state) y el ciclo de vida de los componentes son aspectos fundamentales para construir aplicaciones dinámicas e interactivas en React. En esta sección, exploraremos cómo manejar el estado en componentes de clase y funcionales, así como el ciclo de vida de estos componentes.

---

## Introducción al Estado (State)

**Estado (state)** se refiere a un conjunto de datos o propiedades locales que pertenecen a un componente. A diferencia de las props, el estado:

- **Es mutable**: Puede cambiar a lo largo del tiempo.
- **Es local**: Pertenece únicamente al componente donde se define y no se puede acceder desde otros componentes, salvo que se pase explícitamente como props.

El estado se utiliza para:

1. Controlar el comportamiento dinámico de la interfaz.
2. Guardar valores temporales que pueden cambiar, como datos de entrada, contadores, etc.

---

## Manejo del Estado en Componentes de Clase

En los componentes de clase, el estado se define como un objeto en la propiedad `this.state`. Para actualizar el estado, se usa el método `this.setState`.

### **1. Definir el estado inicial**

Se define en el constructor del componente:

```jsx
class Contador extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      contador: 0,
    };
  }

  render() {
    return <h1>{this.state.contador}</h1>;
  }
}
```

### **2. Actualizar el estado**

Nunca debes modificar el estado directamente (como `this.state.contador = 1`). En su lugar, usa `this.setState`:

```jsx
incrementar = () => {
  this.setState({ contador: this.state.contador + 1 });
};

render() {
  return (
    <div>
      <h1>{this.state.contador}</h1>
      <button onClick={this.incrementar}>Incrementar</button>
    </div>
  );
}
```

### **3. Estado dependiente**

Cuando el nuevo estado depende del estado anterior, usa la forma de función en `setState`:

```jsx
this.setState((estadoAnterior) => ({
  contador: estadoAnterior.contador + 1,
}));
```

---

## El Hook `useState` en Componentes Funcionales

Con la introducción de los hooks en React 16.8, el manejo del estado en componentes funcionales es más sencillo y más limpio.

### **1. Uso básico de `useState`**

`useState` es un hook que permite agregar estado a los componentes funcionales. Retorna un par: el estado actual y una función para actualizarlo.

```jsx
import React, { useState } from "react";

function Contador() {
  const [contador, setContador] = useState(0);

  return (
    <div>
      <h1>{contador}</h1>
      <button onClick={() => setContador(contador + 1)}>Incrementar</button>
    </div>
  );
}
```

### **2. Inicializar con un valor**

El valor inicial del estado se pasa como argumento a `useState`:

```jsx
const [contador, setContador] = useState(10);
```

### **3. Actualizar estado dependiente**

Similar a `setState`, puedes actualizar el estado basado en su valor anterior:

```jsx
setContador((contadorAnterior) => contadorAnterior + 1);
```

### **4. Manejar múltiples estados**

Puedes usar varios hooks `useState` para manejar diferentes variables de estado:

```jsx
const [nombre, setNombre] = useState("");
const [edad, setEdad] = useState(0);
```

---

## Ciclo de Vida de los Componentes de Clase

Los componentes de clase tienen métodos específicos que se ejecutan durante diferentes fases del ciclo de vida:

### **1. Fases del Ciclo de Vida**

1. **Montaje** (Cuando el componente se agrega al DOM):

   - `constructor()`: Configuración inicial, como definir el estado.
   - `componentDidMount()`: Se ejecuta después de que el componente se ha montado. Ideal para peticiones a APIs o suscripción a eventos.

2. **Actualización** (Cuando las props o el estado cambian):

   - `componentDidUpdate(prevProps, prevState)`: Se ejecuta después de una actualización. Útil para manejar efectos secundarios.

3. **Desmontaje** (Cuando el componente se elimina del DOM):
   - `componentWillUnmount()`: Se utiliza para limpiar suscripciones o recursos.

### **2. Ejemplo del ciclo de vida**

```jsx
class EjemploCicloDeVida extends React.Component {
  constructor(props) {
    super(props);
    this.state = { contador: 0 };
    console.log("Constructor");
  }

  componentDidMount() {
    console.log("Component Did Mount");
  }

  componentDidUpdate(prevProps, prevState) {
    console.log("Component Did Update");
  }

  componentWillUnmount() {
    console.log("Component Will Unmount");
  }

  incrementar = () => {
    this.setState({ contador: this.state.contador + 1 });
  };

  render() {
    console.log("Render");
    return (
      <div>
        <h1>{this.state.contador}</h1>
        <button onClick={this.incrementar}>Incrementar</button>
      </div>
    );
  }
}
```

---

## Uso de `useEffect` para Manejar Efectos Secundarios

El hook `useEffect` reemplaza la necesidad de manejar efectos secundarios usando los métodos de ciclo de vida en componentes funcionales.

### **1. Uso básico de `useEffect`**

Se ejecuta después de que el componente se renderiza o actualiza:

```jsx
import React, { useState, useEffect } from "react";

function Ejemplo() {
  const [contador, setContador] = useState(0);

  useEffect(() => {
    console.log(`El contador cambió a ${contador}`);
  });

  return (
    <button onClick={() => setContador(contador + 1)}>
      Incrementar: {contador}
    </button>
  );
}
```

### **2. Controlar cuándo se ejecuta**

Usa un segundo argumento (array de dependencias) para especificar cuándo debe ejecutarse:

- **Sin dependencias:** Se ejecuta en cada renderizado.
- **Con dependencias vacías:** Se ejecuta solo una vez al montar el componente.
- **Con dependencias específicas:** Se ejecuta cuando esas dependencias cambian.

```jsx
useEffect(() => {
  console.log("Componente montado");
}, []); // Solo en montaje

useEffect(() => {
  console.log(`El contador cambió a ${contador}`);
}, [contador]); // Solo cuando `contador` cambia
```

### **3. Limpieza de efectos**

Usa una función de limpieza para limpiar recursos como suscripciones:

```jsx
useEffect(() => {
  const intervalo = setInterval(() => {
    console.log("Intervalo activo");
  }, 1000);

  return () => {
    clearInterval(intervalo); // Limpia el intervalo al desmontar
  };
}, []);
```

---

## Resumen

| Aspecto               | Componentes de Clase                                      | Componentes Funcionales con Hooks      |
| --------------------- | --------------------------------------------------------- | -------------------------------------- |
| **Definir estado**    | `this.state` en el constructor                            | `useState`                             |
| **Actualizar estado** | `this.setState`                                           | Función de actualización de `useState` |
| **Ciclo de vida**     | Métodos como `componentDidMount` y `componentWillUnmount` | `useEffect`                            |

---

# 6. Eventos en React

El manejo de eventos en React es similar al manejo de eventos en JavaScript estándar, pero tiene ciertas diferencias y características específicas que facilitan su uso y aseguran un rendimiento óptimo.

---

## **Manejo de Eventos en React**

En React, los eventos se manejan directamente en el JSX mediante atributos que corresponden a eventos del DOM (como `onClick`, `onChange`, etc.). Sin embargo, en React, los nombres de estos eventos son camelCase en lugar de minúsculas.

### **1. Ejemplo básico de un evento**

```jsx
function Boton() {
  function manejarClick() {
    alert("¡Botón clickeado!");
  }

  return <button onClick={manejarClick}>Haz clic aquí</button>;
}
```

En este ejemplo:

- `onClick` es el atributo del evento.
- `manejarClick` es la función que se ejecuta cuando ocurre el evento.

---

## **Diferencias entre Eventos en React y en JavaScript Estándar**

1. **CamelCase vs Minúsculas:**

   - React usa camelCase para los nombres de los eventos (`onClick`, `onChange`), mientras que el DOM estándar usa minúsculas (`onclick`, `onchange`).

2. **Funciones en lugar de strings:**

   - En React, los manejadores de eventos son funciones de JavaScript, no cadenas de texto como en HTML estándar.

   ```html
   <!-- HTML estándar -->
   <button onclick="alert('Clic!')">Clic</button>
   ```

   ```jsx
   // React
   <button onClick={() => alert("Clic!")}>Clic</button>
   ```

3. **Eventos Sintéticos:**

   - React utiliza un sistema de eventos propio llamado **SyntheticEvent**, que es una envoltura sobre los eventos nativos del navegador. Este sistema asegura compatibilidad entre navegadores y mejora el rendimiento.
   - Los eventos sintéticos tienen la misma interfaz que los eventos nativos (como `target`, `type`, `preventDefault`, etc.), pero no son exactamente iguales.

4. **Diferencias en el manejo de `this`:**

   - En componentes de clase, los manejadores de eventos deben vincular el contexto (`this`) explícitamente (a menos que uses funciones flecha o bind en el constructor):

     ```jsx
     class MiBoton extends React.Component {
       constructor(props) {
         super(props);
         this.manejarClick = this.manejarClick.bind(this);
       }

       manejarClick() {
         console.log(this); // Se refiere al componente actual
       }

       render() {
         return <button onClick={this.manejarClick}>Clic</button>;
       }
     }
     ```

   - En componentes funcionales con hooks, el uso de `this` no es necesario.

---

## **Pasar Parámetros a los Manejadores de Eventos**

A veces, es necesario pasar parámetros personalizados a una función manejadora de eventos. Hay dos formas comunes de hacerlo en React:

### **1. Usar una función flecha dentro del evento**

```jsx
function Boton() {
  function manejarClick(nombre) {
    alert(`Hola, ${nombre}!`);
  }

  return <button onClick={() => manejarClick("Juan")}>Saludar</button>;
}
```

### **2. Usar `bind` para pasar argumentos**

En componentes de clase, puedes usar `bind` para pasar parámetros al manejador de eventos:

```jsx
class Boton extends React.Component {
  manejarClick(nombre) {
    alert(`Hola, ${nombre}!`);
  }

  render() {
    return (
      <button onClick={this.manejarClick.bind(this, "Juan")}>Saludar</button>
    );
  }
}
```

Ambas técnicas son válidas, pero usar funciones flecha es más común y fácil de leer en React moderno.

---

## **Ejemplo Completo: Manejo de Eventos**

```jsx
import React, { useState } from "react";

function Contador() {
  const [contador, setContador] = useState(0);

  function incrementar() {
    setContador(contador + 1);
  }

  function decrementar() {
    setContador(contador - 1);
  }

  return (
    <div>
      <h1>Contador: {contador}</h1>
      <button onClick={incrementar}>Incrementar</button>
      <button onClick={decrementar}>Decrementar</button>
    </div>
  );
}

export default Contador;
```

---

## **Buenas Prácticas para Manejar Eventos**

1. **Funciones manejadoras descriptivas:**
   - Usa nombres claros para los manejadores de eventos (e.g., `manejarClick` en lugar de `handleClick` en inglés, si el equipo usa español).
2. **Evita lógica compleja en los atributos de eventos:**
   - Mueve la lógica a una función separada para mantener el código limpio.
3. **Usa funciones flecha para evitar problemas de contexto (`this`):**
   - Especialmente en componentes funcionales, las funciones flecha son más simples y eficientes.
4. **Limpia recursos al manejar eventos globales:**

   - Si utilizas eventos globales como `window.addEventListener`, asegúrate de eliminarlos en `useEffect` o `componentWillUnmount`:

     ```jsx
     useEffect(() => {
       const manejarScroll = () => console.log("Scrolling...");
       window.addEventListener("scroll", manejarScroll);

       return () => {
         window.removeEventListener("scroll", manejarScroll);
       };
     }, []);
     ```

---

# 7. Comunicación entre Componentes en React

La comunicación entre componentes es una parte esencial del desarrollo de aplicaciones en React. Aquí exploraremos cómo los componentes pueden compartir datos entre sí mediante `props`, funciones, y la **Context API**.

---

## **1. Comunicación Padre-Hijo mediante Props**

La forma más común de pasar datos desde un componente padre a un componente hijo es mediante **props**. Los **props** (abreviatura de "properties") son parámetros que un componente padre pasa a un componente hijo.

### **Ejemplo básico: Pasar datos como props**

```jsx
function Hijo(props) {
  return <h1>Hola, {props.nombre}!</h1>;
}

function Padre() {
  return <Hijo nombre="Juan" />;
}
```

En este ejemplo:

- El componente padre (`Padre`) pasa un valor (`"Juan"`) como prop al componente hijo (`Hijo`).
- El componente hijo accede al valor con `props.nombre`.

### **Props como objetos inmutables**

Los props no pueden ser modificados por el componente hijo. Son de solo lectura.

```jsx
function Hijo(props) {
  // props.nombre = "Carlos"; // ❌ Esto no es válido
  return <h1>{props.nombre}</h1>;
}
```

### **Props por defecto**

Puedes definir valores por defecto para las props usando `defaultProps`:

```jsx
function Hijo(props) {
  return <h1>Hola, {props.nombre}!</h1>;
}

Hijo.defaultProps = {
  nombre: "Invitado",
};
```

---

## **2. Comunicación Hijo-Padre mediante Funciones**

Para enviar datos desde un componente hijo al padre, puedes usar una función pasada como prop desde el componente padre.

### **Ejemplo básico: Comunicación hijo-padre**

```jsx
function Hijo(props) {
  return (
    <button onClick={() => props.enviarMensaje("Hola desde el Hijo")}>
      Enviar mensaje
    </button>
  );
}

function Padre() {
  function recibirMensaje(mensaje) {
    alert(mensaje);
  }

  return <Hijo enviarMensaje={recibirMensaje} />;
}
```

En este ejemplo:

1. El componente padre pasa la función `recibirMensaje` como prop al hijo.
2. El hijo ejecuta esta función cuando ocurre un evento (como un clic), enviando datos al padre.

### **Flujo completo: Padre → Hijo → Padre**

Un ejemplo típico sería una lista de tareas donde el hijo puede enviar datos al padre:

```jsx
function Tarea({ tarea, eliminarTarea }) {
  return (
    <li>
      {tarea} <button onClick={() => eliminarTarea(tarea)}>Eliminar</button>
    </li>
  );
}

function ListaDeTareas() {
  const [tareas, setTareas] = React.useState([
    "Estudiar React",
    "Practicar CSS",
  ]);

  function eliminarTarea(tarea) {
    setTareas(tareas.filter((t) => t !== tarea));
  }

  return (
    <ul>
      {tareas.map((tarea, index) => (
        <Tarea key={index} tarea={tarea} eliminarTarea={eliminarTarea} />
      ))}
    </ul>
  );
}
```

---

## **3. Context API para Compartir Estado Global**

Cuando necesitas compartir datos entre múltiples componentes sin pasar props manualmente en cada nivel del árbol de componentes, puedes usar **Context API**. Esto es útil para manejar estados globales como temas, usuarios, configuraciones, etc.

### **Pasos para usar Context API**

#### **1. Crear el contexto**

```jsx
import React, { createContext } from "react";

const MiContexto = createContext();
```

#### **2. Proveedor de contexto**

El componente **Provider** permite que los datos estén disponibles para todos los componentes hijos dentro de su árbol.

```jsx
function Proveedor({ children }) {
  const valor = "Hola, desde el contexto!";
  return <MiContexto.Provider value={valor}>{children}</MiContexto.Provider>;
}
```

#### **3. Consumidor del contexto**

Los componentes hijos pueden acceder al contexto con el hook `useContext`.

```jsx
import React, { useContext } from "react";

function Consumidor() {
  const valor = useContext(MiContexto);
  return <h1>{valor}</h1>;
}
```

#### **4. Uso completo**

```jsx
import React, { createContext, useContext } from "react";

// Crear el contexto
const MiContexto = createContext();

function Proveedor({ children }) {
  const valor = "Hola desde el contexto!";
  return <MiContexto.Provider value={valor}>{children}</MiContexto.Provider>;
}

function Consumidor() {
  const valor = useContext(MiContexto);
  return <h1>{valor}</h1>;
}

export default function App() {
  return (
    <Proveedor>
      <Consumidor />
    </Proveedor>
  );
}
```

### **Ejemplo práctico: Tema global**

```jsx
import React, { createContext, useContext, useState } from "react";

// Crear contexto
const TemaContexto = createContext();

function ProveedorDeTema({ children }) {
  const [tema, setTema] = useState("claro");

  function alternarTema() {
    setTema((temaActual) => (temaActual === "claro" ? "oscuro" : "claro"));
  }

  return (
    <TemaContexto.Provider value={{ tema, alternarTema }}>
      {children}
    </TemaContexto.Provider>
  );
}

function BotonDeTema() {
  const { tema, alternarTema } = useContext(TemaContexto);

  return (
    <button onClick={alternarTema}>
      Cambiar a {tema === "claro" ? "oscuro" : "claro"}
    </button>
  );
}

function App() {
  return (
    <ProveedorDeTema>
      <BotonDeTema />
    </ProveedorDeTema>
  );
}

export default App;
```

---

## **Resumen**

| Método                 | Usos Principales                                                                |
| ---------------------- | ------------------------------------------------------------------------------- |
| **Props**              | Pasar datos del componente padre al hijo.                                       |
| **Funciones en Props** | Pasar datos o eventos del componente hijo al padre.                             |
| **Context API**        | Compartir estados o datos globales entre múltiples componentes sin pasar props. |

---

# Mini Guía: Uso combinado de `useEffect` y `useContext` en Componentes Funcionales

Cuando trabajamos con **componentes funcionales**, combinar `useEffect` y `useContext` puede ser muy útil para manejar efectos secundarios y consumir datos globales desde un contexto.

A continuación, explicamos cómo integrarlos de manera efectiva.

---

## **1. Conceptos clave**

### **`useEffect`**

- **¿Qué hace?** Permite realizar efectos secundarios en los componentes funcionales (similar a los métodos del ciclo de vida como `componentDidMount` y `componentDidUpdate` en componentes de clase).
- **Ejemplos comunes de uso:**
  - Fetch de datos desde APIs.
  - Configuración de eventos del navegador.
  - Limpieza de recursos (desmontaje del componente).

### **`useContext`**

- **¿Qué hace?** Permite acceder al valor de un contexto compartido en cualquier componente funcional.
- **Ejemplo común:** Compartir un estado global (como el tema, usuario autenticado, etc.) sin necesidad de pasar props.

---

## **2. Ejemplo práctico: Fetch de datos con contexto**

En este ejemplo, usamos un contexto para compartir los datos obtenidos desde una API con varios componentes, y utilizamos `useEffect` para realizar el fetch de datos cuando el componente se monta.

### **Paso 1: Crear el contexto**

Primero, creamos un contexto y un proveedor.

```jsx
import React, { createContext, useState, useEffect } from "react";

const DataContext = createContext();

function DataProvider({ children }) {
  const [data, setData] = useState([]);
  const [loading, setLoading] = useState(true);

  // Fetch de datos usando useEffect
  useEffect(() => {
    async function fetchData() {
      try {
        const response = await fetch(
          "https://jsonplaceholder.typicode.com/posts"
        );
        const result = await response.json();
        setData(result);
      } catch (error) {
        console.error("Error al obtener los datos:", error);
      } finally {
        setLoading(false);
      }
    }

    fetchData();
  }, []); // El array vacío asegura que esto ocurra solo al montar el componente

  return (
    <DataContext.Provider value={{ data, loading }}>
      {children}
    </DataContext.Provider>
  );
}

export { DataContext, DataProvider };
```

### **Paso 2: Consumir el contexto con `useContext`**

Creamos un componente que consuma los datos desde el contexto.

```jsx
import React, { useContext } from "react";
import { DataContext } from "./DataProvider";

function ListaDeDatos() {
  const { data, loading } = useContext(DataContext);

  if (loading) return <p>Cargando datos...</p>;

  return (
    <ul>
      {data.slice(0, 5).map((item) => (
        <li key={item.id}>{item.title}</li>
      ))}
    </ul>
  );
}

export default ListaDeDatos;
```

### **Paso 3: Integrar todo en la aplicación**

Incluimos el proveedor en la jerarquía de componentes.

```jsx
import React from "react";
import { DataProvider } from "./DataProvider";
import ListaDeDatos from "./ListaDeDatos";

function App() {
  return (
    <DataProvider>
      <h1>Mis Datos</h1>
      <ListaDeDatos />
    </DataProvider>
  );
}

export default App;
```

---

## **3. Limpieza en `useEffect`**

Cuando usas `useEffect` para manejar efectos secundarios, es importante limpiar recursos para evitar problemas como fugas de memoria o comportamientos inesperados.

### **Ejemplo con eventos del navegador**

Supongamos que queremos manejar eventos de scroll globales y compartir el valor actual del scroll mediante un contexto.

#### **Paso 1: Crear el contexto**

```jsx
import React, { createContext, useState, useEffect } from "react";

const ScrollContext = createContext();

function ScrollProvider({ children }) {
  const [scrollY, setScrollY] = useState(0);

  useEffect(() => {
    function manejarScroll() {
      setScrollY(window.scrollY);
    }

    window.addEventListener("scroll", manejarScroll);

    // Limpieza del evento al desmontar el componente
    return () => {
      window.removeEventListener("scroll", manejarScroll);
    };
  }, []); // Solo se ejecuta al montar y desmontar

  return (
    <ScrollContext.Provider value={scrollY}>{children}</ScrollContext.Provider>
  );
}

export { ScrollContext, ScrollProvider };
```

#### **Paso 2: Consumir el contexto**

```jsx
import React, { useContext } from "react";
import { ScrollContext } from "./ScrollProvider";

function MostrarScroll() {
  const scrollY = useContext(ScrollContext);

  return <p>Scroll actual: {scrollY}px</p>;
}

export default MostrarScroll;
```

#### **Paso 3: Integrar en la aplicación**

```jsx
import React from "react";
import { ScrollProvider } from "./ScrollProvider";
import MostrarScroll from "./MostrarScroll";

function App() {
  return (
    <ScrollProvider>
      <h1>Ejemplo de Scroll</h1>
      <div style={{ height: "200vh", padding: "20px" }}>
        <MostrarScroll />
      </div>
    </ScrollProvider>
  );
}

export default App;
```

---

## **4. Buenas prácticas**

1. **Mantén el efecto enfocado:**

   - Un `useEffect` debe manejar un único efecto secundario (por ejemplo, fetch de datos, suscripción a eventos, etc.).
   - Divide en varios `useEffect` si es necesario.

2. **Manejo de dependencias:**

   - Asegúrate de incluir todas las dependencias necesarias en el array del segundo argumento de `useEffect`.

   ```jsx
   useEffect(() => {
     console.log(data); // Si `data` cambia, este efecto se ejecutará nuevamente
   }, [data]);
   ```

3. **Limpieza obligatoria:**

   - Siempre incluye una función de limpieza para manejar eventos o recursos que puedan quedar abiertos.

4. **Evita usar contexto para todo:**
   - La Context API es poderosa, pero no es ideal para estados que cambian con alta frecuencia (como una animación). Para eso, considera bibliotecas como Redux o Zustand.

---

# 8. Estilizado en React

El estilizado en React ofrece múltiples formas de manejar estilos, desde métodos tradicionales como clases de CSS hasta soluciones modernas como **CSS-in-JS**. A continuación, veremos las principales opciones para estilizar componentes en React.

---

## **1. CSS en línea y clases**

### **CSS en línea**

React permite aplicar estilos directamente a los elementos mediante el atributo `style`. Los estilos deben escribirse como un objeto de JavaScript.

#### **Ejemplo básico: Estilo en línea**

```jsx
function Boton() {
  const estiloBoton = {
    backgroundColor: "blue",
    color: "white",
    padding: "10px 20px",
    border: "none",
    borderRadius: "5px",
  };

  return <button style={estiloBoton}>Haz clic aquí</button>;
}
```

**Nota:**

- Las propiedades de CSS deben escribirse en formato camelCase (por ejemplo, `backgroundColor` en lugar de `background-color`).
- Este método es ideal para estilos simples o dinámicos que dependen del estado.

---

### **Uso de clases CSS**

Puedes crear un archivo `.css` separado y vincular clases a los elementos mediante el atributo `className`.

#### **Ejemplo básico: Clases CSS**

**Archivo `Boton.css`:**

```css
.boton {
  background-color: blue;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  font-size: 16px;
}
```

**Componente `Boton.js`:**

```jsx
import "./Boton.css";

function Boton() {
  return <button className="boton">Haz clic aquí</button>;
}

export default Boton;
```

**Ventaja:** Separación clara entre lógica (JS) y estilos (CSS).

---

## **2. CSS Modules**

Los **CSS Modules** permiten un estilo localizado por defecto, evitando conflictos de nombres en clases CSS. Cada archivo CSS se convierte en un módulo que expone sus clases como objetos de JavaScript.

#### **Configuración de CSS Modules**

1. Crea un archivo con la extensión `.module.css`.
2. Importa el archivo como un módulo en tu componente.

#### **Ejemplo básico: CSS Modules**

**Archivo `Boton.module.css`:**

```css
.boton {
  background-color: green;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  font-size: 16px;
}
```

**Componente `Boton.js`:**

```jsx
import styles from "./Boton.module.css";

function Boton() {
  return <button className={styles.boton}>Haz clic aquí</button>;
}

export default Boton;
```

**Ventaja:**

- Evita el solapamiento de estilos, ya que los nombres de las clases son únicos a nivel de proyecto.
- Ideal para proyectos grandes o componentes reutilizables.

---

## **3. Styled-components y CSS-in-JS**

### **Styled-components**

Styled-components es una biblioteca que permite escribir estilos directamente en los componentes usando JavaScript. Esto se conoce como **CSS-in-JS**.

#### **Instalación:**

```bash
npm install styled-components
```

#### **Ejemplo básico: Styled-components**

```jsx
import styled from "styled-components";

const Boton = styled.button`
  background-color: purple;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  font-size: 16px;

  &:hover {
    background-color: darkviolet;
  }
`;

function App() {
  return <Boton>Haz clic aquí</Boton>;
}

export default App;
```

**Ventajas:**

- Los estilos están directamente relacionados con los componentes, lo que facilita su mantenimiento.
- Admite herencia, pseudoclases, y temas.

---

### **CSS-in-JS con Emotion**

Emotion es otra biblioteca popular para CSS-in-JS, similar a styled-components.

#### **Instalación:**

```bash
npm install @emotion/react @emotion/styled
```

#### **Ejemplo básico: Emotion**

```jsx
/** @jsxImportSource @emotion/react */
import { css } from "@emotion/react";

function Boton() {
  const estiloBoton = css`
    background-color: orange;
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 5px;
    font-size: 16px;

    &:hover {
      background-color: darkorange;
    }
  `;

  return <button css={estiloBoton}>Haz clic aquí</button>;
}

export default Boton;
```

---

## **4. Integración con Librerías de Estilo**

### **TailwindCSS**

TailwindCSS es una biblioteca de utilidades CSS que permite estilizar componentes rápidamente mediante clases predefinidas.

#### **Instalación:**

1. Configura TailwindCSS según la [documentación oficial](https://tailwindcss.com/docs/installation).
2. Usa clases directamente en tus componentes.

#### **Ejemplo básico: TailwindCSS**

```jsx
function Boton() {
  return (
    <button className="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-700">
      Haz clic aquí
    </button>
  );
}
```

**Ventajas:**

- Prototipado rápido.
- No requiere archivos CSS adicionales.
- Muy flexible y personalizable.

---

### **Bootstrap**

Bootstrap es una de las bibliotecas de CSS más populares, ideal para componentes preestilizados y responsive.

#### **Instalación:**

```bash
npm install bootstrap
```

Importa los estilos en tu proyecto:

```jsx
import "bootstrap/dist/css/bootstrap.min.css";
```

#### **Ejemplo básico: Bootstrap**

```jsx
function Boton() {
  return <button className="btn btn-primary">Haz clic aquí</button>;
}

export default Boton;
```

**Ventajas:**

- Acelera el desarrollo gracias a sus componentes predefinidos.
- Perfecto para proyectos donde el diseño estándar es suficiente.

---

## **5. Comparativa rápida de métodos**

| Método                | Ventajas                                                        | Desventajas                                               |
| --------------------- | --------------------------------------------------------------- | --------------------------------------------------------- |
| **CSS en línea**      | Ideal para estilos dinámicos.                                   | Difícil de reutilizar, menos mantenible.                  |
| **Clases CSS**        | Separación de lógica y estilos.                                 | Puede generar conflictos de nombres en proyectos grandes. |
| **CSS Modules**       | Evita conflictos de nombres.                                    | Configuración adicional en algunos entornos.              |
| **Styled-components** | Estilos encapsulados en componentes, dinámicos y reutilizables. | Introduce dependencia externa.                            |
| **TailwindCSS**       | Prototipado rápido y responsive.                                | Puede ser difícil de leer para proyectos muy complejos.   |
| **Bootstrap**         | Fácil de usar, componentes listos para producción.              | Estilo genérico, menos personalizable.                    |

---

## **Conclusión**

- **Proyectos pequeños:** Clases CSS o CSS Modules funcionan perfectamente.
- **Proyectos medianos o grandes:** CSS Modules o Styled-components son opciones ideales.
- **Prototipado rápido:** TailwindCSS o Bootstrap aceleran el desarrollo.
- **Proyectos modernos:** CSS-in-JS (styled-components o Emotion) brinda flexibilidad y escalabilidad.

---

# 9. Manejo del Estado Global en React

El manejo del estado global es crucial en aplicaciones React de gran tamaño, ya que permite compartir datos y lógica entre múltiples componentes sin necesidad de pasar props manualmente en varios niveles. A continuación, exploramos las principales herramientas y técnicas.

---

## **1. Introducción a Redux**

Redux es una de las bibliotecas más populares para el manejo del estado global en React. Funciona bajo el principio de una **"única fuente de verdad"** donde el estado se almacena en un único objeto y se gestiona a través de un flujo predecible.

### **Conceptos clave en Redux**

1. **Store**: El almacenamiento centralizado de toda la información.
2. **Actions**: Objetos que describen qué tipo de cambio queremos realizar en el estado.
3. **Reducers**: Funciones puras que especifican cómo el estado debe cambiar en respuesta a una acción.
4. **Dispatch**: Método para enviar una acción al store.

### **Instalación**

1. Instala Redux y React Redux:

   ```bash
   npm install redux react-redux
   ```

2. Configura el store:
   **Archivo `store.js`:**

   ```jsx
   import { createStore } from "redux";

   const initialState = {
     contador: 0,
   };

   function contadorReducer(state = initialState, action) {
     switch (action.type) {
       case "INCREMENTAR":
         return { ...state, contador: state.contador + 1 };
       case "DECREMENTAR":
         return { ...state, contador: state.contador - 1 };
       default:
         return state;
     }
   }

   const store = createStore(contadorReducer);

   export default store;
   ```

3. Conecta el store a la aplicación:
   **Archivo `index.js`:**

   ```jsx
   import React from "react";
   import ReactDOM from "react-dom";
   import { Provider } from "react-redux";
   import store from "./store";
   import App from "./App";

   ReactDOM.render(
     <Provider store={store}>
       <App />
     </Provider>,
     document.getElementById("root")
   );
   ```

4. Usa el estado y las acciones en los componentes:
   **Ejemplo:**

   ```jsx
   import React from "react";
   import { useSelector, useDispatch } from "react-redux";

   function Contador() {
     const contador = useSelector((state) => state.contador);
     const dispatch = useDispatch();

     return (
       <div>
         <h1>Contador: {contador}</h1>
         <button onClick={() => dispatch({ type: "INCREMENTAR" })}>+</button>
         <button onClick={() => dispatch({ type: "DECREMENTAR" })}>-</button>
       </div>
     );
   }

   export default Contador;
   ```

---

## **2. Uso de Context API como alternativa**

La **Context API** es una herramienta integrada en React que permite manejar el estado global de manera más simple y ligera que Redux. Es ideal para aplicaciones pequeñas o medianas.

### **Creando un contexto global**

1. **Crear el contexto y proveedor:**
   **Archivo `ContadorContext.js`:**

   ```jsx
   import React, { createContext, useState } from "react";

   const ContadorContext = createContext();

   function ContadorProvider({ children }) {
     const [contador, setContador] = useState(0);

     const incrementar = () => setContador(contador + 1);
     const decrementar = () => setContador(contador - 1);

     return (
       <ContadorContext.Provider value={{ contador, incrementar, decrementar }}>
         {children}
       </ContadorContext.Provider>
     );
   }

   export { ContadorContext, ContadorProvider };
   ```

2. **Consumir el contexto en los componentes:**
   **Ejemplo:**

   ```jsx
   import React, { useContext } from "react";
   import { ContadorContext } from "./ContadorContext";

   function Contador() {
     const { contador, incrementar, decrementar } = useContext(ContadorContext);

     return (
       <div>
         <h1>Contador: {contador}</h1>
         <button onClick={incrementar}>+</button>
         <button onClick={decrementar}>-</button>
       </div>
     );
   }

   export default Contador;
   ```

3. **Incluir el proveedor en la aplicación:**

   ```jsx
   import React from "react";
   import ReactDOM from "react-dom";
   import { ContadorProvider } from "./ContadorContext";
   import Contador from "./Contador";

   ReactDOM.render(
     <ContadorProvider>
       <Contador />
     </ContadorProvider>,
     document.getElementById("root")
   );
   ```

**Ventajas:**

- No requiere configuraciones adicionales.
- Es simple y adecuado para aplicaciones con un estado global no muy complejo.

---

## **3. Librerías modernas: Zustand y Jotai**

### **Zustand**

Zustand es una librería ligera para el manejo del estado global. Es más simple que Redux y más flexible que Context API.

#### **Instalación:**

```bash
npm install zustand
```

#### **Ejemplo básico con Zustand:**

```jsx
import create from "zustand";

const useStore = create((set) => ({
  contador: 0,
  incrementar: () => set((state) => ({ contador: state.contador + 1 })),
  decrementar: () => set((state) => ({ contador: state.contador - 1 })),
}));

function Contador() {
  const { contador, incrementar, decrementar } = useStore();

  return (
    <div>
      <h1>Contador: {contador}</h1>
      <button onClick={incrementar}>+</button>
      <button onClick={decrementar}>-</button>
    </div>
  );
}

export default Contador;
```

**Ventajas:**

- Sintaxis simple.
- Escalable y reactivo.

---

### **Jotai**

Jotai es una librería moderna para manejar el estado global basada en átomos (unidades de estado).

#### **Instalación:**

```bash
npm install jotai
```

#### **Ejemplo básico con Jotai:**

```jsx
import { atom, useAtom } from "jotai";

const contadorAtom = atom(0);

function Contador() {
  const [contador, setContador] = useAtom(contadorAtom);

  return (
    <div>
      <h1>Contador: {contador}</h1>
      <button onClick={() => setContador((c) => c + 1)}>+</button>
      <button onClick={() => setContador((c) => c - 1)}>-</button>
    </div>
  );
}

export default Contador;
```

**Ventajas:**

- Fácil de aprender.
- Enfoque moderno y minimalista.

---

## **Comparativa rápida**

| Herramienta     | Ventajas                             | Desventajas                              |
| --------------- | ------------------------------------ | ---------------------------------------- |
| **Redux**       | Escalabilidad, ecosistema robusto.   | Configuración compleja, boilerplate.     |
| **Context API** | Sin dependencias externas, simple.   | No escalable para estados muy complejos. |
| **Zustand**     | Sintaxis limpia, reactivo, flexible. | Comunidad más pequeña que Redux.         |
| **Jotai**       | Moderno, minimalista, reactivo.      | Similar a Zustand, menos documentación.  |

---

## **Conclusión**

- **Aplicaciones pequeñas o medianas:** Context API, Zustand o Jotai son ideales por su simplicidad.
- **Aplicaciones grandes:** Redux es el estándar gracias a su robustez y ecosistema.
- **Alternativa moderna:** Zustand o Jotai ofrecen un balance entre simplicidad y escalabilidad.

---

# 10. Enrutamiento en React

El enrutamiento en React permite navegar entre diferentes vistas o páginas dentro de una aplicación sin necesidad de recargar toda la página, lo que mejora la experiencia del usuario. La herramienta más comúnmente utilizada para el enrutamiento en React es **React Router**.

---

## **1. Introducción a React Router**

**React Router** es una biblioteca estándar para manejar el enrutamiento en aplicaciones React. Proporciona una manera sencilla de definir rutas y gestionar la navegación entre ellas, sin recargar la página completa.

### **Instalación:**

Para instalar React Router, usa el siguiente comando:

```bash
npm install react-router-dom
```

---

## **2. Configuración de Rutas**

El enrutamiento en React Router se basa en los siguientes componentes clave:

- **BrowserRouter**: Envoltorio para la aplicación, maneja el historial de navegación (basado en el historial de URL del navegador).
- **Route**: Define las rutas y los componentes que deben renderizarse cuando se accede a esa ruta.
- **Switch**: Solo renderiza la primera ruta coincidente dentro de él (opcional en React Router v6+).

### **Ejemplo básico de rutas:**

1. **Configura el enrutador:**
   **Archivo `index.js`:**

   ```jsx
   import React from "react";
   import ReactDOM from "react-dom";
   import { BrowserRouter } from "react-router-dom";
   import App from "./App";

   ReactDOM.render(
     <BrowserRouter>
       <App />
     </BrowserRouter>,
     document.getElementById("root")
   );
   ```

2. **Definir las rutas:**
   **Archivo `App.js`:**

   ```jsx
   import React from "react";
   import { Route, Routes } from "react-router-dom";
   import Home from "./Home";
   import About from "./About";

   function App() {
     return (
       <div>
         <h1>Mi Aplicación React</h1>
         <Routes>
           <Route path="/" element={<Home />} />
           <Route path="/about" element={<About />} />
         </Routes>
       </div>
     );
   }

   export default App;
   ```

3. **Componentes para las rutas:**
   **Archivo `Home.js`:**

   ```jsx
   import React from "react";

   function Home() {
     return <h2>Página de inicio</h2>;
   }

   export default Home;
   ```

   **Archivo `About.js`:**

   ```jsx
   import React from "react";

   function About() {
     return <h2>Acerca de nosotros</h2>;
   }

   export default About;
   ```

### **Nota**: React Router v6+ usa `Routes` en lugar de `Switch`, y el atributo `component` en las rutas ha sido reemplazado por `element`.

---

## **3. Rutas Protegidas y Redirecciones**

### **Rutas protegidas**

Las **rutas protegidas** son aquellas que requieren una condición previa para ser accedidas, como la autenticación de un usuario. Si el usuario no está autenticado, se les puede redirigir a una página de login.

### **Ejemplo de rutas protegidas:**

1. **Componente de ruta protegida:**

   ```jsx
   import { Navigate } from "react-router-dom";

   function RutaProtegida({ children }) {
     const isAuthenticated = false; // Cambia esto según tu lógica de autenticación

     if (!isAuthenticated) {
       return <Navigate to="/login" />;
     }

     return children;
   }
   ```

2. **Uso de rutas protegidas:**
   **Archivo `App.js`:**

   ```jsx
   import React from "react";
   import { Routes, Route } from "react-router-dom";
   import Home from "./Home";
   import About from "./About";
   import Login from "./Login";
   import RutaProtegida from "./RutaProtegida";

   function App() {
     return (
       <div>
         <Routes>
           <Route path="/" element={<Home />} />
           <Route path="/about" element={<About />} />
           <Route
             path="/protected"
             element={
               <RutaProtegida>
                 <h2>Ruta protegida</h2>
               </RutaProtegida>
             }
           />
           <Route path="/login" element={<Login />} />
         </Routes>
       </div>
     );
   }

   export default App;
   ```

3. **Componente de Login:**
   **Archivo `Login.js`:**

   ```jsx
   import React from "react";

   function Login() {
     return <h2>Por favor, inicia sesión</h2>;
   }

   export default Login;
   ```

### **Redirecciones**

Para redirigir a un usuario automáticamente, puedes usar el componente **`Navigate`** de React Router.

#### **Ejemplo de redirección:**

```jsx
import { Navigate } from "react-router-dom";

function ComponenteRedirigir() {
  // Lógica de redirección
  const isAuthenticated = false;

  if (!isAuthenticated) {
    return <Navigate to="/login" />;
  }

  return <h1>Bienvenido a la página protegida</h1>;
}
```

---

## **4. Navegación entre Páginas**

### **Uso de enlaces de navegación**

React Router proporciona el componente **`Link`** para crear enlaces de navegación entre páginas sin recargar la página.

#### **Ejemplo de navegación:**

```jsx
import { Link } from "react-router-dom";

function Navegacion() {
  return (
    <nav>
      <ul>
        <li>
          <Link to="/">Inicio</Link>
        </li>
        <li>
          <Link to="/about">Acerca de</Link>
        </li>
        <li>
          <Link to="/login">Login</Link>
        </li>
      </ul>
    </nav>
  );
}

export default Navegacion;
```

### **Navegación programática**

A veces necesitarás navegar desde un componente sin usar un enlace visual (por ejemplo, después de un formulario de inicio de sesión).

React Router ofrece el hook **`useNavigate`** para hacer esto.

#### **Ejemplo de navegación programática:**

```jsx
import { useNavigate } from "react-router-dom";

function Login() {
  const navigate = useNavigate();

  const handleLogin = () => {
    // Lógica de autenticación
    navigate("/protected"); // Redirigir a la ruta protegida
  };

  return <button onClick={handleLogin}>Iniciar sesión</button>;
}
```

---

## **Comparativa entre React Router y otras alternativas**

| Librería                   | Características                             | Ventajas                                | Desventajas                       |
| -------------------------- | ------------------------------------------- | --------------------------------------- | --------------------------------- |
| **React Router**           | Enrutamiento basado en componentes de React | Popular, flexible, bien documentado.    | Requiere configuración adicional. |
| **Next.js (enrutamiento)** | Enrutamiento basado en archivos y en SSR    | Optimización de rendimiento, SSR fácil. | Menos flexible que React Router.  |
| **Reach Router**           | Ligero y accesible, similar a React Router  | API simple, accesibilidad incorporada.  | Menos popular y mantenido.        |

---

## **Conclusión**

El enrutamiento en React es una pieza esencial para crear aplicaciones con múltiples vistas. **React Router** es la opción más popular y poderosa, pero hay alternativas como **Next.js** o **Reach Router** dependiendo de las necesidades específicas de tu proyecto.

**Recomendación**: Usa **React Router** para la mayoría de los casos, ya que es flexible, ampliamente utilizado y bien soportado.

---

# 11. Formularios en React

El manejo de formularios es una parte fundamental en el desarrollo de aplicaciones React. A continuación, exploraremos los conceptos básicos y avanzados para gestionar formularios, desde los formularios controlados y no controlados hasta el uso de librerías como **Formik** y **React Hook Form**.

---

## **1. Manejo de Formularios Controlados y No Controlados**

### **Formularios controlados**

En un formulario controlado, el estado de los inputs se gestiona dentro del componente React mediante el uso de estados locales. Cada cambio en el input se refleja en el estado, y viceversa.

#### **Ejemplo básico de formulario controlado:**

```jsx
import React, { useState } from "react";

function FormularioControlado() {
  const [nombre, setNombre] = useState("");

  const handleChange = (e) => {
    setNombre(e.target.value);
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    alert(`Nombre enviado: ${nombre}`);
  };

  return (
    <form onSubmit={handleSubmit}>
      <label>
        Nombre:
        <input type="text" value={nombre} onChange={handleChange} />
      </label>
      <button type="submit">Enviar</button>
    </form>
  );
}

export default FormularioControlado;
```

### **Formularios no controlados**

En un formulario no controlado, los valores de los inputs no se gestionan mediante el estado de React, sino que se acceden directamente a través de referencias (`ref`).

#### **Ejemplo básico de formulario no controlado:**

```jsx
import React, { useRef } from "react";

function FormularioNoControlado() {
  const inputRef = useRef();

  const handleSubmit = (e) => {
    e.preventDefault();
    alert(`Nombre enviado: ${inputRef.current.value}`);
  };

  return (
    <form onSubmit={handleSubmit}>
      <label>
        Nombre:
        <input type="text" ref={inputRef} />
      </label>
      <button type="submit">Enviar</button>
    </form>
  );
}

export default FormularioNoControlado;
```

### **Comparativa:**

| **Aspecto**        | **Formulario Controlado**                      | **Formulario No Controlado**                        |
| ------------------ | ---------------------------------------------- | --------------------------------------------------- |
| Manejo de datos    | Usa el estado de React                         | Usa referencias (`ref`)                             |
| Reactividad        | Muy reactivo (cada cambio actualiza el estado) | Menos reactivo (solo se accede cuando es necesario) |
| Escenarios ideales | Formularios complejos con validación dinámica  | Formularios simples o con muchos campos             |

---

## **2. Validaciones Básicas y Avanzadas**

### **Validaciones básicas**

Puedes realizar validaciones básicas directamente en los manejadores de eventos como `onChange` o `onSubmit`.

#### **Ejemplo básico de validación:**

```jsx
function FormularioValidacionBasica() {
  const [nombre, setNombre] = useState("");
  const [error, setError] = useState("");

  const handleChange = (e) => {
    setNombre(e.target.value);

    if (e.target.value.length < 3) {
      setError("El nombre debe tener al menos 3 caracteres");
    } else {
      setError("");
    }
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    if (!error && nombre) {
      alert("Formulario enviado");
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <label>
        Nombre:
        <input type="text" value={nombre} onChange={handleChange} />
      </label>
      {error && <p style={{ color: "red" }}>{error}</p>}
      <button type="submit" disabled={!!error}>
        Enviar
      </button>
    </form>
  );
}
```

### **Validaciones avanzadas**

Las validaciones más complejas pueden incluir reglas como:

- Verificación de formatos específicos (e.g., email).
- Comparación de campos (e.g., confirmar contraseña).
- Validación de múltiples condiciones.

Para estas validaciones, es recomendable usar librerías que simplifiquen el proceso.

---

## **3. Librerías para Formularios**

### **Formik**

Formik es una de las librerías más populares para manejar formularios en React. Facilita el manejo del estado del formulario, las validaciones y la lógica de envío.

#### **Instalación:**

```bash
npm install formik
```

#### **Ejemplo básico con Formik:**

```jsx
import React from "react";
import { useFormik } from "formik";

function FormularioFormik() {
  const formik = useFormik({
    initialValues: {
      nombre: "",
      email: "",
    },
    onSubmit: (values) => {
      alert(JSON.stringify(values, null, 2));
    },
  });

  return (
    <form onSubmit={formik.handleSubmit}>
      <label>
        Nombre:
        <input
          type="text"
          name="nombre"
          value={formik.values.nombre}
          onChange={formik.handleChange}
        />
      </label>
      <label>
        Email:
        <input
          type="email"
          name="email"
          value={formik.values.email}
          onChange={formik.handleChange}
        />
      </label>
      <button type="submit">Enviar</button>
    </form>
  );
}

export default FormularioFormik;
```

### **React Hook Form**

React Hook Form es otra librería popular que utiliza referencias (`ref`) para gestionar formularios, lo que la hace más eficiente en términos de rendimiento.

#### **Instalación:**

```bash
npm install react-hook-form
```

#### **Ejemplo básico con React Hook Form:**

```jsx
import React from "react";
import { useForm } from "react-hook-form";

function FormularioReactHookForm() {
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm();

  const onSubmit = (data) => {
    alert(JSON.stringify(data));
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <label>
        Nombre:
        <input
          {...register("nombre", { required: "El nombre es obligatorio" })}
        />
        {errors.nombre && (
          <p style={{ color: "red" }}>{errors.nombre.message}</p>
        )}
      </label>
      <label>
        Email:
        <input
          {...register("email", {
            required: "El email es obligatorio",
            pattern: {
              value: /^[^\s@]+@[^\s@]+\.[^\s@]+$/,
              message: "Formato de email inválido",
            },
          })}
        />
        {errors.email && <p style={{ color: "red" }}>{errors.email.message}</p>}
      </label>
      <button type="submit">Enviar</button>
    </form>
  );
}

export default FormularioReactHookForm;
```

---

## **Comparativa entre Formik y React Hook Form**

| **Aspecto**          | **Formik**                                       | **React Hook Form**                     |
| -------------------- | ------------------------------------------------ | --------------------------------------- |
| **Manejo de estado** | Basado en estado de React                        | Basado en referencias                   |
| **Facilidad de uso** | Sencillo pero requiere configuración extra       | Intuitivo y minimalista                 |
| **Rendimiento**      | Puede ser menos eficiente en formularios grandes | Altamente eficiente                     |
| **Validaciones**     | Soporta esquemas como Yup                        | Soporta validaciones nativas o externas |

---

## **Conclusión**

React ofrece flexibilidad para manejar formularios, desde opciones nativas con formularios controlados y no controlados hasta el uso de librerías avanzadas como **Formik** y **React Hook Form**. La elección dependerá de la complejidad de tu formulario y tus preferencias en cuanto a rendimiento y facilidad de uso.

---

# 12. Llamadas a APIs y Manejo de Datos

El manejo de datos externos y la interacción con APIs son aspectos esenciales en el desarrollo de aplicaciones React modernas. En esta sección aprenderemos a usar herramientas nativas como `fetch`, librerías como `axios`, y técnicas avanzadas con hooks como `useEffect` y librerías como **React Query** o **SWR**.

---

## **1. Uso de fetch y axios**

### **Uso de fetch**

`fetch` es una API nativa de JavaScript para realizar solicitudes HTTP. Aunque es flexible, requiere manejar manualmente el procesamiento de datos y errores.

#### **Ejemplo básico con fetch:**

```jsx
import React, { useState, useEffect } from "react";

function FetchExample() {
  const [data, setData] = useState(null);
  const [error, setError] = useState(null);

  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/posts")
      .then((response) => {
        if (!response.ok) {
          throw new Error("Error en la solicitud");
        }
        return response.json();
      })
      .then((data) => setData(data))
      .catch((error) => setError(error.message));
  }, []);

  if (error) {
    return <p>Error: {error}</p>;
  }

  if (!data) {
    return <p>Cargando...</p>;
  }

  return (
    <ul>
      {data.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}

export default FetchExample;
```

---

### **Uso de axios**

`axios` es una librería popular que simplifica las solicitudes HTTP y el manejo de errores.

#### **Instalación de axios:**

```bash
npm install axios
```

#### **Ejemplo básico con axios:**

```jsx
import React, { useState, useEffect } from "react";
import axios from "axios";

function AxiosExample() {
  const [data, setData] = useState(null);
  const [error, setError] = useState(null);

  useEffect(() => {
    axios
      .get("https://jsonplaceholder.typicode.com/posts")
      .then((response) => setData(response.data))
      .catch((error) => setError(error.message));
  }, []);

  if (error) {
    return <p>Error: {error}</p>;
  }

  if (!data) {
    return <p>Cargando...</p>;
  }

  return (
    <ul>
      {data.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}

export default AxiosExample;
```

---

## **2. Uso del Hook useEffect para Llamadas a APIs**

El Hook `useEffect` se utiliza para ejecutar efectos secundarios en componentes funcionales, como llamadas a APIs. Aquí hay algunos consejos importantes:

1. **Evitar llamadas infinitas**: Asegúrate de incluir una lista de dependencias vacía (`[]`) si quieres que la llamada se realice solo una vez al montar el componente.
2. **Cancelar solicitudes**: Usa `AbortController` o técnicas similares para evitar efectos secundarios no deseados al desmontar un componente.

#### **Ejemplo de manejo con useEffect y fetch:**

```jsx
import React, { useState, useEffect } from "react";

function ApiWithCleanup() {
  const [data, setData] = useState(null);
  const [error, setError] = useState(null);

  useEffect(() => {
    const controller = new AbortController();
    const signal = controller.signal;

    fetch("https://jsonplaceholder.typicode.com/posts", { signal })
      .then((response) => response.json())
      .then((data) => setData(data))
      .catch((error) => {
        if (error.name !== "AbortError") {
          setError(error.message);
        }
      });

    return () => controller.abort(); // Cancelar solicitud si el componente se desmonta
  }, []);

  if (error) {
    return <p>Error: {error}</p>;
  }

  if (!data) {
    return <p>Cargando...</p>;
  }

  return (
    <ul>
      {data.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}

export default ApiWithCleanup;
```

---

## **3. Gestión de Estado de Datos con Librerías Modernas**

### **React Query**

React Query es una librería que facilita la gestión del estado de los datos, el almacenamiento en caché y la sincronización con APIs.

#### **Instalación de React Query:**

```bash
npm install @tanstack/react-query
```

#### **Ejemplo básico con React Query:**

```jsx
import React from "react";
import {
  useQuery,
  QueryClient,
  QueryClientProvider,
} from "@tanstack/react-query";
import axios from "axios";

const queryClient = new QueryClient();

function FetchPosts() {
  const { data, error, isLoading } = useQuery(["posts"], () =>
    axios
      .get("https://jsonplaceholder.typicode.com/posts")
      .then((res) => res.data)
  );

  if (isLoading) return <p>Cargando...</p>;
  if (error) return <p>Error: {error.message}</p>;

  return (
    <ul>
      {data.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <FetchPosts />
    </QueryClientProvider>
  );
}

export default App;
```

---

### **SWR**

SWR (Stale-While-Revalidate) es otra librería moderna para la gestión de datos, ideal para sincronizar datos con APIs de forma eficiente.

#### **Instalación de SWR:**

```bash
npm install swr
```

#### **Ejemplo básico con SWR:**

```jsx
import useSWR from "swr";
import axios from "axios";

const fetcher = (url) => axios.get(url).then((res) => res.data);

function SwrExample() {
  const { data, error } = useSWR(
    "https://jsonplaceholder.typicode.com/posts",
    fetcher
  );

  if (error) return <p>Error: {error.message}</p>;
  if (!data) return <p>Cargando...</p>;

  return (
    <ul>
      {data.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}

export default SwrExample;
```

---

## **Comparativa entre React Query y SWR**

| **Aspecto**            | **React Query**                                | **SWR**                                        |
| ---------------------- | ---------------------------------------------- | ---------------------------------------------- |
| **Caché**              | Avanzado, con control total                    | Sencillo, basado en "stale-while-revalidate"   |
| **Configuración**      | Requiere un `QueryClient`                      | Más sencillo, no necesita configuración global |
| **Características**    | Reintentos, actualizaciones automáticas        | Centrado en simplicidad y eficiencia           |
| **Escenarios ideales** | Aplicaciones complejas con múltiples endpoints | Aplicaciones ligeras o proyectos simples       |

---

## **Conclusión**

- **Fetch** y **axios** son ideales para tareas básicas o si prefieres un control completo.
- **React Query** y **SWR** ofrecen soluciones más modernas y eficientes para la gestión de datos en aplicaciones complejas.
- La elección depende del tamaño del proyecto, las necesidades de sincronización de datos y tus preferencias de desarrollo.

---

# 13. Pruebas en React

Las pruebas son fundamentales para garantizar que una aplicación funcione correctamente, detectar errores antes de llegar a producción y mantener un código mantenible. En esta sección aprenderemos cómo realizar pruebas unitarias, de componentes, y de integración en React.

---

## **1. Introducción a Pruebas Unitarias con Jest**

**Jest** es un framework de pruebas de JavaScript ampliamente utilizado para realizar pruebas unitarias, desarrollado por Facebook. Viene integrado con aplicaciones creadas con **Create React App**.

### **Instalación de Jest (si no está instalado):**

```bash
npm install --save-dev jest
```

### **Ejemplo básico de prueba unitaria con Jest:**

Supongamos que tienes una función que suma dos números:

```javascript
// sum.js
export const sum = (a, b) => a + b;
```

#### **Escribiendo una prueba para la función:**

```javascript
// sum.test.js
import { sum } from "./sum";

test("suma de 2 + 3 debe ser 5", () => {
  expect(sum(2, 3)).toBe(5);
});
```

### **Ejecutar las pruebas:**

```bash
npm test
```

---

## **2. Testing de Componentes con React Testing Library**

**React Testing Library** es una herramienta enfocada en pruebas de componentes React de una manera que simula la interacción del usuario.

### **Instalación:**

React Testing Library ya está incluida en proyectos creados con **Create React App**, pero si necesitas instalarla manualmente:

```bash
npm install --save-dev @testing-library/react @testing-library/jest-dom
```

### **Prueba básica de un componente:**

Supongamos que tienes el siguiente componente:

```jsx
// Greeting.js
import React from "react";

function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}

export default Greeting;
```

#### **Escribiendo una prueba para el componente:**

```javascript
// Greeting.test.js
import { render, screen } from "@testing-library/react";
import Greeting from "./Greeting";

test("muestra el saludo con el nombre proporcionado", () => {
  render(<Greeting name="John" />);
  const greetingElement = screen.getByText(/hello, john!/i);
  expect(greetingElement).toBeInTheDocument();
});
```

### **Simulando eventos en componentes:**

Si un componente tiene interacción, como clics o formularios:

```jsx
// Counter.js
import React, { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

export default Counter;
```

#### **Prueba del contador:**

```javascript
// Counter.test.js
import { render, screen, fireEvent } from "@testing-library/react";
import Counter from "./Counter";

test("incrementa el contador al hacer clic", () => {
  render(<Counter />);
  const button = screen.getByText(/increment/i);
  fireEvent.click(button);
  expect(screen.getByText(/count: 1/i)).toBeInTheDocument();
});
```

---

## **3. Pruebas de Integración y End-to-End con Cypress**

**Cypress** es una herramienta para pruebas end-to-end que interactúa con tu aplicación tal como lo haría un usuario. Es ideal para simular flujos completos.

### **Instalación de Cypress:**

```bash
npm install --save-dev cypress
```

### **Configuración inicial:**

1. Ejecuta el siguiente comando para abrir la interfaz de Cypress:
   ```bash
   npx cypress open
   ```
2. Esto creará automáticamente una carpeta `cypress` en tu proyecto.

### **Ejemplo básico de prueba con Cypress:**

Supongamos que tienes una aplicación React que muestra una lista de usuarios.

#### **Estructura básica de prueba:**

```javascript
// cypress/e2e/userList.cy.js
describe("Lista de usuarios", () => {
  it("debería mostrar la lista de usuarios", () => {
    cy.visit("http://localhost:3000"); // Cambia la URL según tu app
    cy.contains("Users").should("be.visible");
    cy.get(".user-item").should("have.length.greaterThan", 0);
  });
});
```

### **Ejemplo de flujo con formulario:**

Si tienes un formulario, puedes simular un flujo como este:

```javascript
// cypress/e2e/form.cy.js
describe("Formulario de inicio de sesión", () => {
  it("debería permitir al usuario iniciar sesión", () => {
    cy.visit("http://localhost:3000/login");
    cy.get('input[name="username"]').type("testuser");
    cy.get('input[name="password"]').type("password123");
    cy.get('button[type="submit"]').click();
    cy.contains("Bienvenido, testuser").should("be.visible");
  });
});
```

---

## **Buenas Prácticas en Pruebas**

1. **Pruebas unitarias**:

   - Asegúrate de probar funciones puras y pequeñas.
   - Evita probar la implementación interna, enfócate en la salida esperada.

2. **Pruebas de componentes**:

   - Enfócate en cómo el componente se comporta desde la perspectiva del usuario.
   - Usa mocks para simular props o contextos si es necesario.

3. **Pruebas end-to-end**:

   - Simula flujos reales de usuario.
   - Usa datos de prueba o entornos dedicados para evitar modificar datos reales.

4. **Automatización y CI/CD**:
   - Configura tus pruebas para ejecutarse automáticamente en un servidor de integración continua como GitHub Actions o Jenkins.

---

## **Conclusión**

Con Jest y React Testing Library puedes escribir pruebas unitarias y de componentes de forma eficiente. Para flujos más completos y pruebas end-to-end, Cypress es una herramienta poderosa y fácil de usar. La combinación de estas herramientas garantiza que tu aplicación React sea robusta, confiable y mantenible. ¡Comienza a escribir pruebas y disfruta de un desarrollo más seguro y predecible! 🚀

---

# 14. Optimización de Rendimiento en React

React ofrece varias herramientas y patrones para optimizar el rendimiento de tus aplicaciones. Estas técnicas son útiles para reducir el tiempo de renderizado, evitar renders innecesarios y manejar grandes aplicaciones de manera eficiente. En esta sección, exploraremos **React.memo**, **React.PureComponent**, **lazy loading**, y hooks como **useMemo** y **useCallback**.

---

## **1. React.memo y React.PureComponent**

### **React.memo (para componentes funcionales)**

`React.memo` es una función de orden superior que memoriza el resultado de la renderización de un componente funcional. Solo volverá a renderizarse si las props cambian.

#### **Uso de React.memo:**

```jsx
import React from "react";

const ChildComponent = React.memo(({ value }) => {
  console.log("Renderizando ChildComponent");
  return <p>Valor: {value}</p>;
});

function ParentComponent() {
  const [count, setCount] = React.useState(0);
  const [otherValue, setOtherValue] = React.useState(0);

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Incrementar Count</button>
      <button onClick={() => setOtherValue(otherValue + 1)}>
        Cambiar OtherValue
      </button>
      <p>Count: {count}</p>
      <ChildComponent value={otherValue} />
    </div>
  );
}

export default ParentComponent;
```

**Explicación**:  
En el ejemplo anterior, `ChildComponent` no se volverá a renderizar cuando cambie el estado `count`, ya que sus props (`value`) no cambian. Esto mejora el rendimiento.

---

### **React.PureComponent (para componentes de clase)**

`React.PureComponent` es similar a `React.Component`, pero implementa un renderizado superficial (`shallow rendering`). Solo volverá a renderizar si detecta cambios en las props o el estado.

#### **Uso de React.PureComponent:**

```jsx
import React, { PureComponent } from "react";

class ChildComponent extends PureComponent {
  render() {
    console.log("Renderizando ChildComponent");
    return <p>Valor: {this.props.value}</p>;
  }
}

class ParentComponent extends React.Component {
  state = { count: 0, otherValue: 0 };

  render() {
    return (
      <div>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Incrementar Count
        </button>
        <button
          onClick={() =>
            this.setState({ otherValue: this.state.otherValue + 1 })
          }
        >
          Cambiar OtherValue
        </button>
        <p>Count: {this.state.count}</p>
        <ChildComponent value={this.state.otherValue} />
      </div>
    );
  }
}

export default ParentComponent;
```

**Nota**: Usa `React.PureComponent` cuando trabajes con componentes de clase. Para componentes funcionales, usa `React.memo`.

---

## **2. Lazy Loading con React.lazy y Suspense**

**Lazy loading** carga dinámicamente componentes solo cuando se necesitan, reduciendo el tiempo inicial de carga de la aplicación.

### **React.lazy y Suspense**

#### **Ejemplo de Lazy Loading:**

```jsx
import React, { Suspense } from "react";

const LazyComponent = React.lazy(() => import("./LazyComponent"));

function App() {
  return (
    <div>
      <h1>Aplicación principal</h1>
      <Suspense fallback={<p>Cargando componente...</p>}>
        <LazyComponent />
      </Suspense>
    </div>
  );
}

export default App;
```

**Explicación**:

- `React.lazy` importa el componente de manera dinámica.
- `Suspense` muestra un contenido alternativo (fallback) mientras el componente se carga.

---

### **División de código por rutas**

Puedes usar `React.lazy` junto con un enrutador para cargar páginas o secciones de manera diferida.

```jsx
import React, { Suspense } from "react";
import { BrowserRouter as Router, Route, Routes } from "react-router-dom";

const Home = React.lazy(() => import("./Home"));
const About = React.lazy(() => import("./About"));

function App() {
  return (
    <Router>
      <Suspense fallback={<p>Cargando...</p>}>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/about" element={<About />} />
        </Routes>
      </Suspense>
    </Router>
  );
}

export default App;
```

---

## **3. Optimización con useMemo y useCallback**

### **useMemo**

`useMemo` memoriza el resultado de una función costosa y solo lo recalcula si las dependencias cambian. Es útil para evitar cálculos innecesarios.

#### **Ejemplo básico de useMemo:**

```jsx
import React, { useState, useMemo } from "react";

function ExpensiveCalculation({ number }) {
  const calculate = (num) => {
    console.log("Calculando...");
    let total = 0;
    for (let i = 0; i < 1e7; i++) {
      total += num;
    }
    return total;
  };

  const result = useMemo(() => calculate(number), [number]);

  return <p>Resultado: {result}</p>;
}

function App() {
  const [count, setCount] = useState(0);
  const [number, setNumber] = useState(1);

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Incrementar Count</button>
      <button onClick={() => setNumber(number + 1)}>Cambiar Number</button>
      <ExpensiveCalculation number={number} />
    </div>
  );
}

export default App;
```

**Explicación**:  
La función `calculate` solo se ejecuta cuando `number` cambia, gracias a `useMemo`.

---

### **useCallback**

`useCallback` memoriza una función para evitar que se vuelva a crear en cada renderizado, especialmente útil cuando pasas funciones a componentes hijos.

#### **Ejemplo básico de useCallback:**

```jsx
import React, { useState, useCallback } from "react";

const ChildComponent = React.memo(({ onClick }) => {
  console.log("Renderizando ChildComponent");
  return <button onClick={onClick}>Haz clic</button>;
});

function App() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    console.log("Botón clickeado");
  }, []);

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Incrementar Count</button>
      <p>Count: {count}</p>
      <ChildComponent onClick={handleClick} />
    </div>
  );
}

export default App;
```

**Explicación**:  
La función `handleClick` se memoriza con `useCallback`, evitando que `ChildComponent` se renderice innecesariamente.

---

## **Conclusión**

1. **React.memo** y **React.PureComponent** evitan renderizados innecesarios al memorizar componentes.
2. **Lazy loading** reduce el tiempo inicial de carga al dividir el código en partes cargadas dinámicamente.
3. **useMemo** optimiza cálculos costosos, mientras que **useCallback** optimiza la creación de funciones.
4. Estas herramientas son esenciales para mantener aplicaciones React rápidas y eficientes, especialmente en aplicaciones complejas.

---

# 15. React y TypeScript

React combinado con TypeScript permite escribir aplicaciones más seguras y mantenibles gracias al uso de tipado estático. TypeScript ayuda a prevenir errores comunes en el desarrollo al verificar los tipos en tiempo de compilación.

---

## **1. Configuración de un Proyecto con TypeScript**

### **Crear un nuevo proyecto React con TypeScript**

Puedes crear un proyecto React con TypeScript utilizando `Create React App`:

```bash
npx create-react-app my-app --template typescript
```

Esto generará una estructura de proyecto con soporte para TypeScript.

### **Añadir TypeScript a un proyecto existente**

Si ya tienes un proyecto React sin TypeScript, puedes agregarlo instalando las dependencias necesarias:

```bash
npm install --save typescript @types/react @types/react-dom
```

Luego, renombra los archivos `.js` a `.tsx` y configura un archivo `tsconfig.json` ejecutando:

```bash
npx tsc --init
```

### **Estructura del archivo `tsconfig.json`**

Un archivo básico podría verse así:

```json
{
  "compilerOptions": {
    "target": "es6",
    "module": "esnext",
    "jsx": "react-jsx",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true
  }
}
```

---

## **2. Tipado de Props, Estado y Componentes**

### **Tipado de Props**

En React, las props se pueden tipar definiendo una interfaz o un tipo. Esto ayuda a asegurarse de que los componentes reciban los datos correctos.

#### **Ejemplo básico:**

```tsx
type GreetingProps = {
  name: string;
  age?: number; // Prop opcional
};

const Greeting: React.FC<GreetingProps> = ({ name, age }) => {
  return (
    <div>
      <p>Hola, {name}!</p>
      {age && <p>Tienes {age} años.</p>}
    </div>
  );
};

export default Greeting;
```

---

### **Tipado de Estado**

Cuando se usa el hook `useState`, puedes tipar explícitamente el estado.

#### **Ejemplo:**

```tsx
import React, { useState } from "react";

const Counter: React.FC = () => {
  const [count, setCount] = useState<number>(0);

  return (
    <div>
      <p>Contador: {count}</p>
      <button onClick={() => setCount(count + 1)}>Incrementar</button>
    </div>
  );
};

export default Counter;
```

Si el estado inicial es inferible, TypeScript automáticamente determinará el tipo:

```tsx
const [count, setCount] = useState(0); // TypeScript infiere que es un "number".
```

---

### **Tipado de Componentes**

Hay varias formas de tipar un componente en React con TypeScript:

#### **Usando `React.FC` (Functional Component):**

```tsx
const MyComponent: React.FC = () => {
  return <div>Mi componente</div>;
};
```

Aunque `React.FC` añade ciertas ventajas como el tipado automático de `children`, a veces no es necesario y puedes usar una función estándar:

```tsx
type MyComponentProps = {
  title: string;
};

const MyComponent = ({ title }: MyComponentProps) => {
  return <h1>{title}</h1>;
};
```

#### **Componentes de Clase:**

```tsx
import React, { Component } from "react";

type MyComponentProps = {
  title: string;
};

type MyComponentState = {
  count: number;
};

class MyComponent extends Component<MyComponentProps, MyComponentState> {
  state: MyComponentState = { count: 0 };

  render() {
    return (
      <div>
        <h1>{this.props.title}</h1>
        <p>Contador: {this.state.count}</p>
      </div>
    );
  }
}
```

---

## **3. Beneficios y Mejores Prácticas**

### **Beneficios de usar TypeScript con React**

1. **Tipado Estático**: Detecta errores en tiempo de desarrollo, mejorando la calidad del código.
2. **Autocompletado Mejorado**: Mejora la experiencia de desarrollo en editores como VSCode.
3. **Documentación Implícita**: Los tipos actúan como documentación del código.
4. **Mantenimiento Más Fácil**: Especialmente en equipos grandes, los tipos ayudan a entender el propósito del código rápidamente.

---

### **Mejores Prácticas**

1. **Usa interfaces o tipos para Props y Estado**:
   Define siempre los tipos de las props y del estado. Por ejemplo:

   ```tsx
   interface Props {
     title: string;
     isActive?: boolean;
   }
   ```

2. **Evita `any` siempre que sea posible**:
   En lugar de usar `any`, intenta definir tipos específicos o utiliza `unknown` si no puedes determinar el tipo de inmediato.

3. **Usa el operador de intersección (`&`) para combinar tipos**:
   Esto es útil para extender componentes:

   ```tsx
   type ButtonProps = {
     onClick: () => void;
   } & React.ButtonHTMLAttributes<HTMLButtonElement>;
   ```

4. **Define tipos reutilizables**:
   Usa `type` o `interface` para mantener un código limpio y reutilizable.

5. **Configura reglas estrictas en TypeScript**:
   En el archivo `tsconfig.json`, habilita las opciones `strict` y `noImplicitAny` para forzar un tipado más robusto.

6. **Utiliza tipos específicos para eventos**:
   Cuando trabajes con eventos, usa los tipos de React:

   ```tsx
   const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
     console.log(e.target.value);
   };
   ```

7. **Aprovecha `Enum` y `Union Types` para valores específicos**:

   ```tsx
   type ButtonVariant = "primary" | "secondary";

   const Button = ({ variant }: { variant: ButtonVariant }) => {
     return <button className={variant}>Click me</button>;
   };
   ```

---

## **Conclusión**

TypeScript mejora significativamente el desarrollo en React, proporcionando herramientas de tipado que hacen el código más seguro, predecible y fácil de mantener. Aunque requiere una curva de aprendizaje, sus beneficios a largo plazo son inmensos, especialmente en proyectos grandes y colaborativos. ¡Implementa estas técnicas y lleva tus proyectos React al siguiente nivel! 🚀

---

# 16. Buenas Prácticas en React

Mantener buenas prácticas en tus proyectos React es fundamental para garantizar que tu código sea limpio, mantenible y escalable. En esta sección, veremos cómo estructurar componentes, separar responsabilidades, mantener consistencia en estilos y configurar herramientas de linteo y formateo.

---

## **1. Estructura Limpia de Componentes**

Una estructura bien organizada facilita la lectura, el mantenimiento y la escalabilidad del código.

### **Sugerencias para organizar un proyecto React:**

```plaintext
src/
├── assets/            # Archivos estáticos (imágenes, fuentes, etc.)
├── components/        # Componentes reutilizables
│   ├── Button/
│   │   ├── Button.tsx
│   │   ├── Button.module.css
│   │   └── index.ts   # Exportaciones centralizadas
│   ├── Header/
│   │   ├── Header.tsx
│   │   ├── Header.module.css
├── pages/             # Páginas principales de la aplicación
│   ├── Home/
│   │   ├── Home.tsx
│   │   ├── Home.module.css
├── hooks/             # Hooks personalizados
├── contexts/          # Proveedores de Context API
├── utils/             # Funciones reutilizables
├── services/          # Lógica de comunicación con APIs
├── App.tsx            # Punto de entrada del componente principal
├── index.tsx          # Punto de entrada del proyecto
```

---

## **2. Dividir Componentes en Funcionales y Contenedores**

### **Componentes Funcionales:**

Son componentes simples y reutilizables que no manejan estado o lógica compleja. Se enfocan en la presentación.

#### **Ejemplo:**

```tsx
type ButtonProps = {
  label: string;
  onClick: () => void;
};

export const Button: React.FC<ButtonProps> = ({ label, onClick }) => (
  <button onClick={onClick}>{label}</button>
);
```

---

### **Componentes Contenedores:**

Son responsables de manejar el estado, lógica y las interacciones con la API. Normalmente envuelven a componentes funcionales.

#### **Ejemplo:**

```tsx
import React, { useState } from "react";
import { Button } from "../components/Button";

const Counter: React.FC = () => {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <Button label="Increment" onClick={() => setCount(count + 1)} />
    </div>
  );
};

export default Counter;
```

**Beneficio**: Esta separación promueve la reutilización y claridad.

---

## **3. Documentación y Uso Consistente de Estilos**

### **Documentación de Componentes**

Utiliza comentarios JSDoc para describir las props y el propósito de tus componentes.

#### **Ejemplo:**

```tsx
/**
 * Botón reutilizable
 * @param label - Texto a mostrar en el botón
 * @param onClick - Función que se ejecuta al hacer clic
 */
type ButtonProps = {
  label: string;
  onClick: () => void;
};

const Button: React.FC<ButtonProps> = ({ label, onClick }) => {
  return <button onClick={onClick}>{label}</button>;
};

export default Button;
```

---

### **Consistencia en Estilos**

Asegúrate de usar un enfoque consistente para los estilos en tu proyecto. Algunas opciones son:

1. **CSS Modules**:

   - Beneficio: Evitan conflictos de nombres en clases CSS.
   - Uso:

     ```css
     /* Button.module.css */
     .button {
       background-color: blue;
       color: white;
     }
     ```

     ```tsx
     import styles from "./Button.module.css";

     const Button = () => <button className={styles.button}>Click me</button>;
     ```

2. **CSS-in-JS**:

   - Librerías como `styled-components` permiten definir estilos dentro de los componentes:

     ```tsx
     import styled from "styled-components";

     const Button = styled.button`
       background-color: blue;
       color: white;
     `;

     const App = () => <Button>Click me</Button>;
     ```

3. **Frameworks**:
   - Usa librerías como **TailwindCSS** o **Bootstrap** para agilizar el diseño:
     ```tsx
     const App = () => (
       <button className="bg-blue-500 text-white">Click me</button>
     );
     ```

**Nota**: Define y documenta el enfoque estilístico antes de iniciar el proyecto para mantener la consistencia.

---

## **4. Uso de Linters y Formateadores**

### **Configuración de ESLint**

`ESLint` es una herramienta esencial para detectar errores y mantener un código limpio.

#### **Instalación de ESLint:**

```bash
npx eslint --init
```

Durante la configuración, selecciona:

- Framework: **React**
- Lenguaje: **JavaScript o TypeScript**

#### **Archivo `.eslintrc`:**

Un ejemplo de configuración para TypeScript:

```json
{
  "extends": [
    "eslint:recommended",
    "plugin:react/recommended",
    "plugin:@typescript-eslint/recommended",
    "prettier"
  ],
  "parser": "@typescript-eslint/parser",
  "plugins": ["@typescript-eslint", "react"],
  "settings": {
    "react": {
      "version": "detect"
    }
  },
  "rules": {
    "react/prop-types": "off"
  }
}
```

---

### **Configuración de Prettier**

`Prettier` asegura un formateo consistente de tu código.

#### **Instalación:**

```bash
npm install --save-dev prettier eslint-config-prettier eslint-plugin-prettier
```

#### **Archivo `.prettierrc`:**

Define tus reglas de formato:

```json
{
  "semi": true,
  "singleQuote": true,
  "trailingComma": "all"
}
```

#### **Integración con ESLint:**

Asegúrate de incluir `eslint-config-prettier` para evitar conflictos entre ESLint y Prettier.

---

### **Ejecutar Linters y Formateadores**

Agrega scripts en el `package.json` para automatizar estas tareas:

```json
"scripts": {
  "lint": "eslint 'src/**/*.{js,ts,tsx}'",
  "format": "prettier --write 'src/**/*.{js,ts,tsx,json,css}'"
}
```

---

## **Conclusión**

Adoptar buenas prácticas en React no solo mejora la calidad del código, sino que también facilita la colaboración en equipo. Una estructura limpia, la separación de responsabilidades entre componentes, el uso de herramientas como ESLint y Prettier, y un enfoque consistente para estilos son esenciales para proyectos React de éxito. ¡Comienza a implementar estas técnicas y lleva tus proyectos al siguiente nivel! 🚀

---

# 17. Despliegue de una Aplicación React

Desplegar una aplicación React correctamente garantiza que esté optimizada y lista para ser utilizada por los usuarios finales. En esta sección, cubriremos los pasos esenciales para empaquetar tu aplicación, configurarla para producción y desplegarla en plataformas populares como **Netlify**, **Vercel** o **AWS Amplify**.

---

## **1. Empaquetado con Create React App o Vite**

### **Empaquetado con Create React App (CRA):**

Si creaste tu proyecto con `Create React App`, puedes generar los archivos de producción ejecutando:

```bash
npm run build
```

Esto creará una carpeta llamada `build/`, que contiene los archivos estáticos optimizados para producción, como `index.html`, `main.js`, y los assets.

#### **Archivos importantes dentro de `build/`:**

- **`index.html`**: El punto de entrada principal.
- **Archivos `.js` y `.css`**: Contienen el código minificado y optimizado de tu aplicación.

### **Empaquetado con Vite:**

Si usaste **Vite** para configurar tu proyecto, también puedes construirlo con:

```bash
npm run build
```

El resultado se generará en la carpeta `dist/`, que es similar a `build/` en CRA, con los archivos listos para ser desplegados.

#### **Previsualización del Build:**

Para verificar que todo funcione correctamente antes del despliegue, puedes usar un servidor local:

```bash
npm install -g serve
serve -s build     # Para Create React App
serve -s dist      # Para Vite
```

---

## **2. Configuración para Producción**

Antes de desplegar, asegúrate de que tu aplicación esté optimizada para producción:

### **1. Minificación de Código:**

Las herramientas de empaquetado como CRA y Vite ya se encargan de:

- Minificar archivos JavaScript y CSS.
- Eliminar código innecesario.

### **2. Variables de Entorno (`.env`):**

Define variables de entorno para configurar tu aplicación según el entorno (desarrollo, producción, etc.).

#### **Ejemplo:**

```plaintext
# Archivo .env.production
REACT_APP_API_URL=https://miapi.com
REACT_APP_ENV=production
```

Al generar el build, React usará automáticamente estas variables.

### **3. Configurar el Servidor:**

Si usas un servidor como **NGINX** o **Apache**, configura el reenvío de rutas para aplicaciones SPA (Single Page Applications).

#### **Ejemplo de configuración en NGINX:**

```nginx
server {
  listen 80;
  root /var/www/html/build;

  location / {
    index index.html;
    try_files $uri /index.html;
  }
}
```

---

## **3. Despliegue en Plataformas Populares**

### **1. Despliegue en Netlify:**

Netlify es una de las plataformas más populares para alojar aplicaciones React.

#### **Pasos:**

1. **Inicia sesión en Netlify** y conecta tu repositorio (GitHub, GitLab, etc.).
2. Al configurar el proyecto:
   - Build Command:
     ```bash
     npm run build
     ```
   - Publish Directory:
     ```plaintext
     build/  # Para CRA
     dist/   # Para Vite
     ```
3. **Deploy**: Netlify detectará automáticamente los cambios en tu repositorio y desplegará la aplicación.

#### **Redirecciones en Netlify:**

Si tienes rutas dinámicas, crea un archivo `_redirects` en `public/` o `build/`:

```plaintext
/*    /index.html   200
```

---

### **2. Despliegue en Vercel:**

Vercel es ideal para aplicaciones React y soporta despliegue con un solo clic.

#### **Pasos:**

1. **Inicia sesión en Vercel** y conecta tu repositorio.
2. Configura el proyecto:
   - Build Command:
     ```bash
     npm run build
     ```
   - Output Directory:
     ```plaintext
     build/  # Para CRA
     dist/   # Para Vite
     ```
3. Vercel desplegará automáticamente tu aplicación y proporcionará un dominio gratuito.

#### **Optimización Automática en Vercel:**

Vercel se encarga de optimizar imágenes, implementar CDN y compresión automática para tu proyecto.

---

### **3. Despliegue en AWS Amplify:**

AWS Amplify ofrece una solución robusta para alojar aplicaciones React con integración directa con servicios de AWS.

#### **Pasos:**

1. **Conecta tu repositorio** en la consola de AWS Amplify.
2. Amplify detectará automáticamente tu proyecto React y configurará los comandos de build y la carpeta de salida:
   - Build Command:
     ```bash
     npm run build
     ```
   - Output Directory:
     ```plaintext
     build/  # CRA
     dist/   # Vite
     ```
3. Configura un dominio personalizado si es necesario (opcional).

---

### **4. Otras Opciones de Despliegue:**

#### **GitHub Pages:**

1. Instala `gh-pages`:
   ```bash
   npm install gh-pages
   ```
2. Agrega los siguientes scripts en `package.json`:
   ```json
   "homepage": "https://tu-usuario.github.io/tu-repo",
   "scripts": {
     "predeploy": "npm run build",
     "deploy": "gh-pages -d build"
   }
   ```
3. Ejecuta:
   ```bash
   npm run deploy
   ```

#### **Heroku:**

1. Agrega un archivo `static.json` para configurar el reenvío de rutas.
2. Usa la build con el servidor `serve`.

---

## **Conclusión**

El despliegue de una aplicación React es un proceso sencillo con las herramientas actuales, y cada plataforma tiene sus particularidades que se adaptan a diferentes necesidades. Ya sea que elijas **Netlify**, **Vercel**, **AWS Amplify** u otra alternativa, asegúrate de que tu aplicación esté optimizada y configurada correctamente para brindar la mejor experiencia al usuario.

---

# 18. React Avanzado

A medida que avanzas en tu conocimiento de React, encontrarás conceptos y herramientas que van más allá de lo básico. En esta sección, exploraremos características avanzadas como React Server Components, Suspense para datos asíncronos, el manejo de WebSockets para aplicaciones en tiempo real, y el renderizado del lado del servidor (SSR) utilizando Next.js.

---

## **1. React Server Components (RSC)**

### **¿Qué son los React Server Components?**

Los React Server Components (RSC) permiten que parte de la lógica de renderizado ocurra en el servidor, en lugar de ejecutarse completamente en el cliente. Esto mejora la eficiencia y reduce el tamaño del paquete enviado al cliente.

### **Características principales:**

- Permiten mezclar componentes de cliente y servidor.
- Reducen la cantidad de JavaScript enviado al cliente.
- Mejoran el rendimiento y la experiencia del usuario.

### **Ejemplo básico de RSC:**

#### **Estructura del proyecto:**

```plaintext
src/
├── server/
│   ├── UserList.server.js
├── client/
│   ├── App.client.js
```

#### **Componente del lado del servidor:**

```javascript
// UserList.server.js
import fetch from "node-fetch";

export default async function UserList() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users");
  const users = await response.json();

  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

#### **Componente del lado del cliente:**

```javascript
// App.client.js
import React from "react";
import UserList from "../server/UserList.server";

export default function App() {
  return (
    <div>
      <h1>Lista de Usuarios</h1>
      <UserList />
    </div>
  );
}
```

**Nota**: Actualmente, RSC está en desarrollo y es compatible principalmente con frameworks como **Next.js**.

---

## **2. Suspense para Datos Asíncronos**

### **¿Qué es Suspense?**

`Suspense` es una funcionalidad de React que permite mostrar un estado de carga mientras se resuelven datos o recursos asíncronos.

### **Ejemplo básico de Suspense:**

#### **Simulación de un fetch asíncrono:**

```javascript
// fetchData.js
function fetchData() {
  let data;
  let promise = new Promise((resolve) => {
    setTimeout(() => {
      data = "Datos cargados exitosamente";
      resolve();
    }, 2000);
  });

  return {
    read() {
      if (!data) throw promise;
      return data;
    },
  };
}

export const resource = fetchData();
```

#### **Uso de Suspense:**

```javascript
// App.js
import React, { Suspense } from "react";
import { resource } from "./fetchData";

function DataComponent() {
  const data = resource.read();
  return <div>{data}</div>;
}

export default function App() {
  return (
    <div>
      <h1>Suspense en React</h1>
      <Suspense fallback={<div>Cargando...</div>}>
        <DataComponent />
      </Suspense>
    </div>
  );
}
```

**Nota**: `Suspense` es especialmente poderoso cuando se combina con librerías como **React Query** o **Relay** para manejar datos.

---

## **3. Manejo de WebSockets y Tiempo Real**

### **¿Qué son los WebSockets?**

Los WebSockets permiten la comunicación bidireccional en tiempo real entre el cliente y el servidor. Son útiles para aplicaciones como chats, tableros de notificaciones o cualquier funcionalidad que requiera datos en tiempo real.

### **Ejemplo básico con WebSockets:**

#### **Servidor WebSocket con Node.js:**

```javascript
// server.js
const WebSocket = require("ws");

const wss = new WebSocket.Server({ port: 8080 });

wss.on("connection", (ws) => {
  console.log("Cliente conectado");
  ws.on("message", (message) => {
    console.log("Mensaje recibido:", message);
    ws.send(`Echo: ${message}`);
  });
});
```

#### **Cliente React con WebSocket:**

```javascript
// App.js
import React, { useEffect, useState } from "react";

export default function App() {
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState("");
  const [socket, setSocket] = useState(null);

  useEffect(() => {
    const ws = new WebSocket("ws://localhost:8080");
    ws.onmessage = (event) => {
      setMessages((prev) => [...prev, event.data]);
    };
    setSocket(ws);
    return () => ws.close();
  }, []);

  const sendMessage = () => {
    if (socket && input) {
      socket.send(input);
      setInput("");
    }
  };

  return (
    <div>
      <h1>Chat en Tiempo Real</h1>
      <div>
        {messages.map((msg, index) => (
          <p key={index}>{msg}</p>
        ))}
      </div>
      <input value={input} onChange={(e) => setInput(e.target.value)} />
      <button onClick={sendMessage}>Enviar</button>
    </div>
  );
}
```

---

## **4. Renderizado del Lado del Servidor (SSR) con Next.js**

### **¿Qué es el SSR?**

El SSR (Server-Side Rendering) permite que las páginas de React se rendericen en el servidor antes de enviarlas al cliente. Esto mejora el rendimiento y es útil para SEO.

### **Configuración básica con Next.js:**

#### **Instalación de Next.js:**

```bash
npx create-next-app@latest
```

#### **Ejemplo de SSR:**

```javascript
// pages/index.js
export async function getServerSideProps() {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts");
  const posts = await response.json();

  return {
    props: { posts },
  };
}

export default function Home({ posts }) {
  return (
    <div>
      <h1>SSR con Next.js</h1>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
    </div>
  );
}
```

---

## **Conclusión**

React Avanzado permite construir aplicaciones más eficientes, escalables y capaces de manejar requisitos complejos como datos en tiempo real, renderizado optimizado y carga asíncrona. Dominar estos conceptos, junto con herramientas como **Next.js**, te preparará para enfrentar proyectos ambiciosos con confianza.

---

# 19. Herramientas y Librerías Complementarias

Además de React y sus herramientas integradas, existen muchas librerías y utilidades que complementan el desarrollo, facilitando tareas específicas como la documentación de componentes, la gestión de estado global o la creación de animaciones. En esta sección, exploraremos algunas de las más útiles: **Storybook**, **Recoil**, y librerías de animación como **Framer Motion**.

---

## **1. Storybook para Documentación de Componentes**

### **¿Qué es Storybook?**

Storybook es una herramienta de desarrollo que permite construir y documentar componentes de manera aislada. Es ideal para crear bibliotecas de componentes reutilizables y probarlos en diferentes estados.

### **Ventajas de Storybook:**

- Permite trabajar en componentes de forma independiente.
- Facilita la documentación y visualización de variantes de los componentes.
- Compatible con addons para pruebas visuales, accesibilidad, y más.

### **Instalación de Storybook:**

1. Instala Storybook en tu proyecto React:

   ```bash
   npx storybook@latest init
   ```

2. Ejecuta Storybook:
   ```bash
   npm run storybook
   ```
   Esto abrirá una interfaz donde puedes ver tus componentes y trabajar con ellos.

### **Ejemplo de un Story básico:**

Supongamos que tienes un componente `Button`:

#### **Componente Button:**

```javascript
// src/components/Button.js
import React from "react";

export default function Button({ label, onClick }) {
  return <button onClick={onClick}>{label}</button>;
}
```

#### **Historia para Button:**

```javascript
// src/components/Button.stories.js
import React from "react";
import Button from "./Button";

export default {
  title: "Components/Button",
  component: Button,
};

export const Primary = () => (
  <Button label="Click Me" onClick={() => alert("Clicked!")} />
);
export const Secondary = () => (
  <Button label="Cancel" onClick={() => alert("Canceled!")} />
);
```

Con esto, tendrás dos variantes (`Primary` y `Secondary`) visibles en Storybook.

---

## **2. Recoil para Gestión de Estado Global**

### **¿Qué es Recoil?**

Recoil es una librería de gestión de estado para React que se enfoca en ser simple, escalable y compatible con React Hooks. Es una alternativa ligera a Redux, con una curva de aprendizaje más amigable.

### **Conceptos clave de Recoil:**

- **Átomos:** Representan el estado compartido.
- **Selectores:** Son funciones derivadas del estado que pueden ser usadas para calcular valores.

### **Instalación de Recoil:**

1. Instala Recoil:

   ```bash
   npm install recoil
   ```

2. Envuelve tu aplicación con el `RecoilRoot`:

   ```javascript
   import React from "react";
   import ReactDOM from "react-dom";
   import { RecoilRoot } from "recoil";
   import App from "./App";

   ReactDOM.render(
     <RecoilRoot>
       <App />
     </RecoilRoot>,
     document.getElementById("root")
   );
   ```

### **Ejemplo básico con Recoil:**

#### **Definir un Átomo:**

```javascript
// src/state/counterAtom.js
import { atom } from "recoil";

export const counterState = atom({
  key: "counterState",
  default: 0,
});
```

#### **Usar el Átomo en un Componente:**

```javascript
// src/components/Counter.js
import React from "react";
import { useRecoilState } from "recoil";
import { counterState } from "../state/counterAtom";

export default function Counter() {
  const [count, setCount] = useRecoilState(counterState);

  return (
    <div>
      <h1>Counter: {count}</h1>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <button onClick={() => setCount(count - 1)}>Decrement</button>
    </div>
  );
}
```

---

## **3. Librerías de Animación como Framer Motion**

### **¿Qué es Framer Motion?**

Framer Motion es una poderosa librería de animaciones para React que facilita la creación de transiciones fluidas, animaciones complejas y efectos interactivos con una API sencilla.

### **Ventajas de Framer Motion:**

- Animaciones declarativas y fáciles de usar.
- Compatible con gestos y arrastres.
- Funciona bien con componentes funcionales y hooks.

### **Instalación de Framer Motion:**

1. Instala la librería:
   ```bash
   npm install framer-motion
   ```

### **Ejemplo básico con Framer Motion:**

```javascript
// src/components/Box.js
import React from "react";
import { motion } from "framer-motion";

export default function Box() {
  return (
    <motion.div
      style={{
        width: 100,
        height: 100,
        backgroundColor: "blue",
        margin: "50px auto",
      }}
      animate={{ rotate: 360 }}
      transition={{ duration: 2, repeat: Infinity }}
    />
  );
}
```

Este ejemplo crea una caja que gira continuamente.

### **Animaciones de entrada y salida:**

```javascript
// src/components/FadeInOut.js
import React from "react";
import { motion } from "framer-motion";

export default function FadeInOut({ isVisible }) {
  return (
    <motion.div
      initial={{ opacity: 0 }}
      animate={{ opacity: isVisible ? 1 : 0 }}
      transition={{ duration: 1 }}
    >
      <h1>¡Hola, React!</h1>
    </motion.div>
  );
}
```

Este componente animará la aparición y desaparición de un texto basado en la prop `isVisible`.

---

## **Conclusión**

Herramientas como **Storybook**, **Recoil**, y librerías como **Framer Motion** son recursos valiosos para mejorar la productividad, gestionar el estado de aplicaciones complejas y crear experiencias visuales atractivas. Usarlas adecuadamente te permitirá trabajar de manera más eficiente y llevar tus proyectos React a un nivel superior.

---

# 20. Proyectos Prácticos

La mejor manera de consolidar tus conocimientos en React es mediante la construcción de proyectos prácticos. Aquí te presentamos una lista de proyectos con sus descripciones y características, ideales para aplicar lo aprendido y ganar experiencia real.

---

## **1. Crear un Contador Simple**

**Objetivo:** Practicar el uso de estado con el hook `useState`.

### **Características:**

- Botones para incrementar, decrementar y resetear el contador.
- Mostrar el estado actual del contador.

### **Ejemplo de código:**

```javascript
import React, { useState } from "react";

export default function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div style={{ textAlign: "center" }}>
      <h1>Contador: {count}</h1>
      <button onClick={() => setCount(count + 1)}>Incrementar</button>
      <button onClick={() => setCount(count - 1)}>Decrementar</button>
      <button onClick={() => setCount(0)}>Resetear</button>
    </div>
  );
}
```

---

## **2. To-Do List con Manejo de Estado**

**Objetivo:** Practicar el manejo del estado y renderizado dinámico de listas.

### **Características:**

- Agregar y eliminar tareas.
- Marcar tareas como completadas.

### **Ejemplo de código:**

```javascript
import React, { useState } from "react";

export default function TodoList() {
  const [tasks, setTasks] = useState([]);
  const [task, setTask] = useState("");

  const addTask = () => {
    setTasks([...tasks, { text: task, completed: false }]);
    setTask("");
  };

  const toggleTask = (index) => {
    const newTasks = [...tasks];
    newTasks[index].completed = !newTasks[index].completed;
    setTasks(newTasks);
  };

  const deleteTask = (index) => {
    setTasks(tasks.filter((_, i) => i !== index));
  };

  return (
    <div>
      <h1>Lista de Tareas</h1>
      <input value={task} onChange={(e) => setTask(e.target.value)} />
      <button onClick={addTask}>Agregar</button>
      <ul>
        {tasks.map((task, index) => (
          <li
            key={index}
            style={{ textDecoration: task.completed ? "line-through" : "none" }}
          >
            {task.text}
            <button onClick={() => toggleTask(index)}>Completada</button>
            <button onClick={() => deleteTask(index)}>Eliminar</button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

---

## **3. Blog con Integración de APIs**

**Objetivo:** Practicar el uso de `fetch` o `axios` para consumir APIs.

### **Características:**

- Mostrar una lista de publicaciones obtenidas de una API (por ejemplo, JSONPlaceholder).
- Página de detalles para cada publicación.

### **Ejemplo de código:**

```javascript
import React, { useEffect, useState } from "react";

export default function Blog() {
  const [posts, setPosts] = useState([]);

  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/posts")
      .then((response) => response.json())
      .then((data) => setPosts(data));
  }, []);

  return (
    <div>
      <h1>Blog</h1>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>
            <h3>{post.title}</h3>
            <p>{post.body}</p>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

---

## **4. Tablero Kanban con Drag and Drop**

**Objetivo:** Practicar interacciones avanzadas y manejo de estado para mover elementos entre listas.

### **Características:**

- Tres columnas: "Por hacer", "En progreso", "Completado".
- Arrastrar y soltar tareas entre columnas.

### **Librería recomendada:**

[react-beautiful-dnd](https://github.com/atlassian/react-beautiful-dnd).

### **Ejemplo básico de drag and drop:**

```javascript
import React, { useState } from "react";
import { DragDropContext, Droppable, Draggable } from "react-beautiful-dnd";

export default function Kanban() {
  const [tasks, setTasks] = useState([
    { id: "1", text: "Tarea 1" },
    { id: "2", text: "Tarea 2" },
    { id: "3", text: "Tarea 3" },
  ]);

  const onDragEnd = (result) => {
    if (!result.destination) return;

    const reorderedTasks = Array.from(tasks);
    const [removed] = reorderedTasks.splice(result.source.index, 1);
    reorderedTasks.splice(result.destination.index, 0, removed);

    setTasks(reorderedTasks);
  };

  return (
    <DragDropContext onDragEnd={onDragEnd}>
      <Droppable droppableId="tasks">
        {(provided) => (
          <ul {...provided.droppableProps} ref={provided.innerRef}>
            {tasks.map((task, index) => (
              <Draggable key={task.id} draggableId={task.id} index={index}>
                {(provided) => (
                  <li
                    ref={provided.innerRef}
                    {...provided.draggableProps}
                    {...provided.dragHandleProps}
                  >
                    {task.text}
                  </li>
                )}
              </Draggable>
            ))}
            {provided.placeholder}
          </ul>
        )}
      </Droppable>
    </DragDropContext>
  );
}
```

---

## **5. E-commerce con Carrito de Compras y Autenticación**

**Objetivo:** Construir una aplicación más compleja que incluya múltiples funcionalidades.

### **Características:**

- Lista de productos obtenida de una API.
- Carrito de compras con opción para agregar y eliminar productos.
- Autenticación básica con JWT.

### **Piezas clave:**

- **Estado:** Usa Context API o Redux para manejar el carrito y la autenticación.
- **API:** Una API como [Fake Store API](https://fakestoreapi.com/) para obtener datos de productos.
- **Routing:** Usa React Router para navegación entre páginas como "Productos", "Carrito" y "Perfil".

### **Estructura de un carrito básico:**

```javascript
import React, { useState } from "react";

export default function ECommerce() {
  const [cart, setCart] = useState([]);
  const products = [
    { id: 1, name: "Producto 1", price: 10 },
    { id: 2, name: "Producto 2", price: 15 },
  ];

  const addToCart = (product) => {
    setCart([...cart, product]);
  };

  const removeFromCart = (id) => {
    setCart(cart.filter((item) => item.id !== id));
  };

  return (
    <div>
      <h1>Tienda</h1>
      <h2>Productos</h2>
      <ul>
        {products.map((product) => (
          <li key={product.id}>
            {product.name} - ${product.price}
            <button onClick={() => addToCart(product)}>
              Agregar al Carrito
            </button>
          </li>
        ))}
      </ul>
      <h2>Carrito</h2>
      <ul>
        {cart.map((item) => (
          <li key={item.id}>
            {item.name} - ${item.price}
            <button onClick={() => removeFromCart(item.id)}>Eliminar</button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

---

## **Conclusión**

Estos proyectos cubren desde conceptos básicos hasta funcionalidades más avanzadas, brindándote una base sólida para enfrentar proyectos más complejos. Recuerda personalizar y ampliar cada uno según tus necesidades para aprender aún más sobre React.

---
---
---

# MINIGUIAS:


## REDUX:
Te haré una guía detallada, paso a paso, sobre cómo usar Redux, Redux Toolkit y Redux Persist. Empezaré con una introducción general de Redux y cómo configurarlo, para luego profundizar en las herramientas adicionales.

### 1. **Introducción a Redux**
Redux es una librería de gestión de estado predecible para aplicaciones JavaScript, especialmente útil en aplicaciones de React. Permite centralizar todo el estado de la aplicación en un único "store" y gestionar el flujo de datos de manera estructurada.

#### Principales Conceptos de Redux:
- **Store:** Es el objeto que contiene todo el estado de la aplicación.
- **Action:** Son objetos que describen lo que ocurre en la aplicación.
- **Reducer:** Son funciones puras que gestionan las acciones y actualizan el estado.
- **Dispatch:** Es el método para enviar acciones al reducer.
- **State:** Es la estructura de datos que representa el estado de tu aplicación.

### 2. **Configurando Redux en tu proyecto**

#### Paso 1: Instalar las dependencias
Primero, necesitas instalar `redux` y `react-redux` (que es un binding para integrar Redux con React):

```bash
npm install redux react-redux
```

#### Paso 2: Crear el Reducer
Un reducer es una función que recibe el estado actual y una acción, y devuelve un nuevo estado.

```javascript
// src/reducers/counterReducer.js
const initialState = { count: 0 };

function counterReducer(state = initialState, action) {
  switch (action.type) {
    case 'INCREMENT':
      return { count: state.count + 1 };
    case 'DECREMENT':
      return { count: state.count - 1 };
    default:
      return state;
  }
}

export default counterReducer;
```

#### Paso 3: Crear el Store
El store se crea con la función `createStore`, que recibe el reducer y un estado inicial.

```javascript
// src/store.js
import { createStore } from 'redux';
import counterReducer from './reducers/counterReducer';

const store = createStore(counterReducer);

export default store;
```

#### Paso 4: Proveer el Store a la Aplicación
Utilizamos el componente `Provider` de `react-redux` para pasar el store a toda la aplicación.

```javascript
// src/index.js
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import App from './App';
import store from './store';

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

#### Paso 5: Conectando Componentes con Redux
Usamos el hook `useDispatch` para enviar acciones y `useSelector` para leer el estado desde los componentes.

```javascript
// src/App.js
import React from 'react';
import { useDispatch, useSelector } from 'react-redux';

function App() {
  const dispatch = useDispatch();
  const count = useSelector((state) => state.count);

  return (
    <div>
      <h1>{count}</h1>
      <button onClick={() => dispatch({ type: 'INCREMENT' })}>Incrementar</button>
      <button onClick={() => dispatch({ type: 'DECREMENT' })}>Decrementar</button>
    </div>
  );
}

export default App;
```

Con estos pasos, tienes Redux funcionando en tu aplicación.

---

### 3. **Redux Toolkit**

Redux Toolkit es una librería que facilita la configuración de Redux al ofrecer herramientas como `configureStore`, `createSlice` y `createAsyncThunk`.

#### Paso 1: Instalar Redux Toolkit

```bash
npm install @reduxjs/toolkit react-redux
```

#### Paso 2: Crear un Slice con Redux Toolkit

Un "slice" es una forma simplificada de crear reducers y actions.

```javascript
// src/slices/counterSlice.js
import { createSlice } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: { count: 0 },
  reducers: {
    increment: (state) => {
      state.count += 1;
    },
    decrement: (state) => {
      state.count -= 1;
    },
  },
});

export const { increment, decrement } = counterSlice.actions;
export default counterSlice.reducer;
```

#### Paso 3: Configurar el Store

Usamos `configureStore` para crear el store, que es más sencillo que con `createStore` de Redux.

```javascript
// src/store.js
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from './slices/counterSlice';

const store = configureStore({
  reducer: {
    counter: counterReducer,
  },
});

export default store;
```

#### Paso 4: Conectar los Componentes

Ahora, en los componentes, puedes usar las acciones de `counterSlice`.

```javascript
// src/App.js
import React from 'react';
import { useDispatch, useSelector } from 'react-redux';
import { increment, decrement } from './slices/counterSlice';

function App() {
  const dispatch = useDispatch();
  const count = useSelector((state) => state.counter.count);

  return (
    <div>
      <h1>{count}</h1>
      <button onClick={() => dispatch(increment())}>Incrementar</button>
      <button onClick={() => dispatch(decrement())}>Decrementar</button>
    </div>
  );
}

export default App;
```

Con Redux Toolkit, se simplifica mucho la configuración y el manejo del estado.

---

### 4. **Redux Persist**

Redux Persist permite que el estado de la aplicación se guarde en el almacenamiento local (localStorage o sessionStorage) para persistir el estado entre recargas de la página.

#### Paso 1: Instalar Redux Persist

```bash
npm install redux-persist
```

#### Paso 2: Configurar Redux Persist en el Store

Importamos `persistReducer` y `persistStore` para integrar Redux Persist con nuestro store.

```javascript
// src/store.js
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from './slices/counterSlice';
import { persistStore, persistReducer } from 'redux-persist';
import storage from 'redux-persist/lib/storage'; // Usamos localStorage

const persistConfig = {
  key: 'root',
  storage,
};

const persistedReducer = persistReducer(persistConfig, counterReducer);

const store = configureStore({
  reducer: {
    counter: persistedReducer,
  },
});

const persistor = persistStore(store);

export { store, persistor };
```

#### Paso 3: Configurar el Provider con PersistGate

En el archivo `index.js`, envuelves tu aplicación con `PersistGate` para asegurar que el estado se carga antes de que se renderice la aplicación.

```javascript
// src/index.js
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import { PersistGate } from 'redux-persist/integration/react';
import App from './App';
import { store, persistor } from './store';

ReactDOM.render(
  <Provider store={store}>
    <PersistGate loading={null} persistor={persistor}>
      <App />
    </PersistGate>
  </Provider>,
  document.getElementById('root')
);
```

Con esta configuración, tu estado será guardado en `localStorage` y persistirá incluso si recargas la página.

---

### Resumen

- **Redux:** Permite gestionar el estado global de manera predecible.
- **Redux Toolkit:** Facilita la configuración de Redux con herramientas como `createSlice` y `configureStore`.
- **Redux Persist:** Mantiene el estado entre recargas de la página almacenándolo en el navegador (localStorage).

Esta es la estructura básica para usar Redux con React, Redux Toolkit y Redux Persist. Si necesitas más detalles sobre alguna de estas herramientas, no dudes en preguntar.