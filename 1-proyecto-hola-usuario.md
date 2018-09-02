# Proyecto 1: Hola Usuario

## Tiempo estimado: 60 minutos


## Descripción

- Realizar un programa que escriba en un fichero el texto siguiente:
  
  ```bash
  Hola usuario, tienes 25 años
  ```

  - Usuario se obtendrá de la variable de entorno

    ```bash
    $USERNAME
    ```

  - La edad se obtendrá de una variable


## Objetivos

- Entender el funcionamiento de los módulos
  - Utilizar módulos del núcleo (os, fs)
  - Crear un módulo sencillo
- Usar sintaxis de ES6 (*destructuring* y *template string*)


## Comenzar proyecto

```bash
mkdir holaUsuario
cd holaUsuario
touch app.js
code .
```


## Módulos de sistema

- Están "built-in"
  - Se cargan mediante un *require*
  - No es necesario instalarlos
- Podemos consultarlos en la web de node:
  - https://nodejs.org/es/docs/
    - Elegimos versión de la API
  - Simplemente ejecutando (zsh + plugin node):

  ```bash
  node-docs
  ```


## Añadir texto a un fichero

- Función asíncrona:

```js
console.log('Iniciando app');
const fs = require('fs');
// fs es un objeto con muchas funciones, ver api
fs.appendFile('saludo.txt', 'Hola Usuario');
```


- Salida con warning:

```js
(node:9493) [DEP0013] DeprecationWarning: Calling an asynchronous function without callback is deprecated.
```

- ¡Recoger error o éxito con función de callback para evitar warning!

```js
console.log('Iniciando app');
const fs = require('fs');
// fs es un objeto con muchas funciones, ver api
fs.appendFile('saludo.txt', 'Hola Usuario', (err)=>{
  err ? console.log('Ha habido un error') : console.log('Todo ok');
});
```


- Podríamos utilizar también una función síncrona:

```js
console.log('Iniciando app');
const fs = require('fs');
try {
  fs.appendFileSync('saludo.txt', 'Hola usuario');
  console.log('Todo ok!');
} catch (err) {
  console.log('Ha habido un error');
}
```


## Obtener el nombre del usuario

- Utilizaremos el módulo OS para averiguar el nombre del usuario

```js
const os = require('os');
const user = os.userInfo();
console.log(user); // para ver que datos tiene userInfo()
```


## Implementación ES5

```js
const fs = require('fs');
const os = require('os');
const user = os.userInfo();

console.log('Iniciando app');
const saludo = 'Hola ' + user.username;
fs.appendFile('saludo.txt', saludo, function (err) {
  err ? console.log('Ha habido un error') : console.log('Todo ok');
});
```


## ES6: Destructuring

- Mapeamos una o varias partes de un objeto a una o varias variables:

```js
let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
console.log(x); // 1
console.log(y); // 2
console.log(z); // { a: 3, b: 4 }
```


## ES6: Template Strings

```js
var a = 5;
var b = 10;
console.log(`Fifteen is ${a + b} and not ${2 * a + b}.`);
// "Fifteen is 15 and not 20."
```


## ES6: Object Literal Property Value Shorthand

- Antes (ES5):

```js
function createMonster(name, power) {
  return { type: 'Monster', name: name, power: power };
}
```

- Ahora (ES6):

```js
function createMonster(name, power) {
  return { type: 'Monster', name, power };
}
```


## Implementación ES6

```js
const fs = require('fs');
const os = require('os');
const { username } = os.userInfo();

console.log('Iniciando app');
const saludo = `Hola ${username}`;
fs.appendFile('saludo.txt', saludo, (err) => {
  err ? console.log('Ha habido un error') : console.log('Todo ok');
});
```


## require

- *require* es un módulo que está en el objeto global
- Este código no es necesario:
 
  ```js
  require('require');
  ```

- Funciona de forma síncrona
  - Por eso se ponen al comienzo
  - Podríamos colocarlos más tarde y hacer *lazy loading*


## Uso de módulos

- Vamos a crear un módulo que sea el encargado de proporcionarnos el usuario
- Creamos el fichero user.js con el siguiente texto:
  
  ```js
  console.log('Cargando módulo para el usuario');
  ```

- ¿Cómo lo cargamos dentro de nuestro app.js?
  ```
  const user = require ('./user.js')
  ```

- Comprobamos la ejecución que muestra el texto del módulo requerido por consola.


## Módulos en JavaScript

- En JavaScript no hay módulos ni namespaces.
- Todo va al objeto window y puede haber solapamiento de variables.
- [Se implementan módulos de forma nativa con ES6](http://exploringjs.com/es6/ch_modules.html#sec_modules-in-javascript)


- Fichero index.js

```js
var nombre = "juan";
```

- Fichero index.html

```html
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


## Uso de variables y funciones de otro módulo en node

- El objeto module tiene muchas propiedades, nos interesará **module.exports**

```js
console.log(module)
```

- module.exports puede ser una función, un objeto, un string...
- Será ahí donde tendremos que crear un objeto con el nombre del usuario y la edad.


## Solución proyecto

- Fichero app.js:
  
```js
const fs = require('fs');
const os = require('os');
console.log('Iniciando app');

const { username, edad } = require('./user')

const saludo = `Hola ${username}, tienes ${edad} años`;
fs.appendFile('saludo.txt', saludo, (err) => {
  err ? console.log('Ha habido un error') : console.log('Todo ok');
});
```

- Módulo user.js:

```js
const { username } = require('os').userInfo()
const edad = 25;
module.exports = { username, edad }
```


## Ejercicio carga módulos

- ¿Qué mostraría el siguiente programa?
- ¿Y si comentamos la primera línea de app.js?

app.js:

```js
require('./module1');
require('./module2');
console.log('Iniciando app');
```

module1.js:

```js
console.log('Ejecutando módulo 1');
```

module2.js:

```js
require('./module1')
console.log('Ejecutando módulo 2');
```


## Salida ejercicio

- El texto *Ejecutando módulo 1* se muestra solo una vez
  - Ya está cargado previamente, se usa la caché y no se ejecuta
  - El texto *Inicializando app* sale después del console.log de los require (los require son síncronos).


## Ejercicio leer ficheros

- Crea dos ficheros numero1.txt y numero2.txt y escribe un número en cada uno
- Crea un programa que:
  - Lea el contenido de los dos fichero y lo almacene en variables
  - Muestre por consola la suma de las variables


## Solución lectura ficheros

```js
const fs = require('fs')
const numero1 = fs.readFileSync('./numero1', 'utf-8')
const numero2 = fs.readFileSync('./numero2', 'utf-8')
console.log(`El resultado de la suma es  ${parseInt(numero1)+parseInt(numero2)}`)
```


## ¿Y ahora qué?

- [Proyecto apuntes en markdown](./2-proyecto-apuntes.md)