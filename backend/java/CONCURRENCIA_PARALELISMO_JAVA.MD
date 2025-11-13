# 🚀 Guía Completa de Concurrencia y Paralelismo en Java

## Índice

1. [Introducción a Concurrencia vs Paralelismo](#1-introducción-a-concurrencia-vs-paralelismo)
2. [Conceptos Básicos: Thread y Runnable](#2-conceptos-básicos-thread-y-runnable)
3. [Callable y Future](#3-callable-y-future)
4. [ExecutorService y Pools de Hilos](#4-executorservice-y-pools-de-hilos)
5. [ScheduledExecutorService](#5-scheduledexecutorservice)
6. [CompletableFuture](#6-completablefuture)
7. [Fork/Join Framework](#7-forkjoin-framework)
8. [Mecanismos de Sincronización](#8-mecanismos-de-sincronización)
9. [Framework de Locks](#9-framework-de-locks)
10. [Variables Atómicas](#10-variables-atómicas)
11. [Utilidades de Concurrencia](#11-utilidades-de-concurrencia)
12. [Colecciones Thread-Safe](#12-colecciones-thread-safe)
13. [Virtual Threads (Java 21+)](#13-virtual-threads-java-21)
14. [Patrones y Mejores Prácticas](#14-patrones-y-mejores-prácticas)
15. [Errores Comunes y Cómo Evitarlos](#15-errores-comunes-y-cómo-evitarlos)

---

## 1. Introducción a Concurrencia vs Paralelismo

### ¿Qué es la Concurrencia?

**Concurrencia** es cuando múltiples tareas progresan en períodos de tiempo superpuestos. No necesariamente se ejecutan al mismo tiempo, sino que el sistema cambia entre ellas.

### ¿Qué es el Paralelismo?

**Paralelismo** es cuando múltiples tareas se ejecutan literalmente al mismo tiempo, típicamente en múltiples núcleos de CPU.

### Diferencias Clave

```
Concurrencia: Gestionar múltiples tareas a la vez
Paralelismo: Ejecutar múltiples tareas simultáneamente

Concurrencia: [Task1] [Task2] [Task1] [Task2] (alternando)
Paralelismo:  [Task1] simultáneo con [Task2]
```

**Cuándo usar cada uno:**
- **Concurrencia**: Para operaciones I/O (lectura de archivos, llamadas de red, consultas a BD)
- **Paralelismo**: Para operaciones CPU-intensive (procesamiento de imágenes, cálculos matemáticos)

---

## 2. Conceptos Básicos: Thread y Runnable

### 2.1 Clase Thread

La forma más básica de crear un hilo en Java.

```java
public class MiHilo extends Thread {
    private String nombre;
    
    public MiHilo(String nombre) {
        this.nombre = nombre;
    }
    
    @Override
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println(nombre + " - Iteración: " + i);
            try {
                Thread.sleep(1000); // Pausa de 1 segundo
            } catch (InterruptedException e) {
                System.out.println(nombre + " fue interrumpido");
                return;
            }
        }
        System.out.println(nombre + " ha terminado");
    }
}

// Uso
public class Main {
    public static void main(String[] args) {
        MiHilo hilo1 = new MiHilo("Hilo-1");
        MiHilo hilo2 = new MiHilo("Hilo-2");
        
        hilo1.start(); // Inicia el hilo
        hilo2.start();
        
        System.out.println("Hilo principal continúa...");
    }
}
```

**Características:**
- ✅ Simple y directo
- ❌ No permite herencia múltiple (ya extiende de Thread)
- ❌ Mezcla lógica de negocio con gestión de hilos

### 2.2 Interfaz Runnable

Más flexible que Thread, permite composición.

```java
public class MiTarea implements Runnable {
    private String nombre;
    
    public MiTarea(String nombre) {
        this.nombre = nombre;
    }
    
    @Override
    public void run() {
        System.out.println(nombre + " ejecutándose en: " + 
                          Thread.currentThread().getName());
        
        try {
            // Simular trabajo
            Thread.sleep(2000);
            System.out.println(nombre + " completado");
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
            System.out.println(nombre + " interrumpido");
        }
    }
}

// Uso
public class Main {
    public static void main(String[] args) {
        // Forma tradicional
        Thread hilo1 = new Thread(new MiTarea("Tarea-1"));
        hilo1.start();
        
        // Con lambda (Java 8+)
        Thread hilo2 = new Thread(() -> {
            System.out.println("Tarea ejecutándose en: " + 
                              Thread.currentThread().getName());
        });
        hilo2.start();
        
        // Con referencia de método
        Runnable tarea = new MiTarea("Tarea-3");
        new Thread(tarea).start();
    }
}
```

**Características:**
- ✅ Permite que la clase extienda de otra
- ✅ Separa lógica de negocio de gestión de hilos
- ✅ Puede usarse con ExecutorService
- ✅ Compatible con lambdas

### 2.3 Métodos Importantes de Thread

```java
public class EjemplosThread {
    public static void main(String[] args) throws InterruptedException {
        Thread hilo = new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                System.out.println("Trabajando... " + i);
                try {
                    Thread.sleep(500);
                } catch (InterruptedException e) {
                    System.out.println("Interrumpido!");
                    return;
                }
            }
        });
        
        // Iniciar hilo
        hilo.start();
        
        // Esperar 2 segundos
        Thread.sleep(2000);
        
        // Interrumpir el hilo
        hilo.interrupt();
        
        // Esperar a que termine
        hilo.join(); // El hilo principal espera a que 'hilo' termine
        
        System.out.println("Programa terminado");
    }
}
```

**Métodos clave:**
- `start()`: Inicia la ejecución del hilo
- `run()`: Contiene el código que ejecutará el hilo
- `sleep(millis)`: Pausa el hilo actual
- `join()`: Espera a que el hilo termine
- `interrupt()`: Interrumpe el hilo
- `isAlive()`: Verifica si el hilo está ejecutándose
- `setName()/getName()`: Gestiona el nombre del hilo
- `setPriority()/getPriority()`: Gestiona la prioridad (1-10)

**Cuándo usar Thread/Runnable:**
- ✅ Tareas simples que no requieren resultado
- ✅ Cuando necesitas control directo del hilo
- ❌ Para gestión compleja de múltiples hilos (usar ExecutorService)
- ❌ Cuando necesitas devolver resultados (usar Callable)

---

## 3. Callable y Future

### 3.1 ¿Qué es Callable?

`Callable` es similar a `Runnable` pero puede devolver un resultado y lanzar excepciones.

```java
import java.util.concurrent.*;

public class MiCallable implements Callable<Integer> {
    private int numero;
    
    public MiCallable(int numero) {
        this.numero = numero;
    }
    
    @Override
    public Integer call() throws Exception {
        System.out.println("Calculando factorial de " + numero);
        Thread.sleep(2000); // Simular cálculo costoso
        
        int resultado = 1;
        for (int i = 1; i <= numero; i++) {
            resultado *= i;
        }
        
        return resultado;
    }
}

// Uso básico
public class Main {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newSingleThreadExecutor();
        
        Callable<Integer> tarea = new MiCallable(5);
        Future<Integer> resultado = executor.submit(tarea);
        
        try {
            // Esto bloquea hasta que el resultado esté disponible
            Integer factorial = resultado.get();
            System.out.println("Factorial: " + factorial);
        } catch (InterruptedException | ExecutionException e) {
            e.printStackTrace();
        }
        
        executor.shutdown();
    }
}
```

### 3.2 Trabajando con Future

```java
import java.util.concurrent.*;

public class EjemplosFuture {
    
    public static void main(String[] args) throws Exception {
        ExecutorService executor = Executors.newFixedThreadPool(3);
        
        // Ejemplo 1: Future básico
        Future<String> futuro1 = executor.submit(() -> {
            Thread.sleep(3000);
            return "Resultado después de 3 segundos";
        });
        
        // Ejemplo 2: Verificar si está completo
        System.out.println("¿Está completo? " + futuro1.isDone());
        
        // Ejemplo 3: Esperar con timeout
        try {
            String resultado = futuro1.get(5, TimeUnit.SECONDS);
            System.out.println(resultado);
        } catch (TimeoutException e) {
            System.out.println("Timeout alcanzado");
            futuro1.cancel(true); // Cancelar la tarea
        }
        
        // Ejemplo 4: Múltiples Callables
        Callable<Integer> tarea1 = () -> {
            Thread.sleep(1000);
            return 100;
        };
        
        Callable<Integer> tarea2 = () -> {
            Thread.sleep(2000);
            return 200;
        };
        
        Callable<Integer> tarea3 = () -> {
            Thread.sleep(1500);
            return 300;
        };
        
        // Ejecutar todas y esperar resultados
        Future<Integer> f1 = executor.submit(tarea1);
        Future<Integer> f2 = executor.submit(tarea2);
        Future<Integer> f3 = executor.submit(tarea3);
        
        // Obtener resultados (en orden de envío, no de finalización)
        System.out.println("Suma: " + (f1.get() + f2.get() + f3.get()));
        
        // Ejemplo 5: invokeAll - ejecutar múltiples tareas
        List<Callable<String>> tareas = Arrays.asList(
            () -> { Thread.sleep(1000); return "Tarea 1"; },
            () -> { Thread.sleep(500); return "Tarea 2"; },
            () -> { Thread.sleep(1500); return "Tarea 3"; }
        );
        
        // Espera a que todas terminen
        List<Future<String>> resultados = executor.invokeAll(tareas);
        
        for (Future<String> resultado : resultados) {
            System.out.println(resultado.get());
        }
        
        // Ejemplo 6: invokeAny - retorna el primero que termina
        String primerResultado = executor.invokeAny(tareas);
        System.out.println("Primer resultado: " + primerResultado);
        
        executor.shutdown();
    }
}
```

### 3.3 Métodos de Future

```java
public class MetodosFuture {
    public static void ejemplos() throws Exception {
        ExecutorService executor = Executors.newSingleThreadExecutor();
        
        Future<String> future = executor.submit(() -> {
            Thread.sleep(3000);
            return "Completado";
        });
        
        // get() - bloquea hasta obtener resultado
        String resultado = future.get();
        
        // get(timeout) - bloquea con límite de tiempo
        String resultado2 = future.get(5, TimeUnit.SECONDS);
        
        // isDone() - verifica si terminó
        boolean terminado = future.isDone();
        
        // cancel() - intenta cancelar la tarea
        boolean cancelado = future.cancel(true);
        
        // isCancelled() - verifica si fue cancelada
        boolean estaCancelado = future.isCancelled();
        
        executor.shutdown();
    }
}
```

**Cuándo usar Callable/Future:**
- ✅ Cuando necesitas un valor de retorno de una tarea asíncrona
- ✅ Para tareas que pueden lanzar excepciones
- ✅ Cuando necesitas esperar múltiples resultados
- ❌ Para composición compleja de operaciones asíncronas (usar CompletableFuture)

---

## 4. ExecutorService y Pools de Hilos

### 4.1 ¿Por qué usar ExecutorService?

Crear hilos manualmente tiene desventajas:
- **Alto costo**: Crear/destruir hilos es costoso
- **Difícil de gestionar**: Controlar muchos hilos manualmente es complejo
- **Sin límites**: Puedes crear demasiados hilos y agotar recursos

**ExecutorService** resuelve estos problemas usando un pool de hilos reutilizables.

### 4.2 Tipos de Thread Pools

```java
import java.util.concurrent.*;

public class TiposDeExecutors {
    
    public static void main(String[] args) {
        
        // 1. FixedThreadPool - Número fijo de hilos
        ExecutorService fixed = Executors.newFixedThreadPool(4);
        // Uso: Cuando sabes cuántas tareas concurrentes necesitas
        // Ideal para: Procesamiento de lotes, tareas CPU-bound
        
        // 2. CachedThreadPool - Crea hilos según demanda
        ExecutorService cached = Executors.newCachedThreadPool();
        // Uso: Muchas tareas cortas y asíncronas
        // Ideal para: Tareas I/O-bound, servidores con muchas conexiones cortas
        // Precaución: Puede crear muchos hilos si no se controla
        
        // 3. SingleThreadExecutor - Un solo hilo
        ExecutorService single = Executors.newSingleThreadExecutor();
        // Uso: Tareas que deben ejecutarse secuencialmente
        // Ideal para: Procesamiento de cola FIFO, garantizar orden
        
        // 4. WorkStealingPool - Basado en ForkJoinPool
        ExecutorService workStealing = Executors.newWorkStealingPool();
        // Uso: Tareas que se pueden dividir recursivamente
        // Ideal para: Algoritmos divide y conquista, paralelismo de datos
        
        // Ejecutar tareas en cada tipo
        for (int i = 1; i <= 10; i++) {
            final int taskId = i;
            fixed.execute(() -> {
                System.out.println("Fixed Pool - Tarea " + taskId + 
                                 " en " + Thread.currentThread().getName());
            });
        }
        
        // Cerrar todos
        fixed.shutdown();
        cached.shutdown();
        single.shutdown();
        workStealing.shutdown();
    }
}
```

### 4.3 Ejemplo Completo con FixedThreadPool

```java
import java.util.concurrent.*;
import java.util.List;
import java.util.ArrayList;

public class EjemploFixedThreadPool {
    
    static class TareaProcesamiento implements Callable<String> {
        private int id;
        
        public TareaProcesamiento(int id) {
            this.id = id;
        }
        
        @Override
        public String call() throws Exception {
            String threadName = Thread.currentThread().getName();
            System.out.println("Iniciando tarea " + id + " en " + threadName);
            
            // Simular procesamiento
            Thread.sleep(2000);
            
            String resultado = "Tarea " + id + " completada por " + threadName;
            System.out.println(resultado);
            return resultado;
        }
    }
    
    public static void main(String[] args) {
        // Crear pool con 3 hilos
        ExecutorService executor = Executors.newFixedThreadPool(3);
        
        // Lista para almacenar los Future
        List<Future<String>> futures = new ArrayList<>();
        
        // Enviar 10 tareas al pool
        for (int i = 1; i <= 10; i++) {
            Future<String> future = executor.submit(new TareaProcesamiento(i));
            futures.add(future);
        }
        
        // Recoger resultados
        System.out.println("\n--- Recolectando resultados ---");
        for (Future<String> future : futures) {
            try {
                String resultado = future.get();
                System.out.println("Resultado: " + resultado);
            } catch (InterruptedException | ExecutionException e) {
                e.printStackTrace();
            }
        }
        
        // Apagar el executor
        executor.shutdown();
        
        try {
            // Esperar a que todas las tareas terminen (máximo 1 minuto)
            if (!executor.awaitTermination(60, TimeUnit.SECONDS)) {
                executor.shutdownNow(); // Forzar cierre
            }
        } catch (InterruptedException e) {
            executor.shutdownNow();
        }
        
        System.out.println("Todas las tareas completadas");
    }
}
```

### 4.4 Métodos Importantes de ExecutorService

```java
public class MetodosExecutorService {
    
    public static void ejemplos() throws Exception {
        ExecutorService executor = Executors.newFixedThreadPool(2);
        
        // execute() - Ejecuta Runnable sin retorno
        executor.execute(() -> System.out.println("Tarea sin retorno"));
        
        // submit() - Ejecuta Callable o Runnable, devuelve Future
        Future<String> future = executor.submit(() -> "Resultado");
        
        // invokeAll() - Ejecuta todas las tareas, espera a todas
        List<Callable<String>> tareas = Arrays.asList(
            () -> "Tarea 1",
            () -> "Tarea 2"
        );
        List<Future<String>> resultados = executor.invokeAll(tareas);
        
        // invokeAny() - Retorna el resultado del primero que complete
        String primero = executor.invokeAny(tareas);
        
        // shutdown() - No acepta nuevas tareas, completa las existentes
        executor.shutdown();
        
        // shutdownNow() - Intenta detener todas las tareas activas
        List<Runnable> pendientes = executor.shutdownNow();
        
        // awaitTermination() - Espera a que las tareas terminen
        boolean terminado = executor.awaitTermination(10, TimeUnit.SECONDS);
        
        // isShutdown() - Verifica si está en proceso de cierre
        boolean cerrado = executor.isShutdown();
        
        // isTerminated() - Verifica si todas las tareas terminaron
        boolean completado = executor.isTerminated();
    }
}
```

### 4.5 Configuración Personalizada con ThreadPoolExecutor

```java
import java.util.concurrent.*;

public class ThreadPoolPersonalizado {
    
    public static void main(String[] args) {
        // Parámetros personalizados
        int corePoolSize = 2;        // Número mínimo de hilos
        int maximumPoolSize = 4;     // Número máximo de hilos
        long keepAliveTime = 60;     // Tiempo antes de eliminar hilos inactivos
        TimeUnit unit = TimeUnit.SECONDS;
        BlockingQueue<Runnable> workQueue = new LinkedBlockingQueue<>(10);
        
        ThreadPoolExecutor executor = new ThreadPoolExecutor(
            corePoolSize,
            maximumPoolSize,
            keepAliveTime,
            unit,
            workQueue
        );
        
        // Política de rechazo cuando la cola está llena
        executor.setRejectedExecutionHandler(new ThreadPoolExecutor.CallerRunsPolicy());
        
        // Enviar tareas
        for (int i = 1; i <= 20; i++) {
            final int taskId = i;
            executor.execute(() -> {
                System.out.println("Ejecutando tarea " + taskId + 
                                 " en " + Thread.currentThread().getName() +
                                 " - Pool size: " + executor.getPoolSize());
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            });
        }
        
        // Monitorear el pool
        System.out.println("Core pool size: " + executor.getCorePoolSize());
        System.out.println("Max pool size: " + executor.getMaximumPoolSize());
        System.out.println("Active count: " + executor.getActiveCount());
        System.out.println("Task count: " + executor.getTaskCount());
        
        executor.shutdown();
    }
}
```

**Políticas de Rechazo:**
1. `AbortPolicy` (default): Lanza `RejectedExecutionException`
2. `CallerRunsPolicy`: Ejecuta la tarea en el hilo que la envió
3. `DiscardPolicy`: Descarta silenciosamente la tarea
4. `DiscardOldestPolicy`: Descarta la tarea más antigua y reintenta

**Cuándo usar cada Thread Pool:**
- **FixedThreadPool**: Carga predecible, tareas CPU-bound
- **CachedThreadPool**: Muchas tareas I/O cortas
- **SingleThreadExecutor**: Procesamiento secuencial, orden importante
- **WorkStealingPool**: Tareas paralelas, algoritmos recursivos
- **ThreadPoolExecutor personalizado**: Control fino sobre el comportamiento

---

## 5. ScheduledExecutorService

### 5.1 ¿Qué es ScheduledExecutorService?

`ScheduledExecutorService` permite programar tareas para ejecutarse después de un retraso o periódicamente.

```java
import java.util.concurrent.*;
import java.time.LocalDateTime;

public class EjemploScheduledExecutor {
    
    public static void main(String[] args) throws InterruptedException {
        ScheduledExecutorService scheduler = Executors.newScheduledThreadPool(2);
        
        // 1. Ejecutar tarea con retraso (una sola vez)
        System.out.println("Programando tarea para dentro de 3 segundos...");
        scheduler.schedule(
            () -> System.out.println("Tarea ejecutada a las " + LocalDateTime.now()),
            3,
            TimeUnit.SECONDS
        );
        
        // 2. Ejecutar tarea periódicamente con rate fijo
        System.out.println("Iniciando tarea periódica cada 2 segundos...");
        scheduler.scheduleAtFixedRate(
            () -> System.out.println("[Fixed Rate] Ejecutando a las " + LocalDateTime.now()),
            0,           // Retraso inicial
            2,           // Período
            TimeUnit.SECONDS
        );
        
        // 3. Ejecutar tarea con retraso fijo entre ejecuciones
        scheduler.scheduleWithFixedDelay(
            () -> {
                System.out.println("[Fixed Delay] Inicio a las " + LocalDateTime.now());
                try {
                    Thread.sleep(1000); // Simular trabajo
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
                System.out.println("[Fixed Delay] Fin a las " + LocalDateTime.now());
            },
            0,           // Retraso inicial
            3,           // Retraso entre el fin de una ejecución y el inicio de la siguiente
            TimeUnit.SECONDS
        );
        
        // Dejar ejecutar por 15 segundos
        Thread.sleep(15000);
        
        // Apagar el scheduler
        scheduler.shutdown();
    }
}
```

### 5.2 Diferencia entre scheduleAtFixedRate y scheduleWithFixedDelay

```java
public class DiferenciaScheduling {
    
    public static void main(String[] args) throws InterruptedException {
        ScheduledExecutorService scheduler = Executors.newScheduledThreadPool(2);
        
        System.out.println("=== scheduleAtFixedRate ===");
        // Se ejecuta cada X tiempo DESDE EL INICIO de la tarea anterior
        // Si la tarea tarda más que el período, la siguiente comienza inmediatamente
        scheduler.scheduleAtFixedRate(() -> {
            System.out.println("Fixed Rate - Inicio: " + LocalDateTime.now());
            try {
                Thread.sleep(3000); // La tarea tarda 3 segundos
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
            System.out.println("Fixed Rate - Fin: " + LocalDateTime.now());
        }, 0, 2, TimeUnit.SECONDS); // Período de 2 segundos
        
        Thread.sleep(10000);
        scheduler.shutdownNow();
        
        Thread.sleep(2000);
        
        scheduler = Executors.newScheduledThreadPool(2);
        System.out.println("\n=== scheduleWithFixedDelay ===");
        // Espera X tiempo DESPUÉS del FIN de la tarea anterior
        // Garantiza tiempo de descanso entre ejecuciones
        scheduler.scheduleWithFixedDelay(() -> {
            System.out.println("Fixed Delay - Inicio: " + LocalDateTime.now());
            try {
                Thread.sleep(3000); // La tarea tarda 3 segundos
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
            System.out.println("Fixed Delay - Fin: " + LocalDateTime.now());
        }, 0, 2, TimeUnit.SECONDS); // 2 segundos DESPUÉS de terminar
        
        Thread.sleep(15000);
        scheduler.shutdown();
    }
}
```

### 5.3 Uso Práctico: Sistema de Caché con Limpieza Automática

```java
import java.util.concurrent.*;
import java.util.Map;
import java.util.concurrent.ConcurrentHashMap;
import java.time.LocalDateTime;

public class CacheConLimpieza {
    
    private final Map<String, CacheEntry> cache = new ConcurrentHashMap<>();
    private final ScheduledExecutorService cleaner;
    
    static class CacheEntry {
        Object value;
        LocalDateTime expiryTime;
        
        CacheEntry(Object value, long ttlSeconds) {
            this.value = value;
            this.expiryTime = LocalDateTime.now().plusSeconds(ttlSeconds);
        }
        
        boolean isExpired() {
            return LocalDateTime.now().isAfter(expiryTime);
        }
    }
    
    public CacheConLimpieza() {
        // Limpiar caché cada 5 segundos
        cleaner = Executors.newScheduledThreadPool(1);
        cleaner.scheduleAtFixedRate(this::cleanExpired, 5, 5, TimeUnit.SECONDS);
    }
    
    public void put(String key, Object value, long ttlSeconds) {
        cache.put(key, new CacheEntry(value, ttlSeconds));
    }
    
    public Object get(String key) {
        CacheEntry entry = cache.get(key);
        if (entry == null || entry.isExpired()) {
            cache.remove(key);
            return null;
        }
        return entry.value;
    }
    
    private void cleanExpired() {
        System.out.println("Limpiando caché expirada...");
        cache.entrySet().removeIf(entry -> entry.getValue().isExpired());
        System.out.println("Entradas en caché: " + cache.size());
    }
    
    public void shutdown() {
        cleaner.shutdown();
    }
    
    public static void main(String[] args) throws InterruptedException {
        CacheConLimpieza cache = new CacheConLimpieza();
        
        // Agregar datos con diferentes TTL
        cache.put("dato1", "Valor 1", 10);  // Expira en 10 segundos
        cache.put("dato2", "Valor 2", 20);  // Expira en 20 segundos
        cache.put("dato3", "Valor 3", 5);   // Expira en 5 segundos
        
        // Verificar datos cada 3 segundos
        for (int i = 0; i < 8; i++) {
            Thread.sleep(3000);
            System.out.println("\n--- Verificación " + (i + 1) + " ---");
            System.out.println("dato1: " + cache.get("dato1"));
            System.out.println("dato2: " + cache.get("dato2"));
            System.out.println("dato3: " + cache.get("dato3"));
        }
        
        cache.shutdown();
    }
}
```

### 5.4 Ejemplo: Heartbeat y Health Check

```java
import java.util.concurrent.*;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

public class SistemaHeartbeat {
    
    private final ScheduledExecutorService scheduler;
    private ScheduledFuture<?> heartbeatTask;
    private volatile boolean sistemaActivo = true;
    
    public SistemaHeartbeat() {
        scheduler = Executors.newScheduledThreadPool(2);
    }
    
    public void iniciarHeartbeat() {
        heartbeatTask = scheduler.scheduleAtFixedRate(
            this::enviarHeartbeat,
            0,
            5,
            TimeUnit.SECONDS
        );
        
        System.out.println("Heartbeat iniciado");
    }
    
    private void enviarHeartbeat() {
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("HH:mm:ss");
        String timestamp = LocalDateTime.now().format(formatter);
        System.out.println("[" + timestamp + "] ❤️  Heartbeat - Sistema activo");
    }
    
    public void iniciarHealthCheck() {
        scheduler.scheduleWithFixedDelay(
            this::realizarHealthCheck,
            10,
            15,
            TimeUnit.SECONDS
        );
        
        System.out.println("Health check iniciado");
    }
    
    private void realizarHealthCheck() {
        System.out.println("\n🔍 Ejecutando health check...");
        
        // Simular verificación de recursos
        boolean memoriaOK = verificarMemoria();
        boolean cpuOK = verificarCPU();
        boolean dbOK = verificarBaseDatos();
        
        sistemaActivo = memoriaOK && cpuOK && dbOK;
        
        System.out.println("✓ Memoria: " + (memoriaOK ? "OK" : "ERROR"));
        System.out.println("✓ CPU: " + (cpuOK ? "OK" : "ERROR"));
        System.out.println("✓ Base de Datos: " + (dbOK ? "OK" : "ERROR"));
        System.out.println("Estado del sistema: " + (sistemaActivo ? "✅ HEALTHY" : "❌ UNHEALTHY"));
        System.out.println();
    }
    
    private boolean verificarMemoria() {
        Runtime runtime = Runtime.getRuntime();
        long memoriaUsada = runtime.totalMemory() - runtime.freeMemory();
        long memoriaTotal = runtime.totalMemory();
        double porcentaje = (double) memoriaUsada / memoriaTotal * 100;
        return porcentaje < 90; // OK si usa menos del 90%
    }
    
    private boolean verificarCPU() {
        // Simulación
        return true;
    }
    
    private boolean verificarBaseDatos() {
        // Simulación
        return Math.random() > 0.1; // 90% de éxito
    }
    
    public void detener() {
        if (heartbeatTask != null) {
            heartbeatTask.cancel(false);
        }
        scheduler.shutdown();
        try {
            if (!scheduler.awaitTermination(5, TimeUnit.SECONDS)) {
                scheduler.shutdownNow();
            }
        } catch (InterruptedException e) {
            scheduler.shutdownNow();
        }
    }
    
    public static void main(String[] args) throws InterruptedException {
        SistemaHeartbeat sistema = new SistemaHeartbeat();
        
        sistema.iniciarHeartbeat();
        sistema.iniciarHealthCheck();
        
        // Ejecutar por 1 minuto
        Thread.sleep(60000);
        
        sistema.detener();
        System.out.println("Sistema detenido");
    }
}
```

**Cuándo usar ScheduledExecutorService:**
- ✅ Tareas periódicas (limpieza de caché, health checks, heartbeats)
- ✅ Tareas diferidas (enviar email después de X tiempo)
- ✅ Polling de recursos externos
- ✅ Mantenimiento automático del sistema

---

## 6. CompletableFuture

### 6.1 ¿Qué es CompletableFuture?

`CompletableFuture` es una evolución de `Future` que permite programación asíncrona moderna con composición de operaciones.

### 6.2 Creación de CompletableFuture

```java
import java.util.concurrent.*;

public class CreacionCompletableFuture {
    
    public static void main(String[] args) throws Exception {
        
        // 1. CompletableFuture completado con valor
        CompletableFuture<String> futuro1 = CompletableFuture.completedFuture("Valor inmediato");
        System.out.println(futuro1.get()); // "Valor inmediato"
        
        // 2. CompletableFuture asíncrono con supplyAsync
        CompletableFuture<String> futuro2 = CompletableFuture.supplyAsync(() -> {
            try {
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
            return "Resultado asíncrono";
        });
        
        System.out.println("Esperando resultado...");
        System.out.println(futuro2.get()); // Bloquea hasta obtener resultado
        
        // 3. runAsync - para tareas sin retorno
        CompletableFuture<Void> futuro3 = CompletableFuture.runAsync(() -> {
            System.out.println("Ejecutando tarea sin retorno");
        });
        
        futuro3.get(); // Esperar a que termine
        
        // 4. Con executor personalizado
        ExecutorService executor = Executors.newFixedThreadPool(3);
        CompletableFuture<String> futuro4 = CompletableFuture.supplyAsync(() -> {
            return "Con executor personalizado: " + Thread.currentThread().getName();
        }, executor);
        
        System.out.println(futuro4.get());
        executor.shutdown();
    }
}
```

### 6.3 Transformaciones y Composición

```java
import java.util.concurrent.CompletableFuture;

public class TransformacionesCompletableFuture {
    
    public static void main(String[] args) throws Exception {
        
        // thenApply - Transforma el resultado
        CompletableFuture<Integer> futuro1 = CompletableFuture.supplyAsync(() -> 5)
            .thenApply(n -> n * 2)              // 10
            .thenApply(n -> n + 3);             // 13
        
        System.out.println("thenApply: " + futuro1.get());
        
        // thenCompose - Encadena CompletableFutures (flatMap)
        CompletableFuture<String> futuro2 = CompletableFuture.supplyAsync(() -> "Usuario123")
            .thenCompose(userId -> obtenerNombreUsuario(userId))
            .thenCompose(nombre -> obtenerEmailUsuario(nombre));
        
        System.out.println("thenCompose: " + futuro2.get());
        
        // thenCombine - Combina dos CompletableFutures
        CompletableFuture<Integer> precio = CompletableFuture.supplyAsync(() -> 100);
        CompletableFuture<Double> descuento = CompletableFuture.supplyAsync(() -> 0.15);
        
        CompletableFuture<Double> precioFinal = precio.thenCombine(descuento, 
            (p, d) -> p * (1 - d));
        
        System.out.println("Precio final: " + precioFinal.get());
        
        // thenAccept - Consume el resultado sin devolver nada
        CompletableFuture<Void> futuro3 = CompletableFuture.supplyAsync(() -> "Hola Mundo")
            .thenAccept(mensaje -> System.out.println("Mensaje: " + mensaje));
        
        futuro3.get();
        
        // thenRun - Ejecuta acción sin usar el resultado
        CompletableFuture<Void> futuro4 = CompletableFuture.supplyAsync(() -> "Procesando...")
            .thenRun(() -> System.out.println("Procesamiento completado"));
        
        futuro4.get();
    }
    
    static CompletableFuture<String> obtenerNombreUsuario(String userId) {
        return CompletableFuture.supplyAsync(() -> {
            // Simular consulta a BD
            try { Thread.sleep(500); } catch (InterruptedException e) {}
            return "Juan Pérez";
        });
    }
    
    static CompletableFuture<String> obtenerEmailUsuario(String nombre) {
        return CompletableFuture.supplyAsync(() -> {
            // Simular consulta a BD
            try { Thread.sleep(500); } catch (InterruptedException e) {}
            return nombre.toLowerCase().replace(" ", ".") + "@email.com";
        });
    }
}
```

### 6.4 Manejo de Errores

```java
import java.util.concurrent.CompletableFuture;

public class ManejoErroresCompletableFuture {
    
    public static void main(String[] args) throws Exception {
        
        // exceptionally - Maneja excepciones
        CompletableFuture<String> futuro1 = CompletableFuture.supplyAsync(() -> {
            if (Math.random() > 0.5) {
                throw new RuntimeException("Error aleatorio");
            }
            return "Éxito";
        }).exceptionally(ex -> {
            System.out.println("Error capturado: " + ex.getMessage());
            return "Valor por defecto";
        });
        
        System.out.println("exceptionally: " + futuro1.get());
        
        // handle - Maneja tanto éxito como error
        CompletableFuture<String> futuro2 = CompletableFuture.supplyAsync(() -> {
            if (Math.random() > 0.5) {
                throw new RuntimeException("Fallo en operación");
            }
            return "Operación exitosa";
        }).handle((resultado, excepcion) -> {
            if (excepcion != null) {
                return "Error: " + excepcion.getMessage();
            }
            return resultado;
        });
        
        System.out.println("handle: " + futuro2.get());
        
        // whenComplete - Ejecuta acción al completar (éxito o error)
        CompletableFuture<String> futuro3 = CompletableFuture.supplyAsync(() -> {
            return "Datos procesados";
        }).whenComplete((resultado, excepcion) -> {
            if (excepcion != null) {
                System.out.println("Falló con: " + excepcion.getMessage());
            } else {
                System.out.println("Completado con: " + resultado);
            }
        });
        
        futuro3.get();
    }
}
```

### 6.5 Combinando Múltiples CompletableFutures

```java
import java.util.concurrent.CompletableFuture;
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class CombinacionMultiple {
    
    public static void main(String[] args) throws Exception {
        
        // allOf - Espera a que TODOS completen
        CompletableFuture<String> futuro1 = CompletableFuture.supplyAsync(() -> {
            sleep(1000);
            return "Resultado 1";
        });
        
        CompletableFuture<String> futuro2 = CompletableFuture.supplyAsync(() -> {
            sleep(2000);
            return "Resultado 2";
        });
        
        CompletableFuture<String> futuro3 = CompletableFuture.supplyAsync(() -> {
            sleep(1500);
            return "Resultado 3";
        });
        
        CompletableFuture<Void> todosFuturos = CompletableFuture.allOf(
            futuro1, futuro2, futuro3
        );
        
        // Esperar a que todos completen
        todosFuturos.get();
        
        System.out.println("Todos completados:");
        System.out.println(futuro1.get());
        System.out.println(futuro2.get());
        System.out.println(futuro3.get());
        
        // Combinar resultados de múltiples futuros
        List<CompletableFuture<Integer>> futuros = Arrays.asList(
            CompletableFuture.supplyAsync(() -> 10),
            CompletableFuture.supplyAsync(() -> 20),
            CompletableFuture.supplyAsync(() -> 30)
        );
        
        CompletableFuture<List<Integer>> todosCombinados = CompletableFuture.allOf(
            futuros.toArray(new CompletableFuture[0])
        ).thenApply(v -> 
            futuros.stream()
                   .map(CompletableFuture::join)
                   .collect(Collectors.toList())
        );
        
        System.out.println("Lista combinada: " + todosCombinados.get());
        
        // anyOf - Retorna el primero que complete
        CompletableFuture<Object> primero = CompletableFuture.anyOf(
            CompletableFuture.supplyAsync(() -> { sleep(3000); return "Lento"; }),
            CompletableFuture.supplyAsync(() -> { sleep(1000); return "Rápido"; }),
            CompletableFuture.supplyAsync(() -> { sleep(2000); return "Medio"; })
        );
        
        System.out.println("Primero en completar: " + primero.get());
    }
    
    static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}
```

### 6.6 Ejemplo Práctico: Servicio de Compras Online

```java
import java.util.concurrent.*;
import java.util.*;

public class ServicioCompras {
    
    static class Producto {
        String nombre;
        double precio;
        
        Producto(String nombre, double precio) {
            this.nombre = nombre;
            this.precio = precio;
        }
    }
    
    static class Usuario {
        String id;
        String nombre;
        String email;
        
        Usuario(String id, String nombre, String email) {
            this.id = id;
            this.nombre = nombre;
            this.email = email;
        }
    }
    
    // Simular llamadas a servicios externos
    public static CompletableFuture<Usuario> obtenerUsuario(String userId) {
        return CompletableFuture.supplyAsync(() -> {
            System.out.println("📥 Obteniendo usuario...");
            sleep(1000);
            return new Usuario(userId, "Juan Pérez", "juan@email.com");
        });
    }
    
    public static CompletableFuture<List<Producto>> obtenerProductos(List<String> productIds) {
        return CompletableFuture.supplyAsync(() -> {
            System.out.println("📥 Obteniendo productos...");
            sleep(1500);
            return Arrays.asList(
                new Producto("Laptop", 999.99),
                new Producto("Mouse", 29.99),
                new Producto("Teclado", 79.99)
            );
        });
    }
    
    public static CompletableFuture<Double> calcularDescuento(String userId) {
        return CompletableFuture.supplyAsync(() -> {
            System.out.println("💰 Calculando descuento...");
            sleep(800);
            return 0.10; // 10% de descuento
        });
    }
    
    public static CompletableFuture<Boolean> verificarInventario(List<Producto> productos) {
        return CompletableFuture.supplyAsync(() -> {
            System.out.println("📦 Verificando inventario...");
            sleep(1000);
            return true; // Todos disponibles
        });
    }
    
    public static CompletableFuture<String> procesarPago(double monto, String userId) {
        return CompletableFuture.supplyAsync(() -> {
            System.out.println("💳 Procesando pago de $" + monto + "...");
            sleep(2000);
            return "PAGO-" + UUID.randomUUID().toString().substring(0, 8);
        });
    }
    
    public static CompletableFuture<Void> enviarEmail(String email, String mensaje) {
        return CompletableFuture.runAsync(() -> {
            System.out.println("📧 Enviando email a " + email + "...");
            sleep(500);
            System.out.println("✅ Email enviado: " + mensaje);
        });
    }
    
    public static void procesarCompra(String userId, List<String> productIds) {
        long inicio = System.currentTimeMillis();
        
        // Iniciar múltiples operaciones en paralelo
        CompletableFuture<Usuario> futuroUsuario = obtenerUsuario(userId);
        CompletableFuture<List<Producto>> futuroProductos = obtenerProductos(productIds);
        CompletableFuture<Double> futuroDescuento = calcularDescuento(userId);
        
        // Combinar resultados y continuar el flujo
        CompletableFuture<Void> procesoCompra = futuroUsuario
            .thenCombine(futuroProductos, (usuario, productos) -> {
                System.out.println("\n✓ Usuario y productos obtenidos");
                return new AbstractMap.SimpleEntry<>(usuario, productos);
            })
            .thenCombine(futuroDescuento, (entry, descuento) -> {
                System.out.println("✓ Descuento calculado: " + (descuento * 100) + "%");
                return new Object() {
                    Usuario usuario = entry.getKey();
                    List<Producto> productos = entry.getValue();
                    double desc = descuento;
                };
            })
            .thenCompose(data -> {
                // Verificar inventario
                return verificarInventario(data.productos)
                    .thenCompose(disponible -> {
                        if (!disponible) {
                            return CompletableFuture.failedFuture(
                                new RuntimeException("Productos no disponibles")
                            );
                        }
                        
                        // Calcular total
                        double subtotal = data.productos.stream()
                            .mapToDouble(p -> p.precio)
                            .sum();
                        double total = subtotal * (1 - data.desc);
                        
                        System.out.println("\n💵 Subtotal: $" + subtotal);
                        System.out.println("💵 Total con descuento: $" + total);
                        
                        // Procesar pago
                        return procesarPago(total, data.usuario.id)
                            .thenCompose(pagoId -> {
                                System.out.println("✅ Pago procesado: " + pagoId);
                                
                                // Enviar email de confirmación
                                String mensaje = "Compra confirmada. ID: " + pagoId + 
                                               ", Total: $" + total;
                                return enviarEmail(data.usuario.email, mensaje);
                            });
                    });
            })
            .exceptionally(ex -> {
                System.err.println("❌ Error en la compra: " + ex.getMessage());
                return null;
            })
            .whenComplete((result, ex) -> {
                long fin = System.currentTimeMillis();
                System.out.println("\n⏱️  Tiempo total: " + (fin - inicio) + "ms");
            });
        
        // Esperar a que todo termine
        try {
            procesoCompra.get();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    
    static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
    
    public static void main(String[] args) {
        System.out.println("🛒 Iniciando proceso de compra...\n");
        procesarCompra("USER123", Arrays.asList("PROD1", "PROD2", "PROD3"));
    }
}
```

**Cuándo usar CompletableFuture:**
- ✅ Operaciones asíncronas complejas con múltiples pasos
- ✅ Llamadas a APIs externas en paralelo
- ✅ Composición de operaciones asíncronas
- ✅ Manejo elegante de errores en flujos asíncronos
- ✅ Cuando necesitas más control que Future básico

---

## 7. Fork/Join Framework

### 7.1 ¿Qué es Fork/Join?

El **Fork/Join Framework** está diseñado para paralelismo de tareas que se pueden dividir recursivamente en subtareas más pequeñas.

**Concepto:**
- **Fork**: Dividir una tarea grande en subtareas
- **Join**: Combinar los resultados de las subtareas

### 7.2 RecursiveTask vs RecursiveAction

```java
import java.util.concurrent.*;

// RecursiveTask<V> - Devuelve un resultado
class SumaRecursiva extends RecursiveTask<Long> {
    private final long[] numeros;
    private final int inicio;
    private final int fin;
    private static final int UMBRAL = 1000; // Tamaño mínimo para dividir
    
    public SumaRecursiva(long[] numeros, int inicio, int fin) {
        this.numeros = numeros;
        this.inicio = inicio;
        this.fin = fin;
    }
    
    @Override
    protected Long compute() {
        int longitud = fin - inicio;
        
        // Si la tarea es pequeña, calcular directamente
        if (longitud <= UMBRAL) {
            return calcularDirectamente();
        }
        
        // Si es grande, dividir en subtareas (FORK)
        int medio = inicio + longitud / 2;
        SumaRecursiva tareaIzquierda = new SumaRecursiva(numeros, inicio, medio);
        SumaRecursiva tareaDerecha = new SumaRecursiva(numeros, medio, fin);
        
        // Fork: ejecutar subtarea izquierda en paralelo
        tareaIzquierda.fork();
        
        // Ejecutar subtarea derecha en el hilo actual
        Long resultadoDerecha = tareaDerecha.compute();
        
        // Join: esperar y obtener resultado de la subtarea izquierda
        Long resultadoIzquierda = tareaIzquierda.join();
        
        // Combinar resultados
        return resultadoIzquierda + resultadoDerecha;
    }
    
    private long calcularDirectamente() {
        long suma = 0;
        for (int i = inicio; i < fin; i++) {
            suma += numeros[i];
        }
        return suma;
    }
}

// RecursiveAction - No devuelve resultado
class OrdenamientoRecursivo extends RecursiveAction {
    private final int[] array;
    private final int inicio;
    private final int fin;
    private static final int UMBRAL = 100;
    
    public OrdenamientoRecursivo(int[] array, int inicio, int fin) {
        this.array = array;
        this.inicio = inicio;
        this.fin = fin;
    }
    
    @Override
    protected void compute() {
        if (fin - inicio <= UMBRAL) {
            // Ordenar directamente para arrays pequeños
            Arrays.sort(array, inicio, fin);
            return;
        }
        
        // Dividir y ordenar en paralelo
        int medio = inicio + (fin - inicio) / 2;
        invokeAll(
            new OrdenamientoRecursivo(array, inicio, medio),
            new OrdenamientoRecursivo(array, medio, fin)
        );
        
        // Merge de las dos mitades ordenadas
        merge(inicio, medio, fin);
    }
    
    private void merge(int inicio, int medio, int fin) {
        int[] temp = new int[fin - inicio];
        int i = inicio, j = medio, k = 0;
        
        while (i < medio && j < fin) {
            temp[k++] = array[i] <= array[j] ? array[i++] : array[j++];
        }
        
        while (i < medio) temp[k++] = array[i++];
        while (j < fin) temp[k++] = array[j++];
        
        System.arraycopy(temp, 0, array, inicio, temp.length);
    }
}

public class EjemploForkJoin {
    public static void main(String[] args) {
        // Crear pool ForkJoin (usa todos los procesadores disponibles)
        ForkJoinPool pool = new ForkJoinPool();
        
        // Ejemplo 1: RecursiveTask (suma de array)
        long[] numeros = new long[10_000_000];
        for (int i = 0; i < numeros.length; i++) {
            numeros[i] = i + 1;
        }
        
        long inicio = System.currentTimeMillis();
        SumaRecursiva tarea = new SumaRecursiva(numeros, 0, numeros.length);
        Long resultado = pool.invoke(tarea);
        long fin = System.currentTimeMillis();
        
        System.out.println("Suma total: " + resultado);
        System.out.println("Tiempo: " + (fin - inicio) + "ms");
        
        // Ejemplo 2: RecursiveAction (ordenamiento)
        int[] array = new int[1_000_000];
        Random random = new Random();
        for (int i = 0; i < array.length; i++) {
            array[i] = random.nextInt(1000);
        }
        
        inicio = System.currentTimeMillis();
        pool.invoke(new OrdenamientoRecursivo(array, 0, array.length));
        fin = System.currentTimeMillis();
        
        System.out.println("Array ordenado en: " + (fin - inicio) + "ms");
        System.out.println("Primeros 10 elementos: " + Arrays.toString(
            Arrays.copyOf(array, 10)));
        
        pool.shutdown();
    }
}
```

### 7.3 Ejemplo Práctico: Procesamiento de Imágenes en Paralelo

```java
import java.util.concurrent.*;
import java.util.Random;

public class ProcesamientoImagenes {
    
    static class Imagen {
        int[][] pixeles;
        int ancho, alto;
        
        Imagen(int ancho, int alto) {
            this.ancho = ancho;
            this.alto = alto;
            this.pixeles = new int[alto][ancho];
            
            // Generar imagen aleatoria
            Random random = new Random();
            for (int i = 0; i < alto; i++) {
                for (int j = 0; j < ancho; j++) {
                    pixeles[i][j] = random.nextInt(256);
                }
            }
        }
    }
    
    // Aplicar filtro de desenfoque en paralelo
    static class AplicarFiltro extends RecursiveAction {
        private final Imagen imagen;
        private final int filaInicio;
        private final int filaFin;
        private static final int UMBRAL = 50; // Procesar 50 filas a la vez
        
        AplicarFiltro(Imagen imagen, int filaInicio, int filaFin) {
            this.imagen = imagen;
            this.filaInicio = filaInicio;
            this.filaFin = filaFin;
        }
        
        @Override
        protected void compute() {
            if (filaFin - filaInicio <= UMBRAL) {
                aplicarFiltroDirecto();
                return;
            }
            
            int medio = filaInicio + (filaFin - filaInicio) / 2;
            invokeAll(
                new AplicarFiltro(imagen, filaInicio, medio),
                new AplicarFiltro(imagen, medio, filaFin)
            );
        }
        
        private void aplicarFiltroDirecto() {
            for (int i = filaInicio; i < filaFin; i++) {
                for (int j = 0; j < imagen.ancho; j++) {
                    // Aplicar filtro simple (promedio 3x3)
                    int suma = 0;
                    int count = 0;
                    
                    for (int di = -1; di <= 1; di++) {
                        for (int dj = -1; dj <= 1; dj++) {
                            int ni = i + di;
                            int nj = j + dj;
                            if (ni >= 0 && ni < imagen.alto && 
                                nj >= 0 && nj < imagen.ancho) {
                                suma += imagen.pixeles[ni][nj];
                                count++;
                            }
                        }
                    }
                    
                    imagen.pixeles[i][j] = suma / count;
                }
            }
        }
    }
    
    public static void main(String[] args) {
        Imagen imagen = new Imagen(2000, 2000);
        
        System.out.println("Procesando imagen de " + imagen.ancho + "x" + imagen.alto);
        
        ForkJoinPool pool = new ForkJoinPool();
        
        long inicio = System.currentTimeMillis();
        pool.invoke(new AplicarFiltro(imagen, 0, imagen.alto));
        long fin = System.currentTimeMillis();
        
        System.out.println("Filtro aplicado en: " + (fin - inicio) + "ms");
        System.out.println("Procesadores usados: " + pool.getParallelism());
        
        pool.shutdown();
    }
}
```

### 7.4 Búsqueda en Paralelo

```java
import java.util.concurrent.*;
import java.util.Optional;

public class BusquedaParalela {
    
    static class BuscarElemento extends RecursiveTask<Optional<Integer>> {
        private final int[] array;
        private final int objetivo;
        private final int inicio;
        private final int fin;
        private static final int UMBRAL = 1000;
        
        BuscarElemento(int[] array, int objetivo, int inicio, int fin) {
            this.array = array;
            this.objetivo = objetivo;
            this.inicio = inicio;
            this.fin = fin;
        }
        
        @Override
        protected Optional<Integer> compute() {
            if (fin - inicio <= UMBRAL) {
                return buscarDirecto();
            }
            
            int medio = inicio + (fin - inicio) / 2;
            BuscarElemento tareaIzq = new BuscarElemento(array, objetivo, inicio, medio);
            BuscarElemento tareaDer = new BuscarElemento(array, objetivo, medio, fin);
            
            tareaIzq.fork();
            
            Optional<Integer> resultadoDer = tareaDer.compute();
            if (resultadoDer.isPresent()) {
                return resultadoDer;
            }
            
            return tareaIzq.join();
        }
        
        private Optional<Integer> buscarDirecto() {
            for (int i = inicio; i < fin; i++) {
                if (array[i] == objetivo) {
                    return Optional.of(i);
                }
            }
            return Optional.empty();
        }
    }
    
    public static void main(String[] args) {
        int[] array = new int[10_000_000];
        for (int i = 0; i < array.length; i++) {
            array[i] = i;
        }
        
        int objetivo = 7_654_321;
        
        ForkJoinPool pool = new ForkJoinPool();
        
        long inicio = System.currentTimeMillis();
        Optional<Integer> resultado = pool.invoke(
            new BuscarElemento(array, objetivo, 0, array.length)
        );
        long fin = System.currentTimeMillis();
        
        if (resultado.isPresent()) {
            System.out.println("Elemento encontrado en índice: " + resultado.get());
        } else {
            System.out.println("Elemento no encontrado");
        }
        
        System.out.println("Tiempo de búsqueda: " + (fin - inicio) + "ms");
        
        pool.shutdown();
    }
}
```

**Cuándo usar Fork/Join:**
- ✅ Algoritmos divide y conquista
- ✅ Procesamiento de grandes conjuntos de datos
- ✅ Operaciones recursivas paralelizables
- ✅ Procesamiento de imágenes, videos
- ✅ Operaciones matemáticas complejas
- ❌ Para tareas I/O-bound (mejor usar CompletableFuture)
- ❌ Tareas pequeñas (overhead mayor que beneficio)

---

## 8. Mecanismos de Sincronización

### 8.1 synchronized

La palabra clave `synchronized` asegura que solo un hilo puede ejecutar un bloque de código a la vez.

```java
public class EjemploSynchronized {
    
    // 1. Método sincronizado
    static class Contador {
        private int count = 0;
        
        public synchronized void incrementar() {
            count++;
        }
        
        public synchronized int getCount() {
            return count;
        }
    }
    
    // 2. Bloque sincronizado
    static class ContadorConBloque {
        private int count = 0;
        private final Object lock = new Object();
        
        public void incrementar() {
            synchronized(lock) {
                count++;
            }
        }
        
        public int getCount() {
            synchronized(lock) {
                return count;
            }
        }
    }
    
    // 3. Método estático sincronizado (sincroniza en la clase)
    static class ContadorEstatico {
        private static int count = 0;
        
        public static synchronized void incrementar() {
            count++;
        }
        
        public static synchronized int getCount() {
            return count;
        }
    }
    
    public static void main(String[] args) throws InterruptedException {
        Contador contador = new Contador();
        
        // Crear múltiples hilos que incrementan el contador
        Thread[] hilos = new Thread[10];
        for (int i = 0; i < 10; i++) {
            hilos[i] = new Thread(() -> {
                for (int j = 0; j < 1000; j++) {
                    contador.incrementar();
                }
            });
            hilos[i].start();
        }
        
        // Esperar a que todos terminen
        for (Thread hilo : hilos) {
            hilo.join();
        }
        
        System.out.println("Count final: " + contador.getCount());
        // Sin synchronized, el resultado sería impredecible
        // Con synchronized, siempre es 10,000
    }
}
```

### 8.2 volatile

`volatile` asegura que los cambios a una variable sean visibles para todos los hilos inmediatamente.

```java
public class EjemploVolatile {
    
    // Sin volatile, el hilo puede no ver el cambio
    private static volatile boolean running = true;
    
    public static void main(String[] args) throws InterruptedException {
        Thread trabajador = new Thread(() -> {
            long contador = 0;
            while (running) {
                contador++;
            }
            System.out.println("Hilo detenido después de " + contador + " iteraciones");
        });
        
        trabajador.start();
        
        Thread.sleep(1000);
        
        System.out.println("Deteniendo hilo...");
        running = false; // Este cambio será visible gracias a volatile
        
        trabajador.join();
        System.out.println("Programa terminado");
    }
}
```

**Diferencia entre synchronized y volatile:**
- **volatile**: Solo garantiza visibilidad, no atomicidad
- **synchronized**: Garantiza visibilidad Y atomicidad (exclusión mutua)

```java
public class DiferenciaSyncVolatile {
    
    // volatile NO es suficiente para operaciones no atómicas
    static class ContadorIncorrecto {
        private volatile int count = 0;
        
        public void incrementar() {
            count++; // NO es atómico! (lee, incrementa, escribe)
        }
    }
    
    // synchronized es necesario para operaciones no atómicas
    static class ContadorCorrecto {
        private int count = 0;
        
        public synchronized void incrementar() {
            count++; // Ahora es seguro
        }
    }
    
    // volatile ES suficiente para asignaciones simples
    static class BanderaVolatil {
        private volatile boolean flag = false;
        
        public void setFlag(boolean value) {
            flag = value; // Asignación simple, volatile es suficiente
        }
        
        public boolean getFlag() {
            return flag;
        }
    }
}
```

### 8.3 wait(), notify() y notifyAll()

Permiten comunicación entre hilos.

```java
public class ProducerConsumer {
    
    static class Buffer {
        private int dato;
        private boolean disponible = false;
        
        public synchronized void producir(int valor) {
            while (disponible) {
                try {
                    wait(); // Espera hasta que el consumidor tome el dato
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
            
            dato = valor;
            disponible = true;
            System.out.println("Producido: " + valor);
            notify(); // Notifica al consumidor
        }
        
        public synchronized int consumir() {
            while (!disponible) {
                try {
                    wait(); // Espera hasta que el productor ponga un dato
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
            
            disponible = false;
            System.out.println("Consumido: " + dato);
            notify(); // Notifica al productor
            return dato;
        }
    }
    
    public static void main(String[] args) {
        Buffer buffer = new Buffer();
        
        // Productor
        Thread productor = new Thread(() -> {
            for (int i = 1; i <= 10; i++) {
                buffer.producir(i);
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
        });
        
        // Consumidor
        Thread consumidor = new Thread(() -> {
            for (int i = 1; i <= 10; i++) {
                buffer.consumir();
                try {
                    Thread.sleep(150);
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
        });
        
        productor.start();
        consumidor.start();
    }
}
```

---

## 9. Framework de Locks

### 9.1 ReentrantLock

`ReentrantLock` es más flexible que `synchronized`, ofreciendo funcionalidades avanzadas.

```java
import java.util.concurrent.locks.*;

public class EjemploReentrantLock {
    
    static class ContadorConLock {
        private int count = 0;
        private final Lock lock = new ReentrantLock();
        
        public void incrementar() {
            lock.lock(); // Adquirir lock
            try {
                count++;
            } finally {
                lock.unlock(); // Siempre liberar en finally
            }
        }
        
        public int getCount() {
            lock.lock();
            try {
                return count;
            } finally {
                lock.unlock();
            }
        }
    }
    
    // Características avanzadas de ReentrantLock
    static class CaracteristicasAvanzadas {
        private final Lock lock = new ReentrantLock();
        
        // tryLock - Intenta adquirir el lock sin bloquear
        public boolean intentarProcesar() {
            if (lock.tryLock()) {
                try {
                    System.out.println("Lock adquirido, procesando...");
                    Thread.sleep(1000);
                    return true;
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                    return false;
                } finally {
                    lock.unlock();
                }
            } else {
                System.out.println("No se pudo adquirir el lock");
                return false;
            }
        }
        
        // tryLock con timeout
        public boolean intentarConTimeout() {
            try {
                if (lock.tryLock(2, TimeUnit.SECONDS)) {
                    try {
                        System.out.println("Lock adquirido dentro del timeout");
                        return true;
                    } finally {
                        lock.unlock();
                    }
                } else {
                    System.out.println("Timeout alcanzado");
                    return false;
                }
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
                return false;
            }
        }
        
        // lockInterruptibly - Puede ser interrumpido mientras espera
        public void procesoInterrumpible() throws InterruptedException {
            lock.lockInterruptibly();
            try {
                System.out.println("Procesando (puede ser interrumpido)...");
                Thread.sleep(5000);
            } finally {
                lock.unlock();
            }
        }
    }
    
    public static void main(String[] args) throws InterruptedException {
        ContadorConLock contador = new ContadorConLock();
        
        Thread[] hilos = new Thread[10];
        for (int i = 0; i < 10; i++) {
            hilos[i] = new Thread(() -> {
                for (int j = 0; j < 1000; j++) {
                    contador.incrementar();
                }
            });
            hilos[i].start();
        }
        
        for (Thread hilo : hilos) {
            hilo.join();
        }
        
        System.out.println("Count final: " + contador.getCount());
    }
}
```

### 9.2 ReadWriteLock

Permite múltiples lectores O un solo escritor, optimizando para casos con más lecturas que escrituras.

```java
import java.util.concurrent.locks.*;
import java.util.*;

public class EjemploReadWriteLock {
    
    static class CacheCompartida {
        private final Map<String, String> cache = new HashMap<>();
        private final ReadWriteLock rwLock = new ReentrantReadWriteLock();
        private final Lock readLock = rwLock.readLock();
        private final Lock writeLock = rwLock.writeLock();
        
        // Múltiples hilos pueden leer simultáneamente
        public String leer(String key) {
            readLock.lock();
            try {
                System.out.println(Thread.currentThread().getName() + " leyendo...");
                Thread.sleep(100); // Simular lectura
                return cache.get(key);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
                return null;
            } finally {
                readLock.unlock();
            }
        }
        
        // Solo un hilo puede escribir a la vez
        public void escribir(String key, String value) {
            writeLock.lock();
            try {
                System.out.println(Thread.currentThread().getName() + " escribiendo...");
                Thread.sleep(200); // Simular escritura
                cache.put(key, value);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            } finally {
                writeLock.unlock();
            }
        }
        
        public int size() {
            readLock.lock();
            try {
                return cache.size();
            } finally {
                readLock.unlock();
            }
        }
    }
    
    public static void main(String[] args) throws InterruptedException {
        CacheCompartida cache = new CacheCompartida();
        
        // Escritores (pocos)
        for (int i = 0; i < 2; i++) {
            final int id = i;
            new Thread(() -> {
                for (int j = 0; j < 5; j++) {
                    cache.escribir("key" + j, "value" + id + "-" + j);
                }
            }, "Escritor-" + i).start();
        }
        
        // Lectores (muchos)
        for (int i = 0; i < 10; i++) {
            new Thread(() -> {
                for (int j = 0; j < 10; j++) {
                    String value = cache.leer("key" + (j % 5));
                    System.out.println(Thread.currentThread().getName() + 
                                     " obtuvo: " + value);
                }
            }, "Lector-" + i).start();
        }
        
        Thread.sleep(5000);
        System.out.println("Elementos en caché: " + cache.size());
    }
}
```

### 9.3 StampedLock (Java 8+)

Más eficiente que ReadWriteLock, ofrece lecturas optimistas.

```java
import java.util.concurrent.locks.StampedLock;

public class EjemploStampedLock {
    
    static class Punto {
        private double x, y;
        private final StampedLock lock = new StampedLock();
        
        public void mover(double deltaX, double deltaY) {
            long stamp = lock.writeLock(); // Escritura exclusiva
            try {
                x += deltaX;
                y += deltaY;
            } finally {
                lock.unlockWrite(stamp);
            }
        }
        
        public double distanciaDesdeOrigen() {
            // Lectura optimista (sin bloquear)
            long stamp = lock.tryOptimisticRead();
            double currentX = x;
            double currentY = y;
            
            // Validar que nadie escribió durante la lectura
            if (!lock.validate(stamp)) {
                // Si hubo escritura, obtener lock de lectura
                stamp = lock.readLock();
                try {
                    currentX = x;
                    currentY = y;
                } finally {
                    lock.unlockRead(stamp);
                }
            }
            
            return Math.sqrt(currentX * currentX + currentY * currentY);
        }
        
        // Convertir de read lock a write lock
        public void moverSiEsCero(double newX, double newY) {
            long stamp = lock.readLock();
            try {
                while (x == 0.0 && y == 0.0) {
                    // Intentar convertir a write lock
                    long ws = lock.tryConvertToWriteLock(stamp);
                    if (ws != 0L) {
                        stamp = ws;
                        x = newX;
                        y = newY;
                        break;
                    } else {
                        // No se pudo convertir, liberar y adquirir write lock
                        lock.unlockRead(stamp);
                        stamp = lock.writeLock();
                    }
                }
            } finally {
                lock.unlock(stamp);
            }
        }
    }
    
    public static void main(String[] args) throws InterruptedException {
        Punto punto = new Punto();
        
        // Múltiples lectores
        for (int i = 0; i < 5; i++) {
            new Thread(() -> {
                for (int j = 0; j < 10; j++) {
                    double distancia = punto.distanciaDesdeOrigen();
                    System.out.println(Thread.currentThread().getName() + 
                                     " - Distancia: " + distancia);
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        Thread.currentThread().interrupt();
                    }
                }
            }, "Lector-" + i).start();
        }
        
        // Pocos escritores
        for (int i = 0; i < 2; i++) {
            final int id = i;
            new Thread(() -> {
                for (int j = 0; j < 5; j++) {
                    punto.mover(1.0 * id, 1.0 * id);
                    try {
                        Thread.sleep(200);
                    } catch (InterruptedException e) {
                        Thread.currentThread().interrupt();
                    }
                }
            }, "Escritor-" + i).start();
        }
        
        Thread.sleep(3000);
    }
}
```

### 9.4 Condition

Permite wait/notify más flexible con múltiples condiciones.

```java
import java.util.concurrent.locks.*;
import java.util.*;

public class EjemploCondition {
    
    static class BoundedBuffer<T> {
        private final Queue<T> buffer = new LinkedList<>();
        private final int capacidad;
        private final Lock lock = new ReentrantLock();
        private final Condition notFull = lock.newCondition();
        private final Condition notEmpty = lock.newCondition();
        
        public BoundedBuffer(int capacidad) {
            this.capacidad = capacidad;
        }
        
        public void put(T item) throws InterruptedException {
            lock.lock();
            try {
                while (buffer.size() == capacidad) {
                    System.out.println("Buffer lleno, esperando...");
                    notFull.await(); // Esperar hasta que haya espacio
                }
                
                buffer.offer(item);
                System.out.println("Agregado: " + item + " (tamaño: " + buffer.size() + ")");
                notEmpty.signal(); // Notificar que hay elementos
            } finally {
                lock.unlock();
            }
        }
        
        public T take() throws InterruptedException {
            lock.lock();
            try {
                while (buffer.isEmpty()) {
                    System.out.println("Buffer vacío, esperando...");
                    notEmpty.await(); // Esperar hasta que haya elementos
                }
                
                T item = buffer.poll();
                System.out.println("Tomado: " + item + " (tamaño: " + buffer.size() + ")");
                notFull.signal(); // Notificar que hay espacio
                return item;
            } finally {
                lock.unlock();
            }
        }
    }
    
    public static void main(String[] args) {
        BoundedBuffer<Integer> buffer = new BoundedBuffer<>(5);
        
        // Productor
        Thread productor = new Thread(() -> {
            try {
                for (int i = 1; i <= 10; i++) {
                    buffer.put(i);
                    Thread.sleep(100);
                }
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }, "Productor");
        
        // Consumidor lento
        Thread consumidor = new Thread(() -> {
            try {
                for (int i = 1; i <= 10; i++) {
                    buffer.take();
                    Thread.sleep(300);
                }
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }, "Consumidor");
        
        productor.start();
        consumidor.start();
    }
}
```

**Comparación de Locks:**

| Característica | synchronized | ReentrantLock | ReadWriteLock | StampedLock |
|----------------|--------------|---------------|---------------|-------------|
| Simplicidad | ✅ Muy simple | ❌ Más verbose | ❌ Más complejo | ❌ Más complejo |
| tryLock | ❌ No | ✅ Sí | ✅ Sí | ✅ Sí |
| Timeout | ❌ No | ✅ Sí | ✅ Sí | ✅ Sí |
| Interruptible | ❌ No | ✅ Sí | ✅ Sí | ✅ Sí |
| Condiciones múltiples | ❌ No | ✅ Sí | ✅ Sí | ❌ No |
| Lecturas concurrentes | ❌ No | ❌ No | ✅ Sí | ✅ Sí |
| Lectura optimista | ❌ No | ❌ No | ❌ No | ✅ Sí |
| Rendimiento | Bueno | Bueno | Mejor para lecturas | Mejor para lecturas |

---

## 10. Variables Atómicas

### 10.1 ¿Qué son las Variables Atómicas?

Las clases atómicas del paquete `java.util.concurrent.atomic` proporcionan operaciones thread-safe sin usar locks.

```java
import java.util.concurrent.atomic.*;

public class VariablesAtomicas {
    
    // AtomicInteger
    static class ContadorAtomico {
        private AtomicInteger count = new AtomicInteger(0);
        
        public void incrementar() {
            count.incrementAndGet();
        }
        
        public void decrementar() {
            count.decrementAndGet();
        }
        
        public int get() {
            return count.get();
        }
        
        // Operaciones atómicas avanzadas
        public void ejemplosOperaciones() {
            count.set(10);                    // Establecer valor
            int valorAnterior = count.getAndSet(20); // Obtener y establecer
            int valorActual = count.get();     // Obtener valor actual
            
            count.addAndGet(5);                // Sumar y obtener nuevo valor
            count.getAndAdd(5);                // Obtener valor actual y sumar
            
            // Incrementar si es igual a un valor esperado
            boolean actualizado = count.compareAndSet(25, 30);
            
            // Actualizar con una función
            count.updateAndGet(n -> n * 2);
            count.accumulateAndGet(5, (current, update) -> current + update);
        }
    }
    
    public static void main(String[] args) throws InterruptedException {
        ContadorAtomico contador = new ContadorAtomico();
        
        Thread[] hilos = new Thread[10];
        for (int i = 0; i < 10; i++) {
            hilos[i] = new Thread(() -> {
                for (int j = 0; j < 1000; j++) {
                    contador.incrementar();
                }
            });
            hilos[i].start();
        }
        
        for (Thread hilo : hilos) {
            hilo.join();
        }
        
        System.out.println("Count final: " + contador.get()); // Siempre 10,000
    }
}
```

### 10.2 Tipos de Variables Atómicas

```java
import java.util.concurrent.atomic.*;

public class TiposAtomicos {
    
    public static void main(String[] args) {
        // 1. AtomicInteger
        AtomicInteger atomicInt = new AtomicInteger(0);
        atomicInt.incrementAndGet();
        atomicInt.compareAndSet(1, 10);
        
        // 2. AtomicLong
        AtomicLong atomicLong = new AtomicLong(0L);
        atomicLong.addAndGet(100L);
        
        // 3. AtomicBoolean
        AtomicBoolean atomicBool = new AtomicBoolean(false);
        atomicBool.compareAndSet(false, true);
        if (atomicBool.get()) {
            System.out.println("Es verdadero");
        }
        
        // 4. AtomicReference - para objetos
        AtomicReference<String> atomicRef = new AtomicReference<>("Inicial");
        atomicRef.set("Nuevo valor");
        atomicRef.compareAndSet("Nuevo valor", "Valor final");
        
        // 5. AtomicIntegerArray
        AtomicIntegerArray atomicArray = new AtomicIntegerArray(10);
        atomicArray.set(0, 100);
        atomicArray.incrementAndGet(0);
        
        // 6. AtomicStampedReference - con versionado
        AtomicStampedReference<String> stampedRef = 
            new AtomicStampedReference<>("Valor", 0);
        
        int[] stampHolder = new int[1];
        String valor = stampedRef.get(stampHolder);
        int stamp = stampHolder[0];
        
        stampedRef.compareAndSet(valor, "Nuevo", stamp, stamp + 1);
        
        // 7. LongAdder - Mejor rendimiento para contadores con alta concurrencia
        LongAdder adder = new LongAdder();
        adder.increment();
        adder.add(10);
        System.out.println("LongAdder sum: " + adder.sum());
        
        // 8. LongAccumulator
        LongAccumulator accumulator = new LongAccumulator(Long::sum, 0L);
        accumulator.accumulate(10);
        accumulator.accumulate(20);
        System.out.println("Accumulator: " + accumulator.get());
    }
}
```

### 10.3 Ejemplo Práctico: ID Generator Thread-Safe

```java
import java.util.concurrent.atomic.*;
import java.util.*;

public class IdGenerator {
    
    // Generador simple con AtomicLong
    static class SimpleIdGenerator {
        private final AtomicLong counter = new AtomicLong(0);
        
        public long nextId() {
            return counter.incrementAndGet();
        }
    }
    
    // Generador con prefijo y formato
    static class FormattedIdGenerator {
        private final AtomicLong counter = new AtomicLong(0);
        private final String prefix;
        
        public FormattedIdGenerator(String prefix) {
            this.prefix = prefix;
        }
        
        public String nextId() {
            long id = counter.incrementAndGet();
            return String.format("%s-%08d", prefix, id);
        }
    }
    
    // Generador distribuido con múltiples contadores
    static class DistributedIdGenerator {
        private final LongAdder[] shards;
        private final int shardCount;
        
        public DistributedIdGenerator(int shardCount) {
            this.shardCount = shardCount;
            this.shards = new LongAdder[shardCount];
            for (int i = 0; i < shardCount; i++) {
                shards[i] = new LongAdder();
            }
        }
        
        public long nextId() {
            int shardIndex = (int) (Thread.currentThread().getId() % shardCount);
            shards[shardIndex].increment();
            return getTotalCount();
        }
        
        private long getTotalCount() {
            long total = 0;
            for (LongAdder shard : shards) {
                total += shard.sum();
            }
            return total;
        }
    }
    
    public static void main(String[] args) throws InterruptedException {
        SimpleIdGenerator generator = new SimpleIdGenerator();
        Set<Long> ids = Collections.synchronizedSet(new HashSet<>());
        
        // Generar IDs concurrentemente
        Thread[] hilos = new Thread[10];
        for (int i = 0; i < 10; i++) {
            hilos[i] = new Thread(() -> {
                for (int j = 0; j < 1000; j++) {
                    long id = generator.nextId();
                    ids.add(id);
                }
            });
            hilos[i].start();
        }
        
        for (Thread hilo : hilos) {
            hilo.join();
        }
        
        System.out.println("IDs únicos generados: " + ids.size()); // Debe ser 10,000
        
        // Ejemplo con formato
        FormattedIdGenerator fmtGen = new FormattedIdGenerator("USR");
        for (int i = 0; i < 5; i++) {
            System.out.println("ID formateado: " + fmtGen.nextId());
        }
    }
}
```

### 10.4 Compare-And-Swap (CAS)

La operación fundamental detrás de las variables atómicas.

```java
import java.util.concurrent.atomic.*;

public class EjemploCAS {
    
    static class Stack<T> {
        private static class Node<T> {
            T value;
            Node<T> next;
            
            Node(T value) {
                this.value = value;
            }
        }
        
        private AtomicReference<Node<T>> top = new AtomicReference<>();
        
        // Push lock-free usando CAS
        public void push(T value) {
            Node<T> newNode = new Node<>(value);
            Node<T> oldTop;
            
            do {
                oldTop = top.get();
                newNode.next = oldTop;
            } while (!top.compareAndSet(oldTop, newNode));
            // Reintenta si otro hilo modificó top entre get() y compareAndSet()
        }
        
        // Pop lock-free usando CAS
        public T pop() {
            Node<T> oldTop;
            Node<T> newTop;
            
            do {
                oldTop = top.get();
                if (oldTop == null) {
                    return null;
                }
                newTop = oldTop.next;
            } while (!top.compareAndSet(oldTop, newTop));
            
            return oldTop.value;
        }
    }
    
    public static void main(String[] args) throws InterruptedException {
        Stack<Integer> stack = new Stack<>();
        
        // Múltiples hilos haciendo push
        Thread[] pushers = new Thread[5];
        for (int i = 0; i < 5; i++) {
            final int threadId = i;
            pushers[i] = new Thread(() -> {
                for (int j = 0; j < 10; j++) {
                    stack.push(threadId * 10 + j);
                }
            });
            pushers[i].start();
        }
        
        for (Thread t : pushers) {
            t.join();
        }
        
        // Un hilo haciendo pop
        Thread popper = new Thread(() -> {
            Integer value;
            int count = 0;
            while ((value = stack.pop()) != null) {
                System.out.println("Popped: " + value);
                count++;
            }
            System.out.println("Total popped: " + count);
        });
        
        popper.start();
        popper.join();
    }
}
```

**Cuándo usar Variables Atómicas:**
- ✅ Contadores simples
- ✅ Banderas compartidas
- ✅ Referencias que cambian infrecuentemente
- ✅ Cuando necesitas lock-free algorithms
- ✅ Mejor rendimiento que synchronized para operaciones simples
- ❌ Para operaciones complejas con múltiples pasos (usar locks)

---

## 11. Utilidades de Concurrencia

### 11.1 CountDownLatch

Permite que uno o más hilos esperen hasta que un conjunto de operaciones se complete.

```java
import java.util.concurrent.*;

public class EjemploCountDownLatch {
    
    public static void main(String[] args) throws InterruptedException {
        int numWorkers = 5;
        CountDownLatch latch = new CountDownLatch(numWorkers);
        
        System.out.println("Iniciando workers...");
        
        for (int i = 0; i < numWorkers; i++) {
            final int workerId = i;
            new Thread(() -> {
                try {
                    System.out.println("Worker " + workerId + " iniciando...");
                    Thread.sleep((long) (Math.random() * 3000));
                    System.out.println("Worker " + workerId + " completado");
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                } finally {
                    latch.countDown(); // Decrementar contador
                }
            }).start();
        }
        
        System.out.println("Esperando a que todos los workers terminen...");
        latch.await(); // Bloquear hasta que el contador llegue a 0
        
        System.out.println("¡Todos los workers han terminado!");
    }
}
```

**Ejemplo práctico: Sistema de inicialización**

```java
import java.util.concurrent.*;

public class SistemaInicializacion {
    
    static class Servicio {
        private String nombre;
        
        public Servicio(String nombre) {
            this.nombre = nombre;
        }
        
        public void inicializar() throws InterruptedException {
            System.out.println("Inicializando " + nombre + "...");
            Thread.sleep((long) (Math.random() * 2000));
            System.out.println(nombre + " inicializado ✓");
        }
    }
    
    public static void main(String[] args) throws InterruptedException {
        Servicio[] servicios = {
            new Servicio("Base de Datos"),
            new Servicio("Caché"),
            new Servicio("Servidor Web"),
            new Servicio("Sistema de Mensajería"),
            new Servicio("Monitoreo")
        };
        
        CountDownLatch latch = new CountDownLatch(servicios.length);
        
        System.out.println("🚀 Iniciando sistema...\n");
        
        for (Servicio servicio : servicios) {
            new Thread(() -> {
                try {
                    servicio.inicializar();
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                } finally {
                    latch.countDown();
                }
            }).start();
        }
        
        // Esperar a que todos los servicios se inicialicen
        latch.await();
        
        System.out.println("\n✅ Sistema completamente inicializado y listo");
    }
}
```

### 11.2 CyclicBarrier

Permite que un grupo de hilos se esperen mutuamente en un punto común (barrera).

```java
import java.util.concurrent.*;

public class EjemploCyclicBarrier {
    
    public static void main(String[] args) {
        int numThreads = 4;
        
        // Acción que se ejecuta cuando todos los hilos llegan a la barrera
        Runnable barrierAction = () -> {
            System.out.println("\n=== Todos los hilos han llegado a la barrera ===\n");
        };
        
        CyclicBarrier barrier = new CyclicBarrier(numThreads, barrierAction);
        
        for (int i = 0; i < numThreads; i++) {
            final int threadId = i;
            new Thread(() -> {
                try {
                    for (int fase = 1; fase <= 3; fase++) {
                        System.out.println("Hilo " + threadId + " - Fase " + fase);
                        Thread.sleep((long) (Math.random() * 2000));
                        
                        System.out.println("Hilo " + threadId + 
                                         " esperando en barrera (fase " + fase + ")");
                        barrier.await(); // Esperar a todos
                        
                        System.out.println("Hilo " + threadId + 
                                         " continuando después de fase " + fase);
                    }
                } catch (InterruptedException | BrokenBarrierException e) {
                    e.printStackTrace();
                }
            }).start();
        }
    }
}
```

**Ejemplo práctico: Simulación de carreras**

```java
import java.util.concurrent.*;

public class SimulacionCarrera {
    
    static class Corredor implements Runnable {
        private int id;
        private CyclicBarrier barrier;
        
        public Corredor(int id, CyclicBarrier barrier) {
            this.id = id;
            this.barrier = barrier;
        }
        
        @Override
        public void run() {
            try {
                System.out.println("Corredor " + id + " preparándose...");
                Thread.sleep((long) (Math.random() * 2000));
                
                System.out.println("Corredor " + id + " listo en la línea de salida");
                barrier.await(); // Esperar a que todos estén listos
                
                // ¡Carrera!
                System.out.println("Corredor " + id + " ¡CORRIENDO!");
                long tiempoCarrera = (long) (3000 + Math.random() * 5000);
                Thread.sleep(tiempoCarrera);
                
                System.out.println("Corredor " + id + 
                                 " ha terminado en " + tiempoCarrera + "ms");
                
            } catch (InterruptedException | BrokenBarrierException e) {
                e.printStackTrace();
            }
        }
    }
    
    public static void main(String[] args) {
        int numCorredores = 5;
        
        Runnable inicioCarrera = () -> {
            System.out.println("\n🏁 ¡EN SUS MARCAS, LISTOS, FUERA! 🏁\n");
        };
        
        CyclicBarrier barrier = new CyclicBarrier(numCorredores, inicioCarrera);
        
        for (int i = 1; i <= numCorredores; i++) {
            new Thread(new Corredor(i, barrier)).start();
        }
    }
}
```

### 11.3 Semaphore

Controla el acceso a un recurso mediante un conjunto de permisos.

```java
import java.util.concurrent.*;

public class EjemploSemaphore {
    
    public static void main(String[] args) {
        // Semáforo con 3 permisos (máximo 3 hilos simultáneos)
        Semaphore semaphore = new Semaphore(3);
        
        for (int i = 1; i <= 10; i++) {
            final int userId = i;
            new Thread(() -> {
                try {
                    System.out.println("Usuario " + userId + " esperando acceso...");
                    semaphore.acquire(); // Adquirir permiso (bloquea si no hay)
                    
                    System.out.println("✓ Usuario " + userId + " accedió al recurso");
                    Thread.sleep(2000); // Usar el recurso
                    
                    System.out.println("Usuario " + userId + " liberó el recurso");
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                } finally {
                    semaphore.release(); // Liberar permiso
                }
            }).start();
        }
    }
}
```

**Ejemplo práctico: Pool de conexiones a base de datos**

```java
import java.util.concurrent.*;

public class ConnectionPool {
    
    static class DBConnection {
        private int id;
        
        public DBConnection(int id) {
            this.id = id;
        }
        
        public void executeQuery(String query) {
            System.out.println("  [Conexión " + id + "] Ejecutando: " + query);
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
        
        public int getId() {
            return id;
        }
    }
    
    private final DBConnection[] connections;
    private final Semaphore semaphore;
    private final boolean[] available;
    
    public ConnectionPool(int poolSize) {
        connections = new DBConnection[poolSize];
        available = new boolean[poolSize];
        semaphore = new Semaphore(poolSize, true); // fair=true para FIFO
        
        for (int i = 0; i < poolSize; i++) {
            connections[i] = new DBConnection(i + 1);
            available[i] = true;
        }
    }
    
    public DBConnection getConnection() throws InterruptedException {
        semaphore.acquire();
        return getNextAvailableConnection();
    }
    
    private synchronized DBConnection getNextAvailableConnection() {
        for (int i = 0; i < available.length; i++) {
            if (available[i]) {
                available[i] = false;
                return connections[i];
            }
        }
        return null; // No debería llegar aquí
    }
    
    public void releaseConnection(DBConnection connection) {
        if (markAsAvailable(connection)) {
            semaphore.release();
        }
    }
    
    private synchronized boolean markAsAvailable(DBConnection connection) {
        for (int i = 0; i < connections.length; i++) {
            if (connections[i] == connection) {
                available[i] = true;
                return true;
            }
        }
        return false;
    }
    
    public static void main(String[] args) {
        ConnectionPool pool = new ConnectionPool(3); // Pool con 3 conexiones
        
        // 10 clientes intentando acceder
        for (int i = 1; i <= 10; i++) {
            final int clientId = i;
            new Thread(() -> {
                try {
                    System.out.println("Cliente " + clientId + " solicitando conexión...");
                    DBConnection conn = pool.getConnection();
                    
                    System.out.println("Cliente " + clientId + 
                                     " obtuvo conexión " + conn.getId());
                    conn.executeQuery("SELECT * FROM users WHERE id=" + clientId);
                    
                    System.out.println("Cliente " + clientId + 
                                     " liberando conexión " + conn.getId());
                    pool.releaseConnection(conn);
                    
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }).start();
        }
    }
}
```

### 11.4 Phaser

Similar a CyclicBarrier pero más flexible, permite registrar/desregistrar participantes dinámicamente.

```java
import java.util.concurrent.*;

public class EjemploPhaser {
    
    static class Tarea implements Runnable {
        private int id;
        private Phaser phaser;
        
        public Tarea(int id, Phaser phaser) {
            this.id = id;
            this.phaser = phaser;
        }
        
        @Override
        public void run() {
            System.out.println("Tarea " + id + " - Fase 1: Inicialización");
            sleep(1000);
            phaser.arriveAndAwaitAdvance(); // Esperar fase 1
            
            System.out.println("Tarea " + id + " - Fase 2: Procesamiento");
            sleep(1500);
            phaser.arriveAndAwaitAdvance(); // Esperar fase 2
            
            System.out.println("Tarea " + id + " - Fase 3: Finalización");
            sleep(1000);
            phaser.arriveAndDeregister(); // Finalizar y desregistrarse
            
            System.out.println("Tarea " + id + " completada");
        }
        
        private void sleep(long millis) {
            try {
                Thread.sleep(millis);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
    }
    
    public static void main(String[] args) {
        // Crear phaser con 1 participante (el hilo principal)
        Phaser phaser = new Phaser(1) {
            @Override
            protected boolean onAdvance(int phase, int registeredParties) {
                System.out.println("\n=== Fase " + phase + " completada ===\n");
                return registeredParties == 0; // Terminar cuando no haya participantes
            }
        };
        
        // Registrar y lanzar 3 tareas
        for (int i = 1; i <= 3; i++) {
            phaser.register(); // Registrar nuevo participante
            new Thread(new Tarea(i, phaser)).start();
        }
        
        // El hilo principal también participa
        phaser.arriveAndAwaitAdvance(); // Fase 1
        System.out.println("Hilo principal después de fase 1");
        
        phaser.arriveAndAwaitAdvance(); // Fase 2
        System.out.println("Hilo principal después de fase 2");
        
        phaser.arriveAndDeregister(); // Desregistrarse
        System.out.println("Hilo principal finalizado");
    }
}
```

### 11.5 Exchanger

Permite a dos hilos intercambiar objetos en un punto de sincronización.

```java
import java.util.concurrent.*;

public class EjemploExchanger {
    
    public static void main(String[] args) {
        Exchanger<String> exchanger = new Exchanger<>();
        
        // Productor
        new Thread(() -> {
            try {
                String data = "Datos del productor";
                System.out.println("Productor tiene: " + data);
                
                String received = exchanger.exchange(data);
                
                System.out.println("Productor recibió: " + received);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }).start();
        
        // Consumidor
        new Thread(() -> {
            try {
                Thread.sleep(1000); // Simular procesamiento
                
                String data = "Datos del consumidor";
                System.out.println("Consumidor tiene: " + data);
                
                String received = exchanger.exchange(data);
                
                System.out.println("Consumidor recibió: " + received);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }).start();
    }
}
```

**Comparación de Utilidades:**

| Utilidad | Propósito | Reutilizable | Número de Hilos |
|----------|-----------|--------------|-----------------|
| **CountDownLatch** | Esperar hasta que N operaciones completen | ❌ No | N fijo |
| **CyclicBarrier** | Sincronizar hilos en un punto | ✅ Sí | N fijo |
| **Semaphore** | Controlar acceso a recursos | ✅ Sí | Configurable |
| **Phaser** | Sincronización de múltiples fases | ✅ Sí | Dinámico |
| **Exchanger** | Intercambiar datos entre dos hilos | ✅ Sí | 2 exactamente |

---

## 12. Colecciones Thread-Safe

### 12.1 Colecciones Concurrentes

```java
import java.util.concurrent.*;
import java.util.*;

public class ColeccionesConcurrentes {
    
    public static void main(String[] args) throws InterruptedException {
        
        // 1. ConcurrentHashMap - Mapa thread-safe de alto rendimiento
        ConcurrentHashMap<String, Integer> map = new ConcurrentHashMap<>();
        map.put("A", 1);
        map.putIfAbsent("A", 2); // No sobreescribe si ya existe
        map.computeIfAbsent("B", k -> 10); // Calcula si no existe
        map.merge("A", 5, Integer::sum); // Fusiona valores
        
        // 2. CopyOnWriteArrayList - Lista optimizada para lecturas
        CopyOnWriteArrayList<String> list = new CopyOnWriteArrayList<>();
        list.add("Elemento 1");
        list.add("Elemento 2");
        // Las iteraciones nunca lanzan ConcurrentModificationException
        
        // 3. CopyOnWriteArraySet - Set optimizado para lecturas
        CopyOnWriteArraySet<String> set = new CopyOnWriteArraySet<>();
        set.add("A");
        set.add("B");
        
        // 4. ConcurrentLinkedQueue - Cola no bloqueante
        ConcurrentLinkedQueue<String> queue = new ConcurrentLinkedQueue<>();
        queue.offer("Primero");
        queue.offer("Segundo");
        String elemento = queue.poll();
        
        // 5. BlockingQueue - Cola bloqueante
        BlockingQueue<String> blockingQueue = new LinkedBlockingQueue<>(10);
        blockingQueue.put("Item"); // Bloquea si está llena
        String item = blockingQueue.take(); // Bloquea si está vacía
        
        // 6. ConcurrentSkipListMap - Mapa ordenado thread-safe
        ConcurrentSkipListMap<Integer, String> skipListMap = 
            new ConcurrentSkipListMap<>();
        skipListMap.put(3, "Tres");
        skipListMap.put(1, "Uno");
        skipListMap.put(2, "Dos");
        // Mantiene orden: {1=Uno, 2=Dos, 3=Tres}
        
        // 7. ConcurrentSkipListSet - Set ordenado thread-safe
        ConcurrentSkipListSet<Integer> skipListSet = new ConcurrentSkipListSet<>();
        skipListSet.add(5);
        skipListSet.add(1);
        skipListSet.add(3);
        // Mantiene orden: [1, 3, 5]
    }
}
```

### 12.2 BlockingQueue - Patrón Productor-Consumidor

```java
import java.util.concurrent.*;

public class ProducerConsumerConBlockingQueue {
    
    static class Productor implements Runnable {
        private BlockingQueue<Integer> queue;
        private int maxItems;
        
        public Productor(BlockingQueue<Integer> queue, int maxItems) {
            this.queue = queue;
            this.maxItems = maxItems;
        }
        
        @Override
        public void run() {
            try {
                for (int i = 1; i <= maxItems; i++) {
                    System.out.println("Produciendo: " + i);
                    queue.put(i); // Bloquea si la cola está llena
                    Thread.sleep(100);
                }
                queue.put(-1); // Señal de finalización
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
    }
    
    static class Consumidor implements Runnable {
        private BlockingQueue<Integer> queue;
        
        public Consumidor(BlockingQueue<Integer> queue) {
            this.queue = queue;
        }
        
        @Override
        public void run() {
            try {
                while (true) {
                    Integer item = queue.take(); // Bloquea si la cola está vacía
                    if (item == -1) {
                        break; // Señal de finalización
                    }
                    System.out.println("  Consumiendo: " + item);
                    Thread.sleep(200);
                }
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
    }
    
    public static void main(String[] args) {
        BlockingQueue<Integer> queue = new ArrayBlockingQueue<>(5);
        
        Thread productor = new Thread(new Productor(queue, 20));
        Thread consumidor = new Thread(new Consumidor(queue));
        
        productor.start();
        consumidor.start();
    }
}
```

### 12.3 Tipos de BlockingQueue

```java
import java.util.concurrent.*;

public class TiposBlockingQueue {
    
    public static void main(String[] args) throws InterruptedException {
        
        // 1. ArrayBlockingQueue - Tamaño fijo, basado en array
        BlockingQueue<String> arrayQueue = new ArrayBlockingQueue<>(10);
        arrayQueue.put("Item 1");
        
        // 2. LinkedBlockingQueue - Tamaño opcional, basado en lista enlazada
        BlockingQueue<String> linkedQueue = new LinkedBlockingQueue<>();
        linkedQueue.put("Item 2");
        
        // 3. PriorityBlockingQueue - Cola con prioridad
        BlockingQueue<Integer> priorityQueue = new PriorityBlockingQueue<>();
        priorityQueue.put(5);
        priorityQueue.put(1);
        priorityQueue.put(3);
        System.out.println(priorityQueue.take()); // 1 (menor tiene prioridad)
        
        // 4. DelayQueue - Elementos solo disponibles después de un delay
        DelayQueue<DelayedTask> delayQueue = new DelayQueue<>();
        delayQueue.put(new DelayedTask("Tarea 1", 2000));
        delayQueue.put(new DelayedTask("Tarea 2", 1000));
        
        DelayedTask task = delayQueue.take(); // Espera hasta que esté disponible
        System.out.println("Primera tarea lista: " + task.getName());
        
        // 5. SynchronousQueue - Sin capacidad, transfiere directo
        BlockingQueue<String> syncQueue = new SynchronousQueue<>();
        
        new Thread(() -> {
            try {
                System.out.println("Esperando para poner...");
                syncQueue.put("Directo"); // Bloquea hasta que alguien tome
                System.out.println("Puesto!");
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }).start();
        
        Thread.sleep(1000);
        String item = syncQueue.take();
        System.out.println("Tomado: " + item);
    }
    
    static class DelayedTask implements Delayed {
        private String name;
        private long startTime;
        
        public DelayedTask(String name, long delayMillis) {
            this.name = name;
            this.startTime = System.currentTimeMillis() + delayMillis;
        }
        
        @Override
        public long getDelay(TimeUnit unit) {
            long diff = startTime - System.currentTimeMillis();
            return unit.convert(diff, TimeUnit.MILLISECONDS);
        }
        
        @Override
        public int compareTo(Delayed o) {
            return Long.compare(this.startTime, ((DelayedTask) o).startTime);
        }
        
        public String getName() {
            return name;
        }
    }
}
```

### 12.4 ConcurrentHashMap Avanzado

```java
import java.util.concurrent.*;

public class ConcurrentHashMapAvanzado {
    
    public static void main(String[] args) {
        ConcurrentHashMap<String, Integer> map = new ConcurrentHashMap<>();
        
        // 1. Operaciones atómicas básicas
        map.put("A", 1);
        map.putIfAbsent("A", 2); // No cambia, ya existe
        System.out.println(map.get("A")); // 1
        
        // 2. computeIfAbsent - Calcula y guarda solo si no existe
        map.computeIfAbsent("B", k -> {
            System.out.println("Calculando valor para B");
            return 10;
        });
        
        // 3. computeIfPresent - Calcula nuevo valor solo si existe
        map.computeIfPresent("A", (k, v) -> v + 100);
        System.out.println(map.get("A")); // 101
        
        // 4. compute - Calcula siempre (existe o no)
        map.compute("C", (k, v) -> v == null ? 1 : v + 1);
        
        // 5. merge - Fusiona valores
        map.merge("A", 50, Integer::sum);
        System.out.println(map.get("A")); // 151
        
        // 6. replace - Reemplaza solo si existe
        map.replace("A", 200);
        
        // 7. replace con valor esperado
        boolean replaced = map.replace("A", 200, 300);
        System.out.println("Reemplazado: " + replaced); // true
        
        // 8. remove con valor esperado
        boolean removed = map.remove("B", 10);
        System.out.println("Removido: " + removed); // true
        
        // 9. forEach paralelo
        ConcurrentHashMap<String, Integer> bigMap = new ConcurrentHashMap<>();
        for (int i = 0; i < 1000; i++) {
            bigMap.put("key" + i, i);
        }
        
        bigMap.forEach(100, (k, v) -> {
            // Procesar en paralelo si hay suficientes elementos
            System.out.println(k + " -> " + v);
        });
        
        // 10. reduce paralelo
        int sum = bigMap.reduceValues(100, Integer::sum);
        System.out.println("Suma total: " + sum);
        
        // 11. search paralelo
        String found = bigMap.search(100, (k, v) -> {
            return v > 500 ? k : null;
        });
        System.out.println("Primera clave con valor > 500: " + found);
    }
}
```

**Cuándo usar cada colección:**

| Colección | Uso recomendado |
|-----------|-----------------|
| **ConcurrentHashMap** | Mapa de uso general con alta concurrencia |
| **CopyOnWriteArrayList** | Muchas lecturas, pocas escrituras |
| **CopyOnWriteArraySet** | Set con muchas lecturas, pocas escrituras |
| **ConcurrentLinkedQueue** | Cola FIFO sin bloqueo |
| **LinkedBlockingQueue** | Productor-consumidor con capacidad ilimitada |
| **ArrayBlockingQueue** | Productor-consumidor con capacidad fija |
| **PriorityBlockingQueue** | Cola con prioridad thread-safe |
| **ConcurrentSkipListMap** | Mapa ordenado concurrente |

---

## 13. Virtual Threads (Java 21+)

### 13.1 ¿Qué son los Virtual Threads?

Los **Virtual Threads** (Project Loom) son hilos ligeros gestionados por la JVM, no por el sistema operativo. Permiten crear millones de hilos con bajo overhead.

**Diferencias con Platform Threads:**
- Platform Threads: 1:1 mapeo con hilos del SO, pesados (~1MB stack)
- Virtual Threads: Muchos virtual threads pueden ejecutarse en pocos platform threads, livianos (~1KB)

```java
public class EjemploVirtualThreads {
    
    public static void main(String[] args) throws InterruptedException {
        
        // 1. Crear Virtual Thread con Thread.ofVirtual()
        Thread vThread1 = Thread.ofVirtual().start(() -> {
            System.out.println("Virtual Thread 1: " + Thread.currentThread());
        });
        
        vThread1.join();
        
        // 2. Crear con Thread.startVirtualThread()
        Thread vThread2 = Thread.startVirtualThread(() -> {
            System.out.println("Virtual Thread 2: " + Thread.currentThread());
        });
        
        vThread2.join();
        
        // 3. Crear con factory
        ThreadFactory factory = Thread.ofVirtual().factory();
        Thread vThread3 = factory.newThread(() -> {
            System.out.println("Virtual Thread 3 from factory");
        });
        vThread3.start();
        vThread3.join();
        
        // 4. Usar con ExecutorService
        try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
            for (int i = 0; i < 100; i++) {
                final int taskId = i;
                executor.submit(() -> {
                    System.out.println("Task " + taskId + " en " + 
                                     Thread.currentThread());
                    return taskId;
                });
            }
        } // Cierre automático espera a que todas las tareas terminen
    }
}
```

### 13.2 Creando Millones de Virtual Threads

```java
import java.time.Duration;
import java.util.concurrent.*;
import java.util.stream.IntStream;

public class MillonesDeVirtualThreads {
    
    public static void main(String[] args) throws InterruptedException {
        
        // Intentar esto con platform threads agotaría la memoria
        int numThreads = 1_000_000;
        
        long inicio = System.currentTimeMillis();
        
        try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
            IntStream.range(0, numThreads)
                .forEach(i -> executor.submit(() -> {
                    try {
                        Thread.sleep(Duration.ofSeconds(1));
                    } catch (InterruptedException e) {
                        Thread.currentThread().interrupt();
                    }
                    return i;
                }));
        }
        
        long fin = System.currentTimeMillis();
        
        System.out.println("Ejecutados " + numThreads + " virtual threads");
        System.out.println("Tiempo total: " + (fin - inicio) + "ms");
        // Con virtual threads: ~1-2 segundos
        // Con platform threads: OutOfMemoryError
    }
}
```

### 13.3 Servidor HTTP con Virtual Threads

```java
import java.io.*;
import java.net.*;
import java.util.concurrent.*;

public class ServidorHTTPVirtual {
    
    private static final int PORT = 8080;
    
    public static void main(String[] args) throws IOException {
        try (ServerSocket serverSocket = new ServerSocket(PORT);
             var executor = Executors.newVirtualThreadPerTaskExecutor()) {
            
            System.out.println("Servidor iniciado en puerto " + PORT);
            
            while (true) {
                Socket clientSocket = serverSocket.accept();
                
                // Crear un virtual thread por cada conexión
                executor.submit(() -> handleClient(clientSocket));
            }
        }
    }
    
    private static void handleClient(Socket socket) {
        try (BufferedReader in = new BufferedReader(
                new InputStreamReader(socket.getInputStream()));
             PrintWriter out = new PrintWriter(socket.getOutputStream(), true)) {
            
            // Leer request
            String requestLine = in.readLine();
            System.out.println("Request en " + Thread.currentThread() + 
                             ": " + requestLine);
            
            // Simular procesamiento (I/O, DB query, etc.)
            Thread.sleep(100);
            
            // Enviar response
            out.println("HTTP/1.1 200 OK");
            out.println("Content-Type: text/plain");
            out.println();
            out.println("Hello from Virtual Thread: " + Thread.currentThread());
            
        } catch (IOException | InterruptedException e) {
            e.printStackTrace();
        } finally {
            try {
                socket.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

### 13.4 Mejores Prácticas con Virtual Threads

```java
public class VirtualThreadsBestPractices {
    
    // ✅ BUENO: Usar para tareas I/O-bound
    public void goodUseCase() {
        try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
            executor.submit(() -> {
                // Llamada a API externa
                // Consulta a base de datos
                // Lectura de archivos
                return fetchDataFromAPI();
            });
        }
    }
    
    // ❌ MALO: No usar synchronized con virtual threads
    private final Object lock = new Object();
    
    public void badPractice() {
        Thread.startVirtualThread(() -> {
            synchronized (lock) {
                // Bloquea el carrier thread!
                // Reduce el beneficio de virtual threads
                doSomething();
            }
        });
    }
    
    // ✅ BUENO: Usar ReentrantLock en su lugar
    private final ReentrantLock reentrantLock = new ReentrantLock();
    
    public void goodPractice() {
        Thread.startVirtualThread(() -> {
            reentrantLock.lock();
            try {
                // No bloquea el carrier thread
                doSomething();
            } finally {
                reentrantLock.unlock();
            }
        });
    }
    
    // ❌ MALO: ThreadLocal con muchos virtual threads
    private static final ThreadLocal<ExpensiveResource> threadLocal = 
        ThreadLocal.withInitial(ExpensiveResource::new);
    
    public void avoidThreadLocal() {
        // Con millones de virtual threads, cada uno tendría su copia
        // Puede consumir mucha memoria
        Thread.startVirtualThread(() -> {
            ExpensiveResource resource = threadLocal.get();
            resource.use();
        });
    }
    
    // ✅ BUENO: Usar ScopedValue en su lugar (Java 21+)
    private static final ScopedValue<ExpensiveResource> scopedValue = 
        ScopedValue.newInstance();
    
    public void useScopedValue() {
        ExpensiveResource resource = new ExpensiveResource();
        ScopedValue.where(scopedValue, resource).run(() -> {
            // Uso más eficiente
            scopedValue.get().use();
        });
    }
    
    private String fetchDataFromAPI() {
        return "data";
    }
    
    private void doSomething() {
        // Implementación
    }
    
    static class ExpensiveResource {
        void use() {}
    }
}
```

### 13.5 Structured Concurrency (Java 21+)

```java
import java.util.concurrent.*;

public class StructuredConcurrencyExample {
    
    static class User {
        String name;
        User(String name) { this.name = name; }
    }
    
    static class Order {
        int total;
        Order(int total) { this.total = total; }
    }
    
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        
        String userId = "USER123";
        
        // Structured Concurrency asegura que todas las subtareas se completen
        try (var scope = new StructuredTaskScope.ShutdownOnFailure()) {
            
            // Lanzar múltiples tareas concurrentes
            Subtask<User> userTask = scope.fork(() -> fetchUser(userId));
            Subtask<Order> orderTask = scope.fork(() -> fetchOrder(userId));
            
            // Esperar a que ambas completen
            scope.join();
            scope.throwIfFailed(); // Lanza excepción si alguna falló
            
            // Obtener resultados
            User user = userTask.get();
            Order order = orderTask.get();
            
            System.out.println("Usuario: " + user.name);
            System.out.println("Total de órdenes: " + order.total);
        }
    }
    
    // Cancela todas las demás si una tiene éxito
    public static String findFirstResult(List<String> urls) 
            throws ExecutionException, InterruptedException {
        
        try (var scope = new StructuredTaskScope.ShutdownOnSuccess<String>()) {
            
            for (String url : urls) {
                scope.fork(() -> fetchFromURL(url));
            }
            
            scope.join();
            
            return scope.result(); // Retorna el primer resultado exitoso
        }
    }
    
    static User fetchUser(String userId) throws InterruptedException {
        Thread.sleep(1000);
        return new User("Juan Pérez");
    }
    
    static Order fetchOrder(String userId) throws InterruptedException {
        Thread.sleep(1500);
        return new Order(500);
    }
    
    static String fetchFromURL(String url) throws InterruptedException {
        Thread.sleep((long) (Math.random() * 2000));
        return "Data from " + url;
    }
}
```

**Cuándo usar Virtual Threads:**
- ✅ Aplicaciones con muchas operaciones I/O concurrentes
- ✅ Servidores web, microservicios
- ✅ Procesamiento de muchas requests simultáneas
- ✅ Cuando necesitas escalar a millones de operaciones concurrentes
- ❌ Tareas CPU-intensive (usar ForkJoinPool)
- ❌ Código que usa mucho synchronized (migrar a Locks)

---

## 14. Patrones y Mejores Prácticas

### 14.1 Thread Pool Sizing

```java
public class ThreadPoolSizing {
    
    public static int calcularTamañoOptimo() {
        int numCores = Runtime.getRuntime().availableProcessors();
        
        // Para tareas CPU-bound
        int cpuBound = numCores + 1;
        
        // Para tareas I/O-bound
        // Fórmula: numCores / (1 - factorBloqueo)
        // factorBloqueo = tiempoEspera / (tiempoEspera + tiempoComputo)
        // Ej: si 90% del tiempo es I/O, factorBloqueo = 0.9
        double factorBloqueo = 0.9;
        int ioBound = (int) (numCores / (1 - factorBloqueo));
        
        System.out.println("Núcleos disponibles: " + numCores);
        System.out.println("Tamaño para CPU-bound: " + cpuBound);
        System.out.println("Tamaño para I/O-bound: " + ioBound);
        
        return ioBound;
    }
    
    public static ExecutorService crearPoolOptimizado(String tipoTarea) {
        int numCores = Runtime.getRuntime().availableProcessors();
        
        switch (tipoTarea.toLowerCase()) {
            case "cpu":
                return Executors.newFixedThreadPool(numCores + 1);
            
            case "io":
                return Executors.newFixedThreadPool(numCores * 2);
            
            case "mixed":
                return Executors.newCachedThreadPool();
            
            default:
                return Executors.newFixedThreadPool(numCores);
        }
    }
}
```

### 14.2 Graceful Shutdown

```java
import java.util.concurrent.*;

public class GracefulShutdown {
    
    public static void shutdownGracefully(ExecutorService executor) {
        executor.shutdown(); // No acepta nuevas tareas
        
        try {
            // Esperar 60 segundos a que las tareas actuales terminen
            if (!executor.awaitTermination(60, TimeUnit.SECONDS)) {
                // Si no terminan, forzar cierre
                executor.shutdownNow();
                
                // Esperar un poco más
                if (!executor.awaitTermination(60, TimeUnit.SECONDS)) {
                    System.err.println("El executor no se cerró correctamente");
                }
            }
        } catch (InterruptedException e) {
            // Si este hilo es interrumpido, forzar cierre
            executor.shutdownNow();
            Thread.currentThread().interrupt();
        }
    }
    
    // Ejemplo con shutdown hook
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(4);
        
        // Registrar hook para cierre ordenado
        Runtime.getRuntime().addShutdownHook(new Thread(() -> {
            System.out.println("Cerrando executor...");
            shutdownGracefully(executor);
            System.out.println("Executor cerrado");
        }));
        
        // Ejecutar tareas
        for (int i = 0; i < 10; i++) {
            final int taskId = i;
            executor.submit(() -> {
                System.out.println("Tarea " + taskId);
                try {
                    Thread.sleep(5000);
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            });
        }
    }
}
```

### 14.3 Error Handling en Concurrencia

```java
import java.util.concurrent.*;

public class ErrorHandling {
    
    // 1. Capturar excepciones en Future
    public static void handleFutureErrors() {
        ExecutorService executor = Executors.newSingleThreadExecutor();
        
        Future<Integer> future = executor.submit(() -> {
            if (Math.random() > 0.5) {
                throw new RuntimeException("Error aleatorio");
            }
            return 42;
        });
        
        try {
            Integer result = future.get();
            System.out.println("Resultado: " + result);
        } catch (ExecutionException e) {
            System.err.println("Error en la tarea: " + e.getCause().getMessage());
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
            System.err.println("Interrumpido");
        }
        
        executor.shutdown();
    }
    
    // 2. UncaughtExceptionHandler
    public static void handleUncaughtExceptions() {
        Thread.UncaughtExceptionHandler handler = (thread, throwable) -> {
            System.err.println("Excepción no capturada en " + thread.getName());
            System.err.println("Error: " + throwable.getMessage());
            throwable.printStackTrace();
        };
        
        Thread thread = new Thread(() -> {
            throw new RuntimeException("Error fatal");
        });
        
        thread.setUncaughtExceptionHandler(handler);
        thread.start();
    }
    
    // 3. ThreadFactory con handler por defecto
    public static ExecutorService crearExecutorConErrorHandler() {
        ThreadFactory factory = r -> {
            Thread thread = new Thread(r);
            thread.setUncaughtExceptionHandler((t, e) -> {
                System.err.println("Error en thread " + t.getName() + ": " + e);
            });
            return thread;
        };
        
        return Executors.newFixedThreadPool(4, factory);
    }
    
    // 4. CompletableFuture con error handling
    public static void handleCompletableFutureErrors() {
        CompletableFuture.supplyAsync(() -> {
            if (Math.random() > 0.5) {
                throw new RuntimeException("Fallo en operación");
            }
            return "Éxito";
        })
        .exceptionally(ex -> {
            System.err.println("Error: " + ex.getMessage());
            return "Valor por defecto";
        })
        .thenAccept(result -> {
            System.out.println("Resultado: " + result);
        });
    }
}
```

### 14.4 Deadlock Prevention

```java
import java.util.concurrent.locks.*;

public class DeadlockPrevention {
    
    // ❌ MALO: Puede causar deadlock
    static class BadExample {
        private final Object lock1 = new Object();
        private final Object lock2 = new Object();
        
        public void metodo1() {
            synchronized (lock1) {
                synchronized (lock2) {
                    // Trabajo
                }
            }
        }
        
        public void metodo2() {
            synchronized (lock2) {  // Orden diferente!
                synchronized (lock1) {
                    // Trabajo
                }
            }
        }
    }
    
    // ✅ BUENO: Siempre adquirir locks en el mismo orden
    static class GoodExample {
        private final Object lock1 = new Object();
        private final Object lock2 = new Object();
        
        public void metodo1() {
            synchronized (lock1) {
                synchronized (lock2) {
                    // Trabajo
                }
            }
        }
        
        public void metodo2() {
            synchronized (lock1) {  // Mismo orden
                synchronized (lock2) {
                    // Trabajo
                }
            }
        }
    }
    
    // ✅ MEJOR: Usar tryLock con timeout
    static class BestExample {
        private final Lock lock1 = new ReentrantLock();
        private final Lock lock2 = new ReentrantLock();
        
        public boolean transferir() {
            boolean lock1Acquired = false;
            boolean lock2Acquired = false;
            
            try {
                lock1Acquired = lock1.tryLock(1, TimeUnit.SECONDS);
                if (!lock1Acquired) {
                    return false;
                }
                
                lock2Acquired = lock2.tryLock(1, TimeUnit.SECONDS);
                if (!lock2Acquired) {
                    return false;
                }
                
                // Hacer transferencia
                return true;
                
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
                return false;
            } finally {
                if (lock2Acquired) lock2.unlock();
                if (lock1Acquired) lock1.unlock();
            }
        }
    }
}
```

### 14.5 Immutable Objects para Thread Safety

```java
public final class ImmutablePerson {
    private final String name;
    private final int age;
    private final List<String> hobbies;
    
    public ImmutablePerson(String name, int age, List<String> hobbies) {
        this.name = name;
        this.age = age;
        // Copiar lista para prevenir modificaciones externas
        this.hobbies = new ArrayList<>(hobbies);
    }
    
    public String getName() {
        return name;
    }
    
    public int getAge() {
        return age;
    }
    
    public List<String> getHobbies() {
        // Devolver copia para prevenir modificaciones
        return new ArrayList<>(hobbies);
    }
    
    // No hay setters - el objeto no puede cambiar después de crearse
    // Esto lo hace automáticamente thread-safe
}
```

---

## 15. Errores Comunes y Cómo Evitarlos

### 15.1 Race Conditions

```java
// ❌ PROBLEMA: Race condition
public class RaceCondition {
    private int counter = 0;
    
    public void incrementar() {
        counter++; // No es atómico! (lee, incrementa, escribe)
    }
}

// ✅ SOLUCIÓN 1: synchronized
public class SolucionSynchronized {
    private int counter = 0;
    
    public synchronized void incrementar() {
        counter++;
    }
}

// ✅ SOLUCIÓN 2: AtomicInteger
import java.util.concurrent.atomic.AtomicInteger;

public class SolucionAtomic {
    private AtomicInteger counter = new AtomicInteger(0);
    
    public void incrementar() {
        counter.incrementAndGet();
    }
}
```

### 15.2 Memory Visibility

```java
// ❌ PROBLEMA: Cambios no visibles entre hilos
public class MemoryVisibility {
    private boolean flag = false;
    
    public void escritor() {
        flag = true; // Puede no ser visible para otros hilos
    }
    
    public void lector() {
        while (!flag) {
            // Puede quedar en bucle infinito
        }
    }
}

// ✅ SOLUCIÓN: volatile
public class SolucionVolatile {
    private volatile boolean flag = false;
    
    public void escritor() {
        flag = true; // Cambio visible inmediatamente
    }
    
    public void lector() {
        while (!flag) {
            // Saldrá cuando flag sea true
        }
    }
}
```

### 15.3 Thread Leaks

```java
// ❌ PROBLEMA: Crear hilos sin cerrar ExecutorService
public class ThreadLeak {
    public void procesarTareas() {
        ExecutorService executor = Executors.newFixedThreadPool(10);
        
        for (int i = 0; i < 100; i++) {
            executor.submit(() -> {
                // Procesar tarea
            });
        }
        
        // ¡Nunca se cierra el executor!
        // Los hilos quedan activos consumiendo recursos
    }
}

// ✅ SOLUCIÓN: Siempre cerrar ExecutorService
public class SolucionThreadLeak {
    public void procesarTareas() {
        ExecutorService executor = Executors.newFixedThreadPool(10);
        
        try {
            for (int i = 0; i < 100; i++) {
                executor.submit(() -> {
                    // Procesar tarea
                });
            }
        } finally {
            executor.shutdown();
            try {
                if (!executor.awaitTermination(60, TimeUnit.SECONDS)) {
                    executor.shutdownNow();
                }
            } catch (InterruptedException e) {
                executor.shutdownNow();
                Thread.currentThread().interrupt();
            }
        }
    }
    
    // O usar try-with-resources (Java 19+)
    public void procesarTareasModerno() {
        try (ExecutorService executor = Executors.newFixedThreadPool(10)) {
            for (int i = 0; i < 100; i++) {
                executor.submit(() -> {
                    // Procesar tarea
                });
            }
        } // Cierre automático
    }
}
```

### 15.4 Blocking I/O en Threads Limitados

```java
// ❌ PROBLEMA: I/O bloqueante en pool pequeño
public class BlockingIOProblem {
    public void mal() {
        ExecutorService executor = Executors.newFixedThreadPool(2);
        
        for (int i = 0; i < 10; i++) {
            executor.submit(() -> {
                // Llamada bloqueante que tarda 10 segundos
                hacerLlamadaHTTP();
            });
        }
        
        // Solo 2 tareas se ejecutan a la vez
        // Las demás esperan = muy lento
        
        executor.shutdown();
    }
    
    private void hacerLlamadaHTTP() {
        // Simulación
        try {
            Thread.sleep(10000);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}

// ✅ SOLUCIÓN: Pool más grande o CachedThreadPool
public class SolucionBlockingIO {
    public void mejor() {
        // Opción 1: Pool más grande
        ExecutorService executor = Executors.newFixedThreadPool(20);
        
        // Opción 2: CachedThreadPool
        // ExecutorService executor = Executors.newCachedThreadPool();
        
        // Opción 3: Virtual Threads (Java 21+)
        // ExecutorService executor = Executors.newVirtualThreadPerTaskExecutor();
        
        for (int i = 0; i < 10; i++) {
            executor.submit(() -> {
                hacerLlamadaHTTP();
            });
        }
        
        executor.shutdown();
    }
    
    private void hacerLlamadaHTTP() {
        try {
            Thread.sleep(10000);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}
```

### 15.5 Double-Checked Locking Incorrecto

```java
// ❌ PROBLEMA: Double-checked locking roto
public class DoubleCheckedLockingBroken {
    private Singleton instance;
    
    public Singleton getInstance() {
        if (instance == null) {  // Primera verificación
            synchronized (this) {
                if (instance == null) {  // Segunda verificación
                    instance = new Singleton(); // ¡Puede retornar objeto parcialmente construido!
                }
            }
        }
        return instance;
    }
    
    static class Singleton {
        private int data = 42;
    }
}

// ✅ SOLUCIÓN: Agregar volatile
public class DoubleCheckedLockingCorrect {
    private volatile Singleton instance; // volatile es crucial
    
    public Singleton getInstance() {
        if (instance == null) {
            synchronized (this) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
    
    static class Singleton {
        private int data = 42;
    }
}

// ✅ MEJOR: Usar enum (initialization-on-demand)
public enum SingletonEnum {
    INSTANCE;
    
    private int data = 42;
    
    public int getData() {
        return data;
    }
}
```

---

## 16. Resumen y Conclusiones

### 16.1 Guía Rápida de Selección

```
¿Qué necesitas?
│
├─ Una sola tarea asíncrona sin resultado
│  └─> Runnable + Thread o ExecutorService.execute()
│
├─ Una tarea asíncrona con resultado
│  └─> Callable + Future
│
├─ Múltiples tareas asíncronas con composición
│  └─> CompletableFuture
│
├─ Tareas programadas/periódicas
│  └─> ScheduledExecutorService
│
├─ Procesamiento paralelo recursivo
│  └─> Fork/Join Framework
│
├─ Millones de tareas I/O concurrentes
│  └─> Virtual Threads (Java 21+)
│
├─ Sincronización simple
│  └─> synchronized
│
├─ Sincronización avanzada
│  └─> ReentrantLock, ReadWriteLock
│
├─ Contador thread-safe
│  └─> AtomicInteger
│
├─ Esperar N operaciones
│  └─> CountDownLatch
│
├─ Sincronizar N hilos en un punto
│  └─> CyclicBarrier
│
├─ Controlar acceso a N recursos
│  └─> Semaphore
│
├─ Productor-Consumidor
│  └─> BlockingQueue
│
└─ Mapa/Lista compartida
   └─> ConcurrentHashMap, CopyOnWriteArrayList
```

### 16.2 Cheat Sheet de Concurrencia

| Necesidad | Herramienta | Cuándo Usar |
|-----------|-------------|-------------|
| Thread básico | Thread, Runnable | Tareas simples |
| Resultado de tarea | Callable + Future | Tarea con retorno |
| Pool de hilos | ExecutorService | Gestión eficiente |
| Tareas programadas | ScheduledExecutorService | Cron jobs, heartbeat |
| Async moderno | CompletableFuture | APIs modernas, composición |
| Paralelismo CPU | ForkJoinPool | Procesamiento masivo |
| I/O masivo | Virtual Threads | Servidores, APIs |
| Sincronización | synchronized, Lock | Secciones críticas |
| Variables compartidas | Atomic* | Contadores, flags |
| Esperas coordinadas | CountDownLatch, CyclicBarrier | Sincronización |
| Recursos limitados | Semaphore | Pools, rate limiting |
| Colas | BlockingQueue | Productor-consumidor |
| Colecciones | ConcurrentHashMap | Datos compartidos |

### 16.3 Mejores Prácticas Generales

1. **Preferir inmutabilidad**: Objetos inmutables son automáticamente thread-safe
2. **Usar herramientas de alto nivel**: ExecutorService antes que Thread
3. **Evitar synchronized cuando sea posible**: Usar colecciones concurrentes
4. **Siempre cerrar ExecutorService**: Prevenir thread leaks
5. **Manejo apropiado de interrupciones**: No ignorar InterruptedException
6. **Usar volatile para flags**: Asegurar visibilidad
7. **Pool sizing correcto**: CPU-bound = núcleos+1, I/O-bound = núcleos*2
8. **Evitar deadlocks**: Orden consistente de locks, usar tryLock
9. **Testing de concurrencia**: Usar herramientas como JCStress
10. **Documentar thread-safety**: Indicar si las clases son thread-safe

### 16.4 Recursos Adicionales

- **Java Concurrency in Practice** - Brian Goetz (Libro fundamental)
- **Java Language Specification** - Capítulos de Memory Model
- **Project Loom** - Documentación de Virtual Threads
- **JCStress** - Framework para testing de concurrencia
- **VisualVM** - Profiling y debugging de threads

---

## 🎓 Conclusión

La concurrencia y el paralelismo en Java ofrecen herramientas poderosas para construir aplicaciones eficientes y escalables. Desde los conceptos básicos de Thread y Runnable hasta las innovaciones modernas como Virtual Threads, Java proporciona un ecosistema completo para manejar operaciones concurrentes.

**Puntos clave para recordar:**

- **Concurrencia ≠ Paralelismo**: Concurrencia es sobre estructura, paralelismo es sobre ejecución
- **ExecutorService es tu amigo**: Raramente necesitas crear Thread manualmente
- **CompletableFuture para async moderno**: Composición poderosa de operaciones
- **Virtual Threads revolucionan la escala**: Millones de hilos con bajo overhead
- **Thread-safety no es automática**: Usa las herramientas correctas
- **Testing es crucial**: Los bugs de concurrencia son difíciles de reproducir

Esta guía cubre los aspectos fundamentales y avanzados de la concurrencia en Java. Practica con ejemplos, experimenta con diferentes enfoques y siempre considera las implicaciones de thread-safety en tu código.

¡Feliz programación concurrente! 🚀

