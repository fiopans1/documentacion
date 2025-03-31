# GUIA HOOKS REACT:

### Mini Guía de los Principales Hooks de React

#### Introducción a los Hooks

Los hooks en React son funciones que permiten usar características como el estado y el ciclo de vida en componentes funcionales. Fueron introducidos en la versión 16.8 para simplificar el desarrollo y evitar el uso excesivo de clases.

---

## Hooks Básicos

### 1. `useState`

Este hook permite agregar un estado local a un componente funcional.

**Sintaxis:**

```javascript
const [state, setState] = useState(initialState);
```

**Ejemplo:**

```javascript
import { useState } from "react";

function Contador() {
  const [contador, setContador] = useState(0);

  return (
    <div>
      <p>El contador es: {contador}</p>
      <button onClick={() => setContador(contador + 1)}>Incrementar</button>
    </div>
  );
}
```

---

### 2. `useEffect`

Permite manejar efectos secundarios como llamadas a APIs, suscripciones o manipulación del DOM.

**Sintaxis:**

```javascript
useEffect(() => {
  // Código del efecto
  return () => {
    // Limpieza opcional
  };
}, [dependencias]);
```

**Ejemplo:**

```javascript
import { useEffect, useState } from "react";

function Usuarios() {
  const [usuarios, setUsuarios] = useState([]);

  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/users")
      .then((response) => response.json())
      .then((data) => setUsuarios(data));
  }, []);

  return (
    <ul>
      {usuarios.map((usuario) => (
        <li key={usuario.id}>{usuario.name}</li>
      ))}
    </ul>
  );
}
```

---

### 3. `useContext`

Se usa para consumir un contexto sin necesidad de usar componentes de orden superior.

**Sintaxis:**

```javascript
const value = useContext(Context);
```

**Ejemplo:**

```javascript
import { useContext, createContext } from "react";

const TemaContext = createContext("claro");

function Componente() {
  const tema = useContext(TemaContext);

  return (
    <div style={{ background: tema === "claro" ? "#fff" : "#333" }}>
      Tema: {tema}
    </div>
  );
}

function App() {
  return (
    <TemaContext.Provider value="oscuro">
      <Componente />
    </TemaContext.Provider>
  );
}
```

---

## Hooks Adicionales

### 4. `useReducer`

Ideal para manejar estados complejos o lógica de actualización avanzada.

**Sintaxis:**

```javascript
const [state, dispatch] = useReducer(reducer, initialState);
```

**Ejemplo:**

```javascript
import { useReducer } from "react";

function reducer(state, action) {
  switch (action.type) {
    case "incrementar":
      return { contador: state.contador + 1 };
    case "decrementar":
      return { contador: state.contador - 1 };
    default:
      return state;
  }
}

function Contador() {
  const [state, dispatch] = useReducer(reducer, { contador: 0 });

  return (
    <div>
      <p>Contador: {state.contador}</p>
      <button onClick={() => dispatch({ type: "incrementar" })}>+</button>
      <button onClick={() => dispatch({ type: "decrementar" })}>-</button>
    </div>
  );
}
```

---

### 5. `useRef`

Permite acceder directamente a un elemento del DOM o almacenar valores persistentes.

**Sintaxis:**

```javascript
const refContainer = useRef(initialValue);
```

**Ejemplo:**

```javascript
import { useRef } from "react";

function InputEnfocado() {
  const inputRef = useRef();

  const enfocarInput = () => {
    inputRef.current.focus();
  };

  return (
    <div>
      <input ref={inputRef} type="text" />
      <button onClick={enfocarInput}>Enfocar</button>
    </div>
  );
}
```

---

### 6. `useMemo`

Memoriza valores calculados para optimizar el rendimiento.

**Sintaxis:**

```javascript
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

**Ejemplo:**

```javascript
import { useMemo } from "react";

function SumaPesada({ a, b }) {
  const resultado = useMemo(() => {
    console.log("Calculando...");
    return a + b;
  }, [a, b]);

  return <p>Resultado: {resultado}</p>;
}
```

---

### 7. `useCallback`

Memoriza funciones para evitar su recreación en cada renderizado.

**Sintaxis:**

```javascript
const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);
```

**Ejemplo:**

```javascript
import { useCallback } from "react";

