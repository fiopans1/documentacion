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
````
Si estando en rama main y después de ejecutar este comando, tendrás todos los cambios de mirama listos en el área de preparación, pero no habrás hecho aún un commit. Puedes revisar y luego hacer un solo commit que contenga todos esos cambios para subir a main.

## Comandos que tenemos que mirar
```
git checkout -b nuevarama ramactual

git merge --squash

git stash

git rebase --reapply-cherry-pick :(Cuando se hace un rebase git suele ignorar los commits de los que ya se hayan hecho cherry-pick, con esto se aplica igual)
```
