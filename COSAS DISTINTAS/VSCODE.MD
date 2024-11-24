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