function Lista({ items, onItemClick }) {
  const handleClick = useCallback(
    (item) => {
      onItemClick(item);
    },
    [onItemClick]
  );

  return (
    <ul>
      {items.map((item) => (
        <li key={item} onClick={() => handleClick(item)}>
          {item}
        </li>
      ))}
    </ul>
  );
}
```

---

### 8. `useLayoutEffect`

Similar a `useEffect`, pero se ejecuta después de todas las mutaciones del DOM.

---

### 9. `useImperativeHandle`

Permite personalizar el valor de la referencia expuesta al usar `React.forwardRef`.

---

# Sección 1: `useState`

El hook `useState` permite agregar y manejar el estado en un componente funcional de React. Es una de las herramientas más usadas para trabajar con datos reactivos dentro de una aplicación.

## Introducción

`useState` devuelve un array con dos elementos:

1. El estado actual.
2. Una función que permite actualizar dicho estado.

**Sintaxis:**

```javascript
const [state, setState] = useState(initialState);
```

- `initialState`: Valor inicial del estado. Puede ser de cualquier tipo de dato (número, string, booleano, objeto, array, etc.).
- `state`: Representa el valor actual del estado.
- `setState`: Es una función que actualiza el estado y dispara un nuevo renderizado del componente.

## Ejemplo Básico

El siguiente ejemplo muestra un contador que incrementa su valor al hacer clic en un botón.

```javascript
import { useState } from "react";

function Contador() {
  const [contador, setContador] = useState(0); // Estado inicial en 0

  return (
    <div>
      <p>El contador es: {contador}</p>
      <button onClick={() => setContador(contador + 1)}>Incrementar</button>
    </div>
  );
}
```

En este ejemplo:

- `contador` contiene el valor actual del estado.
- `setContador` se usa para actualizar el valor del contador.

## Manejo de Estados Complejos

Aunque `useState` se utiliza frecuentemente para valores simples, también puede manejar estados más complejos como objetos o arrays.

### Ejemplo con Objetos

```javascript
function Usuario() {
  const [usuario, setUsuario] = useState({ nombre: "Juan", edad: 30 });

  const actualizarEdad = () => {
    setUsuario({ ...usuario, edad: usuario.edad + 1 }); // Mantener las propiedades existentes
  };

  return (
    <div>
      <p>Nombre: {usuario.nombre}</p>
      <p>Edad: {usuario.edad}</p>
      <button onClick={actualizarEdad}>Cumplir Años</button>
    </div>
  );
}
```

En este caso:

- Usamos la sintaxis de propagación (`...usuario`) para mantener las propiedades no modificadas del objeto.

### Ejemplo con Arrays

```javascript
function ListaTareas() {
  const [tareas, setTareas] = useState(["Tarea 1", "Tarea 2"]);

  const agregarTarea = () => {
    setTareas([...tareas, `Tarea ${tareas.length + 1}`]); // Agregar una nueva tarea
  };

  return (
    <div>
      <ul>
        {tareas.map((tarea, index) => (
          <li key={index}>{tarea}</li>
        ))}
      </ul>
      <button onClick={agregarTarea}>Agregar Tarea</button>
    </div>
  );
}
```

Aquí usamos `setTareas` para actualizar el array sin modificar el original (inmutabilidad).

## Reglas de Uso

1. **Solo dentro de componentes funcionales o custom hooks.** No se puede usar `useState` fuera de un componente funcional.
2. **No llames `useState` condicionalmente.** Debe ser invocado siempre en el mismo orden para evitar errores de React.

## Conclusión

`useState` es un hook fundamental en React para manejar estados. Su simplicidad y versatilidad lo convierten en una herramienta esencial para crear interfaces reactivas.

¿Quieres aprender más ejemplos avanzados o integrarlo con otros hooks como `useEffect`?

---

# Sección 2: `useEffect`

El hook `useEffect` permite manejar efectos secundarios en los componentes funcionales. Es esencial para realizar tareas como:

- Fetching de datos desde una API.
- Suscripciones a eventos.
- Manipulación directa del DOM.

## Introducción

`useEffect` acepta dos parámetros:

1. Una función que contiene el efecto a ejecutar.
2. Una lista de dependencias opcional.

**Sintaxis:**

```javascript
useEffect(() => {
  // Código del efecto
  return () => {
    // Código de limpieza opcional
  };
}, [dependencias]);
```

## Uso de `await` en `useEffect`

Para utilizar operaciones asíncronas con `await` dentro de `useEffect`, debes crear una función asíncrona interna y luego invocarla:

```javascript
useEffect(() => {
  // Declaración de la función asíncrona
  const fetchData = async () => {
    try {
      const response = await fetch('https://api.example.com/data');
      const data = await response.json();
      setData(data);
    } catch (error) {
      console.error('Error fetching data:', error);
    }
  };

  // Llamada inmediata a la función
  fetchData();

  // Si es necesario, puedes devolver una función de limpieza
  return () => {
    // Código de limpieza
  };
}, [dependencies]);
```

> Nota: No puedes usar `async` directamente en la función que pasas a `useEffect` porque las funciones async devuelven una Promise, y la función de limpieza de `useEffect` debe devolver una función o nada, no una Promise.

---

- **Efecto:** El código dentro de la función se ejecuta después de que el componente renderiza.
- **Limpieza:** Si el efecto necesita limpiar recursos (por ejemplo, cancelar una suscripción), se puede devolver una función de limpieza.
- **Dependencias:** Especifica cuándo debe ejecutarse el efecto. Si está vacío (`[]`), el efecto se ejecuta solo una vez al montar.

## Ejemplo Básico

El siguiente ejemplo realiza un fetch de datos cuando el componente se monta.

```javascript
import { useState, useEffect } from "react";

