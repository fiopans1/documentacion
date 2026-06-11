# Guía Completa de Zustand

## 1. ¿Qué es Zustand?

**Zustand** es una librería ligera para manejar estado global en aplicaciones React.

Su idea principal es muy simple:

- defines un **store**
- guardas estado y acciones dentro de ese store
- consumes solo la parte que necesitas desde cada componente

Zustand evita mucha de la complejidad típica de otras soluciones de estado global. No necesitas reducers obligatorios, providers enormes ni demasiado boilerplate.

---

## 2. Cuándo usar Zustand

Zustand encaja muy bien cuando necesitas:

- compartir estado entre varios componentes
- evitar prop drilling
- tener una API simple
- separar la lógica de estado de la UI
- mantener buen rendimiento con suscripciones selectivas

Ejemplos típicos:

- carrito de compra
- usuario autenticado
- filtros de búsqueda
- tema claro/oscuro
- notificaciones globales
- datos cacheados en frontend

---

## 3. Cuándo no es la mejor opción

No siempre necesitas Zustand.

En estos casos puede ser mejor otra cosa:

- **estado local simple**: usa `useState`
- **lógica de formulario aislada**: usa `useState` o `react-hook-form`
- **datos remotos con cache, reintentos y sincronización con servidor**: herramientas como TanStack Query suelen encajar mejor

Ejemplo:

Si solo un modal necesita saber si está abierto o cerrado, probablemente no hace falta un store global:

```jsx
function ModalLocal() {
  const [open, setOpen] = useState(false);

  return (
    <>
      <button onClick={() => setOpen(true)}>Abrir</button>
      {open && <div>Contenido del modal</div>}
    </>
  );
}
```

---

## 4. Instalación

### Con npm

```bash
npm install zustand
```

### Con yarn

```bash
yarn add zustand
```

### Con pnpm

```bash
pnpm add zustand
```

---

## 5. Idea clave del store

Un store en Zustand suele contener:

- **estado**: los datos
- **acciones**: funciones para modificar el estado
- opcionalmente **selectores**, **middleware** y lógica derivada

Ejemplo mental:

```jsx
{
  count: 0,
  increment: () => ...,
  decrement: () => ...,
  reset: () => ...
}
```

---

## 6. Primer ejemplo: contador global

### Store

```jsx
import { create } from 'zustand';

export const useCounterStore = create((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
  decrement: () => set((state) => ({ count: state.count - 1 })),
  reset: () => set({ count: 0 }),
}));
```

### Componente

```jsx
import { useCounterStore } from './stores/counterStore';

function Counter() {
  const count = useCounterStore((state) => state.count);
  const increment = useCounterStore((state) => state.increment);
  const decrement = useCounterStore((state) => state.decrement);
  const reset = useCounterStore((state) => state.reset);

  return (
    <div>
      <h2>Contador: {count}</h2>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
      <button onClick={reset}>Reset</button>
    </div>
  );
}
```

### Qué está pasando

- `create(...)` crea el store
- `set(...)` actualiza el estado
- `useCounterStore(...)` permite suscribirte al store desde React
- el selector `(state) => state.count` hace que el componente escuche solo `count`

---

## 7. `set` y `get`

En Zustand normalmente trabajas con dos funciones:

- `set`: actualiza el estado
- `get`: lee el estado actual dentro del store

Ejemplo:

```jsx
import { create } from 'zustand';

export const useAuthStore = create((set, get) => ({
  user: null,
  token: null,
  login: (user, token) => set({ user, token }),
  logout: () => set({ user: null, token: null }),
  isAuthenticated: () => Boolean(get().token),
}));
```

Uso:

```jsx
function Header() {
  const user = useAuthStore((state) => state.user);
  const logout = useAuthStore((state) => state.logout);

  return (
    <header>
      <span>{user ? `Hola ${user.name}` : 'Invitado'}</span>
      {user && <button onClick={logout}>Cerrar sesión</button>}
    </header>
  );
}
```

---

## 8. Actualizaciones parciales y reemplazo

Cuando haces:

```jsx
set({ theme: 'dark' });
```

Zustand actualiza solo esa parte del objeto de estado.

Si tienes:

```jsx
{
  theme: 'light',
  language: 'es'
}
```

y ejecutas:

```jsx
set({ theme: 'dark' });
```

el resultado será:

```jsx
{
  theme: 'dark',
  language: 'es'
}
```

Esto es útil porque normalmente no tienes que reconstruir todo el estado manualmente.

---

