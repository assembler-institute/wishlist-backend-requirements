# Requerimientos para el servidor backend utilizando express para gestionar una lista de cosas por hacer. 📚

  

## Introducción 💡

El objetivo de este documento es especificar los requisitos funcionales y no funcionales del sistema que se va a desarrollar para gestionar una lista de cosas por hacer. El sistema consistirá en un servidor backend que utilizará el framework express para implementar una API RESTful que permita a los usuarios crear, consultar, modificar y eliminar tareas. El sistema se comunicará con una base de datos MongoDB para almacenar y recuperar la información de las tareas.

## Requisitos funcionales 📕

Los requisitos funcionales describen las funcionalidades que el sistema debe ofrecer a los usuarios. Se expresan mediante casos de uso, que son escenarios que ilustran cómo el usuario interactúa con el sistema para lograr un objetivo.

### Caso de uso 1: Crear una tarea ✏️

-   **Actor:** Usuario
    
-   **Precondición:** El usuario está autenticado en el sistema
    
-   **Flujo principal:**
   
	-   El usuario envía una petición POST al servidor con el título y la descripción de la tarea que quiere crear
    
	-   El servidor valida los datos de la petición y crea la tarea en la base de datos
    
	-   El servidor responde con un código 201 (Created) y devuelve la tarea creada con su identificador único
    

-   **Flujo alternativo:**
    

	-   Si los datos de la petición son inválidos o faltan campos obligatorios, el servidor responde con un código 400 (Bad Request) y un mensaje de error
    

### Caso de uso 2: Consultar una tarea 🔎

-   **Actor:** Usuario
    
-   **Precondición:** El usuario está autenticado en el sistema
    
-   **Flujo principal:**
    

	-   El usuario envía una petición GET al servidor con el identificador de la tarea que quiere consultar
    
	-   El servidor busca la tarea en la base de datos y verifica que pertenece al usuario
    
	-   El servidor responde con un código 200 (OK) y devuelve la tarea solicitada
    

-   **Flujo alternativo:**
    

	-   Si el identificador de la tarea es inválido o no existe, el servidor responde con un código 404 (Not Found) y un mensaje de error
    
	-   Si la tarea no pertenece al usuario, el servidor responde con un código 403 (Forbidden) y un mensaje de error
    

### Caso de uso 3: Modificar una tarea 📝

-   **Actor:** Usuario
    
-   **Precondición:** El usuario está autenticado en el sistema
    
-   **Flujo principal:**
    

	-   El usuario envía una petición PUT al servidor con el identificador de la tarea que quiere modificar y los nuevos datos (título, descripción o estado)
    
	-   El servidor busca la tarea en la base de datos y verifica que pertenece al usuario
    
	-   El servidor valida los datos de la petición y actualiza la tarea en la base de datos
    
	-   El servidor responde con un código 200 (OK) y devuelve la tarea modificada
    

-   **Flujo alternativo:**
    

	-   Si el identificador de la tarea es inválido o no existe, el servidor responde con un código 404 (Not Found) y un mensaje error
    
	-   Si la tarea no pertenece al usuario, el servidor responde con un código 403(Forbidden) y un mensaje error.
    
	-   Si los datos de la petición son inválidos o faltan campos obligatorios, el servidor responde con un código 400 (BadRequest) y un mensaje de error.
    

### Caso de uso 4: Eliminar una tarea 🧨

-    **Actor:** Usuario
    
-    **Precondición:** El usuario está autenticado en el sistema
    
-    **Flujo principal:**
    

		-    El usuario envía una petición DELETE al servidor con el identificador de la tarea que quiere eliminar.
    
		-    El servidor busca la tarea en la base de datos y verifica que pertenece al usuario
    
		-    El servidor elimina la tarea de la base de datos.
    
		-    El servidor responde con un código 204 (No Content).
    

-    **Flujo alternativo:**
		-    Si el identificador de la tarea es inválido o no existe, el servidor responde con un código 404 (Not Found) y un mensaje de error
    
		-    Si la tarea no pertenece al usuario, el servidor responde con un código 403 (Forbidden) y un mensaje de error
    

  

