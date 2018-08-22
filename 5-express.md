# Proyecto 5: Servidor web mediante Express

## Tiempo estimado: 60 minutos


## Descripción

- *Montar un servidor web mediante paquetes propios de node*


## Objetivos

- Creación de un servidor web que sirva:
  - Contenido estático
  - Contenido dinámico en base a templates
  - Content-type en JSON (para API posterior)



## Empezamos proyecto

## Documentación

- https://expressjs.com/
  - Extensa pero consisa
  - Numerosos ejemplos

## Primeros pasos

- Fork del repositorio en GitHub y clone de tu repositorio
- npm init
- Instalar y configurar eslint (standard)
- Instalar express
  
  ```bash
  npm i -S express*
  ```

- Crear directorio app y tests para nuestra aplicación
  ¿¿¿los creo yo antes???


  Content-type
  json-viewer

# Creación de una API con node.js


## express

- Utilizaremos [express](http://expressjs.com/es/) para realizar la API
- Instalar express mediante uno de los comandos siguientes:

  ```bash
  npm install --save express
  npm i -S express
  ```
- Creamos el fichero app/server.js donde pondremos el código necesario para testear una API muy básica para probar Express.


- Utiliza el plugin ExpressSnippet de Visual Code para completado



```
const express = require('express')
const app = express()
app.get('', (req, res) => {
    res.send('Hola Mundo')
})
app.listen(3000)
```

- Comprueba el funcionamiento
- Cambia Hola Mundo por un JSON
- Comprueba como cambia el content type
- Mejor res.json (hace el JSON.stringify)
- app.listen permite un segúndo parámetro (función de callback), utilizalo para sacar por consola un mensaje de que la aplicación está arrancada y en que puerto.
- 



## Uso de contenido estático

- Crea un fichero .html en la carpeta public
  - Ayúdate de emmet: *! + tab*
- Configura express para usar un middleware:

  ```js
  app.use(express.static(__dirname + '/public'))
  // app.use('/static', express.static(path.join(__dirname, 'public')))
  ```
- *__dirname* es la raíz del proyecto
- *path.join* para que sea multiplataforma
- Podríamos configurar un directorio virtual (static para public)


- static actual como un mniddleware, si hay un fichero responde el static, si no la ruta configurada.

## Middleware

- Son funciones que tienen acceso al objeto de solicitud (req), al objeto de respuesta (res) y a la siguiente función de middleware en el ciclo de solicitud/respuestas de la aplicación. 

- Se registran mediante:
  ```
  app.use((req, res, next)=>{
    //operaciones del middleware
    next() //para ir al siguiente middleware o a la ruta
  })
  ```
- next() nos sirve para indicar que el middleware ha acabado. Si hacemos algo asíncrono, por ej, el middleware no se moverá hasta que ejecutemos next().
  Podemos probar a quitar next() cuando hacemos la petición, se queda "colgada"


## Ejemplo de middleware

- Podemos crear un middleware que guarde traza de las fechas de acceso:
- Para ver las propiedades que nos hacen falta podemos usar la [API de Express](https://expressjs.com/en/4x/api.html)

  ```js
  var app = express();

  app.use(function (req, res, next) {
    var now = new Date().toString()
    console.log(`Time: ${now} ${req.method} ${req.url}`)
    next();
  });
  ```

  

  Añadimos el appendFile.
  Aunque sea asíncrono.... se ejecuta el next() y si luego hace falta se muestra el error.

  ```js
  app.use((req, res, next) => {
  var now = new Date().toString()
  var log = `${now}: ${req.method} ${req.url}`

  console.log(log)
  fs.appendFile('server.log', log + '\n')
  next()
});


## Práctica middleware y template

- Crea un middleware que muestre una página de mantenimiento (maintenance.hbs si ha hay algún error


  ```js

  app.use((req, res, next)=>{
    res.render('maintenance.hbs')
  })

  ```

- ¡Se tiene que ejecutar antes del static!!!


## Template engine
- Variables
- Reusable markup: header, footer...
- handlebars, ejs...

npm i -S hbs


const hbs = require('hbs')
app.set('view engine', 'hbs'); //key value

Nuevo fichero para la vista

views/about.hbs

res.send -----> res.render('about.hbs')
res.render, admite objeto como segundo parámetro para pasar variables


## Handlebars más avanzado

Partial:

const hbs = require('hbs')
hbs.registerPartials(path.join(__dirname, partials))
app.set('view engine', 'hbs'); //key value

Debemos indicar que vamos a usar partials y el directorio donde están
{{ >footer}} para el partial partials/footer.hbs


Los cambios no los visualizaremos hasta que cambiemos las extensiones que monitoriza nodemon:
nodemon server.js  -e js,hbs


Yo creo el footer.
Ejercicio, crear el header.


helpers -> handlebars funciones para crear contenido dinámico
-> ver ejemplo fecha y poner en mayúsculas