function Usuarios() {
  const [usuarios, setUsuarios] = useState([]);

  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/users")
      .then((response) => response.json())
      .then((data) => setUsuarios(data));
  }, []); // [] asegura que solo se ejecute una vez

  return (
    <ul>
      {usuarios.map((usuario) => (
        <li key={usuario.id}>{usuario.name}</li>
      ))}
    </ul>
  );
}
```

En este ejemplo:

- El efecto realiza un fetch cuando el componente se monta.
- La lista de dependencias vacía asegura que el efecto no se ejecute en cada renderizado.

## Ejemplo con Limpieza

Supongamos que necesitas agregar y eliminar un listener al montar y desmontar el componente.

```javascript
import { useState, useEffect } from "react";

function Ventana() {
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    const actualizarWidth = () => setWidth(window.innerWidth);
    window.addEventListener("resize", actualizarWidth);

    return () => {
      window.removeEventListener("resize", actualizarWidth); // Limpieza
    };
  }, []);

  return <p>El ancho de la ventana es: {width}px</p>;
}
```

## Reglas de Uso

1. **Solo dentro de componentes funcionales o custom hooks.**
2. **No llames `useEffect` condicionalmente.** Debe invocarse siempre en el mismo orden.
3. **Especifica las dependencias correctamente.** Evita dependencias faltantes para prevenir comportamientos inesperados.

## Conclusión

`useEffect` es un hook poderoso para manejar efectos secundarios en React. Con su ayuda, puedes sincronizar componentes con recursos externos, manejar eventos y limpiar recursos eficientemente.

---

# Sección 3: `useContext`

El hook `useContext` se utiliza para consumir datos de un contexto en un componente funcional. Es una alternativa más limpia y sencilla que usar el componente `<Context.Consumer>`, especialmente en aplicaciones donde se necesita compartir información entre múltiples componentes.

## Introducción

Un **contexto** en React es una forma de pasar datos a través del árbol de componentes sin tener que propagar props manualmente en cada nivel. Esto es útil para manejar datos globales como:

- Tema (modo claro/oscuro).
- Usuario autenticado.
- Configuración de idioma.

`useContext` permite acceder directamente al valor del contexto desde cualquier componente funcional.

**Sintaxis:**

```javascript
const valor = useContext(Contexto);
```

- `Contexto`: Es el objeto de contexto creado con `React.createContext()`.
- `valor`: Es el valor actual del contexto.

---

## Ejemplo Básico

A continuación, veremos un ejemplo de cómo crear y usar un contexto para manejar un tema (claro u oscuro).

```javascript
import React, { createContext, useContext, useState } from "react";

// 1. Crear el contexto
const TemaContexto = createContext();

function ProveedorTema({ children }) {
  const [tema, setTema] = useState("claro"); // Estado del tema

  const cambiarTema = () => {
    setTema(tema === "claro" ? "oscuro" : "claro");
  };

  return (
    <TemaContexto.Provider value={{ tema, cambiarTema }}>
      {children}
    </TemaContexto.Provider>
  );
}

