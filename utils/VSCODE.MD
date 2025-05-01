## COMO FUNCIONA VSCODE:

Visual Studio Code (VS Code) es altamente configurable y utiliza varios archivos para ajustar diferentes aspectos de su comportamiento, desde configuraciones generales del editor hasta ajustes específicos de proyectos y sesiones de depuración. A continuación, te presento una guía completa sobre los principales archivos de configuración en VS Code, incluyendo ejemplos detallados y explicaciones.

---

## **1. Archivos principales de configuración en VS Code**

### **a. `settings.json`**
Este archivo se usa para personalizar las configuraciones generales del editor. Tiene dos variantes:
- **Usuario**: Configuración global para el usuario.
- **Espacio de trabajo**: Configuración específica del proyecto.

#### **Ubicación**
- Usuario: `File > Preferences > Settings` (o `Ctrl+,`) y selecciona la vista JSON (ícono en la parte superior derecha).
- Espacio de trabajo: `.vscode/settings.json`.

#### **Ejemplo de configuración**
```json
{
  "editor.tabSize": 4,
  "editor.wordWrap": "on",
  "files.autoSave": "afterDelay",
  "python.defaultInterpreterPath": "/usr/bin/python3",
  "editor.formatOnSave": true,
  "terminal.integrated.defaultProfile.windows": "Git Bash"
}
```

**Algunos ajustes comunes**:
- `editor.tabSize`: Tamaño de las tabulaciones.
- `editor.wordWrap`: Envoltura automática de líneas.
- `files.autoSave`: Auto-guardado (`onFocusChange`, `afterDelay`, etc.).
- `python.defaultInterpreterPath`: Ruta al intérprete de Python.
- `terminal.integrated.defaultProfile`: Perfil predeterminado para la terminal integrada.

---

### **b. `launch.json`**
Este archivo define configuraciones de depuración para el proyecto. Permite especificar cómo se ejecutarán y depurarán los programas.

#### **Ubicación**
Se encuentra en la carpeta `.vscode/launch.json` dentro de tu espacio de trabajo.

#### **Estructura básica**
```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Python: Archivo Actual",
      "type": "python",
      "request": "launch",
      "program": "${file}",
      "console": "integratedTerminal",
      "args": ["--mi-parametro", "valor"],
      "env": {
        "ENV_VAR": "valor"
      }
    },
    {
      "name": "Node.js: Archivo Actual",
      "type": "node",
      "request": "launch",
      "program": "${file}",
      "runtimeArgs": ["--inspect"],
      "args": ["arg1", "arg2"]
    }
  ]
}
```

**Campos importantes**:
- `name`: Nombre del perfil de depuración.
- `type`: Tipo de depurador (`python`, `node`, `java`, etc.).
- `request`: Tipo de acción (`launch` para iniciar el programa, `attach` para conectarse a un programa en ejecución).
- `program`: Ruta del archivo a ejecutar.
- `args`: Argumentos para pasar al programa.
- `env`: Variables de entorno para la ejecución.
- `console`: Dónde se ejecutará el programa (`integratedTerminal`, `externalTerminal`, etc.).

#### **Ejemplo avanzado con argumentos**
Si quieres pasar parámetros específicos a tu programa, usa el campo `args`:
```json
"args": ["--input", "archivo.txt", "--debug"]
```
En este caso, el programa recibirá los argumentos `--input archivo.txt --debug`.

---

### **c. `tasks.json`**
Este archivo se usa para definir tareas automatizadas, como compilar código, ejecutar scripts o cualquier otro proceso repetitivo.

#### **Ubicación**
Se encuentra en `.vscode/tasks.json`.

#### **Estructura básica**
```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Compilar C++",
      "type": "shell",
      "command": "g++",
      "args": ["-o", "main", "main.cpp"],
      "group": {
        "kind": "build",
        "isDefault": true
      },
      "problemMatcher": []
    },
    {
      "label": "Ejecutar Python",
      "type": "process",
      "command": "python",
      "args": ["${file}"]
    }
  ]
}
```

