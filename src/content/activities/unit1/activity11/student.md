#### Respuesta

- Código de p5.js
```
let port; 
let connectBtn;
let position = 200;
let circle1;
let increment = 0;

function setup() {
    createCanvas(400, 400);
    background(220);
    circle1 = new Circle(position);

    // Crear puerto serial
    port = createSerial();

    // Botón de conexión
    connectBtn = createButton('Connect to micro:bit');
    connectBtn.position(80, 300);
    connectBtn.mousePressed(connectBtnClick);

    // Botón para enviar algo al micro:bit
    let sendBtn = createButton('Send Love');
    sendBtn.position(220, 300);
    sendBtn.mousePressed(sendBtnClick);
}

function draw() {
    background(220);
    circle1.update(increment);
    circle1.show();

    if (port.availableBytes() > 0) {
        let dataRx = port.read(1); // leer un byte
        if (dataRx === 'A') {
            circle1.update(-10); // mover a la izquierda
        } else if (dataRx === 'B') {
            circle1.update(10);  // mover a la derecha
        } else {
            circle1.update(0);
        }

        // Mostrar lo que se recibe
        textSize(16);
        fill(0);
        textAlign(CENTER);
        text("Recibido: " + dataRx, width / 2, height - 50);
    }

    if (!port.opened()) {
        connectBtn.html('Connect to micro:bit');
    } else {
        connectBtn.html('Disconnect');
    }
}

function connectBtnClick() {
    if (!port.opened()) {
        port.open('MicroPython', 115200); // abrir puerto a 115200 baudios
    } else {
        port.close();
    }
}

function sendBtnClick() {
    port.write('h'); // ejemplo de envío (opcional)
}

// Clase para manejar el círculo
class Circle {
    constructor(positionX) {
        this.positionX = positionX;
    }

    update(increment) {
        this.positionX += increment;
        this.positionX = constrain(this.positionX, 24, width - 24);
    }

    show() {
        stroke(0);
        fill(127);
        circle(this.positionX, 200, 48);
    }
}
```

- Código en microbit:
```
from microbit import *

while True:
    if button_a.is_pressed():
        uart.write('A')
        sleep(200)
    if button_b.is_pressed():
        uart.write('B')
        sleep(200)
```
