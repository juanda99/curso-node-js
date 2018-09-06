# Curso de node.js

Septiembre 2018

25 horas


## Acceso al curso

- [Fuentes del curso en GitHub](https://github.com/juanda99/curso-node-js)
- Visualización vía diapositivas:

```bash
git clone https://github.com/juanda99/curso-node-js.git .
npm install
npm start
```


## Ponente

- Juan Daniel Burró Aláez (juandacorreo@gmail.com)
  - Profesor informática en CPIFP Los Enlaces
  - Actuálmente en atribución funciones: 
    - Desarrollo [nuevo portal Arasaac](https://www.beta.arasaac.org)
      - SPA con React.js
      - API REST con node.js y MongoDB



## Temario del curso


## Como empezar con Node.js

- Instalación de Node
- Gestión de versiones de node
- Uso de linters
- Debug en Node
- Vistazo general a las características de ES6 y ES7.
  - Qué puedo usar con Node


## Realizar proyectos/paquetes con node

- Uso del gestor de paquetes
- Instalación y configuración de paquetes del repositorio de npm
- Gestión de dependencias
- Creación de una librería
- Publicación en npm


## Crear scripts de servidor mediante node

- Aprender a manejar la naturaleza asíncrona de node: callbacks, promesas, async-await
- Uso de paquetes relacionados con el sistemas de ficheros
- Configuración de microservicios


## Node.js para web:

- Crear un servidor API REST mediante express 
- Operaciones CRUD contra base de datos
- Operaciones CRUD autenticadas mediante tokens según OAUTH2
- Uso de Postman para comprobar API
- Uso de WebSockets



## Enfoque del curso


## [Configuración inicial](./configuracion-inicial.md)

- Trabajamos con una máquina virtual prácticamente preparada:
  - Visual Code (sin configurar)
  - Docker
  - zsh
  - ...


## [Introducción a Node.js](./intro.md)

- Características principales de Node.js


## Proyectos

- Proyectos pequeños donde veremos los contenidos:
  - npm
  - Promises y async-await
  - Servidor web con node, arquitectura API REST y operaciones CRUD
  - Autenticación con json web tokens
  - WebSockets


- Utilizaremos en varios proyectos las siguientes tecnologías:
  - Gestión versiones mediante git
  - Test con enfoque BDD (Behaviour Driven Development) y Code Coverage
  - Integración continua
  - Dockerización de servidores adicionales (MongoDB)
  - Despliegue serverless



## Contenido de los proyectos


## [Proyecto 1: Aplicación ¡Hola Usuario!](./1-proyecto-hola-usuario.md)

- *Escribe un mensaje en un fichero:*
  
  ```bash
  Hola usuario, tienes 17 años
  ```
  - *Usuario es el username del equipo*
  - *Edad es una variable*
  
- Uso de **módulos propios** y **built-in modules**


## [Proyecto 2: Documentación apuntes](./2-proyecto-apuntes.md)
  
- *Generación de slides y pdf a partir de markdown, útil para hacer apuntes*
- Uso de **módulos de terceros**
- Configuración de proyecto y gestión de paquetes mediante **npm**


## [Proyecto 3: Crear una librería](./3-proyecto-libreria.md)

- *Crear una librería a partir de un json de datos*
- Configuración de **linters**
- Uso de **tests** 
- **Publicación en el repositorio de npm**


## [Proyecto 4: Lista de tareas](./4-app-notas.md)

- *Clásico ejercicio de todo list con persistencia en fichero*
- Uso de json


## [Proyecto 5: Servidor web](./5-express.md)

- *Servidor web mediante paquetes de node*
- Uso de Express como servidor web
  - Arquitectura y uso de middlewares
  - Uso de templates
- Despliegue serverless


## [Proyecto 6: API REST](./6-api.md)

- *Servidor web con implementación de API REST*
- Uso básico librería express, middlewares y arquitectura MVC
  - Uso de MongoDB como base de datos
  - Uso de docker para virtualizar servicio de bbdd
- Uso de POSTMAN para tests de la api autenticada


## [Proyecto 7: Autenticación](./7-auth.md)

- *Autenticación en servidor web*
- Uso de json web tokens
- Uso de POSTMAN para tests de la api autenticada


## [Proyecto 8: Websockets](./8-websockets.md)

- Uso de librería socket.io


## [Proyecto 9: Promesas](./9-promises.md)

- Uso de promesas para consumir APIS
- Uso a async-await



## ¿Empezamos?

- [Configuración inicial](./configuracion-inicial.md)