function BotonTema() {
  // 2. Consumir el contexto
  const { tema, cambiarTema } = useContext(TemaContexto);

  return (
    <button onClick={cambiarTema}>
      Cambiar a tema {tema === "claro" ? "oscuro" : "claro"}
    </button>
  );
}

function App() {
  return (
    <ProveedorTema>
      <BotonTema />
    </ProveedorTema>
  );
}

export default App;
```

### Explicación:

1. **Crear el contexto:**  
   Usamos `createContext()` para crear un objeto de contexto (`TemaContexto`) que contendrá el estado y la función para cambiar el tema.

2. **Proveer el contexto:**  
   Envolvemos los componentes hijos con `<TemaContexto.Provider>` y pasamos el valor del contexto (en este caso, un objeto con el estado `tema` y la función `cambiarTema`).

3. **Consumir el contexto:**  
   Usamos `useContext(TemaContexto)` para acceder al valor del contexto en un componente funcional.

---

## Reglas de Uso

1. **Debe usarse con un proveedor (`<Context.Provider>`):** Si intentas consumir un contexto sin envolverlo en un proveedor, obtendrás `undefined`.
2. **Úsalo para datos globales o compartidos:** No abuses de los contextos para datos locales o específicos de un componente.
3. **No llames `useContext` condicionalmente:** Debe ser invocado siempre de manera consistente.

---

> Note: En el contexto puedes meter tantos estados y funciones como quieras

---

# Sección 4: `useReducer`

El hook `useReducer` es una alternativa más avanzada a `useState` que se utiliza cuando se necesita manejar estados complejos o múltiples transiciones de estado. Es ideal para aplicaciones donde las actualizaciones de estado dependen de lógica más estructurada.

## Introducción

`useReducer` se basa en el concepto de un **reductor**, que es una función que toma un estado actual y una acción, y devuelve un nuevo estado.

**Sintaxis:**

```javascript
const [state, dispatch] = useReducer(reducer, initialState);
```

- **`reducer`**: Una función que define cómo cambiará el estado en respuesta a las acciones.
- **`initialState`**: El estado inicial del componente.
- **`dispatch`**: Una función que se utiliza para enviar acciones al reductor.

---

## Ejemplo Básico

Un contador que puede incrementar, decrementar y reiniciar utilizando `useReducer`.

```javascript
import { useReducer } from "react";

// Reducer: define cómo cambia el estado
function contadorReducer(state, action) {
  switch (action.type) {
    case "incrementar":
      return { contador: state.contador + 1 };
    case "decrementar":
      return { contador: state.contador - 1 };
    case "reiniciar":
      return { contador: 0 };
    default:
      throw new Error(`Acción no soportada: ${action.type}`);
  }
}

function Contador() {
  const [state, dispatch] = useReducer(contadorReducer, { contador: 0 });

  return (
    <div>
      <p>Contador: {state.contador}</p>
      <button onClick={() => dispatch({ type: "incrementar" })}>
        Incrementar
      </button>
      <button onClick={() => dispatch({ type: "decrementar" })}>
        Decrementar
      </button>
      <button onClick={() => dispatch({ type: "reiniciar" })}>Reiniciar</button>
    </div>
  );
}

