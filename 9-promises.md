# Proyecto 9: Uso de promesas

## Tiempo estimado: 150 minutos


## Descripción

- *Realizar consultas a servicios API*


## Objetivos

- Entender el concepto de asincronismo
- Saber usar promesas y sus ventajas frente a las funciones de callback
- Uso de Async / Await


## Suma asíncrona de números

- Crea dos ficheros (*numero1* y *numero2*) cuyo contenido sea un número.
- Crea un script que lea los ficheros y por consola muestre la suma de ambos ficheros
- Resuelve el problema de forma asíncrona (funciones de callback)

```js
const fs = require('fs')
const numero1 = fs.readFileSync('./numero1', 'utf-8')
const numero2 = fs.readFileSync('./numero2', 'utf-8')
console.log(`El resultado de la suma es  ${parseInt(numero1)+parseInt(numero2)}`)
```


## Suma asíncrona de números con funciones de callback

```js
const fs = require('fs')
fs.readFile('./numero1', 'utf-8', (err, numero1) => {
  if (err) throw err
  fs.readFile('./numero2', 'utf-8', (err, numero2) => {
    if (err) throw err
    console.log(`El resultado de la suma es  ${parseInt(numero1) + parseInt(numero2)}`)
  })
})
```


## Desventajas funciones de callback

- ¿Y si hubiera que leer 5 ficheros?
  - Excesiva anidación (**callback hell**)
  - Mayor dificultad de desarrollo (peor legibilidad)
    - Lo **ideal** sería utilizar **código secuencial y asíncrono**
  - Se trata de escribir código asíncrono con un estilo síncrono.
  - Podemos tener **muchas llamadas asíncronas** (accesos bbdd, llamadas api....)


- Mayor thoughput **si se leen** los dos ficheros a la vez
- Podríamos incluso hacer otra cosa mientras se leen


## Promesas al rescate

![Promesas](img/promises.png)


```js
function getData(fileName, type) {
  return new Promise(function(resolve, reject){
    fs.readFile(fileName, type, (err, data) => {
        err ? reject(err) : resolve(data);
    });
  });
}
```


## Suma asíncrona con promesas

```js
const fs = require('fs')

var numero1

const getData = (fileName, type) =>
  new Promise((resolve, reject) => {
    fs.readFile(fileName, type, (err, data) => {
      err ? reject(err) : resolve(parsetInt(data))
    })
  })

getData('numero1', 'utf-8')
  .then(fileContent => {
    numero1 = fileContent
    return getData('numero2')
  })
  .then(numero2 => console.log(`El resultado de la suma es  ${numero1 + numero2}`))
  .catch(err => console.log(err))
```


## Intento fallido de suma asíncrona con promesas

```js
const fs = require('fs')
var numero1
var numero2
fs.readFile('./numero1', 'utf-8', (err, numero) => {
  (err) ? console.log(err) : numero1 = numero
})
fs.readFile('./numero2', 'utf-8', (err, numero) => {
  err ? console.log(err) : (numero2 = numero)
})
while (!numero1 && !numero2){}

// las promesas no retornan nunca: Node.js event loop
// nuestro programa se queda colgado :-(

```


## Suma asíncrona con Promise.all

```js
const fs = require('fs')

const getData = (fileName, type) => new Promise(
  (resolve, reject) => {
    fs.readFile(fileName, type, (err, data) => {
        err ? reject(err) : resolve(parseInt(data))
    })
  }
)

var promise1 = getData('numero1', 'utf-8')
var promise2 = getData('numero2', 'utf-8')
Promise.all([promise1, promise2]).
then((arrayValues) => {
  let sum = arrayValues.reduce((sum, x) => sum +x);
  console.log(sum)
})
```


## Otras opciones en promesas

- Usar el módulo [fs-extra](https://github.com/jprichardson/node-fs-extra)
- Que un módulo soporte las promesas es un punto a favor
  - (fs-extra)[(https://github.com/jprichardson/node-fs-extra)] vs fs
  - [request](https://www.npmjs.com/package/request) vs [axios](https://www.npmjs.com/package/axios)


- Usar algún tipo de [promisify](http://bluebirdjs.com/docs/api/promisification.html)

```js
var Promise = require("bluebird")
var fs = require("fs")
Promise.promisifyAll(fs)
fs.readFileAsync("file.js", "utf8").then(...)
```

- Uso de Async-Await (nuestro principal objetivo)
