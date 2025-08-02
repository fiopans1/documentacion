# ¿Qué es el Event Loop?

El **Event Loop** en JavaScript es un mecanismo interno que coordina la ejecución de tareas *sincrónicas* y *asincrónicas*. Aunque JavaScript se ejecuta en un solo hilo, el Event Loop permite manejar múltiples operaciones (por ejemplo, llamadas a APIs externas o temporizadores) sin bloquear el flujo principal del código. Esto significa que mientras esperamos la resolución de una tarea asincrónica, el motor JavaScript puede seguir procesando otras instrucciones. Una vez que la tarea asincrónica finaliza, su callback se añade a la cola de tareas para ser ejecutado cuando el hilo principal esté libre. En resumen, el Event Loop permite gestionar **eventos**, **callbacks** y demás tareas asincrónicas sin detener la ejecución principal del programa.

## Componentes clave del Event Loop

Para entender cómo funciona el Event Loop es útil conocer sus componentes fundamentales:

* **Pila de llamadas (Call Stack):** Es una estructura LIFO donde el motor JavaScript almacena las funciones que se están ejecutando. Cada vez que se llama a una función, el intérprete la añade a la pila; cuando la función termina, se elimina de la pila. Esto permite saber en todo momento qué función está activa y cómo regresamos después de completar su ejecución.

* **APIs Web (Web APIs):** Son funcionalidades provistas por el navegador (o entorno de ejecución) para realizar operaciones asincrónicas, como temporizadores (`setTimeout`/`setInterval`), peticiones HTTP (`fetch`), manipulaciones del DOM o eventos de usuario. Cuando se invocan estas APIs, el trabajo pesado se delega fuera del hilo principal y, al completarse, su callback se encola en la cola de tareas.
Las WebAPIs son acciones que puedes ejecutar en un navegador y añadirlas a la callStack , pero no es propia del lenguaje de javascript, por ejemplo setTimeout, fetch, Promise

* **Cola de tareas (Task Queue o Cola de mensajes):** Es una lista de callbacks pendientes que esperan para ejecutarse. Cada mensaje o tarea en esta cola corresponde a la ejecución de una función asociada a un evento (por ejemplo, la finalización de un temporizador o la llegada de un evento de usuario). Cuando la pila de llamadas está vacía, el Event Loop toma el siguiente mensaje de la cola y ejecuta su callback (añadiéndolo al stack).
Una tarea son acciones que tiene que ejecutar el javascript y se ordenan en la cola de tareas. La cola de tareas es donde se guardan y ordenan las tareas/microtareas

* **Prioridad de las tareas** La prioridad máxima son las tareas de javascript, luego tendremos las microTareas de las webAPI y para finalizar las tareas de la WebAPI.
    - Tarea de Javascript
    - Microtarea de WebAPI
    - Tarea de WebApi

* **Event Loop (bucle de eventos):** Es un bucle continuo que verifica constantemente el estado de la pila de llamadas y las colas de tareas. De forma esquemática, su funcionamiento puede ilustrarse con el pseudocódigo:

  ```js
  while (queue.waitForMessage()) {
    queue.processNextMessage();
  }
  ```

  Es decir, el bucle espera nuevos mensajes (tareas) y, cuando llegan, procesa uno a la vez. Cada tarea se ejecuta completamente (“run-to-completion”) antes de pasar a la siguiente. Esto garantiza que una función en ejecución no sea interrumpida a la mitad por otra, evitando condiciones de carrera en el hilo principal.

## Flujo de ejecución (run-to-completion)

Una propiedad clave del modelo de Event Loop es que **cada tarea se procesa hasta completarse antes de iniciar otra**. Por ejemplo, si el programa está ejecutando un bucle intensivo de CPU, el navegador mostrará una alerta como “Un script está tardando mucho en ejecutarse”, porque mientras esa tarea termine, no podrá atender otros eventos (como clicks del usuario). Para evitar esto, es buena práctica dividir tareas pesadas usando APIs asincrónicas (por ejemplo, llamadas anidadas a `setTimeout`) y así ceder el control periódicamente al Event Loop. De esta forma la interfaz de usuario se mantiene receptiva.

## Microtareas vs Macrotareas

