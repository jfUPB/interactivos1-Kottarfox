- ¿Cómo se conectan el sketch y el microbit?
Pues, basicamente se comunican por el puerto serial. En el código de p5.js se crean unas variables para facilitar la coneccion: una pa’ guardar el puerto, otra pa’ el botón de conectar, y una más que dice si el microbit ya esta conectado o no
```
let port;
let connectBtn;
let microBitConnected = false;

```
Usando la funcion createSerial() se inicia el puerto y también se crea un botón pa’ que el usuario lo pueda abrir. Cuando uno le da click, si el puerto no está abierto, se abre con port.opened(), pero si ya estaba abierto y uno le vuelve a dar click, se cierra con port.close(). Todo eso pasa dentro de la función connectBtnClick().
```
function setup() {
  createCanvas(windowWidth, windowHeight);
  background(255);

  port = createSerial();
  connectBtn = createButton("Connect to micro:bit");
  connectBtn.position(0, 0);
  connectBtn.mousePressed(connectBtnClick);
}

function connectBtnClick() {
  if (!port.opened()) {
    port.open("MicroPython", 115200);
  } else {
    port.close();
  }
}
```
- ¿Qué tipo de datos manda el microbit?
El microbit manda datos en formato ASCII, como un string. Los datos van entre corchetes y separados por comas, y al final se manda un salto de línea. Así es más fácil separar cada dato cuando llegan al sketch, y saber cuándo empieza o termina un paquete.
```
data = "{},{},{},{}\n".format(xValue, yValue, aState,bState)

```

- ¿Qué parte del sketch convierte los datos a coordenadas?
Primero, se reciben los datos del microbit por el puerto serial, y se ajustan pa’ que el punto (0,0) quede justo en el centro de la pantalla. Eso se hace sumando windowWidth / 2 y windowHeight / 2 a los valores que vienen del microbit:
```
if (values.length == 4) {
  microBitX = int(values[0]) + windowWidth / 2;
  microBitY = int(values[1]) + windowHeight / 2;
}
```
Ya con eso, las variables microBitX y microBitY se pueden usar pa’ dibujar en la función draw(), y así lo que haga el microbit controla lo que pasa en la app.

```
if (microBitAState === true) {
  let x = microBitX;
  let y = microBitY;
```

- ¿Cómo se detectan eventos como que se oprima la A o se suelte la B?
Primero, cuando llegan los datos del microbit, se convierten los strings en valores booleanos (o sea, true o false), y se guardan en dos variables: microBitAState y microBitBState. Después se llaman en la función updateButtonStates.

```
microBitAState = values[2].toLowerCase() === "true";
microBitBState = values[3].toLowerCase() === "true";
updateButtonStates(microBitAState, microBitBState);

```
La función updateButtonStates revisa si el botón A se acaba de presionar (o sea, si ahora está en true pero antes estaba en false). Si es así, se hace una acción. Con el botón B es al revés: si antes estaba en true y ahora está en false, significa que se soltó, y pasa otra cosa. Al final, hay que actualizar las variables que guardan el estado anterior (prevmicroBitAState y prevmicroBitBState), pa’ que la comparación funcione en el siguiente frame.
```
function updateButtonStates(newAState, newBState) {
  if (newAState === true && prevmicroBitAState === false) {
    lineModuleSize = random(50, 160);
    clickPosX = microBitX;
    clickPosY = microBitY;
    print("A pressed");
  }
  if (newBState === false && prevmicroBitBState === true) {
    c = color(random(255), random(255), random(255), random(80, 100));
    print("B released");
  }

  prevmicroBitAState = newAState;
  prevmicroBitBState = newBState;
}

```






