### Caso de uso 5: Archivar una tarea 🖨️

-   **Actor:** Usuario
    
-   **Precondición:** El usuario está autenticado en el sistema y la tarea está completada
    
-   **Flujo principal:**
	-   El usuario envía una petición PATCH al servidor con el identificador de la tarea que quiere archivar y el estado “archivada”
    
	-   El servidor busca la tarea en la base de datos y verifica que pertenece al usuario y que está completada
    
	-   El servidor actualiza el estado de la tarea en la base de datos
    
	-   El servidor responde con un código 200 (OK) y devuelve la tarea archivada
    

-   **Flujo alternativo:**
	-   Si el identificador de la tarea es inválido o no existe, el servidor responde con un código 404 (Not Found) y un mensaje de error
    
	-   Si la tarea no pertenece al usuario o no está completada, el servidor responde con un código 403 (Forbidden) y un mensaje de error
    

### Caso de uso 6: Completar una tarea ✅

-   **Actor:** Usuario
    
-   **Precondición:** El usuario está autenticado en el sistema y la tarea está pendiente
    
-   **Flujo principal:**
	-   El usuario envía una petición PATCH al servidor con el identificador de la tarea que quiere completar y el estado “completada”
    
	-   El servidor busca la tarea en la base de datos y verifica que pertenece al usuario y que está pendiente
    
	-   El servidor actualiza el estado de la tarea en la base de datos
    
	-   El servidor responde con un código 200 (OK) y devuelve la tarea completada
    

-   **Flujo alternativo:**
	-   Si el identificador de la tarea es inválido o no existe, el servidor responde con un código 404 (Not Found) y un mensaje de error
    
	-   Si la tarea no pertenece al usuario o no está pendiente, el servidor responde con un código 403 (Forbidden) y un mensaje de error
    

  

### Caso de uso 7: Consultar todas las tareas 📇

-   **Actor:** Usuario
    
-   **Precondición:** El usuario está autenticado en el sistema
    
-   **Flujo principal:**
    

	-   El usuario envía una petición GET al servidor sin ningún parámetro
    
	-   El servidor busca todas las tareas que pertenecen al usuario en la base de datos
    
	-   El servidor responde con un código 200 (OK) y devuelve una lista de todas las tareas del usuario con sus datos
    

-   **Flujo alternativo:**
	-   Si el usuario no tiene ninguna tarea, el servidor responde con un código 200 (OK) y devuelve una lista vacía
    

### Caso de uso 8: Consultar tareas por estado 🔖

-   **Actor:** Usuario
    
-   **Precondición:** El usuario está autenticado en el sistema
    
-   **Flujo principal:**
	-   El usuario envía una petición GET al servidor con el parámetro “estado” y el valor del estado que quiere consultar (“pendiente”, “completada” o “archivada”)
    
	-   El servidor busca todas las tareas que pertenecen al usuario y tienen ese estado en la base de datos
    
	-   El servidor responde con un código 200 (OK) y devuelve una lista de las tareas del usuario con ese estado con sus datos
    

-   **Flujo alternativo:**
	-   Si el parámetro “estado” es inválido o no se envía, el servidor responde con un código 400 (Bad Request) y un mensaje de error
	-   Si el usuario no tiene ninguna tarea con ese estado, el servidor responde con un código 200 (OK) y devuelve una lista vacía
    

### [ Opcional ]Caso de uso 9: Autenticación de Usuario 🔐

-   **Actor:** Usuario
    
-   **Precondiciones:** El usuario tiene una cuenta válida y verificada en la aplicación
    
