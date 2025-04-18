# RESUMEN DE GIT:

Git es el sistema de control de versiones más usada en la actualidad.
Git nos permite tener un historial completo de todo el código y cambios que hemos realizado en nuestra aplicación.

https://gist.github.com/dasdo/9ff71c5c0efa037441b6

Vamos a tener dos repositorios, uno remoto y otro local, en el local vamos haciendo los cambios y una vez tenemos ciertos cambios que queremos añadir a la rama principal los subimos al repositorio remoto

Git está diseñado para que varias personas trabajen a la vez, tenemos el repositorio remoto central, y cada uno se crea una copia local de ese repositorio y van haciendo sus cambios, y una vez los tienen los suben y los juntan más adelante, el cómo lo veremos después.

Con GIT vamos a tener:
- Historial
- Almacenar código
- Trabajar en equipo
- Cuando se introdujo un error (podemos ir viendo en el historial para ver cuando se introdujo un error).


Existen muchas herramientas gráficas, pero lo mejor siempre es usar la terminal.

Para utilizar git en windows lo mejor es usar git Bash, no usar la terminal de windows!!



## CONFIGURACION BASICA:

Para aplicar la configuración global y no por proyecto:
```
git config --global <configuracion>
```

Configurar tu nombre:
```
git config --global user.name "username or name"
```

Configurar tu correo:

```
git config --global user.email correo@correo.com
```

Para indicar nuestro editor por defecto(el --wait es para que el terminal se quede esperando hasta que cerremos el editor):

```
git config --global core.editor "code --wait"
```

Para ver nuestro archivo de configuración global:
```
git config global -e
```

En windows hay 2 caracteres de salto de linea, el CR y el LF, y en linux y en mac solo el LF, entonces para hacer compatible esto tenemos que activar la opción:
```
git config --global core.autocrlf <true || input>
```

Para ayuda con la configuración de git:
```
git config -h
```


## CREAMOS REPOSITORIO:

Lo primero va a ser crear un repositorio de git y para ello comenzamos creando un directorio donde queremos que se localice nuestro repositorio, seguido de esto escribiremos el comando:

```
git init
```
Se creará una carpeta oculta .git donde estarán las entrañas de git

## FLUJO DE TRABAJO CON GIT:

Vamos a tener 4 etapas con git < PROGRAMACION, STAGE, COMMIT, REMOTO >


Primero vamos a ir haciendo nuestros cambios en local y cuando tengamos unos cambios hechos y verifiquemos que esos son los cambios que queremos haremos un 
```
git add <nombrefichero>
```
para añadir el fichero al stage (que es una etapa previa al commit, donde vas metiendo las cosas de las que quieres hacer checkpoint), en el stage están los cambios que nosotros queremos que pasen al repositorio, los que no queramos no los metemos

Cuando ejecutamos el comando de
```
git commit -m "mensaje"
```
se añaden los archivos a una nueva etapa que es la etapa de commit, que son como checkpoints de los archivos que vas guardando poco a poco en local y a las que puedes volver en cualquier momento. s como una "foto" del estado del proyecto en ese momento, con un mensaje descriptivo que explica qué modificaciones se hicieron y por qué. Cada commit tiene un identificador único y permite volver a esa versión en cualquier momento.


Una vez ejecutamos el comando de commit, el stage se vacía, y todos los archivos se guardan en el commit

Cuando hacemos el commit, lo hacemos un mensaje que describe los cambios realizados en dicho commit

Una vez terminemos de desarrollar hasta cierto punto y deseamos comprometer los cambios y juntarlos con los demás haremos un:
```
git push
```
y todos nuestros cambios se subiran al repositorio remoto. Haces un commit para guardar los cambios localmente con una descripción, luego usas git push para enviarlos al repositorio remoto, y así tus cambios se sincronizan y están disponibles en el servidor.

NOTA: Si eliminamos un archivo debemos hacer lo mismo de git add y todo con dicho archivo.

## GITIGNORE:

Cuando hay archivos que no queremos añadir al commit, podemos crear un archivo .gitignore y en él añadir los nombres de los archivos que no queremos que se añadan al commit

## COMENZAMOS TRABAJANDO CON GIT:

```
git status
```
Con git status podemos ver el estado actual del repositorio, nos aparecen los commits que tenemos sin subir, los archivos que tenemos trackeados y los que no