**Campos importantes**:
- `label`: Nombre de la tarea.
- `type`: Tipo de tarea (`shell` para comandos de terminal, `process` para ejecutar un programa).
- `command`: Comando principal que se ejecutará.
- `args`: Argumentos para el comando.
- `group`: Agrupa tareas (`build`, `test`, etc.) y define cuál es la predeterminada.
- `problemMatcher`: Configuración para interpretar errores en la salida.

---

### **d. `extensions.json`**
Este archivo sugiere extensiones que deberían instalarse para el proyecto.

#### **Ubicación**
Se encuentra en `.vscode/extensions.json`.

#### **Estructura**
```json
{
  "recommendations": [
    "ms-python.python",
    "esbenp.prettier-vscode",
    "ms-vscode.cpptools"
  ]
}
```

**Campo importante**:
- `recommendations`: Lista de identificadores de extensiones recomendadas.

---

### **e. `keybindings.json`**
Define atajos de teclado personalizados para VS Code.

#### **Ubicación**
Accede desde `File > Preferences > Keyboard Shortcuts` y haz clic en el ícono de archivo JSON.

#### **Ejemplo**
```json
[
  {
    "key": "ctrl+alt+n",
    "command": "workbench.action.tasks.runTask",
    "args": "Compilar C++"
  },
  {
    "key": "ctrl+shift+d",
    "command": "workbench.action.debug.start"
  }
]
```

---

## **2. Configuraciones específicas de ejecución**

### **a. Pasar parámetros a un programa**
Para pasar parámetros, usa el campo `args` en `launch.json`:
```json
"args": ["--archivo", "data.txt", "--modo", "debug"]
```

El programa recibirá estos argumentos como si los hubieras pasado por línea de comandos:
```bash
python script.py --archivo data.txt --modo debug
```

---

### **b. Definir variables de entorno**
En `launch.json`, usa el campo `env`:
```json
"env": {
  "API_KEY": "12345",
  "DEBUG": "true"
}
```

Esto establece variables de entorno disponibles solo durante la ejecución de este perfil.

---

### **c. Usar variables dinámicas**
Puedes usar variables dinámicas en los archivos de configuración. Algunas comunes:
- `${workspaceFolder}`: Carpeta raíz del proyecto.
- `${file}`: Archivo actual.
- `${relativeFile}`: Ruta relativa del archivo actual.
- `${lineNumber}`: Número de línea actual.
- `${env:VAR}`: Variable de entorno del sistema.

Ejemplo:
```json
"program": "${workspaceFolder}/src/${fileBasenameNoExtension}.py"
```

---

## **3. Configuración combinada para un proyecto típico**
### **Python con argumentos y variables de entorno**
```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Python: Archivo Actual",
      "type": "python",
      "request": "launch",
      "program": "${file}",
      "args": ["--modo", "producción"],
      "env": {
        "API_URL": "https://api.ejemplo.com",
        "DEBUG": "false"
      },
      "console": "integratedTerminal"
    }
  ]
}
```

---

Con estos archivos y configuraciones, puedes personalizar casi todos los aspectos de VS Code para adaptarlo a tus necesidades. ¡Experimenta con estas opciones para optimizar tu flujo de trabajo!

## COMO DEBUGGEAR BIEN:

Debugging en Visual Studio Code (VS Code) es una de las funciones más poderosas y personalizables del editor. Permite depurar código en diversos lenguajes utilizando múltiples configuraciones. Esta guía te llevará a través de **todas las opciones de depuración disponibles** en VS Code y cómo configurarlas.

---

## **1. Conceptos básicos de la depuración en VS Code**

### **¿Qué puedes hacer con el depurador?**
- Colocar **puntos de interrupción** (_breakpoints_).
- Inspeccionar y modificar variables en tiempo de ejecución.
- Monitorear expresiones con el panel **Watch**.
- Navegar por el flujo del programa con herramientas como **Step Over**, **Step Into** y **Step Out**.
- Pasar **parámetros** al programa.
- Ejecutar múltiples configuraciones de depuración simultáneamente.