## 9. Zustand con objetos y arrays

### Actualizar objetos

```jsx
import { create } from 'zustand';

export const useProfileStore = create((set) => ({
  profile: {
    name: 'Ana',
    city: 'Madrid',
  },
  updateCity: (city) =>
    set((state) => ({
      profile: {
        ...state.profile,
        city,
      },
    })),
}));
```

### Actualizar arrays

```jsx
import { create } from 'zustand';

export const useTodoStore = create((set) => ({
  todos: [],
  addTodo: (text) =>
    set((state) => ({
      todos: [
        ...state.todos,
        { id: crypto.randomUUID(), text, done: false },
      ],
    })),
  toggleTodo: (id) =>
    set((state) => ({
      todos: state.todos.map((todo) =>
        todo.id === id ? { ...todo, done: !todo.done } : todo
      ),
    })),
}));
```

Importante: aunque Zustand simplifica mucho, debes seguir tratando arrays y objetos de forma inmutable.

---

## 10. Selectores

Un **selector** es una función que extrae solo la parte del store que un componente necesita.

Ejemplo:

```jsx
const count = useCounterStore((state) => state.count);
```

Eso es mejor que hacer:

```jsx
const state = useCounterStore();
```

porque así el componente no se suscribe a todo el store.

### Ejemplo práctico

```jsx
function CartTotal() {
  const total = useCartStore((state) =>
    state.items.reduce((acc, item) => acc + item.price * item.quantity, 0)
  );

  return <strong>Total: {total} €</strong>;
}
```

Aquí el componente solo se renderiza cuando cambie el resultado del selector.

---

## 11. Seleccionar varias propiedades

Puedes seleccionar varias propiedades a la vez.

```jsx
const { count, increment } = useCounterStore((state) => ({
  count: state.count,
  increment: state.increment,
}));
```

Esto funciona, pero si creas un objeto nuevo en cada render, debes vigilar comparaciones innecesarias.

Una alternativa muy usada es separar selectores:

```jsx
const count = useCounterStore((state) => state.count);
const increment = useCounterStore((state) => state.increment);
```

Suele ser claro y predecible.

---

## 12. Acciones asíncronas

Zustand no te obliga a usar middleware especial para async. Una acción puede ser `async`.

### Ejemplo: cargar usuarios desde una API

```jsx
import { create } from 'zustand';

export const useUserStore = create((set) => ({
  users: [],
  loading: false,
  error: null,
  fetchUsers: async () => {
    set({ loading: true, error: null });

    try {
      const response = await fetch('https://jsonplaceholder.typicode.com/users');

      if (!response.ok) {
        throw new Error('No se pudieron cargar los usuarios');
      }

      const data = await response.json();
      set({ users: data, loading: false });
    } catch (error) {
      set({ error: error.message, loading: false });
    }
  },
}));
```

### Uso en un componente

```jsx
import { useEffect } from 'react';
import { useUserStore } from './stores/userStore';

function UserList() {
  const users = useUserStore((state) => state.users);
  const loading = useUserStore((state) => state.loading);
  const error = useUserStore((state) => state.error);
  const fetchUsers = useUserStore((state) => state.fetchUsers);

  useEffect(() => {
    fetchUsers();
  }, [fetchUsers]);

  if (loading) return <p>Cargando...</p>;
  if (error) return <p>Error: {error}</p>;

  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

---

## 13. Estado derivado

A veces no quieres guardar un dato calculado en el store, sino derivarlo a partir del estado.

Ejemplo:

```jsx
function CartSummary() {
  const items = useCartStore((state) => state.items);

  const totalItems = items.reduce((acc, item) => acc + item.quantity, 0);
  const totalPrice = items.reduce(
    (acc, item) => acc + item.price * item.quantity,
    0
  );

  return (
    <div>
      <p>Productos: {totalItems}</p>
      <p>Total: {totalPrice} €</p>
    </div>
  );
}
```

Buena práctica:

- guarda en el store el **estado fuente**
- calcula en componentes o selectores lo que sea derivado

Así evitas inconsistencias.

---

## 14. Ejemplo realista: carrito de compra

### Store

```jsx
import { create } from 'zustand';