```
git restore <file>
```
Restaura un archivo a los del último stage, es decir si tienes cambios en un archivo sin haberle hecho git add, y le haces un restore lo restaura al anterior stage.
En resumen descarta los cambios que hayamos hecho sobre un archivo

```
git restore --staged <file>
```
Restaura un cambio que nosotros habíamos pasado a la etapa de stage


NOTA: En git cuando subes los cambios, no subes los archivos mismos, lo que subes son las modificaciones que hicimos sobre estos archivos, lo que nos permite ahorrar muchísimo espacio


```
git rm <file>
```
Elimina un archivo y a la vez hace un git add

```
git mv <filename> <newfilename>
```
Renombra un archivo y a la vez hace un git add

```
git status -s
```
Hace un resumen de git status y lo muestra todo de manera mucho más resumida.

```
git diff <filename>
```
Muestra los cambios que hicimos en un archivo

```
git log --oneline
```
Muestra un resumen del historial de commits con un hash y su mensaje

## TRABAJAR CON RAMAS:

Las ramas en Git permiten trabajar en una copia del proyecto sin afectar la versión principal, como hacer una nueva "línea de trabajo". Puedes hacer cambios en tu rama y, cuando estén listos, combinarlos con la principal usando merge, que une ambas ramas manteniendo los cambios de cada una. Otra opción es rebase, que reubica tu trabajo como si hubieras partido desde la última versión de la rama principal, creando un historial más limpio y lineal. Esto ayuda a probar ideas o desarrollar funciones sin interferir con el trabajo de los demás y luego integrarlas de forma organizada.

```
git branch
```
Muestra las ramas actuales

```
git branch newrama
```
Crea la rama newrama sobre la rama actual

```
git checkout newrama
```
Cambiamos a la rama newrama

```
git checkout -b newrama
```
Crea la rama newrama y cambia a ella


## GIT REBASE:
Estando en rama1 aplicamos el comando:
```
git rebase rama2
```
Este comando se usa desde tu rama actual (en este caso, asumimos que estás en rama1). Ejecutar git rebase rama2 moverá el historial de commits de rama1 para que se apliquen a partir del último commit de rama2. Esto esencialmente pone a rama1 al día con los cambios de rama2 en una línea recta.

Es decir, imaginémonos que rama1 es main y rama2 es una rama que parte de main y sobre la que se han hecho unos commits, cuando aplicamos git rebase rama2 va a traer los cambios de rama2 a rama1 colocándolos linearmente sus commits por delante de los de rama1 y poniendo la cabeza de rama1 donde la de rama2.

Caso 1: Haciendo git rebase ramaremota desde mirama
Si estás en la rama mirama y haces git rebase ramaremota, los commits de mirama se reaplicarán encima de los últimos commits de ramaremota. Esto actualizará mirama con los cambios más recientes de ramaremota, creando un historial lineal donde parece que los cambios de mirama se hicieron directamente sobre la versión más reciente de ramaremota.
(ramaremota permanece igual y mirama es la que sufre los cambios)

Caso 2: Haciendo git rebase mirama desde main (ambas ramas con cambios, pero mirama parte de main)
Si estás en la rama main y haces git rebase mirama, los commits de main se moverán para aplicarse después de los commits de mirama. Esto significa que main incluirá primero los cambios de mirama y luego sus propios cambios, resultando en un historial lineal. Sin embargo, este método reescribe el historial de main, lo que puede causar problemas si ya ha sido compartido.

Caso 3: Haciendo git rebase mirama desde main cuando main está por delante
Si en main, que tiene commits adicionales que no están en mirama, haces git rebase mirama, los commits de main se aplicarán después de los cambios de mirama. Esto significa que main incluirá primero los cambios de mirama, seguidos de sus propios commits, creando un historial lineal. Como en el caso anterior, este rebase reescribe el historial de main, lo que puede ser problemático si se ha compartido con otros colaboradores.

## GIT MERGE:

Estoy en main y ejecuto:
```
git merge mirama
```
Al realizar un git merge, simplemente unes dos ramas en una sola, combinando sus cambios. Por ejemplo, si estás en main y haces git merge mirama, Git toma los cambios de mirama y los incorpora en main, creando un nuevo commit de "fusión" que refleja el historial de ambas ramas. Este proceso preserva el historial de ambas ramas, mostrando claramente de dónde provienen los cambios. Es una operación directa y suele ser preferida cuando se trabaja en colaboración, ya que no reescribe el historial, evitando conflictos para otros colaboradores.

