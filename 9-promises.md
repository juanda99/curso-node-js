# Proyecto 9: Uso de promesas

## Tiempo estimado: 150 minutos


## Descripción

- *Realizar consultas a servicios API realizando un conversor en tiempo real de divisas*:


## Objetivos

- Entender el concepto de asincronismo
- Saber usar promesas y sus ventajas frente a las funciones de callback
- Uso de Async / Await


## Primeros pasos

- [Fork del repositorio en GitHub](https://github.com/juanda99/curso-node-js-proyecto-promesas)
- Clonar tu repositorio
- Inicializar proyecto con npm init
- Instalar y configurar eslint extendiendo de standard:

  ```bash
  npm i -D eslint@5.4.0
  node_modules/.bin/eslint --init
  ```

- Personalizar eslint utilizando el fichero *.eslintrc.json*
- Instala y configura nodemon con 3 npm scripts:
  - *npm run suma* -> para sumaFiles.js
  - *npm run promises* -> para promise.js
  - *npm start* -> para api.js


## Suma asíncrona de números

- Modifica script sumaFiles.js para que:
  - Haga lo mismo pero de forma asíncrona
  - Utiliza funciones de callback

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
    })
  })
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
  let sum = arrayValues.reduce((sum, x) => sum +x)
  console.log(sum)
})
```


## Otras opciones en promesas

- Usar el módulo [fs-extra](https://github.com/jprichardson/node-fs-extra)
- Que un módulo soporte las promesas es un punto a favor
  - [fs-extra](https://github.com/jprichardson/node-fs-extra) vs fs
  - [request](https://www.npmjs.com/package/request) vs [axios](https://www.npmjs.com/package/axios)


- Usar algún tipo de [promisify](http://bluebirdjs.com/docs/api/promisification.html)

```js
var Promise = require("bluebird")
var fs = require("fs")
Promise.promisifyAll(fs)
fs.readFileAsync("file.js", "utf8").then(...)
```

- Uso de Async-Await (nuestro principal objetivo)


## Practicar con promesas

- Vamos a trabajar con objetos json
  - Crearemos nosotros las promesas
  - Es habitual usar funciones de terceros que nos devuelvan promesas:
    - En la petición API
    - En la petición a la bbdd 
- Convertiremos código a async-await


## Obtención de datos de opositor

- Utilizaremos fichero *sumaFiles.js* del proyecto actual.
- Creamos una promesa que obtenga el opositor a partir de un id


```js
const getOpositor = (id) => {
  return new Promise((resolve, reject) => {
    const opositor = opositores.find((opositor) => opositor.id === id)
    if (opositor) {
      resolve(opositor)
    } else {
      reject(new Error(`No se ha encontrado al opositor con id: ${id}.`))
    }
  })
}
```

- Comprobamos que funcione:

```js
getOpositor(1).then((opositor) => {
  console.log(opositor)
}).catch((e) => {
  console.log(e)
})
``` 

```
node run sumaFiles
```


## Obtención notas opositor

- Crea la función *getNotas* que obtenga las notas de un opositor
- La función debe recibir un parámetro (id del opositor)


## Implementación función getNotas

```js
const getNotas = (id) => {
  return new Promise((resolve, reject) => {
    const notasOpositor = notas.filter((nota) => nota.id === id)
    if (notasOpositor.length) {
      resolve(notasOpositor)
    } else {
      reject(new Error(`No se ha encontrado notas del opositor con id: ${id}.`))
    }
  })
}
``` 


## Obtención de los datos de un opositor y sus notas
- Crea una función *getResultado* que:
  - Muestre un texto del tipo:
    ```bash
    Pepe tiene una media de 5 en la oposición de Informática
    ```
  - Debes utilizar las funciones getOpositor y getNotas definidas anteriormente.
  - Debes utilizar las [funciones map y reduce](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) para tratar los datos.


## Implementación función getResultado

```js
const getResultado = (id) => {
  let opositor
  return getOpositor(id).then((data) => {
    opositor = data
    return getNotas(opositor.id)
  }).then((notas) => {
    let media = 0
    if (notas.length > 0) {
      media = notas.map((nota) => nota.nota).reduce((a, b) => a + b) / notas.length
    }
    return `${opositor.nombre} tiene una media de ${media} en la oposición de ${opositor.especialidad}`
  })
}
```

## Async - await

- Vamos a implementar la función getResultado mediante async-await
- Se leerá mejor:
  - Evitamos el encadenamiento de las promesas
  - El código se ve más síncrono (secuencial)


## Sintaxis async await
- Utilizamos las etiquetas *async* y *await*
- La función se etiqueta como *async*
  - Requerimiento para utilizar await en su cuerpo
  - La función **siempre devuelve una promesa***

```js
const getResultado = async (id) => {
  await sentence 1
  sentence 2
  await sentence 3
  return <promise>
} 
```


## Retorno función async

- Comprueba la salida de esta función:

```js
const getResultado = async () => {
  return 'Resultado'
}
```


- Sería equivalente a:
```js
const getResultado = async () => {
  return new Promise((resolve, reject) => {
    resolve('Resultado')
  })
}
```

- Y se recogerá mediante

```js
getResultado().then((data)=>...)
```


## reject en función async

```js
const promiseFunction = () => {
  new Promise((resolve, reject) => {
    reject(new Error('Error al ejecutar promesa'))
  })
}
```

```js
const AsyncFunction = () => {
  throw new Error('Error al ejecutar promesa')
}
```

- Y se recogerá mediante

```js
getResultado().then((data) =>...).catch((e) =>...)
```

- En funciones async:
  - *return* = *resolve*
  - *throw* = *reject*

## await

- Si cambiamos nuestra función getResultado:

```js
const getResultado = async (id) => {
  const opositor = await getOpositor(id)
  console.log(notas)
}
```

- await esperará el resolve de la promesa para guardarlo en la variable
- Si quitamos await, recogeremos una promesa y no su resultado


## Práctica consumiendo APIs

- El objetivo es completar una función *covertCurrency* que:
  -  Haga conversiones entre divisas:
  -  Muestre los paises que utilizan la divisa de destino

- Parámetros de entrada:
  - <código moneda origen>, por ej. EUR
  - <código moneda destino>, por ej. USD
  - <cantidad a cambiar>, por ej. 100 (USD)

- Salida requerida, por ej:

  ```bash
  Vendiendo 100 EUR obtienes 115 USD. Los puedes utilizar en los siguientes países....
  ```


## Servicios de API's

- [Fixer](https://fixer.io/)
  - Para obtener los tipos de cambio
  - Es necesario autenticarse

- Necesitaremos crear una función del tipo

  ```js
  const exchangeRate = (from, to) => {
    // consulta a la API de Fixer
  }
  ```

- La cuenta gratuita solo permite EUR como moneda base:
  - http://data.fixer.io/api/latest?access_key=03b3abfc3fe505ecd38bbeebe6211dfe&BASE=EUR
  - Si nuestra moneda base no es EUR, tendremos que hacer otra pequeña conversión por nuestra cuenta


- [Rest Countries](https://restcountries.eu/)
  - Nos interesará para obtener los paises que utilizan una determinada moneda
  - No es necesario autenticarse
  - Hay un endpoint específico para monedas

- Necesitaremos crear una función del tipo

  ```js
  const getCountries = (currencyCode) => {
    // consulta a la API de Rest Countries, endpoint de divisas (currency)
  }
  ```


## Librerías para http requests

- Instalaremos [axios](https://www.npmjs.com/package/axios) (usa promesas)

```bash
npm i -S axios
```

- Nos ofrece directamente los datos parseados (no es necesario *JSON.parse*)


const axios = require('axios');


## Implementación función getExchangeRate

```js
const getExchangeRate = (from, to) => {
  return axios.get('http://data.fixer.io/api/latest?access_key=xxxxxxxxxx').then((response)=>{
    const euro = 1 / response.data.rates[from]
    const rate = euro * response.data.rates[to]
    return rate
  })
}

