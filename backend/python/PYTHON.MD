# 🐍 Guía Completa de Python

## 📋 Índice

1. [Introducción a Python](#1-introducción-a-python)
2. [Instalación y Configuración](#2-instalación-y-configuración)
3. [Sintaxis Básica](#3-sintaxis-básica)
4. [Tipos de Datos](#4-tipos-de-datos)
5. [Operadores](#5-operadores)
6. [Estructuras de Control](#6-estructuras-de-control)
7. [Funciones](#7-funciones)
8. [Estructuras de Datos](#8-estructuras-de-datos)
9. [Programación Orientada a Objetos](#9-programación-orientada-a-objetos)
10. [Módulos y Paquetes](#10-módulos-y-paquetes)
11. [Manejo de Archivos](#11-manejo-de-archivos)
12. [Excepciones](#12-excepciones)
13. [Decoradores](#13-decoradores)
14. [Generadores e Iteradores](#14-generadores-e-iteradores)
15. [Comprehensions](#15-comprehensions)
16. [Programación Funcional](#16-programación-funcional)
17. [Manejo de Fechas y Tiempo](#17-manejo-de-fechas-y-tiempo)
18. [Expresiones Regulares](#18-expresiones-regulares)
19. [Trabajo con APIs REST](#19-trabajo-con-apis-rest)
20. [Bases de Datos](#20-bases-de-datos)
21. [Testing](#21-testing)
22. [Virtual Environments](#22-virtual-environments)
23. [Frameworks Web](#23-frameworks-web)
24. [Async/Await](#24-asyncawait)
25. [Buenas Prácticas](#25-buenas-prácticas)

---

## 1. Introducción a Python

Python es un lenguaje de programación de alto nivel, interpretado y de propósito general. Fue creado por Guido van Rossum y lanzado en 1991.

### Características principales:

- **Sintaxis clara y legible**: El código Python es fácil de leer y escribir
- **Multiparadigma**: Soporta programación orientada a objetos, funcional y procedural
- **Tipado dinámico**: No es necesario declarar el tipo de las variables
- **Interpretado**: No requiere compilación
- **Gran biblioteca estándar**: Incluye módulos para casi cualquier tarea
- **Portable**: Funciona en Windows, Linux, Mac, etc.

### Usos comunes:

- Desarrollo web (Django, Flask, FastAPI)
- Data Science y Machine Learning
- Automatización y scripting
- DevOps y administración de sistemas
- Testing y QA
- Desarrollo de APIs

---

## 2. Instalación y Configuración

### Instalación

#### Windows:
```bash
# Descargar desde python.org
# O usar chocolatey
choco install python

# Verificar instalación
python --version
pip --version
```

#### Linux (Ubuntu/Debian):
```bash
sudo apt update
sudo apt install python3 python3-pip
python3 --version
pip3 --version
```

#### MacOS:
```bash
# Usar Homebrew
brew install python3
python3 --version
```

### Configurar el entorno

```bash
# Crear un entorno virtual
python -m venv myenv

# Activar entorno (Windows)
myenv\Scripts\activate

# Activar entorno (Linux/Mac)
source myenv/bin/activate

# Instalar paquetes
pip install requests flask pandas

# Guardar dependencias
pip freeze > requirements.txt

# Instalar desde requirements.txt
pip install -r requirements.txt
```

---

## 3. Sintaxis Básica

### Hola Mundo

```python
print("¡Hola, Mundo!")
```

### Variables

```python
# No es necesario declarar el tipo
nombre = "Juan"
edad = 25
altura = 1.75
es_estudiante = True

# Asignación múltiple
x, y, z = 1, 2, 3

# Mismo valor para múltiples variables
a = b = c = 100
```

### Comentarios

```python
# Comentario de una línea

"""
Comentario
de múltiples
líneas
"""

'''
También puedes usar
comillas simples
'''
```

### Indentación

```python
# Python usa indentación para definir bloques de código
if edad >= 18:
    print("Eres mayor de edad")
    print("Puedes votar")
else:
    print("Eres menor de edad")
```

---

## 4. Tipos de Datos

### Tipos Numéricos

```python
# Enteros (int)
x = 10
y = -5
z = 0

# Flotantes (float)
pi = 3.14159
temp = -40.5

# Complejos (complex)
c = 3 + 4j
```

### Cadenas de Texto (strings)

```python
# Comillas simples o dobles
nombre = "María"
apellido = 'González'

# Cadenas multilinea
texto = """
Esta es una cadena
de múltiples líneas
"""

# Concatenación
nombre_completo = nombre + " " + apellido

# F-strings (Python 3.6+)
mensaje = f"Hola {nombre}, tienes {edad} años"

# Métodos de strings
texto = "python es genial"
print(texto.upper())        # PYTHON ES GENIAL
print(texto.capitalize())   # Python es genial
print(texto.replace("python", "Python"))
print(texto.split())        # ['python', 'es', 'genial']
```

### Booleanos

```python
es_verdadero = True
es_falso = False

# Valores que evalúan como False
bool(0)         # False
bool("")        # False
bool(None)      # False
bool([])        # False
bool({})        # False
```

### None

```python
# Representa la ausencia de valor
valor = None

if valor is None:
    print("No hay valor")
```

---

## 5. Operadores

### Aritméticos

```python
a = 10
b = 3

suma = a + b        # 13
resta = a - b       # 7
multiplicacion = a * b  # 30
division = a / b    # 3.333...
division_entera = a // b  # 3
modulo = a % b      # 1
potencia = a ** b   # 1000
```

### Comparación

```python
x = 5
y = 10

x == y    # False (igual a)
x != y    # True (diferente de)
x > y     # False (mayor que)
x < y     # True (menor que)
x >= y    # False (mayor o igual)
x <= y    # True (menor o igual)
```

### Lógicos

```python
a = True
b = False

a and b   # False
a or b    # True
not a     # False
```

### Identidad

```python
x = [1, 2, 3]
y = [1, 2, 3]
z = x

x is z      # True (mismo objeto)
x is y      # False (objetos diferentes)
x == y      # True (mismo contenido)
```

### Pertenencia

```python
lista = [1, 2, 3, 4, 5]

3 in lista      # True
10 in lista     # False
10 not in lista # True
```

---

## 6. Estructuras de Control

### If, Elif, Else

```python
edad = 18

if edad < 13:
    print("Niño")
elif edad < 18:
    print("Adolescente")
elif edad < 65:
    print("Adulto")
else:
    print("Adulto mayor")

# Operador ternario
resultado = "Par" if numero % 2 == 0 else "Impar"
```

### Bucle While

```python
# While básico
contador = 0
while contador < 5:
    print(contador)
    contador += 1

# While con break
while True:
    respuesta = input("Escribe 'salir' para terminar: ")
    if respuesta == "salir":
        break

# While con continue
i = 0
while i < 10:
    i += 1
    if i % 2 == 0:
        continue
    print(i)  # Solo imprime impares
```

### Bucle For

```python
# Iterar sobre una lista
frutas = ["manzana", "pera", "uva"]
for fruta in frutas:
    print(fruta)

# Iterar con range
for i in range(5):  # 0, 1, 2, 3, 4
    print(i)

for i in range(2, 10, 2):  # 2, 4, 6, 8
    print(i)

# Enumerar
for indice, fruta in enumerate(frutas):
    print(f"{indice}: {fruta}")

# Iterar sobre diccionario
persona = {"nombre": "Juan", "edad": 30}
for clave, valor in persona.items():
    print(f"{clave}: {valor}")
```

### Match-Case (Python 3.10+)

```python
def obtener_dia(numero):
    match numero:
        case 1:
            return "Lunes"
        case 2:
            return "Martes"
        case 3:
            return "Miércoles"
        case 4:
            return "Jueves"
        case 5:
            return "Viernes"
        case 6 | 7:
            return "Fin de semana"
        case _:
            return "Número inválido"
```

---

## 7. Funciones

### Definición básica

```python
def saludar():
    print("¡Hola!")

saludar()  # Llamada a la función
```

### Parámetros y argumentos

```python
# Parámetros posicionales
def sumar(a, b):
    return a + b

resultado = sumar(5, 3)  # 8

# Parámetros con valores por defecto
def saludar(nombre="Usuario"):
    return f"Hola, {nombre}"

print(saludar())         # Hola, Usuario
print(saludar("Ana"))    # Hola, Ana

# Argumentos nombrados
def crear_usuario(nombre, edad, ciudad):
    return f"{nombre}, {edad} años, de {ciudad}"

usuario = crear_usuario(edad=25, nombre="Juan", ciudad="Madrid")
```

### *args y **kwargs

```python
# *args - argumentos posicionales variables
def sumar_todos(*numeros):
    return sum(numeros)

print(sumar_todos(1, 2, 3, 4, 5))  # 15

# **kwargs - argumentos nombrados variables
def crear_perfil(**datos):
    for clave, valor in datos.items():
        print(f"{clave}: {valor}")

crear_perfil(nombre="Ana", edad=28, ciudad="Barcelona")
```

### Funciones Lambda

```python
# Función lambda (anónima)
cuadrado = lambda x: x ** 2
print(cuadrado(5))  # 25

# Usar con map, filter, reduce
numeros = [1, 2, 3, 4, 5]
cuadrados = list(map(lambda x: x ** 2, numeros))
pares = list(filter(lambda x: x % 2 == 0, numeros))

from functools import reduce
suma_total = reduce(lambda x, y: x + y, numeros)
```

### Docstrings

```python
def calcular_area_circulo(radio):
    """
    Calcula el área de un círculo.
    
    Args:
        radio (float): El radio del círculo
        
    Returns:
        float: El área del círculo
        
    Ejemplo:
        >>> calcular_area_circulo(5)
        78.53981633974483
    """
    import math
    return math.pi * radio ** 2

# Acceder al docstring
print(calcular_area_circulo.__doc__)
```

---

## 8. Estructuras de Datos

### Listas

```python
# Crear listas
numeros = [1, 2, 3, 4, 5]
mixta = [1, "dos", 3.0, True]
vacia = []

# Acceder a elementos
primer_elemento = numeros[0]   # 1
ultimo_elemento = numeros[-1]  # 5

# Slicing
sublista = numeros[1:4]  # [2, 3, 4]
reversa = numeros[::-1]  # [5, 4, 3, 2, 1]

# Métodos de listas
numeros.append(6)          # Agregar al final
numeros.insert(0, 0)       # Insertar en posición
numeros.remove(3)          # Eliminar por valor
elemento = numeros.pop()   # Eliminar y retornar último
numeros.sort()             # Ordenar
numeros.reverse()          # Invertir
cantidad = numeros.count(2)  # Contar ocurrencias
indice = numeros.index(4)    # Encontrar índice
```

### Tuplas

```python
# Crear tuplas (inmutables)
coordenadas = (10, 20)
persona = ("Juan", 30, "Madrid")

# Desempaquetado
x, y = coordenadas
nombre, edad, ciudad = persona

# Tupla de un elemento
una_tupla = (42,)  # Nota la coma

# Métodos
posicion = persona.index("Madrid")
cantidad = persona.count(30)
```

### Sets (Conjuntos)

```python
# Crear sets (sin duplicados, no ordenados)
numeros = {1, 2, 3, 4, 5}
frutas = {"manzana", "pera", "uva"}

# Operaciones de conjuntos
set1 = {1, 2, 3, 4}
set2 = {3, 4, 5, 6}

union = set1 | set2           # {1, 2, 3, 4, 5, 6}
interseccion = set1 & set2    # {3, 4}
diferencia = set1 - set2      # {1, 2}
dif_simetrica = set1 ^ set2   # {1, 2, 5, 6}

# Métodos
numeros.add(6)
numeros.remove(3)
numeros.discard(10)  # No da error si no existe
```

### Diccionarios

```python
# Crear diccionarios
persona = {
    "nombre": "Ana",
    "edad": 28,
    "ciudad": "Madrid"
}

# Acceder a valores
nombre = persona["nombre"]
edad = persona.get("edad")  # Más seguro
altura = persona.get("altura", 1.70)  # Valor por defecto

# Modificar
persona["edad"] = 29
persona["profesion"] = "Ingeniera"

# Métodos
claves = persona.keys()
valores = persona.values()
items = persona.items()

# Eliminar
del persona["ciudad"]
profesion = persona.pop("profesion")

# Diccionario por comprensión
cuadrados = {x: x**2 for x in range(1, 6)}
```

---

## 9. Programación Orientada a Objetos

### Clases y Objetos

```python
class Persona:
    # Constructor
    def __init__(self, nombre, edad):
        self.nombre = nombre
        self.edad = edad
    
    # Método de instancia
    def saludar(self):
        return f"Hola, soy {self.nombre}"
    
    # Método especial __str__
    def __str__(self):
        return f"Persona(nombre={self.nombre}, edad={self.edad})"

# Crear objetos
persona1 = Persona("Juan", 30)
persona2 = Persona("Ana", 25)

print(persona1.saludar())
print(persona1)
```

### Herencia

```python
class Animal:
    def __init__(self, nombre):
        self.nombre = nombre
    
    def hacer_sonido(self):
        pass

class Perro(Animal):
    def hacer_sonido(self):
        return "¡Guau!"

class Gato(Animal):
    def hacer_sonido(self):
        return "¡Miau!"

perro = Perro("Rex")
print(perro.hacer_sonido())  # ¡Guau!
```

### Encapsulamiento

```python
class CuentaBancaria:
    def __init__(self, titular, saldo_inicial):
        self.titular = titular
        self.__saldo = saldo_inicial  # Atributo privado
    
    def depositar(self, cantidad):
        if cantidad > 0:
            self.__saldo += cantidad
            return True
        return False
    
    def retirar(self, cantidad):
        if 0 < cantidad <= self.__saldo:
            self.__saldo -= cantidad
            return True
        return False
    
    def obtener_saldo(self):
        return self.__saldo

cuenta = CuentaBancaria("Juan", 1000)
cuenta.depositar(500)
print(cuenta.obtener_saldo())  # 1500
```

### Métodos de Clase y Estáticos

```python
class Matematicas:
    PI = 3.14159
    
    @classmethod
    def area_circulo(cls, radio):
        return cls.PI * radio ** 2
    
    @staticmethod
    def es_par(numero):
        return numero % 2 == 0

# Usar sin crear instancia
area = Matematicas.area_circulo(5)
resultado = Matematicas.es_par(10)
```

### Propiedades

```python
class Rectangulo:
    def __init__(self, ancho, alto):
        self._ancho = ancho
        self._alto = alto
    
    @property
    def area(self):
        return self._ancho * self._alto
    
    @property
    def ancho(self):
        return self._ancho
    
    @ancho.setter
    def ancho(self, valor):
        if valor > 0:
            self._ancho = valor

rect = Rectangulo(5, 10)
print(rect.area)  # 50
rect.ancho = 7
print(rect.area)  # 70
```

### Herencia Múltiple

```python
class Volador:
    def volar(self):
        return "Estoy volando"

class Nadador:
    def nadar(self):
        return "Estoy nadando"

class Pato(Volador, Nadador):
    def __init__(self, nombre):
        self.nombre = nombre

pato = Pato("Donald")
print(pato.volar())
print(pato.nadar())
```

---

## 10. Módulos y Paquetes

### Importar módulos

```python
# Importar módulo completo
import math
resultado = math.sqrt(16)

# Importar específico
from math import sqrt, pi
resultado = sqrt(16)

# Importar con alias
import numpy as np
array = np.array([1, 2, 3])

# Importar todo (no recomendado)
from math import *
```

### Crear un módulo

```python
# archivo: mi_modulo.py
def saludar(nombre):
    return f"Hola, {nombre}"

PI = 3.14159

class Calculadora:
    @staticmethod
    def sumar(a, b):
        return a + b

# archivo: main.py
import mi_modulo

print(mi_modulo.saludar("Juan"))
print(mi_modulo.PI)
calc = mi_modulo.Calculadora()
```

### Paquetes

```python
# Estructura de paquete
mi_paquete/
    __init__.py
    modulo1.py
    modulo2.py
    subpaquete/
        __init__.py
        modulo3.py

# Importar desde paquete
from mi_paquete import modulo1
from mi_paquete.subpaquete import modulo3
```

### Módulos útiles de la biblioteca estándar

```python
# os - Sistema operativo
import os
directorio_actual = os.getcwd()
os.listdir('.')
os.path.exists('archivo.txt')

# sys - Sistema
import sys
print(sys.version)
sys.exit()

# datetime - Fechas
from datetime import datetime
ahora = datetime.now()

# random - Números aleatorios
import random
numero = random.randint(1, 100)
random.choice(['rojo', 'verde', 'azul'])

# json - JSON
import json
datos = json.loads('{"nombre": "Juan"}')
texto_json = json.dumps({"edad": 30})

# re - Expresiones regulares
import re
patron = re.compile(r'\d+')
numeros = patron.findall('Tengo 2 gatos y 3 perros')
```

---

## 11. Manejo de Archivos

### Leer archivos

```python
# Leer archivo completo
with open('archivo.txt', 'r', encoding='utf-8') as archivo:
    contenido = archivo.read()
    print(contenido)

# Leer línea por línea
with open('archivo.txt', 'r') as archivo:
    for linea in archivo:
        print(linea.strip())

# Leer todas las líneas en una lista
with open('archivo.txt', 'r') as archivo:
    lineas = archivo.readlines()
```

### Escribir archivos

```python
# Escribir (sobreescribe)
with open('archivo.txt', 'w', encoding='utf-8') as archivo:
    archivo.write("Primera línea\n")
    archivo.write("Segunda línea\n")

# Añadir al final
with open('archivo.txt', 'a') as archivo:
    archivo.write("Línea adicional\n")

# Escribir múltiples líneas
lineas = ["Línea 1\n", "Línea 2\n", "Línea 3\n"]
with open('archivo.txt', 'w') as archivo:
    archivo.writelines(lineas)
```

### Trabajar con CSV

```python
import csv

# Leer CSV
with open('datos.csv', 'r') as archivo:
    lector = csv.reader(archivo)
    for fila in lector:
        print(fila)

# Leer CSV como diccionario
with open('datos.csv', 'r') as archivo:
    lector = csv.DictReader(archivo)
    for fila in lector:
        print(fila['nombre'], fila['edad'])

# Escribir CSV
datos = [
    ['nombre', 'edad', 'ciudad'],
    ['Juan', '30', 'Madrid'],
    ['Ana', '25', 'Barcelona']
]

with open('salida.csv', 'w', newline='') as archivo:
    escritor = csv.writer(archivo)
    escritor.writerows(datos)
```

### Trabajar con JSON

```python
import json

# Leer JSON
with open('datos.json', 'r') as archivo:
    datos = json.load(archivo)

# Escribir JSON
datos = {
    "nombre": "Juan",
    "edad": 30,
    "hobbies": ["programar", "leer"]
}

with open('salida.json', 'w') as archivo:
    json.dump(datos, archivo, indent=4, ensure_ascii=False)
```

---

## 12. Excepciones

### Try-Except básico

```python
try:
    numero = int(input("Introduce un número: "))
    resultado = 10 / numero
    print(resultado)
except ValueError:
    print("Error: Debes introducir un número válido")
except ZeroDivisionError:
    print("Error: No se puede dividir por cero")
except Exception as e:
    print(f"Error inesperado: {e}")
```

### Try-Except-Else-Finally

```python
try:
    archivo = open('datos.txt', 'r')
    contenido = archivo.read()
except FileNotFoundError:
    print("El archivo no existe")
except PermissionError:
    print("No tienes permisos para leer el archivo")
else:
    # Se ejecuta si no hay excepciones
    print("Archivo leído correctamente")
finally:
    # Se ejecuta siempre
    if 'archivo' in locals():
        archivo.close()
```

### Lanzar excepciones

```python
def dividir(a, b):
    if b == 0:
        raise ValueError("El divisor no puede ser cero")
    return a / b

try:
    resultado = dividir(10, 0)
except ValueError as e:
    print(e)
```

### Excepciones personalizadas

```python
class EdadInvalidaError(Exception):
    def __init__(self, edad, mensaje="Edad no válida"):
        self.edad = edad
        self.mensaje = mensaje
        super().__init__(self.mensaje)

def validar_edad(edad):
    if edad < 0 or edad > 120:
        raise EdadInvalidaError(edad)
    return True

try:
    validar_edad(150)
except EdadInvalidaError as e:
    print(f"Error: {e.mensaje}, edad recibida: {e.edad}")
```

---

## 13. Decoradores

### Decoradores básicos

```python
def mi_decorador(funcion):
    def wrapper(*args, **kwargs):
        print("Antes de ejecutar la función")
        resultado = funcion(*args, **kwargs)
        print("Después de ejecutar la función")
        return resultado
    return wrapper

@mi_decorador
def saludar(nombre):
    print(f"Hola, {nombre}")

saludar("Juan")
```

### Decoradores con parámetros

```python
def repetir(veces):
    def decorador(funcion):
        def wrapper(*args, **kwargs):
            for _ in range(veces):
                resultado = funcion(*args, **kwargs)
            return resultado
        return wrapper
    return decorador

@repetir(3)
def decir_hola():
    print("¡Hola!")

decir_hola()  # Imprime "¡Hola!" 3 veces
```

### Decoradores útiles

```python
import time
from functools import wraps

# Medir tiempo de ejecución
def medir_tiempo(funcion):
    @wraps(funcion)
    def wrapper(*args, **kwargs):
        inicio = time.time()
        resultado = funcion(*args, **kwargs)
        fin = time.time()
        print(f"{funcion.__name__} tardó {fin - inicio:.4f} segundos")
        return resultado
    return wrapper

@medir_tiempo
def proceso_lento():
    time.sleep(2)
    return "Completado"

# Caché simple
def cache(funcion):
    resultados = {}
    @wraps(funcion)
    def wrapper(*args):
        if args in resultados:
            return resultados[args]
        resultado = funcion(*args)
        resultados[args] = resultado
        return resultado
    return wrapper

@cache
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)
```

---

## 14. Generadores e Iteradores

### Generadores

```python
# Generador simple
def contar_hasta(n):
    i = 1
    while i <= n:
        yield i
        i += 1

for numero in contar_hasta(5):
    print(numero)

# Generador con expresión
cuadrados = (x**2 for x in range(10))
print(list(cuadrados))

# Generador Fibonacci
def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

fib = fibonacci()
for _ in range(10):
    print(next(fib))
```

### Iteradores personalizados

```python
class ContadorDescendente:
    def __init__(self, inicio):
        self.actual = inicio
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.actual <= 0:
            raise StopIteration
        self.actual -= 1
        return self.actual + 1

contador = ContadorDescendente(5)
for num in contador:
    print(num)  # 5, 4, 3, 2, 1
```

---

## 15. Comprehensions

### List Comprehension

```python
# Básico
cuadrados = [x**2 for x in range(10)]

# Con condición
pares = [x for x in range(20) if x % 2 == 0]

# Con if-else
valores = [x if x % 2 == 0 else -x for x in range(10)]

# Anidado
matriz = [[i+j for j in range(3)] for i in range(3)]
```

### Dictionary Comprehension

```python
# Crear diccionario
cuadrados = {x: x**2 for x in range(1, 6)}

# Invertir diccionario
original = {'a': 1, 'b': 2, 'c': 3}
invertido = {v: k for k, v in original.items()}

# Con condición
pares = {x: x**2 for x in range(10) if x % 2 == 0}
```

### Set Comprehension

```python
# Crear set
cuadrados = {x**2 for x in range(-5, 6)}

# Con condición
vocales = {c.lower() for c in "Hola Mundo" if c.lower() in 'aeiou'}
```

---

## 16. Programación Funcional

### Map, Filter, Reduce

```python
from functools import reduce

numeros = [1, 2, 3, 4, 5]

# Map - aplicar función a cada elemento
cuadrados = list(map(lambda x: x**2, numeros))

# Filter - filtrar elementos
pares = list(filter(lambda x: x % 2 == 0, numeros))

# Reduce - reducir a un solo valor
suma = reduce(lambda x, y: x + y, numeros)

# Combinar
resultado = reduce(
    lambda x, y: x + y,
    filter(lambda x: x % 2 == 0,
           map(lambda x: x**2, numeros))
)
```

### Funciones de orden superior

```python
def aplicar_operacion(lista, operacion):
    return [operacion(x) for x in lista]

def cuadrado(x):
    return x ** 2

def cubo(x):
    return x ** 3

numeros = [1, 2, 3, 4, 5]
print(aplicar_operacion(numeros, cuadrado))
print(aplicar_operacion(numeros, cubo))
```

---

## 17. Manejo de Fechas y Tiempo

```python
from datetime import datetime, date, time, timedelta
import calendar

# Fecha y hora actual
ahora = datetime.now()
hoy = date.today()

# Crear fechas específicas
fecha = datetime(2024, 12, 25, 15, 30, 0)
solo_fecha = date(2024, 12, 25)
solo_hora = time(15, 30, 0)

# Formatear fechas
fecha_str = ahora.strftime("%d/%m/%Y %H:%M:%S")
fecha_iso = ahora.isoformat()

# Parsear fechas
fecha_parseada = datetime.strptime("25/12/2024", "%d/%m/%Y")

# Operaciones con fechas
manana = hoy + timedelta(days=1)
la_semana_pasada = ahora - timedelta(weeks=1)

# Diferencia entre fechas
diferencia = fecha - ahora
dias_diferencia = diferencia.days

# Trabajar con calendarios
print(calendar.month(2024, 10))
es_bisiesto = calendar.isleap(2024)
```

---

## 18. Expresiones Regulares

```python
import re

texto = "Mi email es juan@example.com y mi teléfono es 123-456-7890"

# Buscar patrón
email = re.search(r'[\w.-]+@[\w.-]+\.\w+', texto)
if email:
    print(email.group())

# Encontrar todas las coincidencias
numeros = re.findall(r'\d+', texto)

# Reemplazar
texto_limpio = re.sub(r'\d', 'X', texto)

# Compilar patrón para reusar
patron = re.compile(r'\b\w{5}\b')  # Palabras de 5 letras
coincidencias = patron.findall("Hola mundo desde Python")

# Grupos de captura
texto_fecha = "Fecha: 25/12/2024"
match = re.search(r'(\d{2})/(\d{2})/(\d{4})', texto_fecha)
if match:
    dia, mes, año = match.groups()

# Patrones comunes
PATRONES = {
    'email': r'^[\w.-]+@[\w.-]+\.\w+$',
    'telefono': r'^\d{3}-\d{3}-\d{4}$',
    'url': r'https?://[\w.-]+\.\w+',
    'ip': r'\b\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}\b'
}
```

---

## 19. Trabajo con APIs REST

```python
import requests
import json

# GET request
response = requests.get('https://api.example.com/users')
if response.status_code == 200:
    datos = response.json()
    print(datos)

# GET con parámetros
params = {'page': 1, 'limit': 10}
response = requests.get('https://api.example.com/users', params=params)

# POST request
datos_usuario = {
    'nombre': 'Juan',
    'email': 'juan@example.com'
}
response = requests.post(
    'https://api.example.com/users',
    json=datos_usuario,
    headers={'Content-Type': 'application/json'}
)

# PUT request
response = requests.put(
    'https://api.example.com/users/123',
    json={'nombre': 'Juan Actualizado'}
)

# DELETE request
response = requests.delete('https://api.example.com/users/123')

# Manejo de errores
try:
    response = requests.get('https://api.example.com/data', timeout=5)
    response.raise_for_status()  # Lanza excepción si hay error HTTP
    datos = response.json()
except requests.exceptions.Timeout:
    print("La petición tardó demasiado")
except requests.exceptions.HTTPError as e:
    print(f"Error HTTP: {e}")
except requests.exceptions.RequestException as e:
    print(f"Error en la petición: {e}")

# Autenticación
response = requests.get(
    'https://api.example.com/protected',
    auth=('usuario', 'contraseña')
)

# Bearer token
headers = {'Authorization': 'Bearer tu_token_aqui'}
response = requests.get('https://api.example.com/data', headers=headers)
```

---

## 20. Bases de Datos

### SQLite

```python
import sqlite3

# Conectar a base de datos
conn = sqlite3.connect('mi_base_de_datos.db')
cursor = conn.cursor()

# Crear tabla
cursor.execute('''
    CREATE TABLE IF NOT EXISTS usuarios (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        nombre TEXT NOT NULL,
        email TEXT UNIQUE NOT NULL,
        edad INTEGER
    )
''')

# Insertar datos
cursor.execute(
    'INSERT INTO usuarios (nombre, email, edad) VALUES (?, ?, ?)',
    ('Juan', 'juan@example.com', 30)
)

# Insertar múltiples
usuarios = [
    ('Ana', 'ana@example.com', 25),
    ('Pedro', 'pedro@example.com', 35)
]
cursor.executemany(
    'INSERT INTO usuarios (nombre, email, edad) VALUES (?, ?, ?)',
    usuarios
)

# Consultar datos
cursor.execute('SELECT * FROM usuarios')
resultados = cursor.fetchall()
for fila in resultados:
    print(fila)

# Consulta con filtro
cursor.execute('SELECT * FROM usuarios WHERE edad > ?', (25,))
mayores = cursor.fetchall()

# Actualizar
cursor.execute(
    'UPDATE usuarios SET edad = ? WHERE nombre = ?',
    (31, 'Juan')
)

# Eliminar
cursor.execute('DELETE FROM usuarios WHERE nombre = ?', ('Pedro',))

# Confirmar cambios y cerrar
conn.commit()
conn.close()
```

### PostgreSQL con psycopg2

```python
import psycopg2

# Conectar
conn = psycopg2.connect(
    host="localhost",
    database="mi_db",
    user="usuario",
    password="contraseña"
)

cursor = conn.cursor()

# Operaciones similares a SQLite
cursor.execute("SELECT * FROM usuarios")
resultados = cursor.fetchall()

conn.commit()
cursor.close()
conn.close()
```

### MySQL con mysql-connector

```python
import mysql.connector

# Conectar
conn = mysql.connector.connect(
    host="localhost",
    user="usuario",
    password="contraseña",
    database="mi_db"
)

cursor = conn.cursor()

# Operaciones
cursor.execute("SELECT * FROM usuarios")
resultados = cursor.fetchall()

conn.commit()
cursor.close()
conn.close()
```

### SQLAlchemy (ORM)

```python
from sqlalchemy import create_engine, Column, Integer, String
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# Crear engine
engine = create_engine('sqlite:///mi_db.db')
Base = declarative_base()

# Definir modelo
class Usuario(Base):
    __tablename__ = 'usuarios'
    
    id = Column(Integer, primary_key=True)
    nombre = Column(String(50))
    email = Column(String(100), unique=True)
    edad = Column(Integer)

# Crear tablas
Base.metadata.create_all(engine)

# Crear sesión
Session = sessionmaker(bind=engine)
session = Session()

# Insertar
nuevo_usuario = Usuario(nombre='Juan', email='juan@example.com', edad=30)
session.add(nuevo_usuario)
session.commit()

# Consultar
usuarios = session.query(Usuario).all()
usuario = session.query(Usuario).filter_by(nombre='Juan').first()

# Actualizar
usuario.edad = 31
session.commit()

# Eliminar
session.delete(usuario)
session.commit()

session.close()
```

---

## 21. Testing

### unittest

```python
import unittest

def sumar(a, b):
    return a + b

class TestSumar(unittest.TestCase):
    
    def test_sumar_positivos(self):
        self.assertEqual(sumar(2, 3), 5)
    
    def test_sumar_negativos(self):
        self.assertEqual(sumar(-1, -1), -2)
    
    def test_sumar_cero(self):
        self.assertEqual(sumar(5, 0), 5)
    
    def setUp(self):
        # Se ejecuta antes de cada test
        print("Preparando test")
    
    def tearDown(self):
        # Se ejecuta después de cada test
        print("Limpiando test")

if __name__ == '__main__':
    unittest.main()
```

### pytest

```python
# archivo: test_calculadora.py
import pytest

def sumar(a, b):
    return a + b

def dividir(a, b):
    if b == 0:
        raise ValueError("No se puede dividir por cero")
    return a / b

# Tests simples
def test_sumar():
    assert sumar(2, 3) == 5

def test_sumar_negativos():
    assert sumar(-1, -1) == -2

# Parametrizar tests
@pytest.mark.parametrize("a,b,esperado", [
    (2, 3, 5),
    (0, 0, 0),
    (-1, 1, 0),
    (10, -5, 5)
])
def test_sumar_parametrizado(a, b, esperado):
    assert sumar(a, b) == esperado

# Test de excepciones
def test_dividir_por_cero():
    with pytest.raises(ValueError):
        dividir(10, 0)

# Fixtures
@pytest.fixture
def lista_numeros():
    return [1, 2, 3, 4, 5]

def test_usar_fixture(lista_numeros):
    assert len(lista_numeros) == 5
    assert sum(lista_numeros) == 15
```

### doctest

```python
def factorial(n):
    """
    Calcula el factorial de un número.
    
    >>> factorial(5)
    120
    >>> factorial(0)
    1
    >>> factorial(3)
    6
    """
    if n == 0:
        return 1
    return n * factorial(n - 1)

if __name__ == "__main__":
    import doctest
    doctest.testmod()
```

---

## 22. Virtual Environments

### Crear y usar entornos virtuales

```bash
# Crear entorno virtual
python -m venv mi_entorno

# Activar (Windows)
mi_entorno\Scripts\activate

# Activar (Linux/Mac)
source mi_entorno/bin/activate

# Instalar paquetes
pip install requests flask pandas

# Ver paquetes instalados
pip list

# Exportar dependencias
pip freeze > requirements.txt

# Instalar desde requirements.txt
pip install -r requirements.txt

# Desactivar entorno
deactivate
```

### pipenv (alternativa moderna)

```bash
# Instalar pipenv
pip install pipenv

# Crear entorno e instalar paquetes
pipenv install requests flask

# Activar entorno
pipenv shell

# Instalar paquete de desarrollo
pipenv install --dev pytest

# Ver dependencias
pipenv graph

# Ejecutar comando sin activar
pipenv run python script.py
```

### poetry (gestión avanzada)

```bash
# Instalar poetry
pip install poetry

# Crear proyecto nuevo
poetry new mi_proyecto

# Instalar dependencias
poetry add requests

# Instalar dependencias de desarrollo
poetry add --dev pytest

# Activar entorno
poetry shell

# Ejecutar script
poetry run python script.py
```

---

## 23. Frameworks Web

### Flask (Microframework)

```python
from flask import Flask, request, jsonify

app = Flask(__name__)

# Ruta básica
@app.route('/')
def home():
    return '¡Hola, Flask!'

# Ruta con parámetro
@app.route('/usuario/<nombre>')
def usuario(nombre):
    return f'Hola, {nombre}'

# GET request
@app.route('/api/datos')
def obtener_datos():
    datos = {'mensaje': 'Hola', 'codigo': 200}
    return jsonify(datos)

# POST request
@app.route('/api/usuarios', methods=['POST'])
def crear_usuario():
    datos = request.get_json()
    nombre = datos.get('nombre')
    return jsonify({'mensaje': f'Usuario {nombre} creado'}), 201

# Query parameters
@app.route('/buscar')
def buscar():
    termino = request.args.get('q')
    return f'Buscando: {termino}'

if __name__ == '__main__':
    app.run(debug=True)
```

### Django (Framework completo)

```python
# settings.py - Configuración básica
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'mi_app',
]

# models.py - Modelos
from django.db import models

class Usuario(models.Model):
    nombre = models.CharField(max_length=100)
    email = models.EmailField(unique=True)
    fecha_registro = models.DateTimeField(auto_now_add=True)
    
    def __str__(self):
        return self.nombre

# views.py - Vistas
from django.shortcuts import render
from django.http import JsonResponse
from .models import Usuario

def lista_usuarios(request):
    usuarios = Usuario.objects.all()
    return render(request, 'usuarios.html', {'usuarios': usuarios})

def api_usuarios(request):
    usuarios = Usuario.objects.all().values()
    return JsonResponse(list(usuarios), safe=False)

# urls.py - Rutas
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name='home'),
    path('usuarios/', views.lista_usuarios, name='usuarios'),
    path('api/usuarios/', views.api_usuarios, name='api_usuarios'),
]
```

### FastAPI (API moderna y rápida)

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
from typing import List, Optional

app = FastAPI()

# Modelo de datos
class Usuario(BaseModel):
    id: Optional[int] = None
    nombre: str
    email: str
    edad: int

# Base de datos simulada
usuarios_db = []

# GET
@app.get("/")
def read_root():
    return {"mensaje": "¡Hola, FastAPI!"}

# GET con path parameter
@app.get("/usuarios/{usuario_id}")
def obtener_usuario(usuario_id: int):
    for usuario in usuarios_db:
        if usuario['id'] == usuario_id:
            return usuario
    raise HTTPException(status_code=404, detail="Usuario no encontrado")

# GET todos
@app.get("/usuarios", response_model=List[Usuario])
def listar_usuarios():
    return usuarios_db

# POST
@app.post("/usuarios", response_model=Usuario)
def crear_usuario(usuario: Usuario):
    usuario.id = len(usuarios_db) + 1
    usuarios_db.append(usuario.dict())
    return usuario

# PUT
@app.put("/usuarios/{usuario_id}")
def actualizar_usuario(usuario_id: int, usuario: Usuario):
    for i, u in enumerate(usuarios_db):
        if u['id'] == usuario_id:
            usuarios_db[i] = usuario.dict()
            return usuario
    raise HTTPException(status_code=404, detail="Usuario no encontrado")

# DELETE
@app.delete("/usuarios/{usuario_id}")
def eliminar_usuario(usuario_id: int):
    for i, u in enumerate(usuarios_db):
        if u['id'] == usuario_id:
            usuarios_db.pop(i)
            return {"mensaje": "Usuario eliminado"}
    raise HTTPException(status_code=404, detail="Usuario no encontrado")

# Documentación automática en /docs y /redoc
```

---

## 24. Async/Await

### Programación asíncrona básica

```python
import asyncio

# Función asíncrona
async def saludar(nombre, delay):
    print(f"Iniciando saludo para {nombre}")
    await asyncio.sleep(delay)
    print(f"¡Hola, {nombre}!")
    return f"Saludado: {nombre}"

# Ejecutar función asíncrona
async def main():
    resultado = await saludar("Juan", 2)
    print(resultado)

# Correr el programa asíncrono
asyncio.run(main())
```

### Ejecutar múltiples tareas concurrentemente

```python
import asyncio
import time

async def tarea(nombre, duracion):
    print(f"Tarea {nombre} iniciada")
    await asyncio.sleep(duracion)
    print(f"Tarea {nombre} completada")
    return f"Resultado {nombre}"

async def main():
    # Ejecutar en paralelo
    inicio = time.time()
    
    resultados = await asyncio.gather(
        tarea("A", 2),
        tarea("B", 1),
        tarea("C", 3)
    )
    
    fin = time.time()
    print(f"Todas las tareas completadas en {fin - inicio:.2f} segundos")
    print(f"Resultados: {resultados}")

asyncio.run(main())
```

### Peticiones HTTP asíncronas con aiohttp

```python
import aiohttp
import asyncio

async def obtener_datos(url):
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            return await response.json()

async def main():
    urls = [
        'https://api.example.com/data1',
        'https://api.example.com/data2',
        'https://api.example.com/data3'
    ]
    
    tareas = [obtener_datos(url) for url in urls]
    resultados = await asyncio.gather(*tareas)
    
    for resultado in resultados:
        print(resultado)

asyncio.run(main())
```

### FastAPI con operaciones asíncronas

```python
from fastapi import FastAPI
import asyncio

app = FastAPI()

async def proceso_lento(segundos):
    await asyncio.sleep(segundos)
    return f"Proceso completado en {segundos} segundos"

@app.get("/async/{segundos}")
async def endpoint_async(segundos: int):
    resultado = await proceso_lento(segundos)
    return {"resultado": resultado}
```

---

## 25. Buenas Prácticas

### PEP 8 - Estilo de código

```python
# Nombres de variables y funciones: snake_case
mi_variable = 10
def mi_funcion():
    pass

# Nombres de clases: PascalCase
class MiClase:
    pass

# Constantes: MAYUSCULAS
PI = 3.14159
MAX_CONEXIONES = 100

# Líneas de máximo 79 caracteres
# Imports al inicio del archivo
# Una línea en blanco entre funciones
# Dos líneas en blanco entre clases

# Espacios alrededor de operadores
x = 5 + 3  # Correcto
x=5+3      # Incorrecto

# Sin espacios innecesarios
spam(ham[1], {eggs: 2})  # Correcto
spam( ham[ 1 ], { eggs: 2 } )  # Incorrecto
```

### Type Hints

```python
from typing import List, Dict, Optional, Union, Tuple

def saludar(nombre: str) -> str:
    return f"Hola, {nombre}"

def sumar_numeros(numeros: List[int]) -> int:
    return sum(numeros)

def obtener_usuario(user_id: int) -> Optional[Dict[str, any]]:
    # Retorna Dict o None
    pass

def procesar(valor: Union[int, str]) -> bool:
    # Acepta int o str
    pass

def obtener_coordenadas() -> Tuple[float, float]:
    return (10.5, 20.3)
```

### Context Managers

```python
# Usar with para recursos
with open('archivo.txt', 'r') as archivo:
    contenido = archivo.read()
# El archivo se cierra automáticamente

# Crear context manager personalizado
from contextlib import contextmanager

@contextmanager
def mi_contexto():
    print("Entrando al contexto")
    yield
    print("Saliendo del contexto")

with mi_contexto():
    print("Dentro del contexto")
```

### Logging

```python
import logging

# Configurar logging
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    filename='app.log'
)

logger = logging.getLogger(__name__)

# Usar logger en lugar de print
logger.debug("Mensaje de depuración")
logger.info("Información general")
logger.warning("Advertencia")
logger.error("Error")
logger.critical("Error crítico")
```

### Gestión de configuración

```python
# Usar variables de entorno
import os
from dotenv import load_dotenv

load_dotenv()

DATABASE_URL = os.getenv('DATABASE_URL')
API_KEY = os.getenv('API_KEY')
DEBUG = os.getenv('DEBUG', 'False') == 'True'
```

### Principios SOLID

```python
# Single Responsibility Principle
class Usuario:
    def __init__(self, nombre):
        self.nombre = nombre

class UsuarioRepository:
    def guardar(self, usuario):
        # Lógica de persistencia
        pass

class UsuarioService:
    def __init__(self, repository):
        self.repository = repository
    
    def crear_usuario(self, nombre):
        usuario = Usuario(nombre)
        self.repository.guardar(usuario)
        return usuario

# Dependency Injection
class EmailService:
    def enviar(self, destinatario, mensaje):
        pass

class NotificacionService:
    def __init__(self, email_service):
        self.email_service = email_service
    
    def notificar(self, usuario, mensaje):
        self.email_service.enviar(usuario.email, mensaje)
```

### Documentación

```python
"""
Módulo de utilidades matemáticas.

Este módulo proporciona funciones para operaciones matemáticas comunes.
"""

def calcular_promedio(numeros):
    """
    Calcula el promedio de una lista de números.
    
    Args:
        numeros (list): Lista de números (int o float)
    
    Returns:
        float: El promedio de los números
    
    Raises:
        ValueError: Si la lista está vacía
        TypeError: Si la lista contiene elementos no numéricos
    
    Ejemplo:
        >>> calcular_promedio([1, 2, 3, 4, 5])
        3.0
    """
    if not numeros:
        raise ValueError("La lista no puede estar vacía")
    return sum(numeros) / len(numeros)
```

---

## 🎓 Recursos Adicionales

### Documentación Oficial
- [Python.org](https://www.python.org/)
- [PEP 8 - Style Guide](https://peps.python.org/pep-0008/)
- [Python Package Index (PyPI)](https://pypi.org/)

### Librerías Populares
- **Requests**: Cliente HTTP
- **Pandas**: Análisis de datos
- **NumPy**: Computación numérica
- **Matplotlib/Seaborn**: Visualización
- **SQLAlchemy**: ORM para bases de datos
- **Celery**: Tareas asíncronas
- **Beautiful Soup**: Web scraping
- **Pillow**: Procesamiento de imágenes

### Frameworks
- **Django**: Framework web completo
- **Flask**: Microframework web
- **FastAPI**: API moderna y rápida
- **Pyramid**: Framework flexible

### Testing
- **pytest**: Framework de testing moderno
- **unittest**: Testing estándar de Python
- **coverage**: Cobertura de código

---

**¡Feliz programación en Python! 🐍**
