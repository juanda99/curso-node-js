## Objetivos

- Leer y escribir en el sistema de ficheros
- Usar objetos JSON
- Consumir modulos del núcleo (os, fs)
- Consumir módulos de terceros
- Configurar proyecto

## 


- Introducción a modules, se usan con require, propios o de terceros
- Un módulo es una forma de agrupar funciones



## Comenzar proyecto

´´´
mkdir notas
cd notas
touch app.js
code .
´´´

## 

- Añadir texto a un fichero
  - Podemos recoger error o éxito con función de callback sería lo suyo para evitar warning:
    ```js
    (node:9493) [DEP0013] DeprecationWarning: Calling an asynchronous function without callback is deprecated.
    ```
  - Podríamos utilizar una función síncrona

```js
console.log('Iniciando app');
const fs = require('fs');
// fs es un objeto con muchas funciones, ver api
fs.appendFile('saludo.txt', 'Hola Mundo');
```

## Personalizar saludo con nombre de usuario del OS

- Utilizaremos el módulo OS para averiguar el nombre del usuario

```js
const os = require('os')
const user = os.userInfo();
console.log(user); // para ver que datos tiene userInfo()
```

- ¿Sabrías terminar el ejercicio para que mostrara por pantalla 'Hola nombreUsuario?

## Uso de ES6

- Podemos utilizar dos características de ES6:

### Destructuring

```js
let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
console.log(x); // 1
console.log(y); // 2
console.log(z); // { a: 3, b: 4 }
```

### Template Strings

```js
var a = 5;
var b = 10;
console.log(`Fifteen is ${a + b} and not ${2 * a + b}.`);
// "Fifteen is 15 and not 20."
```

# Solución

```
const fs = require('fs')
const os = require('os')
const user = os.userInfo()

console.log('Iniciando app')
const { username } = user;
fs.appendFile('saludo.txt', `Hola ${username}`)
// fs.appendFile('saludo.txt', `Hola ${user.username}`)
```


- Otra opción: 

```js
const fs = require('fs')
const os = require('os')
const { username } = os.userInfo()

console.log('Iniciando app')
fs.appendFile('saludo.txt', `Hola ${username}`)
```


## Módulo propio

tareas.js para escribir y leer notas con el siguiente contenido:

console.log('Cargando módulo para tareas');

- ¿Cómo lo cargamos dentro de nuestro app.js?
const tareas = require ('./tareas.js')

- Comprobamos la ejecución que muestra el texto del módulo requerido por consola.


console.log(modules)