Aunque merge es más simple, el resultado es un historial que puede ser menos lineal que con rebase, lo que podría hacer que sea más difícil de seguir en proyectos grandes. Sin embargo, su simplicidad y la preservación del historial lo hacen adecuado para muchas situaciones, especialmente en entornos de trabajo colaborativo.

IMPORTANTE: Cuando se hace el Merge se realiza un commit de unión.

```
git merge --squash mirama
```
Si estando en rama main y después de ejecutar este comando, tendrás todos los cambios de mirama listos en el área de preparación, pero no habrás hecho aún un commit. Puedes revisar y luego hacer un solo commit que contenga todos esos cambios para subir a main.


## DESHACER CAMBIOS EN GIT:

En Git, tanto **`revert`** como **`reset`** se utilizan para deshacer cambios, pero funcionan de manera diferente y tienen propósitos distintos:

---

### **1. `git revert`**

El comando `git revert` se utiliza para **crear un nuevo commit que deshace los cambios de un commit específico**, sin alterar el historial existente. Esto lo hace ideal para entornos colaborativos.

#### **Características de `git revert`**:
- **Crea un nuevo commit**: No elimina el commit original, sino que agrega un nuevo commit inverso.
- **Seguro en colaboración**: Mantiene el historial completo, lo que evita conflictos con otros desarrolladores.
- **Es reversible**: Puedes volver a revertir si cambias de opinión.

#### **Ejemplo:**
Si tienes un historial como este:
```
A → B → C → D
```
Y decides revertir el commit `C`, el historial se verá así después de usar `git revert C`:
```
A → B → C → D → C' (revert C)
```

El commit `C'` contiene los cambios necesarios para deshacer `C`.

#### **Cuándo usar `git revert`**:
- Cuando trabajas en un repositorio compartido y necesitas deshacer un cambio sin afectar el historial.
- Para mantener el historial limpio y rastreable.

---

### **2. `git reset`**

El comando `git reset` **mueve el puntero de tu rama a un commit anterior**, eliminando los commits posteriores del historial (dependiendo de la opción usada). Puede alterar el historial de manera drástica.

#### **Opciones principales de `git reset`**:

1. **`--soft`**: Conserva los cambios realizados en los commits eliminados en el área de preparación (staging area).
   - Solo mueve el puntero de la rama, pero los cambios siguen listos para commitear.
2. **`--mixed`** (por defecto): Conserva los cambios en el directorio de trabajo, pero los elimina del área de preparación.
   - Los cambios están sin preparar para el commit.
3. **`--hard`**: Elimina los commits y borra los cambios del directorio de trabajo. **Cuidado: los cambios se pierden definitivamente**.

#### **Ejemplo:**
Si tienes un historial como este:
```
A → B → C → D
```
Y haces `git reset --hard B`, el historial se verá así:
```
A → B
```
Los commits `C` y `D` desaparecen y los cambios asociados se pierden (en caso de usar `--hard`).

#### **Cuándo usar `git reset`**:
- Cuando trabajas en un repositorio personal y necesitas eliminar commits recientes.
- Si quieres modificar commits sin mantener su rastro en el historial.
- Para eliminar errores recientes sin necesidad de compartirlos.

---

### **Diferencias principales entre `revert` y `reset`**

| Característica                | `git revert`                       | `git reset`                          |
|-------------------------------|-------------------------------------|---------------------------------------|
| **Efecto en el historial**    | Crea un nuevo commit inverso       | Elimina commits (puede sobrescribir) |
| **Seguridad en colaboración** | Seguro (no altera el historial)    | No seguro (puede causar conflictos)  |
| **Uso principal**             | Deshacer un commit específico      | Retroceder el estado del repositorio |
| **Perdida de datos**          | No (los cambios quedan rastreables)| Depende de la opción (`--hard` borra)|
| **Ideal para**                | Repositorios compartidos           | Repositorios personales              |

---

### **Ejemplo práctico combinado**
1. Cometiste un error en el commit más reciente:
   - Si estás colaborando: Usa `git revert <commit>`.
   - Si trabajas solo y quieres eliminarlo completamente: Usa `git reset --hard <commit>`.

2. Quieres deshacer múltiples commits en el historial:
   - Si quieres eliminar cambios y borrar los commits: Usa `git reset`.
   - Si quieres mantener un registro de los cambios revertidos: Usa `git revert`.