export default Contador;
```

---

# Sección 5. `useRef`

El hook `useRef` se utiliza para crear una referencia mutable que puede ser asignada a un DOM o a cualquier valor que necesitemos persistir entre renders sin causar una nueva renderización del componente. A diferencia del hook `useState`, las actualizaciones del valor referenciado no provocan un re-render del componente.

#### 5.1. Sintaxis básica

```javascript
const myRef = useRef(initialValue);
```

- `initialValue`: el valor inicial de la referencia. Este valor puede ser cualquier tipo de dato, ya sea un objeto, número, string, etc.

#### 5.2. Usos principales de `useRef`

1. **Referencias a elementos del DOM:**
   Se utiliza frecuentemente para referenciar un nodo del DOM directamente, lo cual puede ser útil para acceder a sus propiedades o métodos (como `focus()`, `scrollIntoView()`, etc.).

   ```javascript
   import React, { useRef } from "react";

   function FocusInput() {
     const inputRef = useRef(null);

     const handleClick = () => {
       inputRef.current.focus();
     };

     return (
       <div>
         <input ref={inputRef} type="text" />
         <button onClick={handleClick}>Focus on Input</button>
       </div>
     );
   }
   ```

2. **Persistencia de valores sin re-renderizar:**
   `useRef` es útil para almacenar valores que no necesitan causar un re-render cuando cambian, como el seguimiento de un contador o un valor que no depende del ciclo de vida de la UI.

   ```javascript
   import React, { useRef, useEffect } from "react";

   function Timer() {
     const countRef = useRef(0);

     useEffect(() => {
       const timer = setInterval(() => {
         countRef.current += 1;
         console.log(countRef.current);
       }, 1000);

       return () => clearInterval(timer); // Limpiar interval cuando el componente se desmonte
     }, []);

     return <div>Revisar la consola para ver el contador</div>;
   }
   ```

3. **Mantener un valor entre renders sin causar re-renderización:**
   Si necesitamos mantener una variable que no debe provocar un re-render cuando cambie, `useRef` es la opción.

   ```javascript
   function Timer() {
     const countRef = useRef(0);

     const increment = () => {
       countRef.current += 1;
       console.log(countRef.current);
     };

     return (
       <div>
         <button onClick={increment}>Incrementar contador</button>
       </div>
     );
   }
   ```

#### 5.3. Diferencia entre `useRef` y `useState`

- `useRef`: No provoca un re-render cuando se actualiza la referencia. Ideal para almacenar valores que no afectan la UI.
- `useState`: Provoca un re-render cuando se actualiza el estado. Es usado para manejar valores que afectan la interfaz de usuario.

#### 5.4. Casos de uso avanzados

1. **Comprobaciones previas de renderización:**
   Si necesitas comparar un valor actual con su valor anterior sin causar un re-render, puedes usar `useRef` para almacenar el valor anterior.

   ```javascript
   import React, { useState, useRef, useEffect } from "react";

   function PreviousValue() {
     const [count, setCount] = useState(0);
     const prevCountRef = useRef();

     useEffect(() => {
       prevCountRef.current = count;
     }, [count]);

     return (
       <div>
         <p>Valor actual: {count}</p>
         <p>Valor anterior: {prevCountRef.current}</p>
         <button onClick={() => setCount(count + 1)}>Incrementar</button>
       </div>
     );
   }
   ```

2. **Referencias a funciones y objetos:**
   `useRef` también puede ser útil para mantener referencias a funciones o cualquier objeto mutable que no deba provocar renders.

#### 5.5. Resumen

- `useRef` te permite acceder y modificar el DOM directamente.
- Se utiliza para mantener valores persistentes sin provocar renders.
- Es útil para almacenar cualquier tipo de valor, como contadores o referencias a elementos del DOM.

---

# Sección 6. `useMemo`

El hook `useMemo` se utiliza para memorizar valores calculados, es decir, evitar que una operación costosa se vuelva a ejecutar en cada render. Se ejecuta solo cuando las dependencias especificadas cambian. Esto puede ser útil para mejorar el rendimiento en componentes que realizan cálculos complejos o costosos.

#### 6.1. Sintaxis básica

```javascript
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

- **`computeExpensiveValue(a, b)`**: la función que realiza una operación costosa o un cálculo complejo.
- **`[a, b]`**: las dependencias. `useMemo` solo volverá a ejecutar el cálculo si alguna de las dependencias cambia.

#### 6.2. ¿Por qué usar `useMemo`?

Cuando un componente se renderiza, las funciones y valores dentro de él se vuelven a calcular. Sin embargo, no todos los valores necesitan ser recalculados en cada render. Si tenemos un cálculo costoso, podríamos querer "memorizar" el resultado de ese cálculo y evitar su ejecución innecesaria.

`useMemo` ayuda a evitar cálculos repetidos, lo que puede mejorar el rendimiento, especialmente en componentes con operaciones intensivas.

#### 6.3. Ejemplo básico

Imagina que tenemos una lista de números y necesitamos calcular la suma de los números pares. Si el cálculo es costoso, podemos usar `useMemo` para evitar que se recalculen en cada render, a menos que la lista cambie.

```javascript
import React, { useMemo } from "react";

function SumEvenNumbers({ numbers }) {
  const sumEven = useMemo(() => {
    console.log("Calculando la suma de los números pares...");
    return numbers
      .filter((num) => num % 2 === 0)
      .reduce((acc, num) => acc + num, 0);
  }, [numbers]);

  return <div>Suma de números pares: {sumEven}</div>;
}

export default SumEvenNumbers;
```

