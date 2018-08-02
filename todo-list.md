



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
