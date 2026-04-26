# Guía Completa de React-Bootstrap

## 1. ¿Qué es React-Bootstrap?

**React-Bootstrap** es una librería que reimplementa los componentes de **Bootstrap** como componentes nativos de React.

En lugar de depender de JavaScript imperativo y manipulación directa del DOM, React-Bootstrap te permite usar:

- componentes declarativos
- props en lugar de atributos manuales
- integración natural con el estado y eventos de React
- soporte para accesibilidad en muchos componentes

> Importante: **React-Bootstrap no reemplaza el CSS de Bootstrap**. Debes seguir cargando los estilos de Bootstrap en tu proyecto.

---

## 2. Cuándo usar React-Bootstrap

Úsalo cuando quieras:

- construir interfaces rápido
- reutilizar componentes de UI ya conocidos
- aprovechar el sistema de grid de Bootstrap
- aplicar utilidades CSS sin escribir tanto estilo manual
- mantener una base visual consistente

No es la mejor opción si:

- necesitas un sistema de diseño completamente personalizado
- quieres evitar dependencias de estilo global
- prefieres una solución utility-first como Tailwind

---

## 3. Instalación

## Con npm

```bash
npm install react-bootstrap bootstrap
```

## Con yarn

```bash
yarn add react-bootstrap bootstrap
```

---

## 4. Configuración básica

Importa el CSS global de Bootstrap una sola vez, normalmente en `main.jsx`, `main.tsx`, `index.js` o `index.tsx`.

```jsx
import 'bootstrap/dist/css/bootstrap.min.css';
```

Ejemplo de uso inicial:

```jsx
import Button from 'react-bootstrap/Button';
import Card from 'react-bootstrap/Card';

function App() {
  return (
    <Card className="m-4 shadow-sm">
      <Card.Body>
        <Card.Title>Hola React-Bootstrap</Card.Title>
        <Card.Text>
          Puedes combinar componentes React con utilidades de Bootstrap.
        </Card.Text>
        <Button variant="primary">Acción principal</Button>
      </Card.Body>
    </Card>
  );
}
```

---

## 5. Idea clave: componentes + `className`

Con React-Bootstrap normalmente mezclas dos cosas:

1. **Componentes React-Bootstrap**
   - `Button`
   - `Form`
   - `Modal`
   - `Navbar`
   - `Card`

2. **Clases utilitarias de Bootstrap** mediante `className`
   - `mt-3`
   - `d-flex`
   - `justify-content-between`
   - `text-center`
   - `w-100`

Ejemplo:

```jsx
<Button variant="success" className="w-100 mt-3 rounded-pill">
  Guardar
</Button>
```

- `variant="success"` controla la variante visual del componente
- `className="w-100 mt-3 rounded-pill"` aplica utilidades CSS de Bootstrap

---

## 6. Componentes más usados

## Button

```jsx
import Button from 'react-bootstrap/Button';

<Button variant="primary">Primario</Button>
<Button variant="outline-secondary">Secundario</Button>
<Button variant="danger" size="sm">Eliminar</Button>
```

Props comunes:

- `variant`
- `size`
- `disabled`
- `href`
- `type`

---

## Container, Row y Col

Sistema de layout basado en grid.

```jsx
import Container from 'react-bootstrap/Container';
import Row from 'react-bootstrap/Row';
import Col from 'react-bootstrap/Col';

<Container>
  <Row>
    <Col md={8}>Contenido principal</Col>
    <Col md={4}>Sidebar</Col>
  </Row>
</Container>
```

Muy útil para:

- layouts responsivos
- dashboards
- formularios en columnas
- tarjetas alineadas

---

## Card

```jsx
import Card from 'react-bootstrap/Card';

<Card className="shadow-sm border-0">
  <Card.Body>
    <Card.Title>Título</Card.Title>
    <Card.Text>Contenido de la tarjeta.</Card.Text>
  </Card.Body>
</Card>
```

---

## Form

