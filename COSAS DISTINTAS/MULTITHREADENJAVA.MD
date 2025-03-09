# Guía Completa sobre Multithreading en Java

El multithreading en Java permite ejecutar múltiples tareas concurrentemente, mejorando el rendimiento y la capacidad de respuesta de una aplicación. Existen varias formas de implementar multithreading en Java, y en esta guía exploraremos las principales.

## 1. Creación de Hilos en Java

### 1.1 Usando la Clase `Thread`

Java proporciona la clase `Thread`, que podemos extender para crear nuestros propios hilos.

#### Ejemplo:

```java
class MiHilo extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("Ejecutando hilo: " + Thread.currentThread().getName());
            try {
                Thread.sleep(1000); // Simula una tarea costosa
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

public class Main {
    public static void main(String[] args) {
        MiHilo hilo1 = new MiHilo();
        MiHilo hilo2 = new MiHilo();

        hilo1.start();
        hilo2.start();
    }
}
```

**Explicación:**

- La clase `MiHilo` extiende `Thread` y sobreescribe el método `run()`.
- En el `main()`, creamos e iniciamos dos hilos que ejecutan `run()` concurrentemente.

---

### 1.2 Usando la Interfaz `Runnable`

Otra forma más flexible es implementar `Runnable`, lo que permite extender de otras clases si es necesario.

#### Ejemplo:

```java
class MiRunnable implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("Ejecutando hilo: " + Thread.currentThread().getName());
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Thread hilo1 = new Thread(new MiRunnable());
        Thread hilo2 = new Thread(new MiRunnable());

        hilo1.start();
        hilo2.start();
    }
}
```

**Ventaja:** La interfaz `Runnable` permite que una clase herede de otra y siga teniendo funcionalidad de hilos.

---

### 1.3 Usando la Interfaz `Callable`

A diferencia de `Runnable`, `Callable` permite devolver un resultado y lanzar excepciones.

#### Ejemplo:

```java
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

class MiCallable implements Callable<String> {
    @Override
    public String call() throws Exception {
        Thread.sleep(2000);
        return "Hilo ejecutado por: " + Thread.currentThread().getName();
    }
}

public class Main {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(2);
        Future<String> resultado = executor.submit(new MiCallable());

        try {
            System.out.println(resultado.get()); // Espera el resultado
        } catch (Exception e) {
            e.printStackTrace();
        }

        executor.shutdown();
    }
}
```

**Explicación:**

- `Callable<String>` devuelve un resultado (`String` en este caso).
- `submit()` de `ExecutorService` devuelve un `Future`, que se usa para obtener el resultado.

---

## 2. Usando `ExecutorService`

El uso de `ExecutorService` permite administrar mejor los hilos en comparación con crearlos manualmente.

### 2.1 Tipos de `ExecutorService`

```java
ExecutorService executor1 = Executors.newFixedThreadPool(3);  // Un pool con 3 hilos
ExecutorService executor2 = Executors.newSingleThreadExecutor(); // Un solo hilo
ExecutorService executor3 = Executors.newCachedThreadPool(); // Crea hilos dinámicamente
```

Cada uno de estos tipos tiene diferentes ventajas dependiendo del caso de uso.

#### Ejemplo con `FixedThreadPool`

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

class Tarea implements Runnable {
    private final int id;

    public Tarea(int id) {
        this.id = id;
    }

    @Override
    public void run() {
        System.out.println("Ejecutando tarea " + id + " en " + Thread.currentThread().getName());
    }
}

public class Main {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(3);

        for (int i = 1; i <= 5; i++) {
            executor.execute(new Tarea(i));
        }

        executor.shutdown();
    }
}
```

**Explicación:**

- `execute()` ejecuta múltiples tareas concurrentemente dentro de un pool de 3 hilos.
- `shutdown()` libera recursos tras la ejecución.

---

## 3. Sincronización en Hilos

Cuando varios hilos acceden a una misma variable, pueden ocurrir problemas de concurrencia. Para evitarlos, podemos usar `synchronized`.

### 3.1 Bloque `synchronized`

```java
class Contador {
    private int contador = 0;

    public synchronized void incrementar() {
        contador++;
    }

    public int getContador() {
        return contador;
    }
}

public class Main {
    public static void main(String[] args) throws InterruptedException {
        Contador contador = new Contador();

        Runnable tarea = () -> {
            for (int i = 0; i < 1000; i++) {
                contador.incrementar();
            }
        };

        Thread t1 = new Thread(tarea);
        Thread t2 = new Thread(tarea);

        t1.start();
        t2.start();
        t1.join();
        t2.join();

        System.out.println("Valor final: " + contador.getContador());
    }
}
```

**Explicación:**

- `synchronized` asegura que solo un hilo a la vez modifique `contador`.

---

## 4. Métodos para Controlar Hilos

### 4.1 `join()`

Hace que el hilo actual espere a que otro termine.

```java
Thread t1 = new Thread(() -> {
    try { Thread.sleep(2000); } catch (InterruptedException e) {}
    System.out.println("Hilo 1 terminado");
});
t1.start();
t1.join();
System.out.println("Programa finalizado");
```

### 4.2 `wait()` y `notify()`

Permiten que un hilo espere hasta que otro lo notifique.

```java
class Almacen {
    private boolean disponible = false;

    public synchronized void esperar() {
        while (!disponible) {
            try { wait(); } catch (InterruptedException e) {}
        }
        System.out.println("Consumidor procesando...");
    }

    public synchronized void notificar() {
        disponible = true;
        notify();
    }
}

public class Main {
    public static void main(String[] args) {
        Almacen almacen = new Almacen();

        Thread consumidor = new Thread(almacen::esperar);
        Thread productor = new Thread(() -> {
            try { Thread.sleep(2000); } catch (InterruptedException e) {}
            almacen.notificar();
        });

        consumidor.start();
        productor.start();
    }
}
```

**Explicación:**

- `wait()` pausa el hilo hasta que `notify()` lo despierte.

---

## 5. Conclusión

Java ofrece varias maneras de manejar hilos:

1. Extender `Thread` o implementar `Runnable`.
2. Usar `Callable` para devolver resultados.
3. Manejar hilos eficientemente con `ExecutorService`.
4. Controlar concurrencia con `synchronized`, `wait()` y `notify()`.

