# Debug en node.js

## Programa de ejemplo

* Vamos a utilizar un ejemplo muy sencillo que nos sirva para:
* Aprender a hacer debug
* Conocer como funcionan los módulos en nodejs
* Familiarizarnos con el editor de código
* Fichero suma.js
```
let suma = function(a, b) {
return a+b;
}
module.exports = suma;
```
* La función suma la queremos utilizar desde nuestro programa, por eso lleva el _module.exports_, que indica las partes de este módulo \(fichero\) que se pueden exportar \(utilizar desde otro fichero\).

* Mi aplicación \(cargo las librerías que necesito mediante require y las utilizo\)

```
let suma = require('./suma.js');
console.log (suma(3,5));
```

## Opciones de debug

* Utilizando la consola
* Utilizando Chrome Developer Tools
* Mediante Visual Studio

## Debug en consola

* Ejecutamos el siguiente comando
```
node debug app.js
```
* Podemos incluir puntos de interrrupción añadiendo líneas con la instrucción :
```
debugger;
```
* Las teclas de acceso rápido son:
* Hasta el siguiente breackpoint: c
* Hasta la siguiente línea de código en el mismo fichero: n
* Entrar en función: s
* Salir de función: o

## Mediante Chrome Developer Tools

* Hay dos opciones dependiendo de la versión de node:
- Mediante el propio node
```
node --inspect app.js
```
- Experimental en v6.x
- Mediante un paquete adicional, **node-inspector**
- Deprecated en v7.x

* Mediante el comando **node --inspect**

```
⇒ node --inspect app.js

Debugger listening on port 9229.

Warning: This is an experimental feature and could change at any time.

To start debugging, open the following URL in Chrome:

chrome-devtools://devtools/remote/serve\_file/@60cd6e859b9f557d2312f5bf532f6aec5f284980/inspector.html?experiments=true&v8only=true&ws=127.0.0.1:9229/bb7de882-abe4-4c61-8099-22908239de18
```

* Se suele poner un punto de interrupción al empezar:
```
node --debug-brk --inspect app.js
```

* El enlace que nos proporciona se abre con el navegador Chrome y se puede inspeccionar el código mediante sus herramientas de desarrollo.

* Instalo el paquete node-inspector:
```
npm i -g node-inspector
```
* Ejecuto el ejecutable node-debug:
```
node-debug app.js
```

## Debug en Visual Studio Code

* Primero hay que ir a la vista de debug, pulsando el icono correspondiente:
![](/assets/icono-debug.png)
* Para hacer debug necesitamos un fichero de configuración _launch.json_, que se obtiene pulsandos en la ruleta

![](/assets/sublime-debug.png)

* Ya podemos hacer debug como en cualquier otro programa:
* Play para empezar
* Break points pulsando a la izquierda de la numeración de lineas del código
* Puedes pulsar con el botón derecho y poner breakpoints condicionales.

* [Buen tutorial](https://www.youtube.com/watch?v=hfpkMyvSOp4)