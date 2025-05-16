1. ¿Cuál es la función principal del programa?
La función principal es recibir datos enviados desde un micro:bit por medio de comunicación serial usando p5.webserial, interpretar esos datos para controlar una máquina de estados, y mostrar diferentes pantallas con imágenes y sonido en p5.js según el estado actual, creando una experiencia interactiva controlada por los botones A y B del micro:bit.

2. ¿Cómo se organiza el programa para controlar los diferentes estados de la aplicación?
El programa usa una variable estado que toma valores de 0 a 4 para controlar qué pantalla o estado mostrar. Según el valor de estado, el draw() llama a funciones específicas (pantalla1(), pantalla2(), etc.) que muestran las imágenes y contenidos correspondientes. Los cambios en estado se hacen a partir de la lectura de datos 'A' o 'B' enviados por el micro:bit.

3. ¿Qué estructuras de control utiliza el programa para manejar la interacción?
Condicionales if para evaluar el valor recibido (data == 'A' o data == 'B').
Estructura switch o condicionales if-else para llamar a las funciones de pantalla según el estado.
Lógica para evitar que el estado pase fuera del rango permitido, por ejemplo, cuando estado > 4 se reinicia a 1.

4. ¿Cómo se recibe y procesa la información desde el micro:bit?
Se establece una conexión serial con el micro:bit usando navigator.serial y p5.webserial. Los datos llegan por el puerto serial y se leen con serial.readLine(). Al recibir una línea, se limpia con .trim() y se asigna a la variable data. Luego, en el ciclo principal, se usa ese dato para decidir qué acción tomar.

5. ¿Qué tipos de multimedia utiliza el programa y cómo se integran?
Imágenes (loadImage) que se cargan en preload() y se muestran en cada estado para representar diferentes pantallas.
Sonidos (loadSound), que se reproducen en algunos estados para mejorar la experiencia interactiva.
Texto para mostrar mensajes simples en algunas pantallas.

6. ¿Qué sucede cuando el usuario presiona el botón A o B en el micro:bit?
Al presionar el botón A, el programa incrementa el valor de estado para avanzar a la siguiente pantalla. Si el estado excede 4, vuelve a 1.
Al presionar el botón B, el programa reinicia la experiencia regresando al estado 0, que es la pantalla de inicio.

7. ¿Cuál es la ventaja de usar una máquina de estados en esta aplicación?
Permite organizar la lógica de la aplicación de forma clara y modular, facilitando la gestión de diferentes pantallas y comportamientos según el estado actual. También simplifica la ampliación y mantenimiento del programa, ya que cada estado puede definirse en una función separada.