-   **Flujo principal:**
	-   El usuario envía una petición HTTP GET a la ruta /login y recibe una respuesta HTTP con el formulario de inicio de sesión en el cuerpo
    
	-   El usuario ingresa su correo electrónico y contraseña en el formulario y envía una petición HTTP POST a la ruta /login con los datos en el cuerpo.
	-   El usuario recibe una respuesta HTTP con el código 200 (OK) y un token de autenticación en el cuerpo.
	-   El usuario almacena el token de autenticación en su navegador o dispositivo para futuras peticiones.
    

-   **Flujo alternativo:**
	-   Si el usuario no tiene una cuenta, puede seleccionar la opción de registrarse en el formulario de inicio de sesión y enviar una petición HTTP GET a la ruta /register.
	 
	-   El usuario recibe una respuesta HTTP con el formulario de registro en el cuerpo.
	 
	-   El usuario ingresa sus datos y acepta los términos y condiciones en el formulario y envía una petición HTTP POST a la ruta /register con los datos en el cuerpo.
	 
	-   El usuario recibe una respuesta HTTP con el código 201 (Created) y un mensaje de confirmación en el cuerpo.
	
	-   El usuario recibe un correo electrónico con un código o un enlace para verificar su cuenta.
	 
	-   El usuario introduce el código o hace clic en el enlace y envía una petición HTTP GET a la ruta /verify con el código .como parámetro.
	 
	-   El usuario recibe una respuesta HTTP con el código 200 (OK) y un mensaje de verificación exitosa en el cuerpo
	
-   **Postcondiciones:** 
	- El usuario está autenticado y puede acceder a las funciones de la aplicación.


## 👉 A CONTINUACIÓN OS DEJAMOS UN MINITUTORIAL RESUMIDO DEL BACKEND DE ESTA APLICACIÓN

Esta aplicación backend proporciona una API REST y ofrece la información en formato JSON.

Este tipo de aplicaciones tiene las siguientes ventajas:

- Separación entre el backend y el frontend. 
- Visibilidad, fiabilidad y escalabilidad. 
- La API REST siempre es independiente del tipo de plataforma o lenguaje.

REST: Representational State Transfer