---

En resumen:
- Usa **`git revert`** para mantener un historial limpio y seguro en equipos.
- Usa **`git reset`** para borrar commits y cambios recientes en un entorno controlado.


---
---

## `git rebase --onto`

El comando `git rebase --onto` es una herramienta avanzada en Git que permite mover un rango específico de commits de una rama y re-aplicarlos sobre una nueva base. Aunque el resultado puede parecer similar a un rebase "normal", `--onto` proporciona mayor control sobre qué commits se seleccionan y dónde se aplican.

---

### Estructura del comando

La sintaxis básica es la siguiente:

```bash
git rebase --onto <nueva-base> <desde-dónde> <rama>
```

- **`<nueva-base>`**: Es el commit o rama sobre el cual quieres mover y re-aplicar los commits seleccionados.
- **`<desde-dónde>`**: Define el commit anterior al rango de commits que serán seleccionados. Los commits hasta este punto **no serán re-aplicados**.
- **`<rama>`**: La rama que contiene los commits que serán movidos.

---

### Ejemplo práctico

Imagina que tienes el siguiente historial de commits:

```
A - B - C (main)
         \
          D - E - F (feature)
```

Y tienes otra rama llamada `new-base` con este historial:

```
A - B - X - Y - Z (new-base)
```

#### Objetivo
Quieres mover los commits `D`, `E` y `F` de la rama `feature` para que estén basados en `new-base` (es decir, después de `Z`).

#### Comando
El comando con `--onto` sería:

```bash
git rebase --onto new-base main feature
```

#### ¿Qué hace este comando?
1. **`main` (`C`)**: Define el punto de inicio del rebase. Git seleccionará todos los commits de `feature` que están **después de `main`** (es decir, `D`, `E` y `F`).
2. **`new-base` (`Z`)**: Especifica la nueva base sobre la que se aplicarán los commits seleccionados.
3. **`feature`**: Es la rama que contiene los commits que serán movidos.

#### Resultado
Después del rebase, el historial quedará así:

```
A - B - C (main)
         \
          X - Y - Z - D' - E' - F' (feature)
```

- Los commits `D`, `E` y `F` ahora están basados en `new-base` (`Z`).
- Los commits originales (`D`, `E`, `F`) han sido "re-aplicados" y tienen nuevos identificadores (`D'`, `E'`, `F'`).

---

### Diferencia entre `git rebase` y `git rebase --onto`

En un **rebase normal**, Git selecciona automáticamente el punto de partida como el commit donde la rama se desvía de su base original. Por ejemplo:

#### Rebase normal
Si ejecutas:

```bash
git checkout feature
git rebase new-base
```

Git:
1. Encuentra el punto de divergencia entre `feature` y su base original (`main`, en este caso el commit `C`).
2. Selecciona todos los commits de `feature` después de la divergencia (`D`, `E`, `F`).
3. Re-aplica esos commits sobre la nueva base (`new-base`, después de `Z`).

El resultado es el mismo:

```
A - B - C (main)
         \
          X - Y - Z - D' - E' - F' (feature)
```

Sin embargo, el **rebase normal** siempre selecciona automáticamente todos los commits después del punto de divergencia. No permite controlar el rango de commits que serán movidos.

#### ¿Cuándo usar `--onto`?

El comando `--onto` se vuelve útil en escenarios más complejos, donde necesitas mover solo un subconjunto específico de commits. Por ejemplo:

##### Escenario avanzado
Imagina esta estructura:

```
A - B - C (main)
         \
          D - E - F (feature)
                 \
                  G - H - I (other-branch)
```

Quieres mover **solo los commits `E` y `F`** de `feature` a `new-base`. Si haces un rebase normal:

```bash
git rebase new-base
```

Git movería **todo el rango de commits desde la divergencia con `main`**, es decir, `D`, `E`, y `F`.

Pero con `--onto`, puedes seleccionar exactamente qué parte del historial mover:

```bash
git rebase --onto new-base E feature
```

##### Resultado:

```
A - B - C (main)
         \
          D - E (feature)
         \
          X - Y - Z - F' (feature)
```

- Git seleccionó solo el commit `F` (todo lo después de `E`).
- El commit `F` fue re-aplicado sobre `new-base` (`Z`).

---

### Resumen