Dentro del Event Loop hay una distinción importante entre **macrotareas** y **microtareas**. Las macrotareas (o simplemente “tareas”) incluyen callbacks de APIs como `setTimeout`, `setInterval` o eventos de usuario. Las microtareas son callbacks de promesas (`.then`) o invocaciones a `queueMicrotask()`. Al final de cada iteración del Event Loop, **se procesan primero todas las microtareas pendientes y luego continúa con la siguiente macrotarea**. Esto da prioridad a las microtareas, haciendo que se ejecuten antes de ceder de nuevo al navegador (p.ej. para renderizar).

Por ejemplo, en el siguiente código la promesa (microtarea) se resuelve antes que el `setTimeout` (macrotarea), a pesar de tener ambos “delay” cero:

```js
console.log('Primero');

setTimeout(() => {
  console.log('Segundo');
}, 0);

Promise.resolve().then(() => {
  console.log('Tercero');
});

console.log('Cuarto');
```

La salida será: **Primero**, **Cuarto**, **Tercero**, **Segundo**. Primero se imprimen las llamadas síncronas (`Primero`, `Cuarto`), luego la promesa (`Tercero`), y por último el callback de `setTimeout` (`Segundo`). Esto ocurre porque las microtareas de las promesas se vacían antes de procesar la siguiente tarea de la cola. En general:

* *Macrotareas:* temporizadores (`setTimeout`, `setInterval`), eventos DOM, etc.
* *Microtareas:* `Promise.then`, `queueMicrotask()`, `async/await` (internamente usa promesas), etc.

El motor ejecuta primero la tarea actual y una vez que la pila está vacía procesa todas las microtareas en cola (incluso las que surjan durante su ejecución) antes de continuar con la siguiente macrotarea. Esto permite respuestas más inmediatas a operaciones críticas (por ejemplo, resoluciones de promesas).

## Ejemplos prácticos

Considere este sencillo ejemplo con `setTimeout`:

```js
console.log('Inicio');

setTimeout(() => {
  console.log('Tarea asíncrona');
}, 0);

console.log('Fin');
```

Aunque el temporizador tiene retardo cero, la salida en consola será **Inicio**, **Fin**, **Tarea asíncrona**. El callback de `setTimeout` se ejecuta **después** del último `console.log`, porque su llamada se encola en la cola de tareas y debe esperar a que el Event Loop libere la pila principal. En otras palabras, la función principal sigue hasta completarse antes de atender los callbacks asincrónicos.

En otro ejemplo con promesas:

```js
console.log('Primero');

setTimeout(() => {
  console.log('Segundo');
}, 0);

Promise.resolve().then(() => {
  console.log('Tercero');
});

console.log('Cuarto');
```

Aquí la consola mostrará: **Primero**, **Cuarto**, **Tercero**, **Segundo**. Primero se ejecuta el código sincronizado, luego la promesa (`Tercero` como microtarea) y por último el `setTimeout` (`Segundo` como macrotarea), demostrando la prioridad de las microtareas sobre las macrotareas.

## Representación visual del Event Loop

&#x20;**Figura:** Flujo conceptual del *Event Loop* en JavaScript. El diagrama muestra cómo las **tareas asincrónicas** son encoladas desde las **APIs del navegador** (temporizadores, eventos, etc.) y gestionadas por el bucle de eventos. La **pila de llamadas** (Call Stack) ejecuta las funciones una a una, mientras que las **colas de microtareas y macrotareas** determinan el orden: después de procesar una macrotarea, el motor ejecuta todas las microtareas pendientes antes de continuar. Esto ilustra por qué las promesas (`microtasks`) se resuelven antes que los callbacks de `setTimeout` (`macrotareas`) en los ejemplos anteriores.

## Resumen y recursos adicionales

En resumen, el Event Loop de JavaScript permite ejecutar código de forma **asincrónica en un solo hilo** moviendo las tareas completadas desde la cola al **call stack** cuando éste está vacío. Entre las claves del modelo destacan: la separación entre tareas sincronizadas (pila de llamadas) y asincrónicas (APIs Web + colas), el principio *“run-to-completion”*, y la distinción entre microtareas y macrotareas.

Para profundizar, recomendamos consultar la **documentación oficial de MDN** y recursos especializados (por ejemplo, la guía de *Concurrency model* de MDN y el tutorial de JavaScript.info) que ofrecen más ejemplos y detalles técnicos. Dominar el Event Loop es esencial para escribir código JavaScript eficiente y evitar errores comunes (como bloquear el hilo principal con operaciones sincrónicas pesadas o malinterpretar el orden de ejecución de callbacks).