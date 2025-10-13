# 🦅 Guía Completa de TypeScript y NestJS

## 📋 Índice

### TypeScript
1. [Introducción a TypeScript](#1-introducción-a-typescript)
2. [Configuración y Setup](#2-configuración-y-setup)
3. [Tipos Básicos](#3-tipos-básicos)
4. [Interfaces y Types](#4-interfaces-y-types)
5. [Clases y OOP](#5-clases-y-oop)
6. [Generics](#6-generics)
7. [Decoradores](#7-decoradores)
8. [Módulos](#8-módulos)
9. [TypeScript Avanzado](#9-typescript-avanzado)

### NestJS
10. [Introducción a NestJS](#10-introducción-a-nestjs)
11. [Estructura de un Proyecto NestJS](#11-estructura-de-un-proyecto-nestjs)
12. [Controllers](#12-controllers)
13. [Providers y Services](#13-providers-y-services)
14. [Modules](#14-modules)
15. [Middleware](#15-middleware)
16. [Guards](#16-guards)
17. [Interceptors](#17-interceptors)
18. [Pipes](#18-pipes)
19. [Bases de Datos con TypeORM](#19-bases-de-datos-con-typeorm)
20. [Autenticación y JWT](#20-autenticación-y-jwt)
21. [Testing en NestJS](#21-testing-en-nestjs)
22. [Documentación con Swagger](#22-documentación-con-swagger)

---

## PARTE 1: TypeScript

## 1. Introducción a TypeScript

TypeScript es un superset de JavaScript que añade tipado estático opcional y otras características avanzadas. Es desarrollado y mantenido por Microsoft.

### ¿Por qué TypeScript?

- **Tipado estático**: Detecta errores en tiempo de desarrollo
- **IntelliSense mejorado**: Mejor autocompletado en IDEs
- **Refactoring seguro**: Cambios más seguros en el código
- **Documentación implícita**: Los tipos sirven como documentación
- **Compatible con JavaScript**: Todo código JS es válido en TS

---

## 2. Configuración y Setup

### Instalación

```bash
# Global
npm install -g typescript

# En proyecto
npm install --save-dev typescript

# Verificar instalación
tsc --version
```

### Inicializar proyecto

```bash
# Crear tsconfig.json
tsc --init

# Compilar archivo
tsc archivo.ts

# Modo watch
tsc --watch
```

### tsconfig.json básico

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "lib": ["ES2020"],
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "resolveJsonModule": true,
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

---

## 3. Tipos Básicos

### Tipos primitivos

```typescript
// String
let nombre: string = "Juan";
let apellido: string = 'Pérez';
let saludo: string = `Hola ${nombre}`;

// Number
let edad: number = 30;
let precio: number = 99.99;
let hex: number = 0xf00d;

// Boolean
let activo: boolean = true;
let terminado: boolean = false;

// Any (evitar cuando sea posible)
let cualquierCosa: any = 42;
cualquierCosa = "ahora soy string";
cualquierCosa = true;

// Unknown (más seguro que any)
let valorDesconocido: unknown = 4;
valorDesconocido = "podría ser un string";
// Requiere verificación de tipo antes de usar

// Void
function log(mensaje: string): void {
    console.log(mensaje);
}

// Null y Undefined
let u: undefined = undefined;
let n: null = null;

// Never (funciones que nunca retornan)
function error(mensaje: string): never {
    throw new Error(mensaje);
}
```

### Arrays

```typescript
// Array de números
let numeros: number[] = [1, 2, 3, 4, 5];
let numerosAlt: Array<number> = [1, 2, 3, 4, 5];

// Array de strings
let nombres: string[] = ["Ana", "Juan", "María"];

// Array mixto con union types
let mixto: (string | number)[] = [1, "dos", 3, "cuatro"];

// Array de objetos
let usuarios: { nombre: string; edad: number }[] = [
    { nombre: "Juan", edad: 30 },
    { nombre: "Ana", edad: 25 }
];
```

### Tuplas

```typescript
// Tupla básica
let persona: [string, number] = ["Juan", 30];

// Tupla con nombres
let coordenada: [x: number, y: number] = [10, 20];

// Tupla con opcional y rest
let usuario: [string, number, ...string[]] = ["Juan", 30, "admin", "user"];
```

### Enums

```typescript
// Enum numérico
enum Direccion {
    Arriba,    // 0
    Abajo,     // 1
    Izquierda, // 2
    Derecha    // 3
}

let dir: Direccion = Direccion.Arriba;

// Enum con valores personalizados
enum Estado {
    Activo = 1,
    Inactivo = 0,
    Pendiente = 2
}

// Enum de strings
enum Color {
    Rojo = "RED",
    Verde = "GREEN",
    Azul = "BLUE"
}
```

---

## 4. Interfaces y Types

### Interfaces

```typescript
// Interface básica
interface Usuario {
    id: number;
    nombre: string;
    email: string;
    edad?: number; // Opcional
    readonly fechaCreacion: Date; // Solo lectura
}

const usuario: Usuario = {
    id: 1,
    nombre: "Juan",
    email: "juan@example.com",
    fechaCreacion: new Date()
};

// Interface con métodos
interface Calculadora {
    sumar(a: number, b: number): number;
    restar(a: number, b: number): number;
}

// Extender interfaces
interface Empleado extends Usuario {
    salario: number;
    departamento: string;
}

// Interface para funciones
interface OperacionMatematica {
    (a: number, b: number): number;
}

const sumar: OperacionMatematica = (a, b) => a + b;
```

### Types (Alias de tipos)

```typescript
// Type alias básico
type ID = string | number;
type Callback = (data: string) => void;

// Type para objetos
type Producto = {
    id: ID;
    nombre: string;
    precio: number;
    descripcion?: string;
};

// Union types
type Resultado = "success" | "error" | "pending";

// Intersection types
type Timestamped = {
    createdAt: Date;
    updatedAt: Date;
};

type ProductoConFechas = Producto & Timestamped;
```

### Diferencias Interface vs Type

```typescript
// Interfaces pueden ser extendidas y fusionadas
interface Ventana {
    titulo: string;
}

interface Ventana {
    ancho: number; // Se fusiona automáticamente
}

// Types son más flexibles para unions y primitivos
type EstadoConexion = "conectado" | "desconectado";
type NumeroOString = number | string;

// Recomendación: Usa interface para objetos, type para unions y aliases
```

---

## 5. Clases y OOP

### Clases básicas

```typescript
class Persona {
    // Propiedades
    nombre: string;
    edad: number;
    private _email: string; // Privada
    protected activo: boolean; // Protegida
    
    // Constructor
    constructor(nombre: string, edad: number, email: string) {
        this.nombre = nombre;
        this.edad = edad;
        this._email = email;
        this.activo = true;
    }
    
    // Métodos
    saludar(): string {
        return `Hola, soy ${this.nombre}`;
    }
    
    // Getters y Setters
    get email(): string {
        return this._email;
    }
    
    set email(nuevoEmail: string) {
        if (nuevoEmail.includes('@')) {
            this._email = nuevoEmail;
        }
    }
}

// Sintaxis abreviada en constructor
class PersonaAbreviada {
    constructor(
        public nombre: string,
        public edad: number,
        private email: string
    ) {}
}
```

### Herencia

```typescript
class Animal {
    constructor(public nombre: string) {}
    
    mover(distancia: number = 0): void {
        console.log(`${this.nombre} se movió ${distancia}m`);
    }
}

class Perro extends Animal {
    constructor(nombre: string) {
        super(nombre);
    }
    
    ladrar(): void {
        console.log("¡Guau guau!");
    }
    
    // Override
    mover(distancia: number = 5): void {
        console.log("El perro corre...");
        super.mover(distancia);
    }
}
```

### Clases abstractas

```typescript
abstract class Forma {
    constructor(public color: string) {}
    
    abstract calcularArea(): number;
    
    describir(): string {
        return `Una forma de color ${this.color}`;
    }
}

class Circulo extends Forma {
    constructor(
        color: string,
        public radio: number
    ) {
        super(color);
    }
    
    calcularArea(): number {
        return Math.PI * this.radio ** 2;
    }
}
```

### Modificadores de acceso

```typescript
class CuentaBancaria {
    public titular: string;           // Accesible desde cualquier lugar
    private saldo: number;            // Solo dentro de la clase
    protected numeroCuenta: string;   // Clase y subclases
    readonly banco: string;           // Solo lectura
    
    constructor(titular: string, saldo: number, banco: string) {
        this.titular = titular;
        this.saldo = saldo;
        this.numeroCuenta = this.generarNumeroCuenta();
        this.banco = banco;
    }
    
    private generarNumeroCuenta(): string {
        return Math.random().toString(36).substring(7);
    }
    
    public obtenerSaldo(): number {
        return this.saldo;
    }
}
```

---

## 6. Generics

### Funciones genéricas

```typescript
// Función genérica básica
function identidad<T>(arg: T): T {
    return arg;
}

let resultado1 = identidad<string>("hola");
let resultado2 = identidad<number>(42);
let resultado3 = identidad(true); // Inferencia automática

// Función con múltiples tipos genéricos
function par<T, U>(primero: T, segundo: U): [T, U] {
    return [primero, segundo];
}

let miPar = par<string, number>("edad", 30);
```

### Clases genéricas

```typescript
class Caja<T> {
    private contenido: T;
    
    constructor(valor: T) {
        this.contenido = valor;
    }
    
    obtener(): T {
        return this.contenido;
    }
    
    establecer(valor: T): void {
        this.contenido = valor;
    }
}

let cajaNumero = new Caja<number>(123);
let cajaString = new Caja<string>("Hola");
```

### Interfaces genéricas

```typescript
interface Respuesta<T> {
    data: T;
    status: number;
    mensaje: string;
}

interface Usuario {
    id: number;
    nombre: string;
}

const respuestaUsuario: Respuesta<Usuario> = {
    data: { id: 1, nombre: "Juan" },
    status: 200,
    mensaje: "OK"
};

const respuestaLista: Respuesta<Usuario[]> = {
    data: [
        { id: 1, nombre: "Juan" },
        { id: 2, nombre: "Ana" }
    ],
    status: 200,
    mensaje: "OK"
};
```

### Constraints en genéricos

```typescript
// Restringir tipo genérico
interface ConLongitud {
    length: number;
}

function imprimirLongitud<T extends ConLongitud>(arg: T): number {
    return arg.length;
}

imprimirLongitud("Hola"); // OK
imprimirLongitud([1, 2, 3]); // OK
// imprimirLongitud(123); // Error: number no tiene length

// Usar keyof
function obtenerPropiedad<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
}

let persona = { nombre: "Juan", edad: 30 };
let nombre = obtenerPropiedad(persona, "nombre"); // OK
// let error = obtenerPropiedad(persona, "apellido"); // Error
```

---

## 7. Decoradores

Los decoradores son una característica experimental de TypeScript que permite añadir anotaciones y metadatos a clases y sus miembros.

### Habilitar decoradores

```json
// tsconfig.json
{
  "compilerOptions": {
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true
  }
}
```

### Decoradores de clase

```typescript
function EntidadDB(tabla: string) {
    return function<T extends { new(...args: any[]): {} }>(constructor: T) {
        return class extends constructor {
            tabla = tabla;
            fechaCreacion = new Date();
        };
    };
}

@EntidadDB("usuarios")
class Usuario {
    constructor(public nombre: string) {}
}
```

### Decoradores de método

```typescript
function Log(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    const metodoOriginal = descriptor.value;
    
    descriptor.value = function(...args: any[]) {
        console.log(`Llamando a ${propertyKey} con argumentos:`, args);
        const resultado = metodoOriginal.apply(this, args);
        console.log(`${propertyKey} retornó:`, resultado);
        return resultado;
    };
    
    return descriptor;
}

class Calculadora {
    @Log
    sumar(a: number, b: number): number {
        return a + b;
    }
}
```

### Decoradores de propiedad

```typescript
function MinLength(length: number) {
    return function(target: any, propertyKey: string) {
        let valor: string;
        
        const getter = function() {
            return valor;
        };
        
        const setter = function(newVal: string) {
            if (newVal.length < length) {
                throw new Error(`${propertyKey} debe tener al menos ${length} caracteres`);
            }
            valor = newVal;
        };
        
        Object.defineProperty(target, propertyKey, {
            get: getter,
            set: setter
        });
    };
}

class Usuario {
    @MinLength(5)
    nombre: string;
}
```

---

## 8. Módulos

### Export e Import

```typescript
// math.ts
export function sumar(a: number, b: number): number {
    return a + b;
}

export function restar(a: number, b: number): number {
    return a - b;
}

export const PI = 3.14159;

// usuario.ts
export interface Usuario {
    id: number;
    nombre: string;
}

export class GestorUsuarios {
    private usuarios: Usuario[] = [];
    
    agregar(usuario: Usuario): void {
        this.usuarios.push(usuario);
    }
}

// Default export
export default class Aplicacion {
    iniciar() {
        console.log("Aplicación iniciada");
    }
}

// main.ts
import { sumar, restar, PI } from './math';
import { Usuario, GestorUsuarios } from './usuario';
import Aplicacion from './aplicacion';

// Import todo
import * as math from './math';
console.log(math.sumar(5, 3));
```

---

## 9. TypeScript Avanzado

### Utility Types

```typescript
// Partial<T> - Hace todas las propiedades opcionales
interface Usuario {
    id: number;
    nombre: string;
    email: string;
}

type UsuarioParcial = Partial<Usuario>;
// Equivale a: { id?: number; nombre?: string; email?: string; }

// Required<T> - Hace todas las propiedades requeridas
type UsuarioRequerido = Required<UsuarioParcial>;

// Readonly<T> - Hace todas las propiedades de solo lectura
type UsuarioReadonly = Readonly<Usuario>;

// Pick<T, K> - Selecciona propiedades específicas
type UsuarioBasico = Pick<Usuario, 'id' | 'nombre'>;

// Omit<T, K> - Omite propiedades específicas
type UsuarioSinEmail = Omit<Usuario, 'email'>;

// Record<K, T> - Crea un tipo con claves K y valores T
type EstadosPorID = Record<number, string>;

// ReturnType<T> - Obtiene el tipo de retorno de una función
function crearUsuario() {
    return { id: 1, nombre: "Juan" };
}
type UsuarioCreado = ReturnType<typeof crearUsuario>;
```

### Tipos condicionales

```typescript
// Sintaxis: T extends U ? X : Y
type EsString<T> = T extends string ? true : false;

type Test1 = EsString<string>;  // true
type Test2 = EsString<number>;  // false

// Ejemplo práctico
type ExtraerArrayType<T> = T extends Array<infer U> ? U : T;

type Tipo1 = ExtraerArrayType<string[]>;  // string
type Tipo2 = ExtraerArrayType<number>;    // number
```

### Mapped Types

```typescript
// Transformar todas las propiedades
type Opcional<T> = {
    [P in keyof T]?: T[P];
};

type Readonly<T> = {
    readonly [P in keyof T]: T[P];
};

// Ejemplo personalizado
type ConGetter<T> = {
    [P in keyof T as `get${Capitalize<string & P>}`]: () => T[P];
};

interface Persona {
    nombre: string;
    edad: number;
}

type PersonaConGetters = ConGetter<Persona>;
// { getNombre: () => string; getEdad: () => number; }
```

### Type Guards

```typescript
// typeof guard
function procesar(valor: string | number) {
    if (typeof valor === "string") {
        return valor.toUpperCase();
    } else {
        return valor.toFixed(2);
    }
}

// instanceof guard
class Perro {
    ladrar() { console.log("Guau!"); }
}

class Gato {
    maullar() { console.log("Miau!"); }
}

function hacerSonido(animal: Perro | Gato) {
    if (animal instanceof Perro) {
        animal.ladrar();
    } else {
        animal.maullar();
    }
}

// Custom type guard
interface Pajaro {
    volar(): void;
    tipo: 'pajaro';
}

interface Pez {
    nadar(): void;
    tipo: 'pez';
}

function esPajaro(animal: Pajaro | Pez): animal is Pajaro {
    return animal.tipo === 'pajaro';
}

function moverAnimal(animal: Pajaro | Pez) {
    if (esPajaro(animal)) {
        animal.volar();
    } else {
        animal.nadar();
    }
}
```

---

## PARTE 2: NestJS

## 10. Introducción a NestJS

NestJS es un framework progresivo de Node.js para construir aplicaciones del lado del servidor eficientes y escalables. Está construido con TypeScript y combina elementos de OOP, FP y FRP.

### Características principales

- **Arquitectura modular**: Organización clara del código
- **Decoradores**: Uso extensivo de decoradores de TypeScript
- **Inyección de dependencias**: DI incorporada
- **Testing**: Soporte completo para testing
- **TypeScript first**: Desarrollado con TypeScript
- **Express/Fastify**: Puede usar cualquiera como motor HTTP

### Instalación

```bash
# Instalar CLI
npm i -g @nestjs/cli

# Crear nuevo proyecto
nest new nombre-proyecto

# Entrar al proyecto
cd nombre-proyecto

# Iniciar en modo desarrollo
npm run start:dev
```

---

## 11. Estructura de un Proyecto NestJS

```plaintext
my-nest-app/
├── src/
│   ├── app.controller.ts       # Controller principal
│   ├── app.controller.spec.ts  # Tests del controller
│   ├── app.module.ts            # Módulo raíz
│   ├── app.service.ts           # Service principal
│   ├── main.ts                  # Punto de entrada
│   │
│   ├── users/                   # Módulo de usuarios
│   │   ├── dto/                 # Data Transfer Objects
│   │   │   ├── create-user.dto.ts
│   │   │   └── update-user.dto.ts
│   │   ├── entities/            # Entidades de base de datos
│   │   │   └── user.entity.ts
│   │   ├── users.controller.ts
│   │   ├── users.service.ts
│   │   ├── users.module.ts
│   │   └── users.service.spec.ts
│   │
│   ├── common/                  # Código compartido
│   │   ├── filters/
│   │   ├── guards/
│   │   ├── interceptors/
│   │   ├── pipes/
│   │   └── decorators/
│   │
│   └── config/                  # Configuración
│       └── database.config.ts
│
├── test/                        # Tests e2e
├── nest-cli.json
├── package.json
├── tsconfig.json
└── tsconfig.build.json
```

---

## 12. Controllers

Los controllers manejan las peticiones HTTP y retornan respuestas al cliente.

### Controller básico

```typescript
// users.controller.ts
import { 
    Controller, 
    Get, 
    Post, 
    Put, 
    Delete,
    Body, 
    Param, 
    Query,
    HttpCode,
    HttpStatus
} from '@nestjs/common';

@Controller('users')
export class UsersController {
    constructor(private readonly usersService: UsersService) {}
    
    @Get()
    findAll(@Query() query: any) {
        return this.usersService.findAll(query);
    }
    
    @Get(':id')
    findOne(@Param('id') id: string) {
        return this.usersService.findOne(+id);
    }
    
    @Post()
    @HttpCode(HttpStatus.CREATED)
    create(@Body() createUserDto: CreateUserDto) {
        return this.usersService.create(createUserDto);
    }
    
    @Put(':id')
    update(
        @Param('id') id: string,
        @Body() updateUserDto: UpdateUserDto
    ) {
        return this.usersService.update(+id, updateUserDto);
    }
    
    @Delete(':id')
    @HttpCode(HttpStatus.NO_CONTENT)
    remove(@Param('id') id: string) {
        return this.usersService.remove(+id);
    }
}
```

### Decoradores de ruta

```typescript
@Controller('products')
export class ProductsController {
    // GET /products
    @Get()
    findAll() {}
    
    // GET /products/featured
    @Get('featured')
    findFeatured() {}
    
    // GET /products/:id
    @Get(':id')
    findOne(@Param('id') id: string) {}
    
    // GET /products/:id/reviews
    @Get(':id/reviews')
    getReviews(@Param('id') id: string) {}
    
    // POST /products
    @Post()
    create(@Body() createProductDto: CreateProductDto) {}
    
    // PATCH /products/:id
    @Patch(':id')
    update(
        @Param('id') id: string,
        @Body() updateProductDto: UpdateProductDto
    ) {}
}
```

---

## 13. Providers y Services

Los providers son clases que pueden ser inyectadas como dependencias.

### Service básico

```typescript
// users.service.ts
import { Injectable, NotFoundException } from '@nestjs/common';

interface User {
    id: number;
    name: string;
    email: string;
}

@Injectable()
export class UsersService {
    private users: User[] = [];
    private idCounter = 1;
    
    findAll(): User[] {
        return this.users;
    }
    
    findOne(id: number): User {
        const user = this.users.find(u => u.id === id);
        if (!user) {
            throw new NotFoundException(`User #${id} not found`);
        }
        return user;
    }
    
    create(createUserDto: CreateUserDto): User {
        const newUser: User = {
            id: this.idCounter++,
            ...createUserDto
        };
        this.users.push(newUser);
        return newUser;
    }
    
    update(id: number, updateUserDto: UpdateUserDto): User {
        const user = this.findOne(id);
        Object.assign(user, updateUserDto);
        return user;
    }
    
    remove(id: number): void {
        const index = this.users.findIndex(u => u.id === id);
        if (index === -1) {
            throw new NotFoundException(`User #${id} not found`);
        }
        this.users.splice(index, 1);
    }
}
```

### DTOs (Data Transfer Objects)

```typescript
// create-user.dto.ts
import { IsEmail, IsString, MinLength, IsOptional } from 'class-validator';

export class CreateUserDto {
    @IsString()
    @MinLength(3)
    name: string;
    
    @IsEmail()
    email: string;
    
    @IsString()
    @MinLength(6)
    password: string;
    
    @IsOptional()
    @IsString()
    phone?: string;
}

// update-user.dto.ts
import { PartialType } from '@nestjs/mapped-types';
import { CreateUserDto } from './create-user.dto';

export class UpdateUserDto extends PartialType(CreateUserDto) {}
```

---

## 14. Modules

Los módulos organizan la aplicación en bloques cohesivos.

```typescript
// users.module.ts
import { Module } from '@nestjs/common';
import { UsersController } from './users.controller';
import { UsersService } from './users.service';

@Module({
    imports: [],         // Módulos que este módulo necesita
    controllers: [UsersController],
    providers: [UsersService],
    exports: [UsersService]  // Servicios exportados para otros módulos
})
export class UsersModule {}

// app.module.ts
import { Module } from '@nestjs/common';
import { UsersModule } from './users/users.module';
import { ProductsModule } from './products/products.module';

@Module({
    imports: [UsersModule, ProductsModule],
    controllers: [],
    providers: [],
})
export class AppModule {}
```

---

## 15. Middleware

```typescript
// logger.middleware.ts
import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';

@Injectable()
export class LoggerMiddleware implements NestMiddleware {
    use(req: Request, res: Response, next: NextFunction) {
        console.log(`[${new Date().toISOString()}] ${req.method} ${req.url}`);
        next();
    }
}

// app.module.ts
import { Module, NestModule, MiddlewareConsumer } from '@nestjs/common';

@Module({
    imports: [UsersModule],
})
export class AppModule implements NestModule {
    configure(consumer: MiddlewareConsumer) {
        consumer
            .apply(LoggerMiddleware)
            .forRoutes('*');  // Aplicar a todas las rutas
    }
}
```

---

## 16. Guards

Los guards determinan si una petición debe ser manejada.

```typescript
// auth.guard.ts
import { Injectable, CanActivate, ExecutionContext, UnauthorizedException } from '@nestjs/common';

@Injectable()
export class AuthGuard implements CanActivate {
    canActivate(context: ExecutionContext): boolean {
        const request = context.switchToHttp().getRequest();
        const token = request.headers.authorization;
        
        if (!token) {
            throw new UnauthorizedException('Token not found');
        }
        
        // Validar token
        return true;
    }
}

// Usar el guard
@Controller('users')
@UseGuards(AuthGuard)
export class UsersController {
    @Get()
    findAll() {
        return 'Protected route';
    }
}
```

---

## 17. Interceptors

Los interceptors pueden transformar la respuesta o ejecutar lógica antes/después.

```typescript
// transform.interceptor.ts
import {
    Injectable,
    NestInterceptor,
    ExecutionContext,
    CallHandler
} from '@nestjs/common';
import { Observable } from 'rxjs';
import { map } from 'rxjs/operators';

export interface Response<T> {
    data: T;
    statusCode: number;
    timestamp: string;
}

@Injectable()
export class TransformInterceptor<T> implements NestInterceptor<T, Response<T>> {
    intercept(context: ExecutionContext, next: CallHandler): Observable<Response<T>> {
        return next.handle().pipe(
            map(data => ({
                data,
                statusCode: context.switchToHttp().getResponse().statusCode,
                timestamp: new Date().toISOString()
            }))
        );
    }
}

// Usar el interceptor
@Controller('users')
@UseInterceptors(TransformInterceptor)
export class UsersController {}
```

---

## 18. Pipes

Los pipes transforman o validan datos de entrada.

```typescript
// validation.pipe está incluido en NestJS
import { ValidationPipe } from '@nestjs/common';

// main.ts
async function bootstrap() {
    const app = await NestFactory.create(AppModule);
    
    app.useGlobalPipes(new ValidationPipe({
        whitelist: true,  // Elimina propiedades no definidas en DTO
        forbidNonWhitelisted: true,  // Lanza error si hay propiedades extra
        transform: true  // Transforma payloads a instancias de DTO
    }));
    
    await app.listen(3000);
}

// Custom pipe
@Injectable()
export class ParseIntPipe implements PipeTransform {
    transform(value: string, metadata: ArgumentMetadata): number {
        const val = parseInt(value, 10);
        if (isNaN(val)) {
            throw new BadRequestException('Validation failed');
        }
        return val;
    }
}
```

---

## 19. Bases de Datos con TypeORM

```bash
npm install @nestjs/typeorm typeorm mysql2
```

```typescript
// app.module.ts
import { TypeOrmModule } from '@nestjs/typeorm';

@Module({
    imports: [
        TypeOrmModule.forRoot({
            type: 'mysql',
            host: 'localhost',
            port: 3306,
            username: 'root',
            password: 'password',
            database: 'test',
            entities: [__dirname + '/**/*.entity{.ts,.js}'],
            synchronize: true  // Solo en desarrollo
        }),
        UsersModule
    ]
})
export class AppModule {}

// user.entity.ts
import { Entity, PrimaryGeneratedColumn, Column } from 'typeorm';

@Entity('users')
export class User {
    @PrimaryGeneratedColumn()
    id: number;
    
    @Column()
    name: string;
    
    @Column({ unique: true })
    email: string;
    
    @Column()
    password: string;
    
    @Column({ default: true })
    isActive: boolean;
    
    @Column({ type: 'timestamp', default: () => 'CURRENT_TIMESTAMP' })
    createdAt: Date;
}

// users.module.ts
import { TypeOrmModule } from '@nestjs/typeorm';

@Module({
    imports: [TypeOrmModule.forFeature([User])],
    controllers: [UsersController],
    providers: [UsersService]
})
export class UsersModule {}

// users.service.ts
import { Injectable } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';

@Injectable()
export class UsersService {
    constructor(
        @InjectRepository(User)
        private usersRepository: Repository<User>
    ) {}
    
    findAll(): Promise<User[]> {
        return this.usersRepository.find();
    }
    
    findOne(id: number): Promise<User> {
        return this.usersRepository.findOne({ where: { id } });
    }
    
    create(createUserDto: CreateUserDto): Promise<User> {
        const user = this.usersRepository.create(createUserDto);
        return this.usersRepository.save(user);
    }
    
    async update(id: number, updateUserDto: UpdateUserDto): Promise<User> {
        await this.usersRepository.update(id, updateUserDto);
        return this.findOne(id);
    }
    
    async remove(id: number): Promise<void> {
        await this.usersRepository.delete(id);
    }
}
```

---

## 20. Autenticación y JWT

```bash
npm install @nestjs/jwt @nestjs/passport passport passport-jwt bcrypt
npm install -D @types/passport-jwt @types/bcrypt
```

```typescript
// auth.module.ts
import { Module } from '@nestjs/common';
import { JwtModule } from '@nestjs/jwt';
import { PassportModule } from '@nestjs/passport';

@Module({
    imports: [
        PassportModule,
        JwtModule.register({
            secret: 'YOUR_SECRET_KEY',
            signOptions: { expiresIn: '1h' }
        })
    ],
    providers: [AuthService, JwtStrategy],
    exports: [AuthService]
})
export class AuthModule {}

// auth.service.ts
import { Injectable } from '@nestjs/common';
import { JwtService } from '@nestjs/jwt';
import * as bcrypt from 'bcrypt';

@Injectable()
export class AuthService {
    constructor(
        private jwtService: JwtService,
        private usersService: UsersService
    ) {}
    
    async validateUser(email: string, password: string): Promise<any> {
        const user = await this.usersService.findByEmail(email);
        if (user && await bcrypt.compare(password, user.password)) {
            const { password, ...result } = user;
            return result;
        }
        return null;
    }
    
    async login(user: any) {
        const payload = { email: user.email, sub: user.id };
        return {
            access_token: this.jwtService.sign(payload)
        };
    }
    
    async register(createUserDto: CreateUserDto) {
        const hashedPassword = await bcrypt.hash(createUserDto.password, 10);
        return this.usersService.create({
            ...createUserDto,
            password: hashedPassword
        });
    }
}

// jwt.strategy.ts
import { Injectable } from '@nestjs/common';
import { PassportStrategy } from '@nestjs/passport';
import { ExtractJwt, Strategy } from 'passport-jwt';

@Injectable()
export class JwtStrategy extends PassportStrategy(Strategy) {
    constructor() {
        super({
            jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),
            ignoreExpiration: false,
            secretOrKey: 'YOUR_SECRET_KEY'
        });
    }
    
    async validate(payload: any) {
        return { userId: payload.sub, email: payload.email };
    }
}

// jwt-auth.guard.ts
import { Injectable } from '@nestjs/common';
import { AuthGuard } from '@nestjs/passport';

@Injectable()
export class JwtAuthGuard extends AuthGuard('jwt') {}

// Usar en controller
@Controller('users')
export class UsersController {
    @UseGuards(JwtAuthGuard)
    @Get('profile')
    getProfile(@Request() req) {
        return req.user;
    }
}
```

---

## 21. Testing en NestJS

```typescript
// users.service.spec.ts
import { Test, TestingModule } from '@nestjs/testing';
import { UsersService } from './users.service';

describe('UsersService', () => {
    let service: UsersService;
    
    beforeEach(async () => {
        const module: TestingModule = await Test.createTestingModule({
            providers: [UsersService]
        }).compile();
        
        service = module.get<UsersService>(UsersService);
    });
    
    it('should be defined', () => {
        expect(service).toBeDefined();
    });
    
    it('should create a user', () => {
        const user = service.create({
            name: 'Test User',
            email: 'test@example.com',
            password: 'password'
        });
        
        expect(user).toHaveProperty('id');
        expect(user.name).toBe('Test User');
    });
});

// tests e2e - users.e2e-spec.ts
import { Test, TestingModule } from '@nestjs/testing';
import { INestApplication } from '@nestjs/common';
import * as request from 'supertest';
import { AppModule } from '../src/app.module';

describe('UsersController (e2e)', () => {
    let app: INestApplication;
    
    beforeEach(async () => {
        const moduleFixture: TestingModule = await Test.createTestingModule({
            imports: [AppModule]
        }).compile();
        
        app = moduleFixture.createNestApplication();
        await app.init();
    });
    
    it('/users (GET)', () => {
        return request(app.getHttpServer())
            .get('/users')
            .expect(200)
            .expect('Content-Type', /json/);
    });
    
    afterAll(async () => {
        await app.close();
    });
});
```

---

## 22. Documentación con Swagger

```bash
npm install @nestjs/swagger swagger-ui-express
```

```typescript
// main.ts
import { NestFactory } from '@nestjs/core';
import { SwaggerModule, DocumentBuilder } from '@nestjs/swagger';

async function bootstrap() {
    const app = await NestFactory.create(AppModule);
    
    const config = new DocumentBuilder()
        .setTitle('API Documentation')
        .setDescription('The API description')
        .setVersion('1.0')
        .addBearerAuth()
        .build();
    
    const document = SwaggerModule.createDocument(app, config);
    SwaggerModule.setup('api', app, document);
    
    await app.listen(3000);
}

// Decoradores en DTOs
import { ApiProperty } from '@nestjs/swagger';

export class CreateUserDto {
    @ApiProperty({ description: 'User name', example: 'John Doe' })
    @IsString()
    name: string;
    
    @ApiProperty({ description: 'User email', example: 'john@example.com' })
    @IsEmail()
    email: string;
}

// Decoradores en controllers
@ApiTags('users')
@Controller('users')
export class UsersController {
    @ApiOperation({ summary: 'Get all users' })
    @ApiResponse({ status: 200, description: 'Return all users' })
    @Get()
    findAll() {}
    
    @ApiOperation({ summary: 'Create user' })
    @ApiResponse({ status: 201, description: 'User created successfully' })
    @ApiResponse({ status: 400, description: 'Bad request' })
    @Post()
    create(@Body() createUserDto: CreateUserDto) {}
}
```

---

## 🎓 Mejores Prácticas

1. **Usa DTOs**: Valida y transforma datos de entrada
2. **Separa responsabilidades**: Controllers para rutas, Services para lógica
3. **Inyección de dependencias**: Usa DI en lugar de instanciar manualmente
4. **Usa Guards para autenticación**: No pongas lógica de auth en controllers
5. **Usa Interceptors para transformaciones**: Mantén controllers limpios
6. **Testing**: Escribe tests unitarios y e2e
7. **Documenta con Swagger**: Mantén documentación actualizada
8. **Variables de entorno**: Usa @nestjs/config
9. **Logging**: Implementa logging apropiado
10. **Manejo de errores**: Usa exception filters personalizados

---

**¡Domina TypeScript y NestJS para crear APIs robustas y escalables! 🦅**