export const useCartStore = create((set, get) => ({
  items: [],
  addItem: (product) => {
    const existing = get().items.find((item) => item.id === product.id);

    if (existing) {
      set((state) => ({
        items: state.items.map((item) =>
          item.id === product.id
            ? { ...item, quantity: item.quantity + 1 }
            : item
        ),
      }));
      return;
    }

    set((state) => ({
      items: [...state.items, { ...product, quantity: 1 }],
    }));
  },
  removeItem: (id) =>
    set((state) => ({
      items: state.items.filter((item) => item.id !== id),
    })),
  clearCart: () => set({ items: [] }),
}));
```

### Componente para añadir

```jsx
function ProductCard({ product }) {
  const addItem = useCartStore((state) => state.addItem);

  return (
    <article>
      <h3>{product.name}</h3>
      <p>{product.price} €</p>
      <button onClick={() => addItem(product)}>Añadir al carrito</button>
    </article>
  );
}
```

### Componente resumen

```jsx
function CartPanel() {
  const items = useCartStore((state) => state.items);
  const removeItem = useCartStore((state) => state.removeItem);
  const clearCart = useCartStore((state) => state.clearCart);

  const total = items.reduce(
    (acc, item) => acc + item.price * item.quantity,
    0
  );

  return (
    <aside>
      <h2>Carrito</h2>

      {items.length === 0 ? (
        <p>No hay productos.</p>
      ) : (
        <>
          <ul>
            {items.map((item) => (
              <li key={item.id}>
                {item.name} x {item.quantity}
                <button onClick={() => removeItem(item.id)}>Eliminar</button>
              </li>
            ))}
          </ul>

          <p>Total: {total} €</p>
          <button onClick={clearCart}>Vaciar carrito</button>
        </>
      )}
    </aside>
  );
}
```

Este ejemplo muestra algo importante: el store concentra la lógica de negocio y los componentes solo consumen y disparan acciones.

---

## 15. Organización por slices

Cuando el store crece, conviene dividirlo en **slices**.

Una slice es una parte del estado con sus propias acciones.

### Ejemplo

```jsx
export const createAuthSlice = (set) => ({
  user: null,
  login: (user) => set({ user }),
  logout: () => set({ user: null }),
});

export const createThemeSlice = (set) => ({
  theme: 'light',
  toggleTheme: () =>
    set((state) => ({
      theme: state.theme === 'light' ? 'dark' : 'light',
    })),
});
```

### Store combinado

```jsx
import { create } from 'zustand';

export const useAppStore = create((...args) => ({
  ...createAuthSlice(...args),
  ...createThemeSlice(...args),
}));
```

Ventajas:

- mejor organización
- más fácil de mantener
- más sencillo de testear
- útil cuando la app crece bastante

---

## 16. Persistencia con `persist`

Uno de los middlewares más útiles es `persist`, que guarda estado en `localStorage` o en otro storage.

### Ejemplo: tema persistente

```jsx
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

export const useThemeStore = create(
  persist(
    (set) => ({
      theme: 'light',
      toggleTheme: () =>
        set((state) => ({
          theme: state.theme === 'light' ? 'dark' : 'light',
        })),
    }),
    {
      name: 'theme-storage',
    }
  )
);
```

### Uso

```jsx
function ThemeButton() {
  const theme = useThemeStore((state) => state.theme);
  const toggleTheme = useThemeStore((state) => state.toggleTheme);

  return (
    <button onClick={toggleTheme}>
      Tema actual: {theme}
    </button>
  );
}
```

Cuando el usuario recarga la página, el tema se conserva.

### Persistir solo una parte del estado

```jsx
persist(
  (set) => ({
    token: null,
    tempNotification: null,
    setToken: (token) => set({ token }),
    setTempNotification: (tempNotification) => set({ tempNotification }),
  }),
  {
    name: 'auth-storage',
    partialize: (state) => ({ token: state.token }),
  }
);
```

Aquí solo se persiste `token`.

---

## 17. Devtools

Puedes integrar Zustand con Redux DevTools usando `devtools`.

```jsx
import { create } from 'zustand';
import { devtools } from 'zustand/middleware';

export const useCounterStore = create(
  devtools((set) => ({
    count: 0,
    increment: () => set((state) => ({ count: state.count + 1 }), false, 'counter/increment'),
    decrement: () => set((state) => ({ count: state.count - 1 }), false, 'counter/decrement'),
  }))
);
```

Esto ayuda a:

- ver cambios de estado
- inspeccionar acciones
- depurar flujos complejos

Muy útil en aplicaciones medianas o grandes.

---

## 18. Immer con Zustand

Si no quieres hacer spreads manuales todo el tiempo, puedes usar `immer`.

```bash
npm install immer
```

Ejemplo:

```jsx
import { create } from 'zustand';
import { immer } from 'zustand/middleware/immer';

