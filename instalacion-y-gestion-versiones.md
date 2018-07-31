# Cómo empezar con node.js


## Contenido

- Instalación de Node
- Gestión de versiones de node
- Uso de linters
- Debug en Node
- Vistazo general a las características de ES6 y ES7. Qué puedo usar con Node


## Node vs JavaScript

- El código en ambos es JavaScript
- Ambos se ejecutan con el mismo motor: [Google V8, motor open source, desarrollado en C++](https://chromium.googlesource.com/v8/v8.git)
- Node utiliza el motor fuera del contexto del navegador
  - No hay un *browser sandbox*
  - Tenemos funcionalidad añadida:
    - Acceso al file system
    - Acceso a bbdd *completa*
    - Incluso montar un servidor web


## Funcionamiento código

- Se lee el codigo en JavaScript
- Se compila a código máquina por el V8 y se ejecuta:

>V8 compiles JavaScript directly to native machine code before executing it, instead of more traditional techniques such as interpreting bytecode or compiling the whole program to machine code and executing it from a filesystem. The compiled code is additionally optimized (and re-optimized) dynamically at runtime, based on heuristics of the code's execution profile. 
[(wikipedia)](https://en.wikipedia.org/wiki/Chrome_V8)


## Hola Mundo en node

[](./img/hola-mundo-consola-node.png)


## Hola Mundo en browser

[](./img/hola-mundo-consola-browser.png)


## Window

- Objeto global en JavaScript
- Almacena todo sobre lo que se tiene acceso
![](./img/objeto-window-browser.png)

  

## Global

- El objeto global en node, se llama **global**:

![](./img/objeto-window-browser.png)


## Document

- Lo que se está viendo en el navegador
- Podemos modificarlo en runtime
![](./img/document-browser.png)

## Process

- El equivalente en node a document
- Vemos la información del proceso node que se está ejecutando:
  - pid
  - versión de node
  - métodos
  - ...

```js
$ node
> process
....
> process.exit(3) // también podríamos hacer .exit(3), o CTRL +C dos veces
$ echo $?
3
```

## Característica de node

- No bloqueante (asíncrono) **por naturaleza** (otros lenguajes mediante librerías)
  - Por defecto los métodos son asíncronos
  - Los métodos síncronos, llevan el sufijo sync
- El mayor repositorio de código disponible: [npm](https://www.npmjs.com/)
  - composer(php) o jpm(java) están basados en npm
- Orientado a eventos
- Lo veremos conforme vayamos haciendo código

## Ejemplo código no bloqueante

```js
const fs = require('fs')
fs.readFile('./prueba.txt', 'utf-8', (err, data) => {
  if (err) throw err
  console.log(`El contenido del fichero es este: ${data}`)
})
console.log(`Aquí todavia no tenemos el valor de fs.readFile`)
```

https://nodejs.org/en/docs/guides/blocking-vs-non-blocking/


## Ejemplo código bloqueante

```js
const fs = require('fs');
const data = fs.readFileSync('./prueba.txt', 'utf-8');
console.log(`El contenido del fichero es este: ${data}`)
```

## Consulta de API de node

- El módulo fs pertenence a los core modules de node, no es necesario instalarlo
- Para consultar la API:
  - [Desde el navegador](https://nodejs.org/api/index.html)
  - Desde el terminal (plugin node de zsh)
    ```bash
      $ node-docs
     ```

## Bloqueante vs no bloqueante

- El **código asíncrono** tiene un **throughput mucho mayor**.
- Se puede volver **complejo** el trabajar con el resultado de una función asíncrona.
  - El código asíncrono no se ejecuta de forma secuencial, más dificil de seguir
  - El método asíncrono recibe como último parámetro una **función de callback**

- Lo **ideal** sería utilizar **código secuencial y asíncrono**
  - Para ello utilizaremos **promesas** y **async/await**


## Ejercicio

- Crea dos ficheros (*numero1* y *numero2*) cuyo contenido sea un número.
- Crea un script que lea los ficheros y por consola muestre la suma de ambos ficheros
- Resuelve el problema de forma asíncrona


## Solución síncrona

```js
const fs = require('fs')
const numero1 = fs.readFileSync('./numero1', 'utf-8')
const numero2 = fs.readFileSync('./numero2', 'utf-8')
console.log(`El resultado de la suma es  ${parseInt(numero1)+parseInt(numero2)}`)
```

## Solución asíncrona

```
const fs = require('fs')
fs.readFile('./numero1', 'utf-8', (err, numero1) => {
  if (err) throw err
  fs.readFile('./numero2', 'utf-8', (err, numero2) => {
    if (err) throw err
    console.log(`El resultado de la suma es  ${parseInt(numero1) + parseInt(numero2)}`)
  })
})
```


## Ventajas solución asíncrona

- Mayor thoughput al leer los dos ficheros a la vez
- Podríamos incluso hacer otra cosa mientras se leen


## Desventajas solución asíncrona

- Podemos tener **muchas llamadas asíncronas** (accesos bbdd, llamadas api....)
  - Excesiva anidación (**callback hell**)
  - Mayor dificultad de desarrollo (peor legibilidad)
    - Lo **ideal** sería utilizar **código secuencial y asíncrono**
    - Para ello utilizaremos **promesas** y **async/await**


## Modelos de programación asíncrona

https://www.slideshare.net/jvelez77/presentacion-35264918



## Módulos en JavaScript

- En JavaScript no hay módulos ni namespaces.
- [Se implementan módulos de forma nativa con ES6 (2014)](http://exploringjs.com/es6/ch_modules.html#sec_modules-in-javascript)

```html
# more index.html
<!DOCTYPE html>
<html lang="es">
<head>
  <title>Document</title>
</head>

<body>
  <script>
    var nombre = "pepe";
    console.log(nombre);
  </script>
  <script src="./index.js"></script>
  <script>
    console.log(nombre);
  </script>
</body>

</html>
```

```js
# more index.js:
var nombre = "juan";
```
- Todo va al objeto window y la variable se sobreescribe.



## Gestor de versiones

- Es habitual utilizar varias versiones de node en nuestra máquina de desarrollo o por cada usuario.
- Esto nos permitirá:
- Poder cambiar de versión de node de forma transparente
- Evitar tener que hacer sudo cuando instalemos paquetes de forma global
  - Los paquetes globales se instalan para un único usuario y version de node
  - Los paquetes globales sirven para cualquier proyecto

- Los gestores de versiones más habituales son:

- [nvm](https://github.com/creationix/nvm) para Linux/Mac
- [nvm-windows](https://github.com/coreybutler/nvm-windows) para Windows (no tiene nada que ver con nvm)


## Instalación de nvm en Linux


- Instalaremos y utilizaremos node vía nvm (Node Version Manager)

```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
```

- Instalar una versión de node (instalará hasta su ultimo patch):

```
nvm install 8.11 
```

```
nvm install node --lts
```


- Ver las versiones que hay instaladas:

```
nvm ls
```

- Usar una versión en particular:

```
nvm use 8.11.3
```

- Usar una versión en particular siempre que abrimos un shell:

```
nvm alias default 8.11.3
```

- Para un proyecto en particular, mediante un fichero .nvmrc

- Ojo, hay que ejecutar nvm use "a mano" para que lea la versión
  - Otra opción es modificar el zsh
  - Otra opción es instalar un paquete adicional, avn:
  ```
  npm install -g avn avn-nvm avn-n
  avn setup
```



## Instalación de node

- Si hemos utilizado un gestor de versiones de node, ya habremos instalado node.
- Instalación en Linux:

```
curl -sL https://deb.nodesource.com/setup_5.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install build-essential
```

- En Windows [descargando el paquete msi](https://github.com/coreybutler/nvm-windows)

- Comprobamos que esté instalado:

```
npm -v
node -v
```

## Intalación de linter

- Eslint analiza el código y nos muestra errores.

ESLint is a great tool but it can’t fix all the problems related to code formatting/linting. Just look at this table.

## Editor de código

- Utilizaremos [Visual Code Editor](https://code.visualstudio.com/)
- Es un producto open source de Microsoft realizado mediante node.js (electron)
- Tiene un buen debugger para node.js

## Linter para JavaScript

- Utilizaremos eslint (el más habitual)
- Instalaremos la extensión eslint dentro de Visual Code Editor
- Utilizaremos eslint como dependencia de desarrollo dentro de nuestros proyectos


## Formateador de código
- Eslint avisa de muchos errores, pero [solo arregla algunos](https://eslint.org/docs/rules/)