getExchangeRate('USD', 'CAD').then((rate) => {
  console.log(rate)
})
```


## Implementación función getExchangeRate con async-await

```js
const getExchangeRate = async (from, to) => {
  const response = await axios.get('http://data.fixer.io/api/latest?access_key=xxxxxxxxxx')
  const euro = 1 / response.data.rates[from]
  const rate = euro * response.data.rates[to]
  return rate
}

getExchangeRate('USD', 'CAD').then((rate) => {
  console.log(rate)
})
```


## Implementación función getCountries

```js
const getCountries = async (currencyCode) => {
  return axios.get(`https://restcountries.eu/rest/v2/currency/${currencyCode}`).then((response) => {  return response.data.map((country) => country.name)}
)}

getCountries('CAD').then((countries) => {
  console.log(countries)
})
```

- Intenta ahora implementar getCountries con async-await


## Implementación función getCountries con async-await

```js
const getCountries = async (currencyCode) => {
  const response = await axios.get(`https://restcountries.eu/rest/v2/currency/${currencyCode}`)
  return response.data.map((country) => country.name)
}

getCountries('CAD').then((countries) => {
  console.log(countries)
})
```


## Implementación función convertCurrency

- Utilizaremos las funciones *getExchangeRates* y *getCountries*

```js
const convertCurrency = (from, to, amount) => {
  getExchangedRate(from, to).then((rate)=>{
    const = convertedAmount = (amount)*rate.toFixed(2)
    return getCountries(to)
  }).then ((countries))=>{
    console.log(countries)
    // return `Vendiendo ${amount} ${from} obtienes ${convertedAmount} ${to}. Los puedes utilizar en los siguientes paises: ${countries.join(', ')}`;
  }
}

