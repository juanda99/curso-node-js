# Creación de una API

## Tiempo estimado: 150 minutos


## Descripción

- *Creación de una API REST estándar y completa que pueda clonarse para distintos proyectos*


## Objetivos

- Conocer el objeto Router de express
- Conocer arquitecturas MVC
- Familiarizarse con base de datos no relacional
- Comprender las ventajas en JavaScript de MongoDB frente a bases de datos relacionales
- Familiarizarse con el uso de contenedores (docker)


## Pasos previos

- Entender [como funciona un servidor express (proyecto anterior)](./5-express.md)
- Tener claro [que es una API y una arquitectura API REST](./arquitectura-api-rest.md)


## Primeros pasos

- [Fork del repositorio en GitHub](https://github.com/juanda99/curso-node-js-proyecto-api)
- Clonar tu repositorio
- Inicializar proyecto con npm init
- Instalar y configurar eslint extendiendo de standard:

  ```bash
  npm i -D eslint@5.4.0
  node_modules/.bin/eslint --init
  ```

- Personalizar eslint utilizando el fichero *.eslintrc.json*


## Instalar paquetes y configurar npm start

- Instalamos express
  
  ```bash
  npm i -S express@4.16.3
  ```

- Configuramos package.json para que nuestro servidor arranque mediante *npm start*

  ```json
  "scripts": {
    "start": "node app/server.js"
  },
  ```


## [Postman](https://www.getpostman.com/)

- Utilizaremos [Postman](https://www.getpostman.com/) para testear nuestra API
  - Independiente de que hagamos tests por otro lado
- Es una herramienta muy extendida


## Comprobación API inicial

- Arranca la aplicación y comprueba su funcionamiento mediante [Postman](https://www.getpostman.com/)

```js
var express = require('express') //llamamos a Express
var app = express()

var port = process.env.PORT || 8080  // establecemos nuestro puerto

app.get('/', (req, res) => {
  res.json({ mensaje: '¡Hola Mundo!' })
})

app.get('/cervezas', (req, res) => {
  res.json({ mensaje: '¡A beber cerveza!' })  
})

// iniciamos nuestro servidor
app.listen(port)
console.log('API escuchando en el puerto ' + port)
```



## Implementación API


## Añadir rutas

- Añade la ruta **POST /cervezas** con respuesta:

  ```json
  { "mensaje": "Cerveza guardada" }
  ```

- Añade la ruta **DELETE /cervezas** con respuesta:

  ```json
  { "mensaje": "Cerveza borrada" }
  ```

- Muestra el mensaje *API escuchando en el puerto 8080* **justo cuando se levante el puerto**

- Comprueba funcionamiento mediante [Postman](https://www.getpostman.com/)

- Podemos utilizar la extensión [ExpressSnippet](https://marketplace.visualstudio.com/items?itemName=vladmrnv.expresssnippet) para autocompletado.


## Commit con las nuevas rutas

- Hagamos un commit del repositorio, sin la carpeta node_modules

  ```bash
  git status
  git add -A app/server.js
  git commit -m "Añadido post y delete, arreglado error"
  git push
  ```

- Debemos hacer nuevas instantáneas (commits) en cada paso
  - Aquí no se documentarán por brevedad
  - Aquí hemos mezclado un parche con una funcionalidad nueva. ¡No es lo correcto!


## nodemon

- Es un wrapper de node, para reiniciar nuestro API Server cada vez que detecte modificaciones.

  ```bash
  npm i -D nodemon
  ```

- Cada vez que ejecutemos **npm start** ejecutaremos nodemon en vez de node. Habrá que cambiar el script en el fichero *package.json*:

  ```bash
  "start": "nodemon app/server.js"
  ```


## Uso de enrutadores

- Normalmente una API:
  - Tiene varios recursos (cada uno con múltiples endpoints)
  - Sufre modificaciones -> mantener versiones

- Asociamos enrutadores a la app en vez de rutas:
  - Cada enrutador se asocia a un recurso y a un fichero específico.
  - Se pueden anidar enrutadores (*router.use*)


- El código para un enrutador sería así:

```js
// para establecer las distintas rutas, necesitamos instanciar el express router
var router = express.Router()

//establecemos nuestra primera ruta, mediante get.
router.get('/', (req, res) => {
  res.json({ mensaje: '¡Bienvenido a nuestra API!' })  
})

// nuestra ruta irá en http://localhost:8080/api
// es bueno que haya un prefijo, sobre todo por el tema de versiones de la API
app.use('/api', router)
```


## Configura enrutadores

- Crea un enrutador para el versionado  de la API:
  - Ruta **GET /api** (simulará el versionado de la api):

  ```json
  { mensaje: '¡Bienvenido a nuestra API!' }
  ```

- Crea un enrutador anidado para los endpoints de las cervezas
  - *GET /api/cervezas*
  - *POST /api/cervezas*
  - ...


## server.js con enrutador

```js
var express = require('express') // llamamos a Express
var app = express()

var port = process.env.PORT || 8080 // establecemos nuestro puerto

var router = require('./routes')

app.get('/', (req, res) => {
  res.json({ mensaje: '¡Hola Mundo!' })
})

app.use('/api', router)

// iniciamos nuestro servidor
app.listen(port, () => {
  console.log(`App listening on port ${port}`)
})
```


## Enrutador base para el versionado

```js
var router = require('express').Router()
var cervezasRouter = require('./cervezas')

router.get('/', (req, res) => {
  res.json({ mensaje: 'Bienvenido a nuestra api' })
})

router.use('/cervezas', cervezasRouter)

module.exports = router
```


## Enrutador cervezas

```js
var router = require('express').Router()

router.get('/', (req, res) => {
  res.json({ mensaje: 'Listado de cervezas' })
})

router.post('/', (req, res) => {
  res.json({ mensaje: 'Cerveza guardada' })
})

router.delete('/', (req, res) => {
  res.json({ mensaje: 'Cerveza borrada' })
})

module.exports = router
```


## Envio de parámetros

- Cuando el router recibe una petición, podemos observar que ejecuta una función de callback:

```js
(req, res) => {}
```

- El parámetro **req** representa la petición (request)
  - Aquí es donde se recibe el parámetro


## Tipos de envio de parámetros

- **Mediante la url**
  - Se recogerán mediante:

  ```bash
  req.params.nombreVariable
  req.query.nombreVariable
  ```

- **Mediante post** en http hay dos posiblidades:
  - application/x-www-form-urlencoded ([pocos datos]((http://stackoverflow.com/questions/4007969/application-x-www-form-urlencoded-or-multipart-form-data))
  - multipart/form-data ([muchos datos ¿ficheros?]((http://stackoverflow.com/questions/4007969/application-x-www-form-urlencoded-or-multipart-form-data))


## Parámetros por url

- Vamos a mandar un parámetro *nombre* a nuestra api, de modo que nos de un saludo personalizado.

```bash
GET http://localhost:8080/pepito
```

```js
app.get('/:nombre', (req, res) => {
  res.json({ mensaje: '¡Hola' + req.params.nombre })
})
```

- La variable podría acabar en ? (parámetro opcional)


- Si mandamos una url del tipo:

```bash
GET http://localhost:8080/api?nombre=pepito
```

- El parámetro se recoge mediante *req.query*:

```js
app.get('/', (req, res) => {
  res.json({ mensaje: '¡Hola' + req.query.nombre })
})
```


## Parámetros por post

- ¡Necesitamos **middlewares**!
- **application/x-www-form-urlencoded**:
  - [body-parser](https://www.npmjs.com/package/body-parser): extrae los datos del body y los convierte en json

- **multipart/form-data**
  - [Busboy](https://www.npmjs.com/package/busboy) o [Multer](https://github.com/expressjs/multer) 


## Ejemplo con body-parser

- Instalación:

  ```bash
  npm i -S body-parser@1.18.3
  ```

- body-parser actúa como **middleware**
- El código adicional será similar al siguiente:

  ```js
  var bodyParser = require('body-parser')
  app.use(bodyParser.urlencoded({ extended: true }))
  app.use(bodyParser.json())

  router.post('/', (req,res) => {
    res.json({mensaje: req.body.nombre})  
  })
  ```


## Rutas de nuestra API

![Rutas API](./img/rutas-api.png)


## Enrutado del recurso cervezas

- Intenta configurar una API básica para el recurso cervezas en base a las rutas anteriores
  - Muestra por consola el tipo de petición
  - Muestra por consola el parámetro de entrada


## Solución enrutado recurso cervezas

- Fichero *app/routes/cervezas.js*:

```js
  var router = require('express').Router()
  router.get('/search', (req, res) => {
    res.json({ message: `Vas a buscar una cerveza que contiene ${req.query.q}` })
  }) // ¡¡¡¡antes que la ruta GET /:id!!!!
  router.get('/', (req, res) => {
    res.json({ message: 'Estás conectado a la API. Recurso: cervezas' })
  })
  router.get('/:id', (req, res) => {
    res.json({ message: `Vas a obtener la cerveza con id ${req.params.id}` })
  })
  router.post('/', (req, res) => {
    res.json({ message: 'Vas a añadir una cerveza' })
  })
  router.put('/:id', (req, res) => {
    res.json({ message: `Vas a actualizar la cerveza con id ${req.params.id}` })
  })
  router.delete('/:id', (req, res) => {
    res.json({ message: `Vas a borrar la cerveza con id ${req.params.id}` })
  })
  module.exports = router
```



## Arquitectura


## Situación actual

- Se han definido recursos
  - *Cervezas*, pero podría haber muchos más
  - Cada recurso se asocia a un enrutador
    - Por el momento *routes/cervezas*
  - A cada enrutador se asocian las rutas del recurso


## Arquitectura MVC

- Cada ruta se gestiona por un controlador
  - El controlador es responsable de:
    - Acceder a los datos para leer o guardar
      - Delega en un modelo
        - Utilizaremos un ODM
        - Mongoose para MongoDB
    - Enviar datos de vuelta
      - Al ser JSON, lo haremos directamente desde el controller
      - No necesitamos la capa Vista :-)


## Fat model, thin controller

- El controlador recoge la lógica de negocio.
  - En nuestro caso es muy sencillo:
    - Recoge parámetros
    - Llama al modelo (obtener datos, guardar)
    - Devuelve json
- El modelo puede ser complejo. 
  - Por ej. creación de tokens o encriptación de passwords en el modelo de Usuario
  - Es un código que puede ser reutilizado entre controladores


## Acceso a base de datos

- Para la persistencia de nuestros datos utilizaremos una base de datos
- Elegimos una noSQL: MongoDB
  - Es lo más habitual en arquitecturas MEAN
  - Así operamos con objetos json tanto en node como en bbdd (bson)
  - Y así tenemos otra consola de JavaScript :-)


## Instalación de MongoDB

- Utilizaremos **contenedores docker**:
  - Eliminamos conflictos en la máquina base
  - Podemos cambiar de versiones con facilidad
  - Nuestro proyecto es más portable
  
- [Docker ya está instalado](./configuracion-inicial.md)

- [Otra opción más tradicional sería usar repositorios o paquetes](https://www.mongodb.com/)


## Fichero de instalación mediante docker

- Definiremos un fichero *docker-compose.yml*
- Para ver que imagen necesitamos podemos consultar en el [docker hub](https://hub.docker.com/)

```yml
version: '3'
services:
  mongodb:
    hostname: mongodb
    container_name: mongodb
    image: mongo:4.0.1
    volumes:
      - ./mongodb-data:/data/db
    ports:
      - "27005:27017"
```


## Arrancar y parar MongoDB con docker-compose

![](./img/docker-compose-comandos.png)


## MongoDB: Consola

- Se ejecuta con el comando mongo
  - Como hemos instado MongoDB mediante docker, primero entraremos al contenedor:
    ```bash
    docker-compose exec mongodb bash
    mongo
    ```
    - O mediante *attach shell* de la extensión Docker de Visual Editor
  - Podemos ver:
    - Los ejecutables de MongoDB en el contenedor
    - El volumen mapeado...


## MongoDB: Aplicaciones gráficas

- [mongo-express](https://www.npmjs.com/package/mongo-express)
  - Paquete de node
  - Instalación docker

- [Robo3T](https://robomongo.org/download)
  - Antes llamado **Robomongo**
  - El más extendido, será el que utilicemos


## Robo3T: Instalar y configurar

- Descargamos el paquete de [Robo3T](https://robomongo.org/download)(antes Robomongo)
- Instalamos y ejecutamos


## Robo3T: Conexiones a MongoDB

- Robo3T guarda un listado de conexiones a MongoDB

![Lista conexiones MongoDB](./img/robo3t-conexiones.png)


## Robo3t: Configurar conexión a MongoDB

- Creamos una nueva conexión a localhost y al puerto que hemos mapeado en Docker (27005)

![Nueva conexión MongoDB](./img/robo3t-nueva-conexion.png)


## Conceptos en noSQL

![](img/SQL-MongoDB-correspondence.png)


## Schema en noSQL

![](img/no-sql-schema-vs-sql.jpg)


## Inserción de datos

- Utilizaremos el fichero *cervezas.json*, de la carpeta data

- Importar nuestro cervezas.json a una base de datos

```bash
mongoimport --db web --collection cervezas --drop --file cervezas.json --jsonArray
```

- Otra opción es mediante Robomongo:

```js
  db.getCollection('cervezas').insertMany(array de objetos)
```


- Para hacer una búsqueda por varios campos de texto, tendremos que hacer un índice:

```js
  $ mongo # para entrar en la consola de mongo
  use web; #seleccionamos la bbdd
  db.cervezas.createIndex(
    {
      "nombre": "text",
      "descripción": "text"
    },
    {
      name: "CervezasIndex",
      default_language: "spanish"
    }
  )
```

- Comprobamos que el índice esté bien creado

  ```js
  db.cervezas.getIndexes()
  ```


## Conexión a MongoDB desde node

- Instalaremos [mongoose](https://mongoosejs.com/) como ODM (Object Document Mapper) en vez de trabajar con el [driver nativo de MongoDB](https://mongodb.github.io/node-mongodb-native/) (se utiliza por debajo).

  ```bash
  npm i -S mongoose@5.2.13
  ```


## Abrir conexión con Mongoose

- Mediante el método connect, [siguiendo la documentación:](https://mongoosejs.com/)

  ```js
  const mongoose = require('mongoose');
  mongoose.connect('mongodb://localhost/test');

  const Cat = mongoose.model('Cat', { name: String });

  const kitty = new Cat({ name: 'Zildjian' });
  kitty.save().then(() => console.log('meow'));
  ```


- Creamos el fichero *app/db.js* donde configuraremos nuestra conexión a base de datos mediante mongoose:

```js
const mongoose = require('mongoose')

const MONGO_URL = process.env.MONGO_URL || 'mongodb://localhost:27005/web'
mongoose.connect(MONGO_URL, { useNewUrlParser: true })

mongoose.connection.on('connected', () => {
  console.log(`Conectado a la base de datos: ${MONGO_URL}`)
})

mongoose.connection.on('error', (err) => {
  console.log(`Error al conectar a la base de datos: ${err}`)
})

mongoose.connection.on('disconnected', () => {
  console.log('Desconectado de la base de datos')
})

process.on('SIGINT', function() {
  mongoose.connection.close(function () {
    console.log('Desconectado de la base de datos al terminar la app')
    process.exit(0)
  })
})
```


- En nuestro fichero *app/server.js* incluimos el fichero de configuración de bbdd:

```js
require('./db')
```

- Observa que no es necesario asignar el módulo a una constante
  - El módulo solo abre conexión con la bbdd
  - Registra eventos de mongodb
  - No exporta nada


- La conexión a bbdd se queda abierta durante todo el funcionamiento de la aplicación: 
  - Las conexiones TCP son caras en tiempo y memoria
  - Se reutiliza


## Modelos

- Definimos un esquema para nuestra colección
- Creamos nuestro modelo a partir del esquema
- Fichero *app/models/Cervezas.js*):

```js
const mongoose = require('mongoose')
const Schema = mongoose.Schema

const cervezaSchema = new Schema({
  nombre: {
    type: String,
    required: true
  },
  descripción: String,
  graduación: String,
  envase: String,
  precio: String
})

const Cerveza = mongoose.model('Cerveza', cervezaSchema)

module.exports = Cerveza
```


- Ahora podríamos crear documentos y guardarlos en la base de datos

```js
const miCerveza = new Cerveza({ name: 'Ambar' })
miCerveza.save((err, miCerveza) => {
  if (err) return console.error(err)
  console.log(`Guardada en bbdd ${miCerveza.name}`)
})
```


## Guardar documento

- Crea una cerveza nueva con todos los campos
- Comprueba desde Robo3T que en nuevo documento aparece en la colección Cervezas


## Solución guardar documento

```js
require('./db')

// en fichero app/server.js después de conectar a bbdd:

const miCerveza = new Cerveza({
  nombre: 'Ambar',
  descripción: 'La cerveza de nuestra tierra',
  graduación: '4,8º',
  envase: 'Botella de 75cl',
  precio: '3€'
})
miCerveza.save((err, miCerveza) => {
  if (err) return console.error(err)
  console.log(`Guardada en bbdd ${miCerveza.nombre}`)
})
```


## Uso de controladores

- Desde nuestro fichero de rutas (*app/routes/cervezas.js*), se llama a un controlador, encargado de añadir, borrar o modificar cervezas usando el modelo Cerveza.
- **Endpoint -> Recurso -> Fichero de rutas -> Controlador -> Modelo**


- Creamos un directorio específico para los controladores (*app/controllers*)
- Un controlador específico para cada recurso, por ej.  (*app/controllers/cervezasController.js*):
- Un método en el controlador por cada endpoint del recurso


## Repaso de nuestra API

![](IMG/rutas-api.png)


## Configuración final del router Cervezas

```js
var router = require('express').Router()
var cervezasController = require ('../controllers/cervezasController')

router.get('/search', (req, res) => {
  cervezasController.search(req, res)
})
router.get('/', (req, res) => {
  cervezasController.list(req, res)
})
router.get('/:id', (req, res) => {
  cervezasController.show(req, res)
})
router.post('/', (req, res) => {
  cervezasController.create(req, res)
})
router.put('/:id', (req, res) => {
  cervezasController.update(req, res)
})
router.delete('/:id', (req, res) => {
  cervezasController.remove(req, res)
})
module.exports = router
```



## Implementación del controlador


## Workflow

- Utilizaremos enfoque BDD
  - Desarrollamos un test
  - Implementamos código
  - Comprobamos funcionamiento

- Los tests ya están hechos :-)


### Test desde el navegador o mediante Postman

- Comprobamos que se genera el listado de cervezas
- Comprobamos que se busca por keyword:

```bash
http://localhost:8080/api/cervezas/search?q=regaliz
```
...


## Test de la API

- Utilizaremos [Mocha](https://mochajs.org/) como test framework
- [supertest](https://github.com/visionmedia/supertest) para hacer las peticiones http.
- Chai como librería de aserciones

```bash
npm i -D mocha supertest chai
```

- Tenemos nuestros test en el fichero *test/cerveza.test.js*


## Configuramos nuestro app para los tests

```js
// iniciamos nuestro servidor
// para tests, porque supertest hace el listen por su cuenta
if (!module.parent) {
  app.listen(port, () => console.log(`API escuchando en el puerto ${port}`))
}

// exportamos la app para hacer tests
module.exports = app
```


## Configuración del controlador Cervezas

- Debemos definir los métodos siguientes:
  - search
  - list
  - show
  - create
  - update
  - remove


## Listar cervezas

```js
const Cerveza = require('../models/Cerveza')

const list = (req, res) => {
  Cerveza.find((err, cervezas) => {
    if (err) {
      return res.status(500).json({
        message: 'Error obteniendo la cerveza'
      })
    }
    return res.json(cervezas)
  })
}

module.exports = {
  list
}
```


## Listar cervezas por palabra clave

```js
const Cerveza = require('../models/Cerveza')
const search = (req, res) => {
  const q = req.query.q
  Cerveza.find({ $text: { $search: q } }, (err, cervezas) => {
    if (err) {
      return res.status(500).json({
        message: 'Error en la búsqueda'
      })
    }
    if (!cervezas.length) {
      return res.status(404).json({
        message: 'No hemos encontrado cervezas que cumplan esa query'
      })
    } else {
      return res.json(cervezas)
    }
  })
}

module.exports = {
  list,
  search
}
```


## Mostrar una cerveza

```js
const Cerveza = require('../models/Cervezas')
const { ObjectId } = require('mongodb')
const show = (req, res) => {
  const id = req.params.id
  Cerveza.findOne({ _id: id }, (err, cerveza) => {
    if (!ObjectId.isValid(id)) {
      return res.status(404).send()
    }
    if (err) {
      return res.status(500).json({
        message: 'Se ha producido un error al obtener la cerveza'
      })
    }
    if (!cerveza) {
      return res.status(404).json({
        message: 'No tenemos esta cerveza'
      })
    }
    return res.json(cerveza)
  })
}
module.exports = {
  search,
  list,
  show
}
```


## Crear una cerveza

```js
const Cerveza = require('../models/Cervezas')
const create = (req, res) => {
  const cerveza = new Cerveza(req.body)
  cerveza.save((err, cerveza) => {
    if (err) {
      return res.status(400).json({
        message: 'Error al guardar la cerveza',
        error: err
      })
    }
    return res.status(201).json(cerveza)
  })
}
module.exports = {
  search,
  list,
  show,
  create
}
```


## Actualizar cerveza

```js
const update = (req, res) => {
  const id = req.params.id
  Cerveza.findOne({ _id: id }, (err, cerveza) => {
    if (!ObjectId.isValid(id)) {
      return res.status(404).send()
    }
    if (err) {
      return res.status(500).json({
        message: 'Se ha producido un error al guardar la cerveza',
        error: err
      })
    }
    if (!ObjectId.isValid(id)) {
      return res.status(404).send()
    }
    if (!cerveza) {
      return res.status(404).json({
        message: 'No hemos encontrado la cerveza'
      })
    }

    Object.assign(cerveza, req.body)

    cerveza.save((err, cerveza) => {
      if (err) {
        return res.status(500).json({
          message: 'Error al guardar la cerveza'
        })
      }
      if (!cerveza) {
        return res.status(404).json({
          message: 'No hemos encontrado la cerveza'
        })
      }
      return res.json(cerveza)
    })
  })
}
module.exports = {
  search,
  list,
  show,
  create,
  update
}
```


## Borrar cerveza

```js
const Cerveza = require('../models/Cervezas')
const { ObjectId } = require('mongodb')
const remove = (req, res) => {
  const id = req.params.id

  Cerveza.findOneAndDelete({ _id: id }, (err, cerveza) => {
    if (!ObjectId.isValid(id)) {
      return res.status(404).send()
    }
    if (err) {
      return res.json(500, {
        message: 'No hemos encontrado la cerveza'
      })
    }
    if (!cerveza) {
      return res.status(404).json({
        message: 'No hemos encontrado la cerveza'
      })
    }
    return res.json(cerveza)
  })
}

module.exports = {
  search,
  list,
  show,
  create,
  update,
  remove
}
```



## Análisis de código

- Por último podríamos utilizar un paquete como **istanbul** que nos analice el código y ver si nuestras pruebas recorren todas las instrucciones, funciones o ramas del código:

  ```bash
  npm i -D istanbul
  ./node_modules/.bin/istanbul cover -x "**/tests/**"  ./node_modules/.bin/_mocha  tests/api.test.js
  ```


- Estos datos son facilmente exportables a algún servicio que nos de una estadística de la cobertura de nuestros tests o que haga un seguimiento de los mismos entre las distintas versiones de nuestro código.
- Por último también se podría integrar con un sistema de integración continua tipo [Travis](https://travis-ci.org/).


## Uso de middlewares cors y morgan

- Normalmente utilizaremos middlewares que ya están hechos, por ejemplo Morgan para logs y cors para Cors.
- Los instalamos:

  ```bash
  npm i -S cors morgan
  ```


- Los insertamos en nuestra API (el orden puede ser importante):

```js
const express = require('express') // llamamos a Express
const app = express()
const router = require('./routes')

const cors = require('cors')
const morgan = require('morgan')
app.use(morgan('combined'))
app.use(cors())

require('./db')

const bodyParser = require('body-parser')
app.use(bodyParser.urlencoded({ extended: true }))
app.use(bodyParser.json())

const port = process.env.PORT || 8080 // establecemos nuestro puerto

app.get('/', (req, res) => {
  res.json({ mensaje: '¡Hola Mundo!' })
})

app.use('/api', router)

// iniciamos nuestro servidor
// para tests, porque supertest hace el listen por su cuenta
if (!module.parent) {
  app.listen(port, () => console.log(`API escuchando en el puerto ${port}`))
}

// exportamnos la app para hacer tests
module.exports = app
```
