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
    

### Caso de uso 9: Autenticación de Usuario 🔐

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