```jsx
import Form from 'react-bootstrap/Form';
import Button from 'react-bootstrap/Button';

<Form>
  <Form.Group className="mb-3" controlId="email">
    <Form.Label>Email</Form.Label>
    <Form.Control type="email" placeholder="Introduce tu email" />
  </Form.Group>

  <Form.Group className="mb-3" controlId="password">
    <Form.Label>Contraseña</Form.Label>
    <Form.Control type="password" />
  </Form.Group>

  <Button type="submit" variant="primary">Entrar</Button>
</Form>
```

Subcomponentes comunes:

- `Form.Group`
- `Form.Label`
- `Form.Control`
- `Form.Select`
- `Form.Check`
- `Form.Text`

---

## Navbar

```jsx
import Container from 'react-bootstrap/Container';
import Nav from 'react-bootstrap/Nav';
import Navbar from 'react-bootstrap/Navbar';

<Navbar bg="dark" data-bs-theme="dark" expand="lg">
  <Container>
    <Navbar.Brand>Mi App</Navbar.Brand>
    <Navbar.Toggle aria-controls="main-navbar" />
    <Navbar.Collapse id="main-navbar">
      <Nav className="ms-auto">
        <Nav.Link href="#inicio">Inicio</Nav.Link>
        <Nav.Link href="#docs">Docs</Nav.Link>
      </Nav>
    </Navbar.Collapse>
  </Container>
</Navbar>
```

---

## Alert

```jsx
import Alert from 'react-bootstrap/Alert';

<Alert variant="warning">Revisa los campos obligatorios.</Alert>
```

---

## Modal

```jsx
import { useState } from 'react';
import Button from 'react-bootstrap/Button';
import Modal from 'react-bootstrap/Modal';

function ExampleModal() {
  const [show, setShow] = useState(false);

  return (
    <>
      <Button onClick={() => setShow(true)}>Abrir</Button>

      <Modal show={show} onHide={() => setShow(false)} centered>
        <Modal.Header closeButton>
          <Modal.Title>Confirmación</Modal.Title>
        </Modal.Header>
        <Modal.Body>¿Quieres continuar?</Modal.Body>
        <Modal.Footer>
          <Button variant="secondary" onClick={() => setShow(false)}>
            Cancelar
          </Button>
          <Button variant="primary">Aceptar</Button>
        </Modal.Footer>
      </Modal>
    </>
  );
}
```

---

## Table

```jsx
import Table from 'react-bootstrap/Table';

<Table striped bordered hover responsive>
  <thead>
    <tr>
      <th>#</th>
      <th>Nombre</th>
      <th>Rol</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>Ana</td>
      <td>Admin</td>
    </tr>
  </tbody>
</Table>
```

---

## Spinner

```jsx
import Spinner from 'react-bootstrap/Spinner';

<Spinner animation="border" variant="primary" />
```

---

## Badge

```jsx
import Badge from 'react-bootstrap/Badge';

<h2>
  Notificaciones <Badge bg="danger">4</Badge>
</h2>
```

---

## Accordion

```jsx
import Accordion from 'react-bootstrap/Accordion';

<Accordion>
  <Accordion.Item eventKey="0">
    <Accordion.Header>Pregunta 1</Accordion.Header>
    <Accordion.Body>Respuesta 1</Accordion.Body>
  </Accordion.Item>
</Accordion>
```

---

## Tabs

```jsx
import Tab from 'react-bootstrap/Tab';
import Tabs from 'react-bootstrap/Tabs';

<Tabs defaultActiveKey="perfil" className="mb-3">
  <Tab eventKey="perfil" title="Perfil">
    Contenido del perfil
  </Tab>
  <Tab eventKey="config" title="Configuración">
    Contenido de configuración
  </Tab>
</Tabs>
```

---

## Offcanvas