export const useTodoStore = create(
  immer((set) => ({
    todos: [],
    addTodo: (text) =>
      set((state) => {
        state.todos.push({
          id: crypto.randomUUID(),
          text,
          done: false,
        });
      }),
    toggleTodo: (id) =>
      set((state) => {
        const todo = state.todos.find((item) => item.id === id);
        if (todo) {
          todo.done = !todo.done;
        }
      }),
  }))
);
```

Esto puede hacer el código más cómodo cuando el estado es profundo.

---

## 19. `subscribe` y acceso fuera de React

Zustand puede usarse también fuera de componentes React.

### Leer estado fuera de un componente

```jsx
const currentUser = useAuthStore.getState().user;
```

### Ejecutar acciones fuera de un componente

```jsx
useAuthStore.getState().logout();
```

### Suscribirse manualmente

```jsx
const unsubscribe = useAuthStore.subscribe((state) => {
  console.log('Nuevo usuario:', state.user);
});

unsubscribe();
```

Esto es útil para:

- listeners externos
- integraciones con WebSocket
- analytics
- sincronización con servicios externos

---

## 20. Zustand vanilla

Si quieres un store sin depender de React, puedes usar la API vanilla.

```jsx
import { createStore } from 'zustand/vanilla';

const counterStore = createStore((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
}));

counterStore.getState().increment();
console.log(counterStore.getState().count);
```

Esto puede servir en:

- utilidades compartidas
- código no React
- integraciones entre distintos entornos

---

## 21. Zustand con TypeScript

Zustand se lleva muy bien con TypeScript.

### Ejemplo tipado

```tsx
import { create } from 'zustand';

type User = {
  id: string;
  name: string;
};

type AuthStore = {
  user: User | null;
  token: string | null;
  login: (user: User, token: string) => void;
  logout: () => void;
};

export const useAuthStore = create<AuthStore>((set) => ({
  user: null,
  token: null,
  login: (user, token) => set({ user, token }),
  logout: () => set({ user: null, token: null }),
}));
```

Ventajas:

- autocompletado
- menos errores
- contratos más claros

---

## 22. Zustand con Next.js y SSR

Con renderizado en servidor debes tener cuidado con stores compartidos entre peticiones.

Idea general:

- evita crear estado global mutable que se comparta entre usuarios en el servidor
- si necesitas hidratar datos del servidor, inicializa el store correctamente por request o en cliente según el caso
- si usas `persist`, recuerda que `localStorage` solo existe en el navegador

Ejemplo de protección simple:

```tsx
const isBrowser = typeof window !== 'undefined';
```

Si una parte del store depende de `localStorage`, no asumas que existe durante SSR.

---

## 23. Testing de stores

Como la lógica está separada de la UI, probar stores suele ser bastante directo.

### Ejemplo

```jsx
import { useCounterStore } from './counterStore';

describe('counterStore', () => {
  beforeEach(() => {
    useCounterStore.setState({ count: 0 });
  });

  test('incrementa el contador', () => {
    useCounterStore.getState().increment();
    expect(useCounterStore.getState().count).toBe(1);
  });

  test('resetea el contador', () => {
    useCounterStore.setState({ count: 5 });
    useCounterStore.getState().reset();
    expect(useCounterStore.getState().count).toBe(0);
  });
});
```

Esto ayuda a validar lógica de negocio sin montar componentes completos.

---

## 24. Buenas prácticas

### 1. Usa selectores específicos

Mejor:

```jsx
const theme = useThemeStore((state) => state.theme);
```

Peor:

```jsx
const state = useThemeStore();
```

### 2. No metas absolutamente todo en Zustand

No conviertas el store global en un cajón desastre.

Ejemplo:

- estado local de un input → `useState`
- estado global del usuario autenticado → Zustand

### 3. Mantén acciones con nombres claros

Mejor:

- `addItem`
- `removeItem`
- `toggleTheme`
- `setUser`

Peor:

- `change`
- `updateStuff`
- `setData`

### 4. Separa lógica de negocio y presentación

Cuanta más lógica pongas dentro del store y menos lógica dispersa tengas en componentes, más fácil será mantener la aplicación.

### 5. Usa slices si el store crece

No esperes a tener un archivo inmanejable.

---

## 25. Errores comunes

### Error 1: guardar estado derivado innecesario

Malo:

```jsx
{
  items: [],
  total: 0
}
```

si `total` siempre puede calcularse desde `items`.

Mejor:

- guarda `items`
- calcula `total` cuando lo necesites

### Error 2: mutar estado directamente

Malo:

```jsx
set((state) => {
  state.user.name = 'Pedro';
  return state;
});
```

Eso rompe el enfoque inmutable, salvo que estés usando middleware como `immer` correctamente.

### Error 3: suscribirte a todo el store

Si un componente usa todo el store, se vuelve más fácil provocar renders innecesarios.

### Error 4: mezclar demasiadas responsabilidades

No metas autenticación, UI, filtros, websocket y carrito en un archivo gigantesco si puedes separarlo.

---

## 26. Zustand vs Context API vs Redux

| Herramienta | Ventajas | Desventajas |
|------------|----------|-------------|
| **Context API** | Nativa de React, útil para casos simples | Puede provocar renders amplios si se usa mal |
| **Zustand** | Muy simple, poco boilerplate, flexible, selectores claros | Menos estructurado que Redux si el equipo no define convenciones |
| **Redux Toolkit** | Muy robusto, gran ecosistema, herramientas maduras | Más ceremonioso para casos pequeños |

### Regla rápida

- **Context API**: estado compartido simple
- **Zustand**: equilibrio entre simplicidad y escalabilidad
- **Redux Toolkit**: flujos complejos o equipos que necesitan mucha estructura

---

## 27. Ejemplo completo: store de autenticación

```jsx
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

