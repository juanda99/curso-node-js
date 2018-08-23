## Conceptos

- Cuentas de usuario
  - User Model en Mongoose
- Hashing
- Encriptación
- Json web tokens


## Hashing

- El valor se guarda hasheado
- Se compara el valor que introduce el usuario con el valor que está ya hasheado
- No se puede deshashear.
- Tokens: un array de objetos
  - Token de acceso
  - Token de resetear password
  - Token de "uso"

## User Model

```js
{
  email: "pepe@pepe.com,
  password: "añdfkjasdñfkljad ñ",
  tokens: [{
    access: 'auth',
    token: 'adfadsfñj3ñjkr3'
  }]
}
```

## Validación de email valido

- Necesitamos una validación "a medida"
- [Ver *custom validators* en la documentación de Mongoose](https://mongoosejs.com/docs/validation.html):
```
var userSchema = new Schema({
  phone: {
    type: String,
    validate: {
      validator: function(v) {
        return /\d{3}-\d{3}-\d{4}/.test(v);
      },
      message: props => `${props.value} is not a valid phone number!`
    },
    required: [true, 'User phone number required']
  }
});
```

## Función para validar email

- Usaremos el [módulo npm validator](https://www.npmjs.com/package/validator)

```js
const mongoose = require('mongoose')
const validator = require('validator')

var UserSchema = new mongoose.Schema({
  email: {
    type: String,
    required: true,
    trim: true,
    minlength: 1,
    unique: true,
    validate: {
      validator: validator.isEmail,
      message: '{VALUE} is not a valid email'
    }
  },
  password: {
    type: String,
    require: true,
    minlength: 6
  },
  tokens: [{
    access: {
      type: String,
      required: true
    },
    token: {
      type: String,
      required: true
    }
  }]
})

var User = mongoose.model('User', UserSchema)
module.exports = {User}
```

## Ruta para crear usuarios

- Siguiendo reglas API REST

```js
app.post('/users', (req, res) => {
  var body = _.pick(req.body, ['email', 'password']);
  var user = new User(body);

  user.save().then((user) => {
    res.send(user);
  }).catch((e) => {
    res.status(400).send(e);
  })
});
```

- Si todo va bien, se devuelve 201 y el documento.
- Puede haber errores: 
  - Falta campo required
  - Longitud mínima password
  - Se devuelve código error http

wipe database para que user email uniq validation funcione ????? 11.56


## jwt y hashing

- Las rutas son públicas
- Algunas queremos hacerlas privadas
  - Solo accederán usuarios autenticados
  - Al autenticarse se obtiene un token
  - El token se envía al querer acceder a la ruta privada


## hashing

- Instalación librería
  
  ```bash
  npm i -S crypto-js
  ```

- Uso librería:
  
  ```js
  const {SHA256} = require('crypto-js');
  const password='Esta es mi contraseña'
  const hashedPassword = SHA256(password)
  console.log(`Password: ${password}`)
  console.log(`Hashed Password: ${hashedPassword})
  ```


- Siempre tiene el mismo valor el hash si la password es la misma
- No confundir con un algorítmo de encriptación
  - No hay posibilidad de desencriptar, es "one way"
  - Es igual a la comprobación de un fichero descargado
    - Se comprueba que la descarga sea correcta
    - No esté infectado


## generación de token

- Si el password es correcto, le envíamos datos al usuario:

  ```js
  var data = {id: 4}
  ```

- Por ejemplo el id de usuario (no el sessionID)
- Como el usuario lo puede cambiar, y acceder a otro usuario, debemos enviar otra cosa... algo que nos de integridad:

  ```js
  var token = {
    data,
    hash: SHA256(JSON.stringify(data)).toString()
  }
  ```

- El usuario podría cambiar también el hash
  - Necesitamos "sal en el hash"
  - Añadir algún valor aleatorio que solo conozca el generador del hash


  ```js
  var token = {
    data,
    hash: SHA256(JSON.stringify(data) + 'somesecret').toString()
  }
  ```

## Comprobación token

- Nos llega el siguiente token del usuario:

  ```js
  var token = {
    data: {id: 5},
    hash: "añdalñdkfñadj añsdlfgjk añdlk añlfdj dfñjk jjafdkjdfdfk"
  }
  ```

- Comprobamos si es correcto:

  ```js
  var resultHash = SHA256(JSON.stringify(token.data) + 'somesecret').toString()
  if (resultHash === token.hash) {
    console.log('Los datos no se han modificado')
  } else {
      console.log('Los datos están modificados, no se puede confiar')
  }
  ```


## Manipulación del token

- Generamos el token desde el cliente (no conocemos la sal):

  ```js
  var token = {
    data: {id: 5},
    hash: SHA256(JSON.stringify(token.data)
  }
  ```

- El token no coincidirá porque se desconoce la sal.

  ```js
  var resultHash = SHA256(JSON.stringify(token.data) + 'somesecret').toString()
  if (resultHash === token.hash) {
    console.log('Los datos no se han modificado')
  } else {
      console.log('Los datos están modificados, no se puede confiar')
  }
  ```

## Estandar JWT

- Objeto compuesto de:
  - Header: algorítmo y tipo de token (jwt en nuestro caso)
  - Payload: los datos
  - Verify Signature: hash para comprobar integridad
- Es un estándar y existen librerías que lo hacen más sencillo
  - Utilizaremos [jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken)
  - Tiene basicamente dos funciones
    - *jwt.sign*: para firmar el token
    - *jwt.verify*: para verificar el token


## Uso librería jsonwebtoken

```
const jwt = require('jsonwebtoken')
var data = {id: 1}
var token = jwt.sign('data', 'privatePassword')
console.log(token)
```

- Se puede verificar:
  - A través de la web http://jwt.io
  -  Mediante la propia librería:

  ```js
  var decoded = jwt.verify('token', 'privatePassword')
  ```

  - Si el token ha sido manipulado arrojará un error de **Invalid Signature**.


## Generar token al crear usuario

- Definimos un método a nivel de instancia
  - Si definimos el método a nivel de modelo no podríamos usar this (datos de la instancia)
  - Para usar this, la función no la podemos poner como arrow function

  ```js
  UserSchema.methods.generateAuthToken = function () {
    var user = this
    var access = 'auth'
    var token = jwt.sign({_id: user._id.toHexString(), access}, 'abc123').toString()

    user.tokens.push({access, token}) // si no funciona usar concat

    return user.save().then(() => {
      return token
    })
  }
  ```
  - ¡¡Ojo, retornamos una promesa!!!

## Envío de token al crear usuario

- Lo envíamos como header propio (prefijo x-)
- 

```
app.post('/users', (req, res) => {
  var body = _.pick(req.body, ['email', 'password'])
  var user = new User(body)

  user.save().then(() => {
    return user.generateAuthToken()
  }).then((token) => {
    res.header('x-auth', token).send(user)
  }).catch((e) => {
    res.status(400).send(e)
  })
})
```

## Workflow de acceso a rutas privadas

- Se solicita un Auth token
- Se valida el token
- Se busca el usuario asociado al token
- Y entonces se accede a la ruta privada


## Convertir ruta pública a privada

- La ruta de signUp o signIn siempre debe ser pública
- Puede haber muchas rutas privadas
  - Usaremos un middleware en dichas rutas para no duplicar código

app.get('/users/me', (req, res)=>{
  var token = req.header('x-auth')
  User.findByToken(token)
})

----------- repasar todo esto con vídeo 91 ----------

## Creamos función que haga la autenticación

- En este caso no dependemos de una instancia concreta, haremos un método estático:

```js
UserSchema.statics.findByToken = function (token) {
  var User = this
  var decoded

  try {
    decoded = jwt.verify(token, 'abc123')
  } catch (e) {
    return Promise.reject()
  }

  return User.findOne({
    '_id': decoded._id,
    'tokens.token': token,
    'tokens.access': 'auth'
  })
}
```
- El método devuelve una promesa:
  - Puede haber una excepción en jwt.verify
  - En caso de excepción retornaremos Promise.reject()


----------- repasar todo esto con vídeo 91 ----------


ruta /users/me.....

## Middleware en un fichero separado

- Adelgazamos nuestro server.js
- Creamos carpeta middlewares
- Creamos fichero authenticate:

  ```js
  var {User} = require('../models/user')

  var authenticate = (req, res, next) => {
    var token = req.header('x-auth')

    User.findByToken(token).then((user) => {
      if (!user) {
        return Promise.reject()
      }
      req.user = user
      req.token = token
      next()
    }).catch((e) => {
      res.status(401).send()
    })
  }

  module.exports = {authenticate}
  ```

## Usar middleware authenticate

- Eliminamos la función authenticate
- Cargamos el módulo authenticate

  ```js
  const {authenticate} = require('./middleware/authenticate')
  ```


## Hash de la contraseña de usuario

- Ahora el usuario hace un POST /users
  - Envía su contraseña sin encriptar
  - Se guarda como texto plano
- Usaremos el módulo bcrypt
  - Guardaremos la contraseña encriptada
  - Utilizaremos una sal específica para cada encriptación
  
  ```bash
    npm i -S bcryptjs
  ```

- Hay otras librerías basadas en bcrypt, por ej. bcrypt. 
- La que usamos esta toda hecha en js y es más portable, da menos problemas


## Ejemplo encriptación mediante bcrypt

- Se deben ejecutar dos métodos
  - **bcrypt.genSalt** para la generación de la sal
  - **bcrypt.hash** para crear el hash

```js
const bcrypt = require('bcryptjs');
const password = process.argv[2];
console.log(`password: ${password} `);
bcrypt.genSalt(10, (err, salt) => {
  console.log(`Salt: ${salt}`)
  bcrypt.hash(password, salt, (err, hash) => {
      console.log(hash) // esto es lo que queremos guardar en bbdd no el passwd
})
```

## Salida de bcrypt

- Guardamos la sal con la contraseña

  ```bash
  $<algorithm>$<iterations>$<salt>$<hash>
  ```

  - el algoritmo de hash
  - El númer de iteraciones o factor de trabajo
  - La sal aleatoria
  - La contraseña resultante o hash


![Ejemplo hash con bcrypt](img/bcrypt-salida.png)



## Chequeo de contraseña


bcrypt.compare(password, hashedPassword, (err, res)=>{
  // res es true o false
})


## Ejercicio bcrypt

- Utiliza **bcrypt.compara**:
  - Comprueba que devuelve true si el password es correcto
  - Comprueba que devuelve false si el password es erróneo


## Solución ejecicio bcrypt

```js
const bcrypt = require('bcryptjs');
const password = process.argv[2];

hashedPassword='$2a$10$dH5q2dYWbwLMXrPnCMQ52epdQlyvhUqZrUg5iEhXXIkHzPx3G.C/2'
bcrypt.compare(password, hashedPassword, (err, res)=>{
  if (err) console.log(`Error: ${err}`)
  console.log(`El resultado de la comparación es: ${res}`)
})
```


## Mongoose middleware

- Permite ejecutar cierto código antes o después de ciertos eventos
- ¿Para que nos va a servir?
  - Antes de guardar el documento (usuario)
  - Cambiaremos el password por el hash
- [Ver documentación](https://mongoosejs.com/docs/middleware.html)
  - Se pueden ejecutar en serie o en paralelo
  - Podemos utilizar async-await (se verá más adelante)

  ```js
  var schema = new Schema(..);
  schema.pre('save', function(next) {
    // do stuff
    next();
  });
  ```


## Ejercicio implementar middleware

- Implementa middleware según la siguiente funcionalidad:
  - Al guardar un usuario, su contraseña se debe cambiar por el hash
  - Si la contraseña no ha cambiado, no se debe modificar el hash
    - El método save puede llamarse en una actualización por ej.
    - Ayúdate del método isModified que proporciona MongoDB.

  ```js
  UserSchema.pre('save', function (next) {
    var user = this

    if (user.isModified('password')) {
      // user.password

      // user.password = hash
      // next()
    } else {
      next()
    }
  })
  ```

## Solución implementación middleware

```js
UserSchema.pre('save', function (next) {
  var user = this

  if (user.isModified('password')) {
    bcrypt.genSalt(10, (err, salt) => {
      bcrypt.hash(user.password, salt, (err, hash) => {
        user.password = hash
        next()
      })
    })
  } else {
    next()
  }
})
```

- Y probar que funcione :-)