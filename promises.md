# Programación asíncrona


## Evitar el callback en el navegador

- Mediante el [uso de promesas](https://www.promisejs.org/)
- Se trata de escribir código asíncrono con un estilo síncrono.
- Opciones más actuales:
- Generators / Yields (ES6)
- Async / Await (ES7)
- El soporte de ES6 en node es limitado (--harmony) y también en el navegador => TRANSPILERS (babel)
- Ver [comparativa de métodos asíncronos](https://thomashunter.name/blog/the-long-road-to-asyncawait-in-javascript/)





## Modelos de programación asíncrona

https://www.slideshare.net/jvelez77/presentacion-35264918  


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

- Mayor thoughput **si se leen** los dos ficheros a la vez
- Podríamos incluso hacer otra cosa mientras se leen


## Desventajas solución asíncrona

- Podemos tener **muchas llamadas asíncronas** (accesos bbdd, llamadas api....)
  - Excesiva anidación (**callback hell**)
  - Mayor dificultad de desarrollo (peor legibilidad)
    - Lo **ideal** sería utilizar **código secuencial y asíncrono**
    - Para ello utilizaremos **promesas** y **async/await**


## Uso de promesas
- Con promesas si podríamos leer ambos a la vez

```
function getData(fileName, type) {
  return new Promise(function(resolve, reject){
    fs.readFile(fileName, type, (err, data) => {
        err ? reject(err) : resolve(data);
    });
  });
}
```

- Otra opción es usar el módulo [fs-extra](https://github.com/jprichardson/node-fs-extra)





https://medium.com/@sahilkkrazy/fetch-vs-axios-http-request-c9afa43f804e