convertCurrency('USD', 'USD', 20).then((message) => {
  console.log(message);
})
```

## Problema d scope

- Para devolver el mensaje hay variables que están fuera del scope
- Ya nos pasó al hacer la suma de números de ficheros... ¿sabrías solucionarlo?


## Solución problema de scope

 ------> poner aquí la solución
 ------> cambiar mondedas de CAD a EUR


## Propuesta final

- convertCurrency mediante async-await
- Y ahora, ¿lo sabrías refactorizar usando async-await?


## Solución convertCurrency

```js
const convertCurrency = async (from, to, amount) => {
  const rate = await getExchangeRate(from, to);
  const countries = await getCountries(to);
  const convertedAmount = (amount * rate).toFixed(2);
  return `Vendiendo ${amount} ${from} obtienes ${convertedAmount} ${to}. Los puedes utilizar en los siguientes paises: ${countries.join(', ')}`;
}
Vendiendo 100 EUR obtienes 115 USD. Los puedes utilizar en los siguientes países....

convertCurrency('USD', 'USD', 20).then((message) => {
  console.log(message);
})
```


## Solución completa con gestión de errores

```js
const axios = require('axios')

const getExchangeRate = async (from, to) => {
  try {
    const response = await axios.get('http://data.fixer.io/api/latest?access_key=xxxxxxx')
    const euro = 1 / response.data.rates[from]
    const rate = euro * response.data.rates[to]

    if (isNaN(rate)) {
      throw new Error()
    }

    return rate
  } catch (e) {
    throw new Error(`Unable to get exchange rate for ${from} and ${to}.`)
  }
}

const getCountries = async (currencyCode) => {
  try {
    const response = await axios.get(`https://restcountries.eu/rest/v2/currency/${currencyCode}`)
    return response.data.map((country) => country.name)
  } catch (e) {
    throw new Error(`Unable to get countries that use ${currencyCode}.`)
  }
};

const convertCurrency = async (from, to, amount) => {
  const rate = await getExchangeRate(from, to)
  const countries = await getCountries(to)
  const convertedAmount = (amount * rate).toFixed(2)
  return `${amount} ${from} is worth ${convertedAmount} ${to}. You can spend it in the following countries: ${countries.join(', ')}`
};

convertCurrency('USD', 'CAD', 20).then((message) => {
  console.log(message)
}).catch((e) => {
  console.log(e.message)
})
```js