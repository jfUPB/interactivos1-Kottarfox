¿Por qué en la unidad 4 se necesitaba el salto de línea?
En ese caso, como no sabíamos cuántos bytes exactos iba a tener cada paquete de datos (porque se enviaban como texto), necesitábamos una forma de saber cuándo terminaba uno y empezaba otro. Por eso se usaba el salto de línea como separador. Ahora, al enviar los datos en formato binario, sabemos exactamente cuántos bytes vienen en cada paquete, así que ya no hace falta ese "marcador" al final.

¿Qué tienen de distintos ambos códigos?
La diferencia principal está en cómo se manejan los datos que llegan. En el código que usa lectura binaria, el sistema espera a que haya al menos 6 bytes disponibles en el puerto serial, y luego toma justo esos 6 y los mete en un arreglo. Luego, de forma muy precisa, convierte los dos primeros bytes en un número (con una función llamada getInt16 que no conocía), hace lo mismo con los siguientes dos, y con los últimos dos bytes obtiene el estado de los botones A y B usando getUint8. Es una forma mucho más estructurada de interpretar los datos.

¿Por qué puede ocurrir el error de sincronización?
Eso pasa porque al usar binario, los paquetes son mucho más pequeños y se pueden enviar más rápido que antes (cuando usábamos texto). El problema es que seguimos usando el mismo sleep de 100 milisegundos que se pensó para el envío en ASCII, así que el micro:bit puede empezar a mandar datos más rápido de lo que p5.js puede procesarlos. Eso genera desajustes y hace que a veces se lean datos incorrectos.

¿Qué se ve después de los cambios?
Con los cambios que se hicieron, ahora cada paquete que se envía tiene un formato mucho más ordenado: comienza con un byte especial (0xAA) que actúa como encabezado, y termina con un byte que es como una firma o verificación (checksum). En p5.js lo que se hace es buscar ese encabezado para asegurarse de que estamos empezando a leer un paquete real. Luego, se toman los siguientes 6 bytes (que contienen los datos reales), se valida que todo esté bien con el checksum (que está en la posición 7), y si todo cuadra, se usan esos datos en la app. Así se evita procesar datos corruptos o incompletos.
