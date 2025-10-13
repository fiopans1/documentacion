# ⚛️ Guía de Window y Eventos en React

## 📋 Índice

1. [Introducción](#1-introducción)
2. [El Objeto Window](#2-el-objeto-window)
3. [addEventListener en React](#3-addeventlistener-en-react)
4. [Eventos del DOM](#4-eventos-del-dom)
5. [Eventos de Window](#5-eventos-de-window)
6. [Custom Events](#6-custom-events)
7. [Event Bubbling y Capturing](#7-event-bubbling-y-capturing)
8. [Eventos con useEffect](#8-eventos-con-useeffect)
9. [Manejo de Teclado](#9-manejo-de-teclado)
10. [Manejo de Mouse](#10-manejo-de-mouse)
11. [Scroll Events](#11-scroll-events)
12. [Resize Events](#12-resize-events)
13. [Storage Events](#13-storage-events)
14. [Debounce y Throttle](#14-debounce-y-throttle)
15. [Mejores Prácticas](#15-mejores-prácticas)

---

## 1. Introducción

En React, el manejo de eventos del navegador y el objeto `window` requiere consideraciones especiales, especialmente con hooks y el ciclo de vida de los componentes.

### Conceptos clave:
- **window**: Objeto global del navegador
- **addEventListener**: Método para escuchar eventos
- **removeEventListener**: Limpieza de event listeners
- **useEffect**: Hook para efectos secundarios
- **Event object**: Objeto que contiene información del evento

---

## 2. El Objeto Window

### Acceder a window en React

```jsx
import { useEffect, useState } from 'react';

function WindowInfo() {
    const [windowSize, setWindowSize] = useState({
        width: window.innerWidth,
        height: window.innerHeight
    });
    
    // Propiedades útiles de window
    const info = {
        // Dimensiones
        innerWidth: window.innerWidth,
        innerHeight: window.innerHeight,
        outerWidth: window.outerWidth,
        outerHeight: window.outerHeight,
        
        // Scroll
        scrollX: window.scrollX,
        scrollY: window.scrollY,
        
        // Pantalla
        screenX: window.screenX,
        screenY: window.screenY,
        
        // Información del navegador
        navigator: window.navigator.userAgent,
        
        // Location
        href: window.location.href,
        pathname: window.location.pathname,
        search: window.location.search,
        hash: window.location.hash
    };
    
    return (
        <div>
            <h2>Window Info</h2>
            <pre>{JSON.stringify(info, null, 2)}</pre>
        </div>
    );
}
```

### Window methods en React

```jsx
function WindowMethods() {
    const handleAlert = () => {
        window.alert('Este es un alert');
    };
    
    const handleConfirm = () => {
        const result = window.confirm('¿Estás seguro?');
        console.log('Confirmado:', result);
    };
    
    const handlePrompt = () => {
        const name = window.prompt('¿Cuál es tu nombre?');
        console.log('Nombre:', name);
    };
    
    const handleOpen = () => {
        window.open('https://google.com', '_blank');
    };
    
    const handleScrollTo = () => {
        window.scrollTo({
            top: 0,
            behavior: 'smooth'
        });
    };
    
    const handlePrint = () => {
        window.print();
    };
    
    return (
        <div>
            <button onClick={handleAlert}>Alert</button>
            <button onClick={handleConfirm}>Confirm</button>
            <button onClick={handlePrompt}>Prompt</button>
            <button onClick={handleOpen}>Open New Window</button>
            <button onClick={handleScrollTo}>Scroll To Top</button>
            <button onClick={handlePrint}>Print</button>
        </div>
    );
}
```

---

## 3. addEventListener en React

### Patrón básico con useEffect

```jsx
import { useEffect } from 'react';

function EventListenerExample() {
    useEffect(() => {
        // Definir el handler
        const handleEvent = (event) => {
            console.log('Evento capturado:', event);
        };
        
        // Añadir listener
        window.addEventListener('eventname', handleEvent);
        
        // Cleanup: remover listener
        return () => {
            window.removeEventListener('eventname', handleEvent);
        };
    }, []); // [] = solo se ejecuta una vez al montar
    
    return <div>Component with event listener</div>;
}
```

### Custom Hook para addEventListener

```jsx
import { useEffect } from 'react';

function useEventListener(eventName, handler, element = window) {
    useEffect(() => {
        // Asegurar que el elemento soporte addEventListener
        const isSupported = element && element.addEventListener;
        if (!isSupported) return;
        
        // Añadir event listener
        element.addEventListener(eventName, handler);
        
        // Cleanup
        return () => {
            element.removeEventListener(eventName, handler);
        };
    }, [eventName, handler, element]);
}

// Uso del hook
function App() {
    const handleClick = (e) => {
        console.log('Window clicked:', e.clientX, e.clientY);
    };
    
    useEventListener('click', handleClick);
    
    return <div>App con event listener global</div>;
}
```

### Hook avanzado con opciones

```jsx
function useEventListener(
    eventName,
    handler,
    element = window,
    options = {}
) {
    useEffect(() => {
        const isSupported = element && element.addEventListener;
        if (!isSupported) return;
        
        // Crear evento estable con useCallback si es necesario
        element.addEventListener(eventName, handler, options);
        
        return () => {
            element.removeEventListener(eventName, handler, options);
        };
    }, [eventName, element, options]);
}

// Uso con capture phase
function App() {
    useEventListener('click', handleClick, window, { capture: true });
}
```

---

## 4. Eventos del DOM

### Eventos sintéticos de React

React usa eventos sintéticos que normalizan el comportamiento entre navegadores.

```jsx
function DOMEvents() {
    // onClick
    const handleClick = (e) => {
        console.log('Click:', e.target);
        e.preventDefault(); // Prevenir comportamiento por defecto
        e.stopPropagation(); // Detener propagación
    };
    
    // onChange
    const handleChange = (e) => {
        console.log('Value:', e.target.value);
    };
    
    // onSubmit
    const handleSubmit = (e) => {
        e.preventDefault();
        const formData = new FormData(e.target);
        console.log(Object.fromEntries(formData));
    };
    
    // onFocus / onBlur
    const handleFocus = () => console.log('Input focused');
    const handleBlur = () => console.log('Input blurred');
    
    return (
        <div>
            <button onClick={handleClick}>Click me</button>
            
            <input 
                type="text"
                onChange={handleChange}
                onFocus={handleFocus}
                onBlur={handleBlur}
            />
            
            <form onSubmit={handleSubmit}>
                <input name="username" />
                <button type="submit">Submit</button>
            </form>
        </div>
    );
}
```

### Event object properties

```jsx
function EventProperties() {
    const handleClick = (e) => {
        console.log({
            // Target
            target: e.target,
            currentTarget: e.currentTarget,
            
            // Mouse position
            clientX: e.clientX,
            clientY: e.clientY,
            pageX: e.pageX,
            pageY: e.pageY,
            screenX: e.screenX,
            screenY: e.screenY,
            
            // Keys pressed
            altKey: e.altKey,
            ctrlKey: e.ctrlKey,
            shiftKey: e.shiftKey,
            metaKey: e.metaKey,
            
            // Type
            type: e.type,
            
            // Timestamp
            timeStamp: e.timeStamp
        });
    };
    
    return <button onClick={handleClick}>Click para ver properties</button>;
}
```

---

## 5. Eventos de Window

### Resize Event

```jsx
import { useState, useEffect } from 'react';

function WindowResize() {
    const [size, setSize] = useState({
        width: window.innerWidth,
        height: window.innerHeight
    });
    
    useEffect(() => {
        const handleResize = () => {
            setSize({
                width: window.innerWidth,
                height: window.innerHeight
            });
        };
        
        window.addEventListener('resize', handleResize);
        
        return () => window.removeEventListener('resize', handleResize);
    }, []);
    
    return (
        <div>
            <h2>Window Size</h2>
            <p>Width: {size.width}px</p>
            <p>Height: {size.height}px</p>
        </div>
    );
}
```

### Scroll Event

```jsx
function ScrollTracker() {
    const [scrollPos, setScrollPos] = useState({
        x: 0,
        y: 0
    });
    
    useEffect(() => {
        const handleScroll = () => {
            setScrollPos({
                x: window.scrollX,
                y: window.scrollY
            });
        };
        
        window.addEventListener('scroll', handleScroll);
        
        return () => window.removeEventListener('scroll', handleScroll);
    }, []);
    
    return (
        <div style={{ position: 'fixed', top: 10, right: 10 }}>
            Scroll: {scrollPos.y}px
        </div>
    );
}
```

### Online/Offline Events

```jsx
function OnlineStatus() {
    const [isOnline, setIsOnline] = useState(navigator.onLine);
    
    useEffect(() => {
        const handleOnline = () => setIsOnline(true);
        const handleOffline = () => setIsOnline(false);
        
        window.addEventListener('online', handleOnline);
        window.addEventListener('offline', handleOffline);
        
        return () => {
            window.removeEventListener('online', handleOnline);
            window.removeEventListener('offline', handleOffline);
        };
    }, []);
    
    return (
        <div>
            Status: {isOnline ? '🟢 Online' : '🔴 Offline'}
        </div>
    );
}
```

### Visibility Change

```jsx
function VisibilityTracker() {
    const [isVisible, setIsVisible] = useState(!document.hidden);
    
    useEffect(() => {
        const handleVisibilityChange = () => {
            setIsVisible(!document.hidden);
        };
        
        document.addEventListener('visibilitychange', handleVisibilityChange);
        
        return () => {
            document.removeEventListener('visibilitychange', handleVisibilityChange);
        };
    }, []);
    
    return (
        <div>
            Page is: {isVisible ? 'Visible' : 'Hidden'}
        </div>
    );
}
```

### BeforeUnload Event

```jsx
function UnsavedChangesWarning() {
    const [hasUnsavedChanges, setHasUnsavedChanges] = useState(false);
    
    useEffect(() => {
        const handleBeforeUnload = (e) => {
            if (hasUnsavedChanges) {
                e.preventDefault();
                e.returnValue = ''; // Chrome requiere esto
            }
        };
        
        window.addEventListener('beforeunload', handleBeforeUnload);
        
        return () => {
            window.removeEventListener('beforeunload', handleBeforeUnload);
        };
    }, [hasUnsavedChanges]);
    
    return (
        <div>
            <input 
                onChange={() => setHasUnsavedChanges(true)}
                placeholder="Edita para activar warning"
            />
        </div>
    );
}
```

---

## 6. Custom Events

### Crear y disparar custom events

```jsx
import { useEffect } from 'react';

// Componente que dispara el evento
function EventEmitter() {
    const emitCustomEvent = () => {
        const event = new CustomEvent('myCustomEvent', {
            detail: { message: 'Hola desde custom event!', timestamp: Date.now() }
        });
        window.dispatchEvent(event);
    };
    
    return <button onClick={emitCustomEvent}>Emit Event</button>;
}

// Componente que escucha el evento
function EventListener() {
    const [lastEvent, setLastEvent] = useState(null);
    
    useEffect(() => {
        const handleCustomEvent = (e) => {
            console.log('Custom event received:', e.detail);
            setLastEvent(e.detail);
        };
        
        window.addEventListener('myCustomEvent', handleCustomEvent);
        
        return () => {
            window.removeEventListener('myCustomEvent', handleCustomEvent);
        };
    }, []);
    
    return (
        <div>
            {lastEvent && (
                <p>Last event: {lastEvent.message} at {new Date(lastEvent.timestamp).toLocaleTimeString()}</p>
            )}
        </div>
    );
}
```

### Event Bus pattern

```jsx
// eventBus.js
class EventBus {
    constructor() {
        this.events = {};
    }
    
    on(event, callback) {
        if (!this.events[event]) {
            this.events[event] = [];
        }
        this.events[event].push(callback);
    }
    
    off(event, callback) {
        if (this.events[event]) {
            this.events[event] = this.events[event].filter(cb => cb !== callback);
        }
    }
    
    emit(event, data) {
        if (this.events[event]) {
            this.events[event].forEach(callback => callback(data));
        }
    }
}

export const eventBus = new EventBus();

// Uso en componentes
import { eventBus } from './eventBus';

function ComponentA() {
    const sendMessage = () => {
        eventBus.emit('message', { text: 'Hello from A' });
    };
    
    return <button onClick={sendMessage}>Send Message</button>;
}

function ComponentB() {
    const [messages, setMessages] = useState([]);
    
    useEffect(() => {
        const handleMessage = (data) => {
            setMessages(prev => [...prev, data.text]);
        };
        
        eventBus.on('message', handleMessage);
        
        return () => eventBus.off('message', handleMessage);
    }, []);
    
    return (
        <ul>
            {messages.map((msg, i) => <li key={i}>{msg}</li>)}
        </ul>
    );
}
```

---

## 7. Event Bubbling y Capturing

### Bubbling (por defecto)

```jsx
function BubblingExample() {
    const handleGrandparentClick = () => {
        console.log('Grandparent clicked');
    };
    
    const handleParentClick = () => {
        console.log('Parent clicked');
    };
    
    const handleChildClick = (e) => {
        console.log('Child clicked');
        // e.stopPropagation(); // Detener bubbling
    };
    
    return (
        <div onClick={handleGrandparentClick} style={{ padding: '20px', background: 'red' }}>
            Grandparent
            <div onClick={handleParentClick} style={{ padding: '20px', background: 'green' }}>
                Parent
                <div onClick={handleChildClick} style={{ padding: '20px', background: 'blue' }}>
                    Child (Click aquí)
                </div>
            </div>
        </div>
    );
    // Al hacer click en Child, se ejecutan: Child -> Parent -> Grandparent
}
```

### Capturing phase

```jsx
function CapturingExample() {
    useEffect(() => {
        const handleCapture = (e) => {
            console.log('Captured at:', e.currentTarget);
        };
        
        // Usar capture: true para fase de captura
        document.getElementById('outer').addEventListener('click', handleCapture, true);
        document.getElementById('inner').addEventListener('click', handleCapture, true);
        
        return () => {
            document.getElementById('outer')?.removeEventListener('click', handleCapture, true);
            document.getElementById('inner')?.removeEventListener('click', handleCapture, true);
        };
    }, []);
    
    return (
        <div id="outer">
            Outer
            <div id="inner">Inner (Click aquí)</div>
        </div>
    );
}
```

### Event delegation

```jsx
function TodoList() {
    const [todos, setTodos] = useState([
        { id: 1, text: 'Task 1' },
        { id: 2, text: 'Task 2' },
        { id: 3, text: 'Task 3' }
    ]);
    
    // Un solo handler para todos los botones
    const handleListClick = (e) => {
        if (e.target.matches('.delete-btn')) {
            const id = Number(e.target.dataset.id);
            setTodos(todos.filter(todo => todo.id !== id));
        }
    };
    
    return (
        <ul onClick={handleListClick}>
            {todos.map(todo => (
                <li key={todo.id}>
                    {todo.text}
                    <button className="delete-btn" data-id={todo.id}>
                        Delete
                    </button>
                </li>
            ))}
        </ul>
    );
}
```

---

## 8. Eventos con useEffect

### Cleanup apropiado

```jsx
function ProperCleanup() {
    const [count, setCount] = useState(0);
    
    useEffect(() => {
        const handleClick = () => {
            // Usar functional update para acceder al estado más reciente
            setCount(c => c + 1);
        };
        
        window.addEventListener('click', handleClick);
        
        // Cleanup
        return () => {
            window.removeEventListener('click', handleClick);
        };
    }, []); // Dependencias vacías, el handler se crea una sola vez
    
    return <div>Clicks: {count}</div>;
}
```

### Múltiples event listeners

```jsx
function MultipleListeners() {
    const [events, setEvents] = useState([]);
    
    useEffect(() => {
        const logEvent = (eventName) => (e) => {
            setEvents(prev => [...prev, {
                type: eventName,
                timestamp: Date.now()
            }]);
        };
        
        const events = [
            'click',
            'keydown',
            'scroll',
            'resize'
        ];
        
        events.forEach(eventName => {
            window.addEventListener(eventName, logEvent(eventName));
        });
        
        return () => {
            events.forEach(eventName => {
                window.removeEventListener(eventName, logEvent(eventName));
            });
        };
    }, []);
    
    return (
        <ul>
            {events.slice(-10).map((event, i) => (
                <li key={i}>{event.type} at {new Date(event.timestamp).toLocaleTimeString()}</li>
            ))}
        </ul>
    );
}
```

---

## 9. Manejo de Teclado

### Keyboard events

```jsx
function KeyboardHandler() {
    const [key, setKey] = useState('');
    const [keyCode, setKeyCode] = useState('');
    
    useEffect(() => {
        const handleKeyDown = (e) => {
            setKey(e.key);
            setKeyCode(e.code);
            
            // Detectar combinaciones
            if (e.ctrlKey && e.key === 's') {
                e.preventDefault();
                console.log('Ctrl+S pressed');
            }
            
            // Detectar teclas específicas
            switch(e.key) {
                case 'Escape':
                    console.log('Escape pressed');
                    break;
                case 'Enter':
                    console.log('Enter pressed');
                    break;
                case 'ArrowUp':
                    console.log('Arrow Up pressed');
                    break;
            }
        };
        
        window.addEventListener('keydown', handleKeyDown);
        
        return () => window.removeEventListener('keydown', handleKeyDown);
    }, []);
    
    return (
        <div>
            <p>Presiona cualquier tecla</p>
            <p>Key: {key}</p>
            <p>Code: {keyCode}</p>
        </div>
    );
}
```

### Keyboard shortcuts

```jsx
function useKeyboardShortcut(key, callback, options = {}) {
    const { ctrl = false, alt = false, shift = false } = options;
    
    useEffect(() => {
        const handleKeyDown = (e) => {
            if (
                e.key === key &&
                e.ctrlKey === ctrl &&
                e.altKey === alt &&
                e.shiftKey === shift
            ) {
                e.preventDefault();
                callback(e);
            }
        };
        
        window.addEventListener('keydown', handleKeyDown);
        
        return () => window.removeEventListener('keydown', handleKeyDown);
    }, [key, callback, ctrl, alt, shift]);
}

// Uso
function App() {
    useKeyboardShortcut('s', () => console.log('Save'), { ctrl: true });
    useKeyboardShortcut('n', () => console.log('New'), { ctrl: true });
    useKeyboardShortcut('z', () => console.log('Undo'), { ctrl: true });
    
    return <div>Press Ctrl+S, Ctrl+N, or Ctrl+Z</div>;
}
```

---

## 10. Manejo de Mouse

### Mouse events

```jsx
function MouseTracker() {
    const [position, setPosition] = useState({ x: 0, y: 0 });
    const [isDown, setIsDown] = useState(false);
    
    useEffect(() => {
        const handleMouseMove = (e) => {
            setPosition({ x: e.clientX, y: e.clientY });
        };
        
        const handleMouseDown = () => setIsDown(true);
        const handleMouseUp = () => setIsDown(false);
        
        window.addEventListener('mousemove', handleMouseMove);
        window.addEventListener('mousedown', handleMouseDown);
        window.addEventListener('mouseup', handleMouseUp);
        
        return () => {
            window.removeEventListener('mousemove', handleMouseMove);
            window.removeEventListener('mousedown', handleMouseDown);
            window.removeEventListener('mouseup', handleMouseUp);
        };
    }, []);
    
    return (
        <div>
            <p>Mouse: {position.x}, {position.y}</p>
            <p>Button: {isDown ? 'Down' : 'Up'}</p>
        </div>
    );
}
```

### Click outside detection

```jsx
function useClickOutside(ref, callback) {
    useEffect(() => {
        const handleClick = (e) => {
            if (ref.current && !ref.current.contains(e.target)) {
                callback();
            }
        };
        
        document.addEventListener('mousedown', handleClick);
        
        return () => document.removeEventListener('mousedown', handleClick);
    }, [ref, callback]);
}

// Uso
function Modal() {
    const [isOpen, setIsOpen] = useState(false);
    const modalRef = useRef(null);
    
    useClickOutside(modalRef, () => setIsOpen(false));
    
    return (
        <>
            <button onClick={() => setIsOpen(true)}>Open Modal</button>
            
            {isOpen && (
                <div className="modal-overlay">
                    <div ref={modalRef} className="modal">
                        <h2>Modal Content</h2>
                        <button onClick={() => setIsOpen(false)}>Close</button>
                    </div>
                </div>
            )}
        </>
    );
}
```

---

## 11. Scroll Events

### Scroll position tracking

```jsx
function useScrollPosition() {
    const [scrollPosition, setScrollPosition] = useState(0);
    
    useEffect(() => {
        const handleScroll = () => {
            const position = window.pageYOffset;
            setScrollPosition(position);
        };
        
        window.addEventListener('scroll', handleScroll, { passive: true });
        
        return () => window.removeEventListener('scroll', handleScroll);
    }, []);
    
    return scrollPosition;
}

// Uso: Mostrar botón "scroll to top"
function ScrollToTopButton() {
    const scrollPosition = useScrollPosition();
    const isVisible = scrollPosition > 300;
    
    const scrollToTop = () => {
        window.scrollTo({ top: 0, behavior: 'smooth' });
    };
    
    return isVisible ? (
        <button 
            onClick={scrollToTop}
            style={{ position: 'fixed', bottom: 20, right: 20 }}
        >
            ↑ Top
        </button>
    ) : null;
}
```

### Infinite scroll

```jsx
function InfiniteScroll() {
    const [items, setItems] = useState(Array.from({ length: 20 }, (_, i) => i));
    const [loading, setLoading] = useState(false);
    
    useEffect(() => {
        const handleScroll = () => {
            const scrollHeight = document.documentElement.scrollHeight;
            const scrollTop = document.documentElement.scrollTop;
            const clientHeight = document.documentElement.clientHeight;
            
            if (scrollTop + clientHeight >= scrollHeight - 100 && !loading) {
                setLoading(true);
                // Simular carga
                setTimeout(() => {
                    setItems(prev => [
                        ...prev,
                        ...Array.from({ length: 20 }, (_, i) => prev.length + i)
                    ]);
                    setLoading(false);
                }, 1000);
            }
        };
        
        window.addEventListener('scroll', handleScroll);
        return () => window.removeEventListener('scroll', handleScroll);
    }, [loading]);
    
    return (
        <div>
            {items.map(item => (
                <div key={item} style={{ height: 100, border: '1px solid #ddd' }}>
                    Item {item}
                </div>
            ))}
            {loading && <p>Loading...</p>}
        </div>
    );
}
```

---

## 12. Resize Events

### Window resize hook

```jsx
function useWindowSize() {
    const [size, setSize] = useState({
        width: window.innerWidth,
        height: window.innerHeight
    });
    
    useEffect(() => {
        const handleResize = () => {
            setSize({
                width: window.innerWidth,
                height: window.innerHeight
            });
        };
        
        window.addEventListener('resize', handleResize);
        
        return () => window.removeEventListener('resize', handleResize);
    }, []);
    
    return size;
}

// Uso: Responsive component
function ResponsiveComponent() {
    const { width } = useWindowSize();
    
    const isMobile = width < 768;
    const isTablet = width >= 768 && width < 1024;
    const isDesktop = width >= 1024;
    
    return (
        <div>
            {isMobile && <MobileView />}
            {isTablet && <TabletView />}
            {isDesktop && <DesktopView />}
        </div>
    );
}
```

### Media query hook

```jsx
function useMediaQuery(query) {
    const [matches, setMatches] = useState(
        () => window.matchMedia(query).matches
    );
    
    useEffect(() => {
        const mediaQuery = window.matchMedia(query);
        const handleChange = (e) => setMatches(e.matches);
        
        mediaQuery.addEventListener('change', handleChange);
        
        return () => mediaQuery.removeEventListener('change', handleChange);
    }, [query]);
    
    return matches;
}

// Uso
function App() {
    const isMobile = useMediaQuery('(max-width: 768px)');
    const isDark = useMediaQuery('(prefers-color-scheme: dark)');
    
    return (
        <div>
            <p>Mobile: {isMobile ? 'Yes' : 'No'}</p>
            <p>Dark mode: {isDark ? 'Yes' : 'No'}</p>
        </div>
    );
}
```

---

## 13. Storage Events

### LocalStorage sync entre tabs

```jsx
function useLocalStorage(key, initialValue) {
    const [storedValue, setStoredValue] = useState(() => {
        try {
            const item = window.localStorage.getItem(key);
            return item ? JSON.parse(item) : initialValue;
        } catch (error) {
            return initialValue;
        }
    });
    
    const setValue = (value) => {
        try {
            const valueToStore = value instanceof Function ? value(storedValue) : value;
            setStoredValue(valueToStore);
            window.localStorage.setItem(key, JSON.stringify(valueToStore));
        } catch (error) {
            console.error(error);
        }
    };
    
    // Sincronizar entre tabs
    useEffect(() => {
        const handleStorageChange = (e) => {
            if (e.key === key && e.newValue !== null) {
                setStoredValue(JSON.parse(e.newValue));
            }
        };
        
        window.addEventListener('storage', handleStorageChange);
        
        return () => window.removeEventListener('storage', handleStorageChange);
    }, [key]);
    
    return [storedValue, setValue];
}

// Uso
function App() {
    const [name, setName] = useLocalStorage('name', '');
    
    return (
        <div>
            <input 
                value={name}
                onChange={(e) => setName(e.target.value)}
                placeholder="Escribe tu nombre"
            />
            <p>Abre otra tab para ver la sincronización</p>
        </div>
    );
}
```

---

## 14. Debounce y Throttle

### Debounce

```jsx
function useDebounce(value, delay) {
    const [debouncedValue, setDebouncedValue] = useState(value);
    
    useEffect(() => {
        const handler = setTimeout(() => {
            setDebouncedValue(value);
        }, delay);
        
        return () => clearTimeout(handler);
    }, [value, delay]);
    
    return debouncedValue;
}

// Uso: Search con debounce
function Search() {
    const [searchTerm, setSearchTerm] = useState('');
    const debouncedSearch = useDebounce(searchTerm, 500);
    
    useEffect(() => {
        if (debouncedSearch) {
            // Hacer búsqueda
            console.log('Searching for:', debouncedSearch);
        }
    }, [debouncedSearch]);
    
    return (
        <input
            value={searchTerm}
            onChange={(e) => setSearchTerm(e.target.value)}
            placeholder="Search..."
        />
    );
}
```

### Throttle

```jsx
function useThrottle(callback, delay) {
    const lastRun = useRef(Date.now());
    
    return useCallback((...args) => {
        const now = Date.now();
        
        if (now - lastRun.current >= delay) {
            callback(...args);
            lastRun.current = now;
        }
    }, [callback, delay]);
}

// Uso: Scroll tracking con throttle
function ScrollTracker() {
    const [scrollY, setScrollY] = useState(0);
    
    const handleScroll = useThrottle(() => {
        setScrollY(window.scrollY);
    }, 100);
    
    useEffect(() => {
        window.addEventListener('scroll', handleScroll);
        return () => window.removeEventListener('scroll', handleScroll);
    }, [handleScroll]);
    
    return <div>Scroll: {scrollY}px</div>;
}
```

---

## 15. Mejores Prácticas

### 1. Siempre hacer cleanup

```jsx
// ✅ Correcto
useEffect(() => {
    const handler = () => {};
    window.addEventListener('event', handler);
    return () => window.removeEventListener('event', handler);
}, []);

// ❌ Incorrecto - memory leak
useEffect(() => {
    const handler = () => {};
    window.addEventListener('event', handler);
    // No hay cleanup!
}, []);
```

### 2. Usar passive listeners para scroll

```jsx
useEffect(() => {
    const handler = () => {};
    window.addEventListener('scroll', handler, { passive: true });
    return () => window.removeEventListener('scroll', handler);
}, []);
```

### 3. Debounce/Throttle eventos costosos

```jsx
// Eventos que deben optimizarse:
// - scroll
// - resize
// - mousemove
// - keydown/keyup (para búsqueda)

const handleResize = useThrottle(() => {
    // Lógica costosa
}, 100);
```

### 4. Considerar dependencias en useEffect

```jsx
// Si el handler usa state/props, inclúyelos en dependencias
// o usa functional updates
useEffect(() => {
    const handler = () => {
        setState(prev => prev + 1); // Functional update
    };
    
    window.addEventListener('click', handler);
    return () => window.removeEventListener('click', handler);
}, []); // Dependencias vacías seguras con functional update
```

### 5. Verificar soporte del navegador

```jsx
useEffect(() => {
    if (!('IntersectionObserver' in window)) {
        console.log('IntersectionObserver not supported');
        return;
    }
    
    // Usar API
}, []);
```

---

**¡Domina el manejo de eventos y window en React! ⚛️**
