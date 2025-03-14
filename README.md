# FrontBlazor
Front-end en Blazor para consumir API genérica en C#
*Documentación del Componente **"Crear Registro"***
***Permite crear registros de manera genérica en diversas tablas de una base de datos. Al cargar la página, se muestra un desplegable que contiene las tablas disponibles, y el usuario debe seleccionar la tabla en la que desea crear un registro. Una vez realizada la selección, se despliega un formulario dinámico generado a partir de un diccionario que mapea cada tabla con sus respectivas columnas; cada campo de entrada se enlaza a un diccionario que almacena los valores ingresados por el usuario. Cuando se envía el formulario, se transforma el diccionario de datos (de tipo string) en uno compatible (de tipo object) para que el servicio inyectado pueda interactuar con la API y crear el registro en el proyecto correspondiente. Finalmente, se muestra un mensaje de retroalimentación que indica si la operación fue exitosa o si se presentó algún error, permitiendo incluso reiniciar los campos del formulario para nuevos registros.***

*Documentación del Componente **"Leer Registros"***
***Permite al usuario visualizar dinámicamente el contenido de diversas tablas disponibles en la base de datos. Al ingresar a la página, se muestra un desplegable con la lista de tablas definidas; cuando el usuario selecciona una tabla, se dispara el evento OnTablaChanged, el cual actualiza la variable de la tabla seleccionada y reinicia los datos previamente cargados. Si se selecciona una tabla válida, se realiza una llamada asíncrona a la API mediante el servicio inyectado para obtener todos los registros de la tabla. Mientras se recuperan los datos, se muestra un mensaje de "Cargando datos..."; si no se encuentran registros, se notifica al usuario, y en caso de obtener datos, se genera una tabla HTML estilizada con Bootstrap, en la que los encabezados se crean dinámicamente a partir de las claves del primer registro, y cada fila de datos se despliega en celdas correspondientes.***

*Documentación del Componente **"Actualizar Registro"***
***Permite actualizar registros de forma genérica en diversas tablas de una base de datos. Su flujo comienza con la selección de una tabla mediante un desplegable, el cual carga dinámicamente los campos definidos para dicha tabla. Una vez elegida la tabla, el usuario ingresa el ID del registro a modificar y, al pulsar el botón correspondiente, se consulta la API a través de un servicio inyectado para cargar los datos actuales del registro. Si se encuentra el registro, se despliega un formulario que muestra cada campo (con la clave primaria deshabilitada para evitar su modificación) permitiendo al usuario realizar los cambios deseados. Finalmente, al enviar el formulario, se actualiza el registro mediante una llamada asíncrona a la API y se informa al usuario del resultado de la operación.***

*Métodos:*
1. OnTablaChanged(ChangeEventArgs e)
Descripción:
Se ejecuta cuando el usuario cambia la selección en el desplegable de tablas. Su función es actualizar el estado del componente según la nueva tabla seleccionada.

Parámetro:

e (ChangeEventArgs): Contiene el valor seleccionado en el <select>.
Funcionamiento:

Actualiza la tabla seleccionada:
Asigna el valor seleccionado a la variable tablaSeleccionada.
Reinicia el estado de la búsqueda:
Limpia el campo registroId para que el usuario ingrese un nuevo ID.
Establece registroCargado en false, ya que al cambiar la tabla, cualquier registro previamente cargado ya no es válido.
Limpia el diccionario registroActual que almacena los datos del registro.
Elimina cualquier mensaje previo (variable mensaje).

2. CargarRegistro()
Descripción:
Carga los datos del registro a actualizar según la tabla seleccionada y el ID ingresado por el usuario. Se comunica con la API a través del servicio inyectado (servicioEntidad).

Funcionamiento:

Validación Inicial:
Comprueba que se haya seleccionado una tabla.
Verifica que se haya ingresado un ID.
Si alguna de estas validaciones falla, asigna un mensaje de error en mensaje y termina la ejecución.
Consulta a la API:
Llama a servicioEntidad.ObtenerPorClaveAsync pasando:
El nombre del proyecto (ej. "NombreProyecto").
La tabla seleccionada.
La clave primaria ("id").
El ID ingresado.
Procesamiento de la Respuesta:
Si se encuentra el registro, convierte cada valor del resultado a string y lo almacena en el diccionario registroActual.
Marca la variable registroCargado como true y asigna un mensaje de éxito.
Si no se encuentra el registro, limpia registroActual, establece registroCargado en false y asigna un mensaje indicando que no se encontró el registro.

3. OnModificar()
Descripción:
Se invoca cuando el usuario envía el formulario para modificar el registro. Este método se encarga de enviar los datos modificados a la API para actualizar el registro correspondiente.

Funcionamiento:

Validación Previa:
Verifica que un registro se haya cargado previamente (registroCargado debe ser true).
Si no es así, asigna un mensaje de error en mensaje y termina la ejecución.
Preparación de Datos:
Convierte el diccionario registroActual (que almacena valores en string) a un nuevo diccionario datosActualizados de tipo Dictionary<string, object>.
Llamada al Servicio de Actualización:
Llama a servicioEntidad.ActualizarAsync pasando:
El nombre del proyecto.
La tabla seleccionada (asegurándose de que no es nula).
La clave primaria ("id").
El ID del registro.
Los datos actualizados.
Manejo de la Respuesta:
Si la actualización es exitosa, asigna un mensaje de éxito en mensaje.
En caso de error, asigna un mensaje indicando que ocurrió un error durante la actualización.

*Documentación del Componente **"Eliminar Registro"***
***Facilita la eliminación de registros de manera dinámica a través de una interfaz sencilla. Al cargar la página, se muestra un desplegable poblado con una lista de tablas disponibles, permitiendo al usuario seleccionar aquella en la que desea realizar la eliminación. Una vez que se elige una tabla, se habilita un campo de entrada para que el usuario ingrese el ID del registro que quiere eliminar. Al presionar el botón "Eliminar", se ejecuta un método que primero valida que tanto la tabla como el ID hayan sido seleccionados o ingresados correctamente; luego, utiliza un diccionario para obtener la clave primaria correspondiente a la tabla seleccionada y llama de manera asíncrona a un servicio que interactúa con la API para proceder a eliminar el registro. Finalmente, la página muestra un mensaje de retroalimentación que informa si la eliminación fue exitosa o si ocurrió algún error, ofreciendo así una experiencia de usuario clara y controlada.***