- En este ejemplo, el cálculo de la suma de los números pares solo se realiza cuando la lista `numbers` cambia.
- Si los números no cambian, `useMemo` devolverá el valor memorizado sin ejecutar de nuevo la operación de filtro y reducción.

#### 6.4. ¿Cuándo usar `useMemo`?

Aunque `useMemo` puede ayudar con el rendimiento en situaciones específicas, **no debe usarse de forma excesiva**. En la mayoría de los casos, React es bastante eficiente con su proceso de renderizado, y memorizar valores puede agregar complejidad innecesaria.

Es útil en los siguientes casos:

1. **Operaciones de cálculo costosas**: Como en el caso de cálculos matemáticos complejos, transformaciones de datos o procesamiento de grandes volúmenes de datos.
2. **Listas grandes de elementos**: Cuando tienes una lista de elementos y necesitas realizar transformaciones o filtrados que no deberían ejecutarse en cada render.
3. **Componentes con múltiples renderizados**: En situaciones donde un componente se renderiza muchas veces con las mismas entradas.

#### 6.5. Comparación de objetos y arrays

Uno de los casos más comunes en los que `useMemo` es útil es cuando trabajamos con objetos o arrays, porque React realiza una comparación superficial de las dependencias y no verifica si los elementos dentro de un array u objeto han cambiado. Si pasamos un objeto o array como dependencia, la referencia se actualizará cada vez, incluso si su contenido no cambia.

```javascript
import React, { useMemo } from "react";

function List({ items }) {
  const listItems = useMemo(
    () => items.map((item) => <li key={item.id}>{item.name}</li>),
    [items]
  );

  return <ul>{listItems}</ul>;
}
```

En este caso, si `items` no cambia, `useMemo` garantiza que el cálculo de la lista de elementos solo ocurra una vez.

#### 6.6. Resumen

- `useMemo` memoriza un valor calculado para evitar recomputaciones innecesarias entre renders.
- Debe usarse cuando el cálculo es costoso o la operación afecta significativamente el rendimiento.
- Las dependencias deben ser las variables que, al cambiar, requieren un nuevo cálculo del valor memorizado.
- **No abuses de `useMemo`**. En muchos casos, React maneja los renders de manera eficiente sin la necesidad de optimizaciones adicionales.

---

# Sección 7. `useCallback`

El hook `useCallback` se utiliza para memorizar funciones. Su objetivo es evitar que se vuelvan a crear funciones en cada render. Al igual que con `useMemo`, `useCallback` devuelve una versión memorizada de una función, y solo se vuelve a crear si sus dependencias cambian.

#### 7.1. Sintaxis básica

```javascript
const memoizedCallback = useCallback(() => {
  // Código de la función
}, [dependencias]);
```

- **`memoizedCallback`**: La versión memorizada de la función.
- **`[dependencias]`**: Array de dependencias. `useCallback` solo vuelve a crear la función si alguna de las dependencias cambia.

#### 7.2. ¿Por qué usar `useCallback`?

Por defecto, las funciones se recrean en cada renderizado de un componente. En algunos casos, esto puede ser innecesario o incluso perjudicial para el rendimiento, especialmente si esas funciones se pasan como propiedades a componentes hijos. Si el componente hijo depende de la función, React puede considerar que la función ha cambiado, lo que puede provocar renders adicionales. `useCallback` permite evitar estos renders innecesarios.

`useCallback` es especialmente útil en los siguientes escenarios:

1. **Funciones pasadas como props a componentes hijos**: Si un componente hijo está optimizado con `React.memo` o tiene dependencias pesadas, `useCallback` asegura que la referencia de la función no cambie innecesariamente.
2. **Funciones que se utilizan en hooks de efectos**: Si tienes funciones que dependen de valores dentro de un `useEffect` o `useLayoutEffect`, `useCallback` puede asegurarse de que la función no se vuelva a crear entre renders.

#### 7.3. Ejemplo básico de `useCallback`

Imagina que tienes un componente con un botón que, al hacer clic, incrementa un contador. Queremos pasar la función de incremento a un componente hijo sin que esta función se recree en cada render.