```jsx
import { useState } from 'react';
import Button from 'react-bootstrap/Button';
import Offcanvas from 'react-bootstrap/Offcanvas';

function Sidebar() {
  const [show, setShow] = useState(false);

  return (
    <>
      <Button onClick={() => setShow(true)}>Menú</Button>
      <Offcanvas show={show} onHide={() => setShow(false)}>
        <Offcanvas.Header closeButton>
          <Offcanvas.Title>Navegación</Offcanvas.Title>
        </Offcanvas.Header>
        <Offcanvas.Body>Contenido lateral</Offcanvas.Body>
      </Offcanvas>
    </>
  );
}
```

---

## 7. Variants más útiles

Muchos componentes aceptan `variant` o `bg` con valores como:

- `primary`
- `secondary`
- `success`
- `danger`
- `warning`
- `info`
- `light`
- `dark`

Ejemplo:

```jsx
<Button variant="warning">Editar</Button>
<Alert variant="danger">Error inesperado</Alert>
<Badge bg="success">Activo</Badge>
```

---

## 8. Classnames más útiles de Bootstrap para usar con React-Bootstrap

> Estas clases pertenecen a **Bootstrap** y se suelen aplicar con `className` en componentes React-Bootstrap o en elementos JSX normales.

## 8.1 Espaciado

Muy usadas para separación rápida entre elementos.

- `m-0` a `m-5` → margen general
- `mt-*`, `mb-*`, `ms-*`, `me-*`, `mx-*`, `my-*`
- `p-0` a `p-5` → padding general
- `pt-*`, `pb-*`, `ps-*`, `pe-*`, `px-*`, `py-*`
- `gap-1` a `gap-5` → separación entre hijos flex/grid

Ejemplos:

```jsx
<div className="p-4 mb-3 rounded bg-light">Caja</div>
<div className="d-flex gap-2">...</div>
```

---

## 8.2 Display

Controlan cómo se muestra un elemento.

- `d-none`
- `d-block`
- `d-inline`
- `d-inline-block`
- `d-flex`
- `d-grid`

Responsivas:

- `d-sm-none`
- `d-md-block`
- `d-lg-flex`

Ejemplo:

```jsx
<div className="d-none d-md-block">Solo visible desde md</div>
```

---

## 8.3 Flexbox

Muy importantes en layouts modernos.

- `flex-row`
- `flex-column`
- `flex-wrap`
- `justify-content-start`
- `justify-content-center`
- `justify-content-between`
- `justify-content-end`
- `align-items-start`
- `align-items-center`
- `align-items-end`
- `align-self-start`
- `align-self-center`
- `align-self-end`

Ejemplo:

```jsx
<div className="d-flex justify-content-between align-items-center">
  <span>Título</span>
  <Button size="sm">Acción</Button>
</div>
```

---

## 8.4 Grid responsive

Clases fundamentales cuando no usas `Row` y `Col` o cuando las combinas.

- `container`
- `container-fluid`
- `row`
- `col`
- `col-12`
- `col-md-6`
- `col-lg-4`
- `row-cols-1`
- `row-cols-md-2`
- `row-cols-lg-3`

Ejemplo:

```jsx
<div className="row g-3">
  <div className="col-12 col-md-6 col-lg-4">A</div>
  <div className="col-12 col-md-6 col-lg-4">B</div>
  <div className="col-12 col-md-6 col-lg-4">C</div>
</div>
```

---

## 8.5 Texto

- `text-start`
- `text-center`
- `text-end`
- `text-uppercase`
- `text-lowercase`
- `text-capitalize`
- `text-wrap`
- `text-nowrap`
- `text-break`
- `fw-light`
- `fw-normal`
- `fw-semibold`
- `fw-bold`
- `fst-italic`
- `lh-1`
- `lh-sm`
- `lh-base`
- `lh-lg`

Colores de texto:

- `text-primary`
- `text-secondary`
- `text-success`
- `text-danger`
- `text-warning`
- `text-info`
- `text-muted`
- `text-white`

Ejemplo:

```jsx
<p className="text-muted fw-semibold mb-1">Estado del sistema</p>
```

---

## 8.6 Fondos y color

- `bg-primary`
- `bg-secondary`
- `bg-success`
- `bg-danger`
- `bg-warning`
- `bg-info`
- `bg-light`
- `bg-dark`
- `bg-body`
- `bg-white`
- `bg-transparent`

