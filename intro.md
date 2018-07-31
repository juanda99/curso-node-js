
# Curso de node.js

## Temario del curso

1. Como empezar con Node.js:
- Instalación de Node
- Gestión de versiones de node
- Uso de linters
- Debug en Node
- Vistazo general a las características de ES6 y ES7. Qué puedo usar con Node

2. Realizar proyectos/paquetes con node
- Uso del gestor de paquetes
- Instalación y configuración de paquetes del repositorio de npm
- Gestión de dependencias
- Creación de una librería
- Publicación en npm

3. Crear scripts de servidor mediante node
- Aprender a manejar la naturaleza asíncrona de node: callbacks, promosas, async-await
- Uso de paquetes relacionados con el sistemas de ficheros
- Configuración de microservicios
 
4. Node.js para web:
- Crear un servidor API REST mediante express 
- Operaciones CRUD contra base de datos
- Operaciones CRUD autenticadas mediante tokens según OAUTH2
- Uso de Postman para comprobar API
- Uso de WebSockets



## Pasos previos
- Máquina virtual Ubuntu mediante Vmware
  - Usuario: curso
  - pwd: P@ssw0rd
- Instalación Visual Studio Code
- 
1. Node: como empezar:
- Instalación de node.
- Gestión de versiones de node.
- Debug en node con Visual Studio Code
- Vistazo general a las características de ES6 y ES7. Qué puedo usar con node (https://node.green/).


# JavaScript en servidor

![](nodejs.png)


## Qué es nodejs

- NodeJS es un intérprete de JavaScript que se ejecuta en servidor.
- Está basado en el motor de JavaScript que utiliza Google Chrome (V8), escrito en C++


## Características principales

- Mismo lenguaje en cliente y servidor
  - Permite a cualquier persona desarrollar en backend o en frontend
  - Permite reusar código o incluso mover código de cliente a servidor o al revés

- Está orientado a eventos y utiliza un modelo asíncrono (propio de JavaScript).


- Al contrario que en el navegador, encontramos muchas llamadas asíncronas:
  - Llamadas a APIs
  - Lectura y escritura de ficheros
  - Ejecución de cálculos en el servidor
  - ....


- Llamadas síncronas en servidor serían fatales:
  - ¡Bloqueariamos las conexiones al servidor hasta que acabase la instrucción bloqueante!
  - Al ser asíncrono podremos tener muchas sesiones concurrentes


- Es monohilo
- Utiliza un solo procesador
- Si queremos usar toda la potencia de la CPU, tendremos que levantar varias instancias de node y utilizar un balanceador de carga ([por ejemplo con pm2](https://github.com/Unitech/pm2))




## Desventajas
- Trabajar con código asíncrono hace que a veces el código no sea excesivamente legible
- Imagina que guardamos un registro de los accesos de los usuarios a nuestra app:

```
trackUser = function(userId) {
users.findOne({userId: userId}, function(err, user) {
var logIn = {userName: user.name, when: new Date};
logIns.insert(logIn, function(err, done) {
console.log('wrote log-in!');
});
});
```

- Tenemos 3 funciones anidadas en una simple operación.
- Esto es lo que se conoce como [callback hell](https://strongloop.com/strongblog/node-js-callback-hell-promises-generators/)


## Evitar el callback en el navegador

- Mediante el [uso de promesas](https://www.promisejs.org/)
- Se trata de escribir código asíncrono con un estilo síncrono.
- Opciones más actuales:
- Generators / Yields (ES6)
- Async / Await (ES7)
- El soporte de ES6 en node es limitado (--harmony) y también en el navegador => TRANSPILERS (babel)
- Ver [comparativa de métodos asíncronos](https://thomashunter.name/blog/the-long-road-to-asyncawait-in-javascript/)


## Hola Mundo en node

- Editamos un fichero en JavaScript, *holaMundo.js*:
```
console.log ("Hola Mundo");
```
- Lo ejecutamos mediante *node holaMundo.js*
- Si escribimos *node* sin más, podemos acceder a la consola de node, un intérprete de JavaScript, igual que el que tenemos en el navegador


## npm
- Es el gestor de paquetes de node
- Propongo hacer dos prácticas para coger la dinámica del uso de npm y sus librerías y de trabajar con node:
- Crear una librería en node.js
- Crear una api rest mediante node.js

## Qué es node 
- De la página de node:
Node.js® is a JavaScript runtime built on Chrome's V8 JavaScript engine.
- JavaScript en servidor
- Se ejecutan ambos con el mismo (v8 engine, motor de código abierto, escrito en C++) que compila a código máquina

$node
>console.log("Hola Mundo");


Si vamos a Chrome -> Developer tools haríamos lo mismo. En ambos casos ejecutamos el v8 engine.
Las diferencias es que node puede manipular por ej el sistema de ficheros, y javascript otras cosas como la url.
- Ver objeto window (todas las variables cuelgan de ahí) 
- Lo mismo en node con el objeto global    

- Document en browser
- process en node: ejemplo process.exit(0);   



## Ventajas
- Compañías como Uber, Wallmart, Netflix... 
- Blocking vs non blocking (dibujar esperas.... para ver las ventajas)
- Eventos attach event listener en el event loop.... con su función de callback

- Imagina un web server... blocking, no sirve más peticiones hasta que acaba la primera.. (blocking I/O, NET requests...)
- npm es el ecosistema más grande de librerías open source en el mundo

node es de naturaleza non blocking, hay libererías non-blocking en otros lenguajes (ej pyhton pero esas propias liberías están escritas en modo blocking)

- ¿Lo coge otro hilo? No, ....Nodejs tiene un solo thread....
- Levantar más threads no siempre es la solución (más gasto de memoria RAM o CPU por ej)

Ejemplo de uso de node: npm validate object (búsqueda en google o en npm, yo prefiero google)


## Hello World
- Ejemplo típico

require para usar modules!!


console.log(module);
module.exports.edad = (edad) => `Tienes ${edad} años`


const fechas = require('./edad.js');
console.log("Hola Mundo");
console.log(fechas.edad(15));


lodash
fs ---------> ¿No necesito npm install?
os
 
Install nodemon, development purposes (install with -g but better with --dev)



## Obtener input del usuario
- socketIO para obtener datos en tiempo real de una aplicación web
- Mediante una API y ajax requests
- Desde la línea de comandos

## Input desde la línea de comandos
- No utilizaremos nodemon porque vamos a cambiar la llamada a la línea de comandos
console.log(process.argv);

$ node app.js prueba
[ '/Users/juandaniel/.nvm/versions/node/v8.11.3/bin/node',
  '/Users/juandaniel/Code/kk/app.js',
  'prueba' ]

- Para recoger el parámetro prueba:
var command = process.argv[2];
console.log(`command: ${command}`);

if (command === 'add´) {
  console.log('adding user');
} else if (command === 'remove') {

} else {
  console.log('Command not recognized');
}

node app.js remove --user='pepe'
Entre comillas por si hay espacios, comidas dobles en windows!!! en Linux/Mac da igual

Pero como parseamos este segundo argumento????

Y si lo hacemos así:
node app.js remove --user pepe
¡¡Tendremos un tercer argumento!!!

Usaremos yargs para hacer el parseo desde línea de comandos. ----> Obtenemos un objeto
const yargs = require('yargs');
const argv = yargs.argv;
console.log(`Process: ${process.argv}`);
console.log(`yargs: ${argv}`);



## Working with json
const obj = {name: "pepe"};

var stringObj = JSON.stringify(obj); para obtener el string del objeto
console.log(typeof stringObj);
console.log(stringOjbj);

Observamos que los atributos también van con comillas.

var person = JSON.parse (string);
Ahora podemos obtener por ej:
console.log(typeof person);
console.log(person.name);
