# FrontBlazor
Front-end en Blazor para consumir API genérica en C#
Documentación del Componente "Actualizar Registro "
Este componente Blazor permite actualizar registros en diferentes tablas. A continuación se documentan los principales métodos y su funcionamiento:

Métodos
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