- **`git rebase` (normal)**: Reorganiza commits desde el punto de divergencia automáticamente. Es útil para mover ramas enteras.
- **`git rebase --onto`**: Te permite seleccionar un rango específico de commits y moverlos explícitamente a una nueva base. Ofrece más flexibilidad para manejar ramas complejas.

Usar `--onto` es especialmente útil cuando necesitas control preciso sobre el rango de commits a mover, o en flujos de trabajo avanzados donde varias ramas interactúan y divergen.

## Miniguía sobre Git Tags

### ¿Qué es un tag en Git?
Un **tag** en Git es un marcador o referencia que apunta a un commit específico en el historial del repositorio. Generalmente, los tags se usan para identificar puntos importantes en la historia, como versiones de software (por ejemplo, `v1.0.0`).

### Tipos de tags
1. **Lightweight tags**: Un marcador simple que apunta a un commit específico.
2. **Annotated tags**: Contienen metadatos adicionales como el nombre del autor, la fecha y un mensaje asociado. Estos se almacenan como objetos completos en la base de datos de Git.

---

### Comandos básicos para trabajar con tags

#### Crear un tag
1. **Lightweight tag**:
   ```bash
   git tag <nombre-del-tag>
   ```
   Ejemplo:
   ```bash
   git tag v1.0.0
   ```

2. **Annotated tag**:
   ```bash
   git tag -a <nombre-del-tag> -m "Mensaje descriptivo"
   ```
   Ejemplo:
   ```bash
   git tag -a v1.0.0 -m "Primera versión del proyecto"
   ```

#### Listar tags existentes
```bash
git tag
```

Para buscar tags específicos con un patrón:
```bash
git tag -l "v1.*"
```

#### Ver detalles de un tag anotado
```bash
git show <nombre-del-tag>
```

---

### Subir tags al repositorio remoto

Por defecto, los tags no se suben automáticamente al remoto. Para subirlos:

1. Subir un tag específico:
   ```bash
   git push origin <nombre-del-tag>
   ```
   Ejemplo:
   ```bash
   git push origin v1.0.0
   ```

2. Subir todos los tags:
   ```bash
   git push --tags
   ```

---

### Eliminar un tag
1. Eliminar un tag local:
   ```bash
   git tag -d <nombre-del-tag>
   ```
   Ejemplo:
   ```bash
   git tag -d v1.0.0
   ```

2. Eliminar un tag en el remoto:
   ```bash
   git push origin --delete <nombre-del-tag>
   ```
   Ejemplo:
   ```bash
   git push origin --delete v1.0.0
   ```

---

### Mover un tag a otro commit
Si necesitas que un tag apunte a otro commit, puedes eliminarlo y recrearlo:

1. Elimina el tag local:
   ```bash
   git tag -d <nombre-del-tag>
   ```

2. Crea el tag apuntando al nuevo commit:
   ```bash
   git tag -a <nombre-del-tag> <commit-id> -m "Nuevo mensaje"
   ```

3. Actualiza el remoto:
   ```bash
   git push --force origin <nombre-del-tag>
   ```

---

### Usar tags para checkout
Puedes usar un tag para hacer checkout a un commit específico:

```bash
git checkout <nombre-del-tag>
```

Nota: Esto crea un **detached HEAD**, es decir, no estarás en una rama activa.

---

### Ejemplo práctico

1. Crear un tag para la versión `v1.0.0`:
   ```bash
   git tag -a v1.0.0 -m "Lanzamiento inicial"
   ```

2. Subir el tag al remoto:
   ```bash
   git push origin v1.0.0
   ```

3. Listar los tags:
   ```bash
   git tag
   ```

4. Ver detalles del tag:
   ```bash
   git show v1.0.0
   ```

---

### Conclusión
Los tags en Git son una excelente herramienta para marcar versiones importantes o hitos en el desarrollo de un proyecto. Usar tags anotados es recomendable para agregar contexto adicional, y siempre es buena práctica subir los tags relevantes al repositorio remoto para colaborar con otros.

## Comandos que tenemos que mirar
```
git checkout -b nuevarama ramactual

git merge --squash

git stash

git rebase --reapply-cherry-pick :(Cuando se hace un rebase git suele ignorar los commits de los que ya se hayan hecho cherry-pick, con esto se aplica igual)

Diferencia git reset hard y soft

Diferencia git reset y git revert
```