### **Lenguajes compatibles**
VS Code soporta depuración para lenguajes como Python, JavaScript/TypeScript, C++, Java, Go, PHP, entre otros, mediante extensiones.

---

## **2. Configuración inicial para depuración**

### **a. Añadir un archivo `launch.json`**
1. Ve a la pestaña **Run and Debug** (Ctrl+Shift+D).
2. Haz clic en **Create a launch.json file**.
3. Selecciona el entorno de tu lenguaje (por ejemplo, Python, Node.js, etc.).
4. Esto generará un archivo `.vscode/launch.json` con configuraciones predeterminadas.

### **b. Entender el archivo `launch.json`**
```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Ejecutar archivo actual",
      "type": "python",
      "request": "launch",
      "program": "${file}",
      "console": "integratedTerminal",
      "args": ["--param1", "value1"]
    }
  ]
}
```

#### Campos clave:
- `name`: Nombre de la configuración (aparece en el menú desplegable de configuraciones).
- `type`: El depurador a usar (por ejemplo, `python`, `node`, `java`).
- `request`: Define si se **lanza** (`launch`) un programa o se conecta a uno existente (`attach`).
- `program`: Archivo principal que se ejecutará.
- `args`: Argumentos que se pasarán al programa.
- `console`: Terminal donde se ejecutará (`integratedTerminal` o `externalTerminal`).

---

## **3. Opciones y herramientas del depurador**

### **a. Breakpoints (Puntos de interrupción)**
Los puntos de interrupción permiten pausar la ejecución en líneas específicas del código.

1. **Añadir un breakpoint**:
   - Haz clic en el margen izquierdo junto a una línea de código.
   - Se mostrará un círculo rojo indicando el punto de interrupción.

2. **Tipos de breakpoints**:
   - **Condicionales**:
     - Haz clic derecho sobre un breakpoint y selecciona **Edit Breakpoint**.
     - Escribe una condición (por ejemplo: `x > 5`).
   - **Logpoints**:
     - Genera mensajes de registro sin detener la ejecución.
     - Haz clic derecho en el margen, selecciona **Add Logpoint** y escribe el mensaje.
     - Ejemplo: `Valor de x es: {x}`.

---

### **b. Paneles de depuración**
Durante una sesión de depuración, tendrás acceso a varios paneles interactivos:

1. **Variables**:
   - Muestra las variables locales, globales y de argumentos en el alcance actual.
   - Puedes expandir objetos y listas para inspeccionarlos.
   - Haz doble clic en un valor para modificarlo.

2. **Watch**:
   - Permite monitorear expresiones personalizadas.
   - Haz clic en el botón `+` para agregar una expresión (por ejemplo: `mi_variable * 2`).

3. **Call Stack**:
   - Muestra la pila de llamadas actual.
   - Puedes navegar por las funciones en la pila para inspeccionar valores en diferentes contextos.

4. **Breakpoints**:
   - Lista todos los breakpoints activos en el proyecto.
   - Puedes habilitar/deshabilitar o eliminar breakpoints desde aquí.

---

### **c. Controles de flujo del programa**
Durante la depuración, verás un conjunto de botones para controlar el flujo del programa:

1. **Continue (▶)**:
   - Continúa la ejecución hasta el próximo breakpoint.

2. **Step Over (⤴)**:
   - Avanza a la siguiente línea, pero no entra en funciones llamadas en esa línea.

3. **Step Into (⤵)**:
   - Entra en la función llamada en la línea actual.

4. **Step Out (⤶)**:
   - Sale de la función actual y vuelve al contexto de la llamada.

5. **Restart (⟳)**:
   - Reinicia la sesión de depuración.

6. **Stop (■)**:
   - Termina la sesión de depuración.

---

### **d. Pasar argumentos y variables de entorno**
Puedes pasar argumentos y variables de entorno al programa desde `launch.json`:

- **Ejemplo de argumentos**:
  ```json
  "args": ["--modo", "debug", "--archivo", "datos.txt"]
  ```