Importante tener Node.js instalado, podemos recurrir al sitio oficial de Node.js ([Descargar node.js](https://nodejs.org/es/download/)) o podemos comprobar si lo tenemos instalado en nuestro terminal con el siguiente comando:

```
node -v
```

## Inicio de un proyecto

Para iniciar el proyecto hacemos:

```
mkdir  wishlistbackend
cd     wishlistbackend

npm  init  -y
```

La última sentencia nos crea un archivo **`package.json`** con la metainformación del proyecto. La opción `y` o `--yes` es para que no nos pregunte y escriba una configuración por defecto en dicho archivo. Siempre podemos editarlo más adelante y modificar la version, añadir el autor, ...


## Edición de package.json

El archivo **`package.json`** es el archivo de gestión de proyecto y dependencias. En él podremos editar el nombre del autor, la versión, el tipo de licencia, etc.

Una parte muy importante es indicar el punto de entrada. En este proyecto será el archivo **`server.js`**, que crearemos más adelante.

Para definir dicho punto de entrada, lo hacemos con la línea:

```
  "main": "index.js",
```

El archivo `package.json` tendrá una apariencia semejante a la siguiente:

```
{
  "name": "wishlist",
  "version": "1.0.0",
  "description": "Backend of a Fullstack app",
  "author": "AIT",
  "license": "GPL",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "nodemon server.js"
  },
  "keywords": [
    "webapp",
    "backend",
    "fullstack"
  ]
}
```

También hemos modificado una de las líneas de `scripts`. En concreto:

```
    "dev": "nodemon index.js"
```

Esta línea indica que cuando ejecutemos `npm run dev` en el terminal, lo que se va a ejecutar en última instancia es el comando `nodemon server.js`.  

NOTA: Los scripts se ejecutan desde el terminal de texto con `npm run` *nombre_script*.

NOTA: `nodemon` es un paquete de Node.js que ejecuta node en modo monitor, es decir, está comprobando constantente cualquier cambio en nuestros archivos, y si detecta alguno, entonces vuelve a reiniciar el entorno de ejecución con los nuevos cambios. Esto es muy útil para el proceso de desarrollo de la aplicación. Versiones modernas de node.js no necesitan del paquete de nodemon y funcionan con el comando --watch. No obstante se encuentra en fase beta y no optimizado para producción, así que de momento desaconsejamos su uso.



## Servidor web básico

En el archivo **[`index.js`](index.js)** escribiremos el código para crear nuestro propio servidor web. En su versión mínima, solamente son necesarias 3 líneas.


```javascript
const express = require('express');

const app = express();

app.listen(3000);
```

Como nuestro backend se va a destinar a proporcionar una API REST y el intercambio de información se va a realizar en formato JSON, modificaremos el archivo anterior para que tenga la siguiente apariencia:

```javascript
const express = require('express');

const app = express();

// MIDDLEWARE
app.use(express.json());    

// SERVIDOR WEB
app.listen(3000, () => console.log("Servidor iniciado..."));
```

Hemos añadido el *midleware* de soporte de formato JSON y un callback en la última línea para que, cuando el servidor web esté iniciado, nos muestre un mensaje indicando tal circunstancia. El *midleware* es el software disponible para su ejecución entre la petición de un cliente y la respuesta del servidor.

Para probar nuestro servidor web:

```bash
npm  run  dev
```

No obstante, esto dará un error. El motivo es que necesitamos instalar los paquetes **`express`** y **`nodemon`**.

El primero se instalará como dependencia de aplicación y el segundo como dependencia de desarrollo. La diferencia entre uno y otro es que el primero es necesario para el funcionamiento de la aplicación, mientras que el segundo sólo es necesario para facilitar el proceso de desarrollo.

Deberemos ejecutar:

```bash
npm  install  express
npm  install  nodemon  -D
```

Si echamos un vistazo al archivo **`package.json`** veremos que dichos paquetes (también llamados módulos) han quedado registrados en dicho archivo:


```
{
  "name": "wishlist",
  "version": "1.0.0",
  "description": "Backend of a Fullstack app",
  "author": "AIT",
  "license": "GPL",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",    
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "nodemon index.js"
  },
  "keywords": [
    "webapp",
    "backend",
    "fullstack"
  ],
  "devDependencies": {
    "nodemon": "^2.0.20"
  },
  "dependencies": {
    "express": "^4.18.1"
  }
}
```

También veremos que se ha creado una carpeta `node_modules` con dichos módulos en su interior, además de muchos otros que son dependencias de los anteriores.

Por último, también se ha creado un archivo `package-lock.json` que contiene la versión exacta de cada dependencia. Este archivo es muy importante, puesto que indicará al servidor de producción que utilice exactamente la mismas versiones de las dependencias que usamos en nuestro entorno de desarrollo, evitando así problemas en el despligue. 

Ahora, ya podremos ejecutar `npm run dev`, y si no hay errores, podremos abrir el navegador y acceder a la url `http://localhost:3000`.


## Servidor web completo

### Sirviendo código estático

Podemos servir código estático (HTML, CSS, imágenes, ...) añadiendo el siguiente *middleware*. 

```javascript
app.use(express.static('public'));
```
Esto pondrá a disposición de todo el mundo el contenido alojado en la carpeta `public`. 

No obstante, es mejor poner una ruta absoluta. Ello se hace mediante el siguiente código:

```javascript
const path = require('path');

app.use(express.static(path.join(__dirname , 'public')));
```

Es normal encontrar documentación acerca de la API. Esto lo encontraréis en varias ocasiones y algunos servicios que podéis usar son por ejemplo [Swagger](https://petstore.swagger.io/).


### Haciendo pública nuestra API

**IMPORTANTE:** Debemos instalar el módulo `cors`
```
npm  install  cors
```

Este módulo proporciona funcionalidad de [Cross-Origin Resource Sharing](https://es.wikipedia.org/wiki/Intercambio_de_recursos_de_origen_cruzado)

El código a añadir es:

```javascript
const cors = require('cors');

app.use(cors()); 
```

### Obteniendo información de configuración desde las variables de entorno

**IMPORTANTE:** Debemos instalar el módulo `dotenv`:
```
npm  install  dotenv
```

Utilizaremos **variables de entorno** para guardar la información de conexión a la base de datos.

Para ello usaremos un archivo `.env` y el módulo `dotenv` para leer dicho archivo.

Ejemplo de contenido del archivo `.env`:

```
PORT=3000
DB_URI=mongodb://localhost:27017/basedatos
```

Código a añadir al servidor web:

```javascript
require('dotenv').config();

const PORT   = process.env.PORT || 3000;
const DB_URI = process.env.DB_URI;
```

Si la variable `PORT` no está definida en el archivo `.env`, entonces se utiliza el valor 3000. En nuestro caso, es mejor no definir dicha variable.

La variable `DB_URI` debe estar definida en el archivo `.env` sino la conexión a la base de datos fallará. Dicha variable contiene la URL de la base de datos. Consulta más abajo, en el apartado [Base de datos](https://github.com/jamj2000/tiendabackend#base-de-datos).


### Conectando a una base de datos

**IMPORTANTE:** Debemos instalar el módulo `mongoose`
```
npm  install  mongoose
```

Para conectar a una base de datos MongoDB usaremos el módulo `mongoose`.

```javascript
const mongoose = require('mongoose');

// CONEXIÓN A BASE DE DATOS
mongoose.connect(DB_URI, { useNewUrlParser: true, useUnifiedTopology: true })
    .then(db => console.log("Conexión a BD correcta"))
    .catch(error => console.log("Error al conectarse a la BD" + error));
```

### Indicando el archivo que contiene las rutas

Lo hacemos con el siguiente código:

```javascript
const apiRoutes = require('./routes');

app.use('/api', apiRoutes);
```

Todo el código fuente del servidor está disponible en el archivo **[`server.js`](server.js)**.


## Rutas

Sirviendo a modo de guía, este backend podría proporpocionar una **API Rest** con los siguientes **end-points**.

```
(GET)    /api/users/:id     (Lista    users :id)
(PUT)    /api/users/:id     (Modifica users :id)
(DELETE) /api/users/:id     (Elimina  users :id)

(GET)    /api/wishlist         (Lista    todos los deseos)
(POST)   /api/wish             (Crea     deseo)
(GET)    /api/wishlist/:id     (Lista    artículo :id)
(PUT)    /api/wishlist/:id     (Modifica artículo :id)
(DELETE) /api/wishlist/:id     (Elimina  artículo :id)

// ...
```

El código fuente usado es:

```javascript
const cors = require('cors')
const express = require("express");
const controller = require("./controllers.js");

const router = express.Router();

// --------------- API REST CRUD

router.get    ("/users",      cors(), controller.readUsers);   // Read All
router.get    ("/users/:id",  cors(), controller.readUser);    // Read
router.delete ("/users/:id",  cors(), controller.deleteUser);  // Delete
router.put    ("/users/:id",  cors(), controller.updateUser);  // Update
router.post   ("/users",      cors(), controller.createUser);  // Create

// ...

module.exports = router;
```
En este caso hemos habilitado mediante `cors` el acceso a cada **end-point** de nuestra **API** desde cualquier URL. 

Todo el código fuente de las rutas está disponible en el archivo **[`routes.js`](routes.js)**.


## Controladores

Los controladores son los encargados de realizar las operaciones CRUD. Para ello hacen uso de los modelos definidos.

```javascript
const { Users, Wishes } = require("./models.js");

exports.readUsers = (req, res) => 
    Users.find({}, (err, data) => {
        if (err) res.json({ error: err });
        else     res.json(data);
    });

// ...
```

Todo el código fuente de los controladores está disponible en el archivo **[`controllers.js`](controllers.js)**.

## Modelos

Tenemos 2 modelos:

- Users 
- Wishes

Cada uno tiene un esquema asociado que, en este caso, es bastante simple. Cada modelo tiene únicamente 2 propiedades:

```javascript
const Users = mongoose.model('Users',
  new mongoose.Schema({ nombre: String, apellidos: String })
);

const Wishes = mongoose.model('Wishes',
  new mongoose.Schema({ nombre: String, precio: Number })
);
```

Toda esta información se volcará en el **[`models.js`](models.js)**.

Mongoose proporciona muchos más tipos y opciones para definición de esquemas. Puedes consultar en [Tipos de esquemas en Mongoose](https://mongoosejs.com/docs/schematypes.html)


## ¿Vistas?

NO HAY. 

Esto NO es una aplicación MVC (Modelo-Vista-Controlador).  

Este **backend** proporciona una **API Rest**, por tanto no genera vistas, sino que ofrece la información en formato **JSON** para que la aplicación frontend la renderice a su gusto.


## Base de datos

Como servidor de base de datos vamos a usar MongoDB en su versión Cloud. Para ello podemos registrarnos en [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) en su modalidad *Free*, que nos proporciona 512 MB de almacenamiento, más que suficiente para lo que queremos hacer.

Una vez registrados, crearemos un cluster (por defecto son de 3 máquinas), luego una base de datos y un usuario y contraseña para acceder a dicha base de datos. A dicho usuario le daremos permisos de lectura y escritura.

Una vez realizados estos pasos, conseguiremos la URL de acceso para aplicación de Node.js. Tiene un formato similar al siguiente:

`mongodb+srv://`***`usuario`***`:`***`contraseña`***`@`***`servidor`***`/`***`basedatos`***`?retryWrites=true&w=majority`

### Guardamos datos de conexión en variable de entorno

En el archivo **`.env`** (abreviatura de *environment*) pondremos las **variables de entorno**, tales con la URL de conexión a la base de datos. En él escribiremos la línea:

`DB_URI=mongodb+srv://`***`usuario`***`:`***`contraseña`***`@`***`servidor`***`/`***`basedatos`***`?retryWrites=true&w=majority`

Deberemos sustituir `usuario`, `contraseña`, `servidor` y `basedatos` por los que nos sean propios.

> Nota: 
>
> Una forma más sencilla de trabajar, al menos durante el proceso de desarrollo, es utilizar una base de datos local. 
> Aunque cuando vayamos a desplegar la aplicación en Internet deberemos recurrir a una base de datos on-line.
>
> Si utilizamos un servidor MongoDB local, la URL tendrá la forma:
>
> `mongodb://localhost:27017/`*basedatos*


## Control de versiones (Git)

Para el control de versiones se usará **git** y **[GitHub](https://github.com)**.

Seguiremos los siguientes pasos:

1. Inicializa el repositorio local:

```
git  init
```

2. Edita el archivo `.gitignore` con el siguiente contenido:

```
node_modules/
.env
```

De esta forma indicamos que la carpeta `node_modules` y el archivo `.env` no serán incluidos en el repositorio, sólo permanecerán en el directorio de trabajo. 

`node_modules` contiene las dependencias y no es aconsejable añadirlo al repositorio. Su contenido será *re-**creado*** a partir  del archivo `package-lock.json` una vez se despliegue en producción.

`.env` es el archivo que guarda las **variables de entorno**. Dicho contenido nunca debe añadirse al repositorio, puesto que puede contener información sensible, tal como URLs, usuarios, contraseñas, ... 


3. Añade todo el contenido al repositorio:

```
git  add  .
git  commit  -m "Añadido contenido"
```

4. Crea un repositorio totalmente vacío en GitHub.

Una vez hecho, copia la URL de dicho repositorio.

5. Añade el vínculo al repositorio remoto de GitHub creado previamente.

`git  remote  add  origin  https://github.com/` *usuario* `/` *repositorio.git*

Sustituye *usuario* y *repositorio.git* por tu usuario y tu repositorio. 


6. Sube el contenido al repositorio remoto de Github.

```
git  push  -u  origin main
```

