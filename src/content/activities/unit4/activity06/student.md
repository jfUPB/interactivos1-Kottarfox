1. Un protocolo de comunicación es un conjunto de reglas que permiten que dos dispositivos intercambien datos de forma estructurada y comprensible. En el caso de la comunicación serial, es importante porque asegura que tanto el micro:bit como p5.js entiendan cuándo comienza y termina un mensaje, y qué datos contiene. En nuestro proyecto, usamos un protocolo basado en texto plano (ASCII), donde los valores se separan por comas y terminan con un salto de línea.

2. Los datos se separan con comas para facilitar su separación en el lado receptor. En p5.js, se utiliza la función .split(",") para convertir la cadena recibida en un arreglo de datos individuales. Esto permite extraer fácilmente valores como x, y, o el estado de los botones.

3. Es necesario terminar los datos con un carácter especial (como \n) para marcar el fin del mensaje. De esta manera, p5.js sabe cuándo ha recibido un mensaje completo y puede procesarlo sin riesgo de cortar la información o mezclarla con otros mensajes.

4. Fue necesario usar una máquina de estados para controlar los cambios causados por los botones del micro:bit. Esto nos permite detectar cuando un botón se presiona por primera vez, evitando repeticiones no deseadas mientras el botón permanece presionado. Por ejemplo, si buttonA cambia el modo de dibujo, queremos que cambie solo una vez por pulsación.

5. En el micro:bit, los datos se formatean convirtiéndolos en una cadena con .format() en Python. Se separan por comas y terminan con \n. Esto los convierte en texto ASCII que puede ser interpretado fácilmente por p5.js.

6. Los datos enviados por el micro:bit están codificados en ASCII, lo que significa que cada carácter (como los dígitos de un número o las comas) se envía como su equivalente en código ASCII. Por ejemplo, el número 123 se envía como los caracteres '1', '2', '3'.

7. Es necesario en p5.js verificar si hay bytes disponibles en el puerto serial antes de leerlos, usando condiciones como if (port.availableBytes() > 0) o equivalentes. Esto evita errores al intentar leer cuando no hay datos, lo cual podría causar fallos o valores undefined.

8. Para eliminar el retorno de carro o el salto de línea de un string en p5.js se puede usar .trim(). Esta función remueve espacios en blanco y caracteres como \n o \r al principio y al final de una cadena.

9. Si una cadena tiene información separada por espacios y queremos dividirla en varias cadenas, usamos la función .split(" "). Esto permite transformar una sola cadena en un arreglo de partes individuales.

10. Es necesario convertir las cadenas a números enteros en p5.js usando funciones como parseInt() porque los datos llegan como texto. Para usarlos en cálculos o condiciones (como posiciones, comparaciones o incrementos) deben convertirse a tipo numérico.

11. Si el micro:bit tiene los siguientes datos: xValue: 123, yValue: 756, aState: False, bState: True, enviaría por el puerto serial la cadena "123,756,0,1\n". Cada carácter de esa cadena es un byte en ASCII, por lo tanto se enviarían los bytes equivalentes a esos caracteres.

12. Aprendí que el micro:bit puede enviar información de sus sensores internos como el acelerómetro y los botones por medio de comunicación serial, usando uart.write() y que este tipo de comunicación puede integrarse con aplicaciones interactivas como p5.js.

13. Aprendí a conectar p5.js con dispositivos externos como el micro:bit usando Web Serial. También aprendí a leer datos desde el puerto serial, dividirlos, convertirlos en números y usarlos para controlar visualmente una aplicación en tiempo real.