- **Ejemplo de variables de entorno**:
  ```json
  "env": {
    "API_KEY": "12345",
    "DEBUG": "true"
  }
  ```

---

## **4. Depuración para lenguajes específicos**

### **a. Python**
- Instala la extensión oficial de Python.
- Configura `launch.json`:
  ```json
  {
    "name": "Python: Archivo Actual",
    "type": "python",
    "request": "launch",
    "program": "${file}",
    "console": "integratedTerminal"
  }
  ```

- Opciones adicionales:
  - Usa `pytest` para depuración de pruebas unitarias.
  - Configura entornos virtuales con `python.defaultInterpreterPath`.

---

### **b. JavaScript/Node.js**
- Configura `launch.json` para ejecutar un archivo Node.js:
  ```json
  {
    "name": "Node.js: Archivo Actual",
    "type": "node",
    "request": "launch",
    "program": "${file}",
    "runtimeArgs": ["--inspect"]
  }
  ```

- **Depuración de aplicaciones web**:
  Instala la extensión **Debugger for Chrome** o **Debugger for Edge** para depurar aplicaciones en navegadores.

---

### **c. C/C++**
- Instala la extensión **C/C++ by Microsoft**.
- Configura `launch.json` para compilar y depurar:
  ```json
  {
    "name": "Depurar C++",
    "type": "cppdbg",
    "request": "launch",
    "program": "${workspaceFolder}/main.exe",
    "args": [],
    "stopAtEntry": false,
    "cwd": "${workspaceFolder}",
    "environment": [],
    "externalConsole": false
  }
  ```

---

### **d. Java**
- Instala la extensión **Debugger for Java**.
- Configura `launch.json` para ejecutar programas Java:
  ```json
  {
    "type": "java",
    "name": "Depurar Java",
    "request": "launch",
    "mainClass": "com.ejemplo.Main",
    "projectName": "MiProyecto"
  }
  ```

---

## **5. Depuración remota**
VS Code permite conectarse a programas que ya están ejecutándose en otro sistema o contenedor.

1. **Configurar depuración remota**:
   - En `launch.json`, usa `request: "attach"` y especifica el host y puerto:
     ```json
     {
       "name": "Adjuntar a un proceso remoto",
       "type": "python",
       "request": "attach",
       "connect": {
         "host": "127.0.0.1",
         "port": 5678
       }
     }
     ```

2. **Ejemplo con Docker**:
   - Usa extensiones como **Remote - Containers** para depurar dentro de contenedores.

---

## **6. Consejos avanzados**
- Usa **multitarget debugging** para depurar varios procesos simultáneamente.
- Configura **debugging snapshots** para monitorear valores sin detener la ejecución (usando Logpoints).
- Usa extensiones como **Error Lens** para combinar depuración con mensajes de error en tiempo real.

---

## WATCHES DEBUGGER:
### **¿Qué son los Watches en VS Code?**

Los **Watches** en Visual Studio Code permiten monitorear valores o expresiones específicas durante una sesión de depuración. Son útiles para inspeccionar cómo cambian las variables o los resultados de cálculos a lo largo del programa **sin necesidad de añadir líneas de impresión (`print`) al código**.

---

### **Cómo usar los Watches en VS Code**

1. **Abrir el panel de depuración**:
   - Haz clic en el ícono de depuración en la barra lateral izquierda (Ctrl+Shift+D) para abrir el **panel de Depuración**.

2. **Añadir una expresión al Watch**:
   - Durante una sesión de depuración, pausa la ejecución en un _breakpoint_.
   - En el panel **Watch**, haz clic en el botón `+` o escribe directamente una expresión, por ejemplo:
     - Una variable: `mi_variable`
     - Una operación: `mi_variable * 2`
     - Una llamada a una función: `mi_funcion(5)`

3. **Ver los valores actualizados**:
   - Cada vez que la ejecución del programa se detenga (en un breakpoint o paso a paso), VS Code evaluará automáticamente las expresiones en el panel **Watch**.
   - Puedes expandir estructuras como listas, objetos o diccionarios para inspeccionar detalles específicos.

