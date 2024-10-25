# GUIA BASE:
## PRIMER PASO:
-Instalar node.js

## Crear proyecto:
npx create-react-app nombreproyecto

## Arrancar proyecto:
npm start

## Componentes

Recomendado crear un folder para componentes

Nosotros vamos a componer la app a base de componentes

Un componente no deja de ser una función que devuelve un trozo de html que va a renderizar

Una de las mejores cosas de react es que podemos pasar variables desde nuestro template a nuestra vista

Si abres llaves puedes recorrer usar javascript dentro de html, esto es jsx,
por ejemplo dentro de un div, puedes abrir llaves y iterar algo

Yo siempre tengo que hacer un return que devuelva un trozo de html

Otra cosa es que cada elemento va a tener que tener una key


Otra de las cosas importantes que podemos hacer con react son los estados

## ESTADOS:
Podemos definir por ejemplo un botón, y que cambie el valor de una variable, pero para esto vamos a tener que usar estados, que esto viene siendo como una variable que está sujeta a cambios

Lo primero es importar un state

Para el tema de los estados vamos a usar useState, en el cual vamos a declarar una variable como constante como 

>const [nombreVariable, setFuncionVariable] = useState(valor);


## ESTILOS:

Si queremos crear un archivo de estilos para nuestra clase lo que vmaos a hacer es llamar al css:
>nombre.module.css

Y luego cuando importamos el estilo lo importamos como:

>import classes from "nombre.module.css";

Y luego cuando lo referenciamos en el html dentro de react hacemos por ejemplo:

>className = {classes.nombreClase}

NOTA: Para esto lo mejor es usar camelCase