```javascript
import React, { useState, useCallback } from "react";

function Child({ increment }) {
  console.log("El componente hijo se renderiza");

  return <button onClick={increment}>Incrementar</button>;
}

function Parent() {
  const [count, setCount] = useState(0);

  const increment = useCallback(() => {
    setCount((prevCount) => prevCount + 1);
  }, []); // La función solo se recrea si las dependencias cambian

  return (
    <div>
      <p>Contador: {count}</p>
      <Child increment={increment} />
    </div>
  );
}

export default Parent;
```

En este ejemplo:

- La función `increment` solo se vuelve a crear si alguna de sus dependencias cambia (en este caso no tiene dependencias, así que no se vuelve a crear entre renders).
- El componente `Child` solo se renderiza cuando el estado del contador cambia, no cuando la función `increment` cambia (porque `useCallback` asegura que la referencia de la función sea la misma).

#### 7.4. `useCallback` vs `useMemo`

Aunque ambos hooks, `useCallback` y `useMemo`, sirven para memorizar valores y evitar recalculaciones innecesarias, la principal diferencia radica en el tipo de valor que están memorizando:

- **`useMemo`**: Memoriza un valor calculado (como el resultado de una función).
- **`useCallback`**: Memoriza una función.

En resumen:

- Usa **`useMemo`** cuando quieras memorizar el resultado de un cálculo.
- Usa **`useCallback`** cuando quieras memorizar una función.

#### 7.5. Casos en los que `useCallback` es útil

1. **Funciones pasadas como props a componentes hijos optimizados (con `React.memo`)**:
   Si un componente hijo está envuelto en `React.memo` (para evitar renders innecesarios), y ese componente recibe una función como prop, React comparará la referencia de la función para decidir si debe volver a renderizar el componente. Si la función se recrea en cada render, el componente hijo se renderiza nuevamente, incluso si no es necesario. Usar `useCallback` para memorizar la función evita este comportamiento.

2. **Optimización de efectos secundarios**:
   Si tienes un efecto que depende de una función y no deseas que se ejecute innecesariamente cada vez que el componente se renderiza, puedes usar `useCallback` para memorizar esa función y asegurarte de que el efecto solo se ejecute cuando realmente sea necesario.

#### 7.6. Ejemplo con `React.memo` y `useCallback`

```javascript
import React, { useState, useCallback } from "react";

// Componente hijo que solo se renderiza si las props cambian
const Child = React.memo(({ onClick }) => {
  console.log("Componente hijo renderizado");
  return <button onClick={onClick}>Incrementar</button>;
});

function Parent() {
  const [count, setCount] = useState(0);

  const increment = useCallback(() => {
    setCount((prev) => prev + 1);
  }, []); // La función no se recreará entre renders

  return (
    <div>
      <p>Contador: {count}</p>
      <Child onClick={increment} />
    </div>
  );
}

export default Parent;
```

En este caso, `Child` solo se renderiza cuando `increment` cambia, lo que se logra gracias a `useCallback` y `React.memo`.

#### 7.7. Resumen

- `useCallback` memoriza funciones para evitar que se creen nuevamente en cada render.
- Es útil cuando las funciones se pasan como props a componentes optimizados o cuando se usan dentro de hooks de efectos.
- Como `useMemo`, no debe usarse en exceso, ya que React maneja de manera eficiente la creación de funciones en la mayoría de los casos.

---

# 8. `useLazy` en React

En realidad, no existe un hook llamado `useLazy` en la API oficial de React. Sin embargo, el concepto de **lazy loading** (carga perezosa) en React se maneja generalmente mediante `React.lazy` junto con `Suspense`.

Lo que probablemente estés refiriendo con `useLazy` es el **cargando de componentes de manera diferida** o **perezosa** (lazy loading), un patrón de optimización que React maneja con `React.lazy` y `Suspense`.

A continuación, explicaré cómo utilizar **`React.lazy`** para la carga diferida de componentes, lo que podría estar relacionado con lo que buscas.

### 8.1. `React.lazy` y `Suspense` en React

#### ¿Qué es `React.lazy`?

`React.lazy` es una función que te permite definir un componente que se carga de manera diferida, es decir, solo cuando se necesita. Esto ayuda a dividir el código en partes más pequeñas y cargarlas solo cuando el usuario las necesite, mejorando el rendimiento de la aplicación.

#### Sintaxis básica

```javascript
const LazyComponent = React.lazy(() => import("./LazyComponent"));
```