4. **Editar o eliminar Watches**:
   - Haz clic derecho en un Watch existente para:
     - **Editar**: Modificar la expresión.
     - **Eliminar**: Quitar el Watch.
   - También puedes eliminarlo con el ícono de la papelera al lado del Watch.

---

### **Cómo usar Watches con ejemplos**

#### **Ejemplo 1: Inspeccionar una variable**
Supongamos que tienes este código en Python:
```python
x = 10
y = 20

def suma(a, b):
    return a + b

resultado = suma(x, y)
print(resultado)
```

Pasos:
1. Coloca un _breakpoint_ en la línea `resultado = suma(x, y)`.
2. Inicia la depuración (F5).
3. En el panel **Watch**, añade las expresiones:
   - `x`
   - `y`
   - `suma(x, y)`

Cuando la ejecución se detenga en el _breakpoint_, verás los valores de `x`, `y` y el resultado de `suma(x, y)` en tiempo real.

#### **Ejemplo 2: Monitorear estructuras complejas**
Para el siguiente código:
```python
data = {"a": 1, "b": [10, 20, 30], "c": {"d": 4}}

def obtener_valor(clave):
    return data.get(clave, "No encontrado")

valor = obtener_valor("b")
```

Pasos:
1. Detén la ejecución en la línea `valor = obtener_valor("b")`.
2. Añade al Watch:
   - `data`
   - `data['c']`
   - `obtener_valor("a")`

El panel **Watch** mostrará:
- Los valores completos del diccionario `data`.
- El valor anidado `data['c']`.
- El resultado de `obtener_valor("a")`.

---

### **Cómo reevaluar una función o expresión varias veces**

Reevaluar una función o expresión manualmente durante la depuración te permite comprobar resultados dinámicos.

#### **1. Usando el Panel Watch**
- Cuando añades una expresión al **Watch**, esta se reevaluará automáticamente cada vez que el programa:
  - Se detenga en un _breakpoint_.
  - Ejecute pasos (Step Over, Step Into, etc.).
  
Por ejemplo:
   - Si tienes un Watch con `mi_funcion(x)`, cambiar el valor de `x` en la consola reevaluará automáticamente la expresión.

#### **2. Reevaluar manualmente en la consola de depuración**
1. Detén la ejecución en un _breakpoint_.
2. Abre la consola de depuración (`Ctrl+Shift+Y` o desde la pestaña **Debug Console**).
3. Escribe el nombre de la función o expresión que quieres reevaluar.
   - Ejemplo: `mi_funcion(10)` o `resultado + 5`.
4. Cada vez que lo escribas y presiones `Enter`, se ejecutará de nuevo en el contexto actual.

---

### **Ventajas de usar Watches para reevaluar funciones**
- Los Watches son ideales para **monitoreo continuo**.
- Sin embargo, si quieres modificar parámetros y ejecutar una función varias veces, la **consola de depuración** ofrece más flexibilidad.

---

### **Consejos avanzados**
1. **Combina Watches y modificación de valores**:
   - Puedes editar el valor de una variable desde el panel **Variables** (doble clic sobre el valor) y ver cómo afectan los cambios a las expresiones del Watch.
   
2. **Usa Watches con Logpoints**:
   - Si no quieres pausar el programa, usa un _Logpoint_ para imprimir valores en la consola sin detener la ejecución.

3. **Monitorea funciones puras**:
   - Las funciones que no dependen del estado del programa (funciones puras) son ideales para los Watches, ya que puedes reevaluarlas fácilmente sin afectar el flujo del programa.

4. **Monitoreo complejo**:
   - Usa expresiones complejas como `mi_lista[0] if len(mi_lista) > 0 else "Vacío"` en el Watch para manejar casos especiales.

---

Con los **Watches** y la consola de depuración, puedes monitorear variables y ejecutar funciones múltiples veces de manera interactiva, sin detener ni reiniciar tu programa. Esto facilita la experimentación y el entendimiento del flujo lógico del código.