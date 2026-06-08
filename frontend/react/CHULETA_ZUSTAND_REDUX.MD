# Chuleta rápida: Zustand y Redux

## Cuándo usar cada uno

- **Zustand**: cuando quieres algo rápido, simple y con poco boilerplate.
- **Redux Toolkit**: cuando quieres más estructura, reglas claras y escalar mejor.

---

## 1. Zustand con varios slices

### Instalar

```bash
npm install zustand
```

### Store

```jsx
import { create } from 'zustand';

const createAuthSlice = (set) => ({
  user: null,
  login: (user) => set({ user }),
  logout: () => set({ user: null }),
});

const createCartSlice = (set, get) => ({
  items: [],
  addItem: (product) =>
    set({ items: [...get().items, product] }),
  clearCart: () => set({ items: [] }),
});

export const useStore = create((...args) => ({
  ...createAuthSlice(...args),
  ...createCartSlice(...args),
}));
```

### Uso en un componente

```jsx
function App() {
  const user = useStore((state) => state.user);
  const items = useStore((state) => state.items);
  const login = useStore((state) => state.login);
  const addItem = useStore((state) => state.addItem);

  return (
    <div>
      <button onClick={() => login({ name: 'Fran' })}>Login</button>
      <button onClick={() => addItem({ id: 1, name: 'Teclado' })}>
        Añadir producto
      </button>

      <p>Usuario: {user ? user.name : 'No logueado'}</p>
      <p>Carrito: {items.length} productos</p>
    </div>
  );
}
```

### Idea rápida

- `create(...)` crea el store.
- cada slice guarda una parte del estado.
- el componente lee solo lo que necesita.

---

## 2. Redux Toolkit con varios slices

### Instalar

```bash
npm install @reduxjs/toolkit react-redux
```

### `authSlice.js`

```jsx
import { createSlice } from '@reduxjs/toolkit';

const authSlice = createSlice({
  name: 'auth',
  initialState: { user: null },
  reducers: {
    login: (state, action) => {
      state.user = action.payload;
    },
    logout: (state) => {
      state.user = null;
    },
  },
});

export const { login, logout } = authSlice.actions;
export default authSlice.reducer;
```

### `cartSlice.js`

```jsx
import { createSlice } from '@reduxjs/toolkit';

const cartSlice = createSlice({
  name: 'cart',
  initialState: { items: [] },
  reducers: {
    addItem: (state, action) => {
      state.items.push(action.payload);
    },
    clearCart: (state) => {
      state.items = [];
    },
  },
});

export const { addItem, clearCart } = cartSlice.actions;
export default cartSlice.reducer;
```

### `store.js`

```jsx
import { configureStore } from '@reduxjs/toolkit';
import authReducer from './authSlice';
import cartReducer from './cartSlice';

export const store = configureStore({
  reducer: {
    auth: authReducer,
    cart: cartReducer,
  },
});
```

### `main.js`

```jsx
import ReactDOM from 'react-dom/client';
import { Provider } from 'react-redux';
import { store } from './store';
import App from './App';

ReactDOM.createRoot(document.getElementById('root')).render(
  <Provider store={store}>
    <App />
  </Provider>
);
```

### Uso en un componente

```jsx
import { useDispatch, useSelector } from 'react-redux';
import { login } from './authSlice';
import { addItem } from './cartSlice';

function App() {
  const dispatch = useDispatch();
  const user = useSelector((state) => state.auth.user);
  const items = useSelector((state) => state.cart.items);

  return (
    <div>
      <button onClick={() => dispatch(login({ name: 'Fran' }))}>
        Login
      </button>
      <button onClick={() => dispatch(addItem({ id: 1, name: 'Teclado' }))}>
        Añadir producto
      </button>

      <p>Usuario: {user ? user.name : 'No logueado'}</p>
      <p>Carrito: {items.length} productos</p>
    </div>
  );
}
```

### Idea rápida

- cada archivo slice tiene su estado y acciones.
- `configureStore` junta todos los slices.
- `dispatch(...)` lanza acciones y `useSelector(...)` lee estado.

---

## 3. Resumen corto

| Opción | Lo mejor |
|---|---|
| **Zustand** | Más simple y rápido de montar |
| **Redux Toolkit** | Más ordenado para proyectos grandes |

### Regla rápida

- proyecto pequeño o mediano: **Zustand**
- proyecto grande o en equipo: **Redux Toolkit**
