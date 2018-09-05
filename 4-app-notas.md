# Proyecto 4: Aplicación de gestión de notas

## Tiempo estimado: 150 minutos


## Descripción

- *Generación de una lista de notas con almacenamiento mediante json y ficheros*


## Objetivos

- Repaso de lo visto en proyectos anteriores
- Usar objetos JSON
  - Serializar
  - Deserializar



## Empezamos proyecto


## Pasos previos

- Pasos similares a la práctica anterior
  - [Fork del proyecto](https://github.com/juanda99/curso-node-js-proyecto-notas)
  - Clone al equipo local
  - Configuración del linter
- Vemos capturas de la práctica anterior


## Crear repositorio en GitHub

- Realizamos un fork de mi proyecto
  - Así tendremos el fichero para la práctica
  - .gitignore correctamente configurado
  - Puedo hacer seguimiento de vuestros desarrollos


![](img/hacer-fork.png)


![](img/visualizacion-fork.png)


![](img/listado-fork.png)


## Clonar repositorio a local

```bash
git clone <url proyecto>
```

- La url la copiamos del repo de GitHub (ver captura)
  - ssh normalmente en linux (necesitas importar la clave pública a GitHub)
  - https normalmente en windows / mac


![](img/clonar-repo.png)


## Crear proyecto

```bash
  cd <url proyecto>
  npm init
```

- package_name debe ser único: **no puede haber dos proyectos con el mismo nombre en npm**
- El *entry-point* lo pondremos en *src/index.js*, así separaremos nuesto código fuente de los tests.
- El resto de parámetros con sus valores por defecto
- ¡Ya tenemos nuestro **package.json** creado!


## Estilo de código

- Puede que colabore más gente en nuestra librería
  - Queremos un estilo uniforme
- Y si nos detecta fallos mejor

- Instalaremos eslint (*D* o *--save-dev*)

  ```bash
  npm i -D eslint
  ```


## Configuración de eslint

```bash
$ node_modules/.bin/eslint --init
? How would you like to configure ESLint?
  Use a popular style guide
? Which style guide do you want to follow?
  Standard
? What format do you want your config file to be in?
  JSON
? Would you like to install them now with npm?
  Yes
```




## Empezamos nuestra app

- Creamos *app.js* como punto de entrada de nuestra aplicación
- Creamos un módulo específico para nuestras notas
  - Servirá para añadir o quitar notas

  ```js
    console.log('Modulo de notas cargado')
  ```

- Lo cargamos en nuestra aplicación (*app.js*)

  ```js
  console.log('Aplicación de notas arrancada.')
  const notes = require('./notes')
  ```


## Estructura módulo notes.js

- module.exports debe ser un objeto

```js
module.exports.addNote = function () {
  console.log('Nueva nota')
  return 'Nueva nota'
}
```

- *app.js* hace el require del módulo y lo almacena en una variable

```js
const notes = require('notes')
const notaAñadida = notas.addNote()
```

- ¿Sabrías crear la función removeNote?


## Solución

- Notes.js:

```js
module.exports.removeNote = function (id) {
  console.log(`Nota borrada con id=${id}`)
  return 'Nota borrada'
}
```

- app.js

```js
const notes = require('notes')
const notaAñadida = notas.addNote()
const notaEliminada = notas.removeNote(5)
```


## Repaso ES6

- Arrow functions
- Object Literal Property Value Shorthand


## Arrow functions

```js
const sumarNumeros =  function (a, b) { return a + b }
const sumarNumerosES6 = (a, b) => { return a + b }
const sumarNumerosES6bis = (a, b) => a + b
```


## Sintaxis Arrow functions

- Sintaxis general:

  ```js
  (param1, param2, …, paramN) => { statements }
  ```

- Si solo hay una expresión, se pueden omitir las llaves:

  ```js
  (param1, param2, …, paramN) => expression
  ```

- Si no hay parámetros, paréntesis obligatorios:

  ```js
  () => { statements }
  ```

- Si solo hay un parámetro, parámetros opcionales:

  ```js
  singleParam => { statements }
  ```


## ES6: Object Literal Property Value Shorthand

- Antes (ES5):

```js
function createMonster(name, power) {
  return { type: 'Monster', name: name, power: power }
}
```

- Ahora (ES6):

```js
function createMonster(name, power) {
  return { type: 'Monster', name, power }
}
```


- Y con arrow functions:

```js
const createMonster = (name, power) =>  ({ type: 'Monster', name, power })
}
```

- Esta opción no sería correcta:
  - El compilador piensa que hay un block statement, y no un objeto
  - Retorna *undefined*

```js
const createMonster = (name, power) =>  { type: 'Monster', name, power }
}
```


## Solución con ES6

```js
const addNote =  () => {
  console.log('Nueva nota')
  return 'Nueva nota'
}
const removeNote =  id => {
  console.log(`Nota borrada con id: ${id}`)
  return 'Nota borrada'
}
module.exports = {
  addNote,
  removeNote
}
```



## Instalar módulos de terceros

- Necesitamos guardar la referencia a los módulos que guardamos:

```bash
# este comando ya lo hemos ejecutado previamente
# y tenemos la dependencia eslint guardada
npm init # para tener un package.json & package-lock.json
```

- Configuraremos el script start para que ejecute nuestra aplicación


## Guardar módulos de terceros

- Se guardan en node_modules
- No se deben sincronizar con GitHub (usar *.gitignore*)
- Usar *npm install* para descargarlos


## nodemon

- Es un comando relativo al CLI
- Instalamos

  ```bash
  npm install --save-dev nodemon # o más corto:
  npm i -D nodemon
  ```

- Cambiaremos en script *npm start* para utilizarlo


## Input de una aplicación

- Una aplicación puede obtener el input de distintas formas:
  - Práctica socket.io
    - Mediante websockets
  - Práctica API:
    - Parámetros en URL
    - Parámetros en el header
  - Práctica actual
    - Mediante línea de comandos


## Input de CLI

- Los podemos obtener de *process*:

  ```js
  console.log(process.argv)
  ```

  - *argv[0]* es el ejecutable de node
  - *argv[1]* es *app.js*
  - *argv[2]* es el parámetro


## Ejercicio

- Recoger los distintos input que puede recibir la aplicación *if-else
*:
  - add: para añadir una nota
  - list: para sacar la lista de notas
  - read: para leer una nota en particular
  - remove: para eliminar una nota
  - cualquier otro caso: avisar de que el comando no se reconoce


## Solución con if-else

```js
console.log('Aplicación de notas arrancada.')
const notes = require('./notes')

var command = process.argv[2]
console.log('Command: ', command)
console.log(process.argv)

if (command === 'add') {
  console.log('Añadiendo nueva nota')
} else if (command === 'list') {
  console.log('Listado de todas las notas')
} else if (command === 'read') {
  console.log('Leyendo notas')
} else if (command === 'remove') {
  console.log('Borrando nota')
} else {
  console.log('Comando desconocido')
}
```


## Solución con switch

```js
console.log('Aplicación de notas arrancada.')
// const notes = require('./notes')

var command = process.argv[2]
console.log('Command: ', command)
console.log(process.argv)

switch (command) {
  case 'add':
    console.log('Añadiendo nueva nota')
    break
  case 'list':
    console.log('Listado de todas las notas')
    break
  case 'read':
    console.log('Leyendo notas')
    break
  case 'remove':
    console.log('Borrando nota')
    break
  default:
    console.log('Comando desconocido')
    break
}
```


## Problemática solución

- Los parámetros pueden llevar argumentos extra:

```bash
node app.js remove --title="Nota 1"
```

- Los argumentos extra pueden escribirse de múltiples maneras:

  ```js
  node app.js remove --title="Nota 1"
  node app.js remove --title "Nota 1"
  node app.js remove -t="Nota 1"
  node app.js remove -t "Nota 1"
  node app.js remove --author="Peter"
  ```


- Nos interesa recibir los argumentos de una forma sencilla
  - Sin tener que crear un parser
  - Si nuestra entrada fuera un objeto sería más sencillo

- Usaremos [yargs](https://www.npmjs.com/package/yargs)


## Uso de [yargs](https://www.npmjs.com/package/yargs)

- Instalación:

```bash
npm i -S yargs
```

- Comparar salidas:

```js
console.log('Aplicación de notas arrancada.')
const notes = require('./notes')
// const yargs = require('yargs')
// const argv = yargs.argv
const { argv } = require('yargs')

// var command = process.argv[2]
var command = argv._[0]
console.log('Command: ', command)
console.log(process.argv)
console.log('Yargs', argv)
```


## Ejecución

- Comprueba que la salida de estos comandos no aportan grandes diferencias si usamos *yargs* o *process*:

  ```js
  node app add
  node app add encrypted
  ```

- Pero si usamos parejas key-value, la cosa cambia:

  ```js
  node app add --title="test"
  node app add --title "test"
  node app add --title test
  ```


## Añadir una nota

- Debemos llamar a la función addNote con dos parámetros: title y body:

```js
if (command === 'add') {
  notes.addNote(argv.title, argv.body);
}
```

- La nueva función addNote sería:

```js
const addNote = (title, body) => {
  console.log('Nota añadida: ', title, body)
}
```

- Probamos:

```js
node app.js add --title=test1 --body="Esta es mi nota de prueba"
```


## Ejercicio procesar parámetros

- Implementa las funciones relativas a las notas, según el siguiente código:

  ```js
  if (command === 'add') {
    notes.addNote(argv.title, argv.body);
  } else if (command === 'list') {
    notes.getAll();
  } else if (command === 'read') {
    notes.getNote(argv.title);
  } else if (command === 'remove') {
    notes.removeNote(argv.title);
  } else {
    console.log('Comando desconocido');
  }
  ```


## Solución proceso de parámetros

```js
console.log('Módulo de notas cargado')
const addNote = (title, body) => {
  console.log('Nota añadida: ', title, body)
}
const getAll = () => {
  console.log('Obtenidas todas las notas')
}
const getNote = (title) => {
  console.log('Obtenida nota: ', title)
}
const removeNote = (title) => {
  console.log('Nota borrada', title)
}
module.exports = {
  addNote,
  getAll,
  getNote,
  removeNote
}
```


## Almacenamiento de notas

- Utilizamos objetos para trabajar en nuestro código:

  ```js
  const nota1 = {
    title: titulo1,
    body: body1
  }
  ```

- Si se inserta una nueva nota, lo suyo sería guardarla en algún sitio
  - Array de objetos
  - Ficheros / bbdd
  - API


## Serialización

- Para almacenar en ficheros o envio via API debemos utilizar *strings* o *buffers*.
- Se hace necesario serializar nuestros objetos de notas.
- Haremos un par de ejercicios para ver si sabemos:
  - Convertir un JSON  a string (escritura de nota)
  - Parsear un string a JSON (lectura de nota)


## Ejercicio serialización objeto

- Convierte el siguente objeto a un string y muestra su tipo y valor por consola:

  ```js
  var persona = {
    nombre: 'Pepe',
    edad: '25'
  }
  ```

- Se utiliza *JSON* para hacer la conversión
- Se utiliza *typeof* para ver el tipo de datos


## Solución serialización

```js
const persona = {
  nombre: 'pepe',
  edad: 25
}
console.log(persona)
const serializedPersona = JSON.stringify(persona)
console.log(serializedPersona)
console.log(typeof persona)
console.log(typeof serializedPersona)
```


## Ejercicio deserializar objeto

- Convierte el string siguiente a JSON y obten la edad:

  ```js
  var personaString = '{"nombre": "Pepe", "edad": 25}'
  ```


## Solución deserialización

```js
var personaString = '{"nombre": "Pepe", "edad": 25}'
var persona = JSON.parse(personaString)
console.log(persona.edad)
```


## Implementar inserción de notas

- Leemos fichero con lista de notas
- Añadimos la nota recibida
- Las volvemos a llevar al fichero
- Serializamos/deserializamos según proceda


```js
const fs = require('fs')

const addNote = (title, body) => {
  let notes = []
  const note = {
    title,
    body
  }

  const notesString = fs.readFileSync('notes-data.json')
  notes = JSON.parse(notesString)

  notes.push(note)
  fs.writeFileSync('notes-data.json', JSON.stringify(notes))
}
```


## Probar inserción notas

- Saltará un error porque el fichero *notes-data.json* no existe
- Soluciones:
  - Crear previamente el fichero
  - Gestionar la excepción desde el código

```bash
node app.js add --title='titulo' --body='texto de la nota'
```


## Gestión de la excepción

```js
const fs = require('fs')

const addNote = (title, body) => {
  let notes = []
  const note = {
    title,
    body
  }

  try {
    const notesString = fs.readFileSync('notes-data.json')
    notes = JSON.parse(notesString)
  } catch (error) {

  }
  notes.push(note)
  fs.writeFileSync('notes-data.json', JSON.stringify(notes))
}
```


## Gestión de duplicados

```js
const fs = require('fs')

const addNote = (title, body) => {
  let notes = []
  const note = {
    title,
    body
  }

  try {
    const notesString = fs.readFileSync('notes-data.json')
    notes = JSON.parse(notesString)
  } catch (error) {}
  const duplicateNotes = notes.filter(note => note.title === title)

  if (duplicateNotes.length === 0) {
    notes.push(note)
    fs.writeFileSync('notes-data.json', JSON.stringify(notes))
  }
```


## Funciones para refactorizar código

- Leer y guardar notas se va a hacer varias veces

```js
var fetchNotes = () => {
  try {
    var notesString = fs.readFileSync('notes-data.json');
    return JSON.parse(notesString);
  } catch (e) {
    return [];
  }
}

var saveNotes = (notes) => {
  fs.writeFileSync('notes-data.json', JSON.stringify(notes));
}
```


## Ejercicio refactorizar

- Utiliza las funciones anteriores para refactorizar el código
- Devuelve la nota creada a *app.js* de modo que la muestre por consola


## Solución ejercicio refactorizar

- notes.js:

```js
var addNote = (title, body) => {
  var notes = fetchNotes()
  var note = {
    title,
    body
  }
  var duplicateNotes = notes.filter((note) => note.title === title)

  if (duplicateNotes.length === 0) {
    notes.push(note)
    saveNotes(notes)
    return note
  }
}
```


- app.js:

```js
if (command === 'add') {
  var note = notes.addNote(argv.title, argv.body)
  if (note) {
    console.log('Nota creada')
    console.log(`\tTítulo: ${note.title}`)
    console.log(`\tTexto: ${note.body}`)
  } else {
    console.log('Ya existe una nota con este título');
  }
} else if (command === 'list') {
  notes.getAll()
} else if (command === 'read') {
  notes.getNote(argv.title)
} else if (command === 'remove') {
  notes.removeNote(argv.title)
} else {
  console.log('Comando desconocido')
}
```


## Implementar removeNote

- Basándote en la función addNote, implementa la función removeNote
- Muestra por consola el resultado (fichero app.js)

  ```js
  var removeNote = (title) => {
    // obtener notas
    // utilizar filter para eliminar nota según title
    // guardar notas

    return true // or false
  }
  ```


## Solución removeNote

- notes.js:

```js
var removeNote = (title) => {
  var notes = fetchNotes()
  var filteredNotes = notes.filter((note) => note.title !== title)
  saveNotes(filteredNotes)
  return notes.length !== filteredNotes.length
}
```

- app.js

```js
else if (command === 'remove') {
  var noteRemoved = notes.removeNote(argv.title)
  var message = noteRemoved ? 'Nota borrada' : 'Nota no encontrada'
  console.log(message)
}
```


## Implementar leer una nota

- Implementa el método readNote en función del título de la nota
- Si usas la función filter, ten en cuenta que siempre devuele un array
- Refactoriza el código usando la función logNote

```js
var logNote = (note) => {
  console.log(`\tTítulo: ${note.title}`)
  console.log(`\tTexto: ${note.body}`)
}
```


## Solución leerNota

- note.js:

``` js
var getNote = (title) => {
  var notes = fetchNotes()
  var filteredNotes = notes.filter((note) => note.title === title)
  return filteredNotes[0]
}
```

- Recuerda exportar la función para utilizarla en app.js


- app.js:

  ```js
  else if (command === 'read') {
    var note = notes.getNote(argv.title)
    if (note) {
      console.log('Nota encontrada:')
      notes.logNote(note)
    } else {
      console.log('Nota no entontrada')
    }
  }
  ```

- utiliza logNote en otras partes del código (al añadir nota)


## Ejercicio: listado de notas

- Implementa la función getAll para obtener todas las notas
- Muestra las notas por consola desde *app.js*


## Solución:

- notes.js:

```js
var getAll = () => {
  return fetchNotes()
}
```

- app.js:

```js
else if (command === 'list') {
  var allNotes = notes.getAll();
  console.log(`Mostrando ${allNotes.length} notas.`);
  allNotes.forEach((note) => notes.logNote(note));
}
```


## Y mucho más

- yargs avanzado, parámetros requeridos...
  - [Ver documentación](https://github.com/yargs/yargs/blob/master/docs/examples.md)

- Ejecución al estilo *bash script*:

  ```bash
  #!/usr/bin/env node
  ```


## ¿Continuamos?

- Siguiente proyecto:
  - [Servidor Web mediante Express.js](./5-express.md)