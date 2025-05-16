¿Por qué es importante usar librerías?
Pues si escribo todo lo que trae una librería directamente en mi código, este se hace súper largo y pesado, como que no da ni para entenderlo. Mejor es importar la librería y usar solo lo que necesito, además así puedo reutilizar ese código fácil en otros proyectos sin tener que copiar y pegar todo, que sería un lío enorme y nada eficiente.

¿Qué pasa si cambio el nombre de la carpeta?
Entonces me sale un mensaje que dice: Cannot GET /page1.html, o algo así. Esto es porque el servidor está buscando la página dentro de una carpeta que ya no existe o que le cambié el nombre, entonces no la encuentra y me da ese error.

¿Qué pasa si cambio el request de un navegador?
El navegador tiene que pedir algo que exista en el servidor, o sea, la URL tiene que coincidir con lo que el servidor espera. Si cambio el request, o sea, la ruta o la dirección, el servidor no sabe qué le están pidiendo y no puede responder, entonces da error o no funciona.

ID y Usuarios
No me salió el ID específico de usuario, pero la consola sí me decía cuando un usuario entraba o salía, igual en las dos páginas. Yo creo que el ID es el mismo porque estoy usando la misma IP para acceder a ambas, aunque no estoy segura del todo, solo es una suposición.

Escuchando Mensajes de Clientes
Las páginas mandan info todo el tiempo, como la posición x y y, y otras cosas en paquetes llamados Data. Si quito el broadcast, ya no se actualizan bien los datos y entonces por ejemplo page1 no cambia como debería, al menos eso me pasó a mí.

Poner en Marcha el Servidor
Si cierro el servidor y lo abro en otro puerto, por ejemplo 3001 en vez de 3000, ya no puedo entrar por el puerto viejo porque es como si moviera la puerta de la tienda: la puerta está en otro lugar y la vieja ya no sirve para entrar.