- **`import('./LazyComponent')`**: Esta es una importación dinámica que se hace solo cuando el componente es renderizado.
- **`React.lazy`**: Se utiliza para envolver la importación dinámica y permitir que React maneje el componente de manera diferida.

#### Uso de `React.lazy` con `Suspense`

Como `React.lazy` carga los componentes de manera diferida, debemos envolver estos componentes en un `Suspense` para manejar el estado de carga (loading state) mientras el componente está siendo cargado.

```javascript
import React, { Suspense } from "react";

// Cargar LazyComponent de manera perezosa
const LazyComponent = React.lazy(() => import("./LazyComponent"));

function App() {
  return (
    <div>
      <h1>Mi aplicación con carga perezosa</h1>

      {/* Suspense maneja el fallback mientras el componente se carga */}
      <Suspense fallback={<div>Cargando...</div>}>
        <LazyComponent />
      </Suspense>
    </div>
  );
}

export default App;
```

- **`Suspense`**: Se utiliza para envolver los componentes que usan `React.lazy`. El prop `fallback` define lo que se muestra mientras el componente se carga (por ejemplo, un mensaje o un spinner).

### 8.2. ¿Por qué usar `React.lazy`?

`React.lazy` y `Suspense` permiten a los desarrolladores cargar componentes bajo demanda, lo que optimiza el rendimiento de la aplicación al dividir el código en partes más pequeñas y cargar solo lo que es necesario en ese momento. Esto es especialmente útil en aplicaciones grandes donde la carga inicial puede ser costosa si se incluyen todos los componentes de una vez.

### 8.3. Ejemplo de carga perezosa con `React.lazy` y `Suspense`

```javascript
import React, { Suspense } from "react";

// Cargar el componente 'LargeComponent' de manera diferida
const LargeComponent = React.lazy(() => import("./LargeComponent"));

function App() {
  return (
    <div>
      <h1>Aplicación React con carga perezosa</h1>

      {/* Suspense maneja el estado de carga */}
      <Suspense fallback={<div>Esperando...</div>}>
        <LargeComponent />
      </Suspense>
    </div>
  );
}

export default App;
```

En este ejemplo:

- `LargeComponent` solo se carga cuando el componente es renderizado, y durante ese tiempo, el texto "Esperando..." se muestra como un **fallback**.
- Esto mejora la experiencia del usuario, ya que no se cargan todos los componentes de inmediato, sino solo los necesarios.

### 8.4. Consideraciones

- **Error Handling**: Es importante tener en cuenta que si la carga del componente falla (por ejemplo, si el archivo no está disponible o hay un error en la red), se puede manejar el error utilizando `ErrorBoundary`. Puedes envolver el componente `Suspense` con un `ErrorBoundary` para mostrar un mensaje de error si algo sale mal.

#### Ejemplo de manejo de errores con `ErrorBoundary`

```javascript
import React, { Suspense } from "react";

class ErrorBoundary extends React.Component {
  state = { hasError: false };

  static getDerivedStateFromError(error) {
    return { hasError: true };
  }

  componentDidCatch(error, info) {
    console.log("Error:", error);
    console.log("Info:", info);
  }

  render() {
    if (this.state.hasError) {
      return <h1>Algo salió mal al cargar el componente.</h1>;
    }

    return this.props.children;
  }
}

const LazyComponent = React.lazy(() => import("./LazyComponent"));

function App() {
  return (
    <div>
      <h1>Aplicación con carga perezosa</h1>

      {/* Suspense con manejo de errores */}
      <ErrorBoundary>
        <Suspense fallback={<div>Cargando...</div>}>
          <LazyComponent />
        </Suspense>
      </ErrorBoundary>
    </div>
  );
}

export default App;
```

### 8.5. Resumen

- **`React.lazy`** permite cargar componentes de manera diferida (lazy loading).
- Se utiliza con **`Suspense`** para manejar el estado de carga mientras se espera que el componente se cargue.
- Es útil para optimizar el rendimiento de aplicaciones grandes, cargando solo los componentes necesarios.
- Puedes envolver `Suspense` con un `ErrorBoundary` para manejar posibles errores durante la carga de componentes.

Aunque no hay un hook `useLazy` como tal, el patrón de lazy loading en React generalmente se implementa con `React.lazy` y `Suspense`. Si tienes más preguntas o ejemplos específicos en mente, ¡dímelo!
