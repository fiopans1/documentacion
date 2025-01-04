# GUIA BASE:
## INICIO RAPIDO CON REACT:
Crear una plantilla básica para una aplicación en React es un excelente punto de partida para construir aplicaciones más complejas. A continuación, te explico los pasos para crear una plantilla con **React** y opcionalmente usar **React-Bootstrap** si lo necesitas.

---

### 1. **Configurar el proyecto**
Primero, asegúrate de tener **Node.js** y **npm** instalados. Luego, crea una nueva aplicación de React usando `create-react-app` o una alternativa moderna como **Vite**.

#### Con Create React App:
```bash
npx create-react-app mi-plantilla
cd mi-plantilla
```

#### Con Vite:
```bash
npm create vite@latest mi-plantilla --template react
cd mi-plantilla
npm install
```

---

### 2. **Instalar React-Bootstrap (opcional)**
Si deseas usar **React-Bootstrap** en tu plantilla, instálalo junto con **Bootstrap**.

```bash
npm install react-bootstrap bootstrap
```

Luego, importa el archivo CSS de Bootstrap en `src/index.js` o `src/main.jsx` (si usas Vite):
```javascript
import 'bootstrap/dist/css/bootstrap.min.css';
```

---

### 3. **Estructura del proyecto**
Crea una estructura de carpetas organizada en el directorio `src`:

```
src/
│
├── components/         // Componentes reutilizables
│   ├── Header.jsx
│   ├── Footer.jsx
│   └── Navbar.jsx
│
├── pages/              // Vistas principales
│   ├── Home.jsx
│   ├── About.jsx
│   └── Contact.jsx
│
├── App.jsx             // Componente principal
├── index.js            // Punto de entrada
└── styles/             // Estilos personalizados
    └── App.css
```

---

### 4. **Crear los Componentes Básicos**

#### `src/components/Header.jsx`
```javascript
import React from 'react';
import { Navbar, Container } from 'react-bootstrap';

const Header = () => {
  return (
    <Navbar bg="dark" variant="dark" expand="lg">
      <Container>
        <Navbar.Brand href="#home">Mi App</Navbar.Brand>
      </Container>
    </Navbar>
  );
};

export default Header;
```

#### `src/components/Footer.jsx`
```javascript
import React from 'react';

const Footer = () => {
  return (
    <footer className="text-center mt-4">
      <p>© 2024 Mi App. Todos los derechos reservados.</p>
    </footer>
  );
};

export default Footer;
```

---

### 5. **Crear las Páginas**

#### `src/pages/Home.jsx`
```javascript
import React from 'react';

const Home = () => {
  return (
    <div>
      <h1>Bienvenido a mi aplicación</h1>
      <p>Esta es la página principal.</p>
    </div>
  );
};

export default Home;
```

#### `src/pages/About.jsx`
```javascript
import React from 'react';

const About = () => {
  return (
    <div>
      <h1>Sobre Nosotros</h1>
      <p>Información sobre nuestra aplicación.</p>
    </div>
  );
};

export default About;
```

---

### 6. **Configurar el Router**
Instala **React Router** para gestionar las rutas de la aplicación:
```bash
npm install react-router-dom
```

Luego, configura las rutas en `App.jsx`:

#### `src/App.jsx`
```javascript
import React from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import Header from './components/Header';
import Footer from './components/Footer';
import Home from './pages/Home';
import About from './pages/About';

const App = () => {
  return (
    <Router>
      <Header />
      <div className="container mt-4">
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/about" element={<About />} />
        </Routes>
      </div>
      <Footer />
    </Router>
  );
};

export default App;
```

---

### 7. **Personalizar estilos**
Crea un archivo CSS para personalizar los estilos:

#### `src/styles/App.css`
```css
body {
  font-family: Arial, sans-serif;
  background-color: #f8f9fa;
}
h1 {
  color: #333;
}
```

Importa este archivo en `src/App.jsx` o `index.js`:
```javascript
import './styles/App.css';
```

---

### 8. **Ejecutar la aplicación**
Inicia el servidor de desarrollo para probar la plantilla:
```bash
npm start
```


## PRIMER PASO:
-Instalar node.js

## Crear proyecto:
npx create-react-app nombreproyecto

## Arrancar proyecto:
npm start

## Componentes

Recomendado crear un folder para componentes

Nosotros vamos a componer la app a base de componentes

Un componente no deja de ser una función que devuelve un trozo de html que va a renderizar

Una de las mejores cosas de react es que podemos pasar variables desde nuestro template a nuestra vista

Si abres llaves puedes recorrer usar javascript dentro de html, esto es jsx,
por ejemplo dentro de un div, puedes abrir llaves y iterar algo

Yo siempre tengo que hacer un return que devuelva un trozo de html

Otra cosa es que cada elemento va a tener que tener una key


Otra de las cosas importantes que podemos hacer con react son los estados

## ESTADOS:
Podemos definir por ejemplo un botón, y que cambie el valor de una variable, pero para esto vamos a tener que usar estados, que esto viene siendo como una variable que está sujeta a cambios

Lo primero es importar un state

Para el tema de los estados vamos a usar useState, en el cual vamos a declarar una variable como constante como 

>const [nombreVariable, setFuncionVariable] = useState(valor);


## ESTILOS:

Si queremos crear un archivo de estilos para nuestra clase lo que vmaos a hacer es llamar al css:
>nombre.module.css

Y luego cuando importamos el estilo lo importamos como:

>import classes from "nombre.module.css";

Y luego cuando lo referenciamos en el html dentro de react hacemos por ejemplo:

>className = {classes.nombreClase}

NOTA: Para esto lo mejor es usar camelCase