export const useAuthStore = create(
  persist(
    (set) => ({
      user: null,
      token: null,
      loading: false,
      error: null,
      login: async (email, password) => {
        set({ loading: true, error: null });

        try {
          const response = await fetch('/api/login', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ email, password }),
          });

          if (!response.ok) {
            throw new Error('Credenciales inválidas');
          }

          const data = await response.json();

          set({
            user: data.user,
            token: data.token,
            loading: false,
          });
        } catch (error) {
          set({
            error: error.message,
            loading: false,
          });
        }
      },
      logout: () =>
        set({
          user: null,
          token: null,
          error: null,
        }),
    }),
    {
      name: 'auth-storage',
      partialize: (state) => ({
        user: state.user,
        token: state.token,
      }),
    }
  )
);
```

### Componente login

```jsx
import { useState } from 'react';
import { useAuthStore } from './stores/authStore';

function LoginForm() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  const login = useAuthStore((state) => state.login);
  const loading = useAuthStore((state) => state.loading);
  const error = useAuthStore((state) => state.error);

  const handleSubmit = async (event) => {
    event.preventDefault();
    await login(email, password);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="email"
        value={email}
        onChange={(event) => setEmail(event.target.value)}
        placeholder="Email"
      />
      <input
        type="password"
        value={password}
        onChange={(event) => setPassword(event.target.value)}
        placeholder="Contraseña"
      />
      <button type="submit" disabled={loading}>
        {loading ? 'Entrando...' : 'Entrar'}
      </button>
      {error && <p>{error}</p>}
    </form>
  );
}
```

Este patrón es muy común porque centraliza autenticación, persistencia y estado de carga en un solo sitio.

---

## 28. Estructura de carpetas recomendada

Ejemplo:

```plaintext
src/
├── components/
├── pages/
├── stores/
│   ├── authStore.js
│   ├── cartStore.js
│   ├── themeStore.js
│   └── index.js
├── hooks/
├── services/
└── utils/
```

Si el proyecto crece:

```plaintext
src/
├── stores/
│   ├── auth/
│   │   ├── authStore.ts
│   │   └── authSlice.ts
│   ├── cart/
│   │   ├── cartStore.ts
│   │   └── cartSelectors.ts
│   └── ui/
│       └── uiStore.ts
```

---

## 29. Resumen práctico

Zustand destaca por:

- ser fácil de aprender
- requerir poco código
- separar bien estado y componentes
- permitir acciones async sin complejidad extra
- ofrecer middleware útil como `persist`, `devtools` e `immer`

Si vienes de Context API, sentirás que ganas orden.

Si vienes de Redux, sentirás que eliminas mucho boilerplate.

---

## 30. Conclusión

Zustand es una muy buena opción para manejar estado global en React cuando quieres:

- simplicidad
- flexibilidad
- buen rendimiento
- una curva de aprendizaje baja

No intenta imponerte una arquitectura rígida. Eso es una ventaja, pero también implica que debes mantener buenas prácticas de organización.

La mejor forma de aprenderlo es empezar con un store pequeño:

- contador
- tema
- auth
- carrito

y luego evolucionar a slices, persistencia, devtools y testing.