A menudo se combinan con texto:

```jsx
<div className="bg-dark text-white p-3 rounded">Panel</div>
```

---

## 8.7 Bordes y radios

- `border`
- `border-0`
- `border-top`
- `border-bottom`
- `border-primary`
- `border-secondary`
- `border-danger`
- `rounded`
- `rounded-0`
- `rounded-1`
- `rounded-2`
- `rounded-3`
- `rounded-circle`
- `rounded-pill`

Ejemplo:

```jsx
<div className="border border-primary rounded-3 p-3">Contenido</div>
```

---

## 8.8 Sombras

- `shadow-none`
- `shadow-sm`
- `shadow`
- `shadow-lg`

Ejemplo:

```jsx
<Card className="shadow-sm border-0">...</Card>
```

---

## 8.9 Tamaños

- `w-25`, `w-50`, `w-75`, `w-100`, `w-auto`
- `h-25`, `h-50`, `h-75`, `h-100`, `h-auto`
- `mw-100`
- `vh-100`
- `min-vh-100`

Ejemplo:

```jsx
<Button className="w-100">Enviar</Button>
```

---

## 8.10 Posicionamiento

- `position-static`
- `position-relative`
- `position-absolute`
- `position-fixed`
- `position-sticky`
- `top-0`
- `top-50`
- `bottom-0`
- `start-0`
- `start-50`
- `end-0`
- `translate-middle`

Ejemplo:

```jsx
<div className="position-relative">
  <Badge bg="danger" className="position-absolute top-0 start-100 translate-middle">
    9+
  </Badge>
</div>
```

---

## 8.11 Overflow y visibilidad

- `overflow-auto`
- `overflow-hidden`
- `overflow-scroll`
- `overflow-visible`
- `visible`
- `invisible`

---

## 8.12 Interacción y accesibilidad

- `visually-hidden`
- `user-select-none`
- `pe-none`
- `pe-auto`

Ejemplo:

```jsx
<span className="visually-hidden">Abrir menú principal</span>
```

---

## 8.13 Utilidades responsivas muy prácticas

Bootstrap usa breakpoints estándar:

- `sm`
- `md`
- `lg`
- `xl`
- `xxl`

Puedes aplicarlos en muchas utilidades:

- `d-md-flex`
- `text-lg-start`
- `col-lg-6`
- `px-md-4`
- `mb-xl-0`

Ejemplo:

```jsx
<div className="d-flex flex-column flex-md-row gap-3">
  ...
</div>
```

---

## 9. Classnames que más se usan en proyectos reales

Si buscas un conjunto corto y muy útil para recordar, prioriza estas:

- `container`, `row`, `col-*`
- `d-flex`, `d-grid`
- `justify-content-between`, `justify-content-center`
- `align-items-center`
- `gap-2`, `gap-3`
- `mt-*`, `mb-*`, `p-*`, `px-*`, `py-*`
- `w-100`
- `text-center`, `text-muted`, `fw-bold`
- `bg-light`, `bg-dark`, `text-white`
- `border`, `rounded`, `shadow-sm`
- `position-relative`, `position-absolute`
- `d-none d-md-block`

---

## 10. Patrones comunes

## Formulario limpio

```jsx
<Card className="shadow-sm border-0 mx-auto mt-4" style={{ maxWidth: 480 }}>
  <Card.Body className="p-4">
    <h2 className="h4 mb-3 text-center">Iniciar sesión</h2>
    <Form>
      <Form.Group className="mb-3">
        <Form.Label>Email</Form.Label>
        <Form.Control type="email" />
      </Form.Group>

      <Form.Group className="mb-3">
        <Form.Label>Contraseña</Form.Label>
        <Form.Control type="password" />
      </Form.Group>

      <Button className="w-100" variant="primary">
        Entrar
      </Button>
    </Form>
  </Card.Body>
</Card>
```

---

## Toolbar horizontal

