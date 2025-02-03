#### Solución

- ¿Cómo funciona?
El micro:bit se conecta al ordenador por USB y usa el puerto serial para enviar datos a p5.js. En el código de p5.js, se abre el puerto serial para recibir estos datos. Cuando el micro:bit detecta que el botón A se presiona, envía el mensaje "BUTTON_PRESSED", y p5.js cambia el color del cuadrado en la pantalla al recibir este mensaje. La comunicación se mantiene activa mientras ambos dispositivos están conectados.

- Código de p5.js
```
let buttonState = 0; 
let squareColor = [255, 0, 0]; 

function setup() {
  createCanvas(400, 400);
  background(220);
  
  // Iniciar la conexión con el micro:bit
  serial = new p5.SerialPort();
  serial.list();  // Listar los puertos disponibles
  serial.open('COM3');  // Cambiar 'COM3' por el puerto correcto de tu micro:bit cuando lo logré hacer funcionar
  serial.on('data', serialEvent); 
}

function draw() {
  fill(squareColor);
  rect(150, 150, 100, 100);
}

// Esta función se ejecuta cada vez que se recibe datos del micro:bit
function serialEvent() {
  let inData = serial.readLine();  // Leer la línea de datos enviada desde el micro:bit

 // Si los datos indican que el botón fue presionado
  if (inData === "BUTTON_PRESSED") { 
    // Cambiar el color del cuadrado a un color aleatorio
    squareColor = [random(255), random(255), random(255)];
  }
}
```

- Código para microbit
```
let serial;
let buttonAState = 0; // Estado inicial del botón A

function setup() {
  serial = new p5.SerialPort();
  serial.open('/dev/tty.usbmodem1421');  // Cambiar por el puerto correcto del micro:bit

  serial.on('open', onSerialOpen);
  serial.on('data', onSerialData);
}

function loop() {
  buttonAState = input.buttonA.isPressed();  
  if (buttonAState) {
    serial.write("BUTTON_PRESSED");  
  }
  delay(100);
}
```