```jsx
<div className="d-flex flex-wrap justify-content-between align-items-center gap-2 mb-3">
  <h1 className="h3 mb-0">Usuarios</h1>
  <div className="d-flex gap-2">
    <Button variant="outline-secondary">Exportar</Button>
    <Button variant="primary">Nuevo usuario</Button>
  </div>
</div>
```

---

## Tarjetas responsivas

```jsx
<Row className="g-3">
  <Col md={6} lg={4}>
    <Card className="h-100 shadow-sm border-0">
      <Card.Body>
        <Card.Title>Métrica 1</Card.Title>
        <Card.Text className="text-muted">Detalle</Card.Text>
      </Card.Body>
    </Card>
  </Col>
</Row>
```

---

## 11. Personalización

Puedes personalizar React-Bootstrap de varias formas:

### 1. Con `className`

```jsx
<Button className="rounded-pill px-4">Guardar</Button>
```

### 2. Con estilos propios

```jsx
<Card className="mi-card-personalizada">...</Card>
```

### 3. Sobrescribiendo variables de Bootstrap con Sass

Útil si quieres cambiar:

- colores de marca
- breakpoints
- tipografías
- spacing
- radios

---

## 12. Buenas prácticas

- importa Bootstrap CSS una sola vez
- combina componentes con utilidades, pero sin abusar de `className`
- usa `Container`, `Row` y `Col` para estructura general
- usa `Stack`, `d-flex` o `gap-*` para alineaciones simples
- mantén consistencia en `variant`
- evita sobreescribir Bootstrap con selectores demasiado agresivos
- revisa accesibilidad en modales, formularios y navegación
- no mezcles demasiadas utilidades si el componente necesita estilos propios reutilizables

---

## 13. Errores comunes

### 1. Instalar React-Bootstrap pero no importar Bootstrap CSS

Síntoma: los componentes se renderizan sin estilo.

### 2. Pensar que todas las clases son de React-Bootstrap

En realidad, muchas clases útiles (`d-flex`, `mt-3`, `text-center`) son **utilidades de Bootstrap**, no de React-Bootstrap.

### 3. Sobrecargar `className`

Si un elemento tiene 12 o 15 clases, quizá conviene mover parte del estilo a una clase propia.

### 4. No aprovechar props del componente

Por ejemplo, usar muchas clases para un botón cuando `variant`, `size` o props del componente ya resuelven el caso.

---

## 14. React-Bootstrap vs Bootstrap clásico

| Aspecto | Bootstrap clásico | React-Bootstrap |
|---|---|---|
| Uso principal | HTML + clases + JS de Bootstrap | Componentes React |
| Interacción | Imperativa | Declarativa |
| Integración con estado | Manual | Natural con React |
| Reutilización | Basada en markup | Basada en componentes |
| JS de Bootstrap | Más necesario | Menos necesario en muchos casos |

---

## 15. Chuleta rápida

## Componentes clave

- `Button`
- `Form`
- `Modal`
- `Navbar`
- `Card`
- `Table`
- `Alert`
- `Accordion`
- `Tabs`
- `Offcanvas`
- `Spinner`
- `Badge`
- `Container`, `Row`, `Col`

## Classnames más rentables

- `d-flex`
- `justify-content-between`
- `align-items-center`
- `gap-2` / `gap-3`
- `mt-3` / `mb-3` / `p-3`
- `w-100`
- `text-center`
- `text-muted`
- `bg-light`
- `text-white`
- `border`
- `rounded`
- `shadow-sm`
- `position-relative`
- `d-none d-md-block`

---

## 16. Conclusión

React-Bootstrap es una solución muy práctica cuando quieres construir interfaces rápidas y consistentes en React usando el ecosistema Bootstrap.

La clave para usarlo bien es entender esta combinación:

- **componentes React-Bootstrap** para estructura y comportamiento
- **classnames utilitarias de Bootstrap** para layout, espaciado, alineación y detalle visual

Si dominas ambas piezas, podrás montar interfaces limpias, responsivas y mantenibles con mucha velocidad.
