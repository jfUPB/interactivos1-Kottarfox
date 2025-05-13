Recordando la actividad del botón "Send Love" de la unidad anterior, el micro:bit puede recibir datos desde una computadora u otro dispositivo mediante comunicación serial.

- Código python
```
from microbit import *
import uart

uart.init(baudrate=115200)

while True:
    if uart.any():
        char = uart.read(1)
        if char:
            display.show(char.decode('utf-8'))
```

- Código p5.js
```
  let port;
let connectBtn;

function setup() {
    createCanvas(400, 400);
    background(220);

    port = createSerial();

    connectBtn = createButton('Connect to micro:bit');
    connectBtn.position(80, 300);
    connectBtn.mousePressed(connectBtnClick);

    // Botón para enviar el carácter
    let sendBtn = createButton('Send Love');
    sendBtn.position(220, 300);
    sendBtn.mousePressed(sendBtnClick);
    
    fill('white');
    ellipse(width / 2, height / 2, 100, 100);
}

function draw() {
    // Escuchar datos entrantes (opcional en este caso)
    if (port.availableBytes() > 0) {
        let dataRx = port.read(1);
        console.log('Recibido:', dataRx); // Puedes visualizar datos de respuesta aquí si lo deseas
    }


    if (!port.opened()) {
        connectBtn.html('Connect to micro:bit');
    } else {
        connectBtn.html('Disconnect');
    }
}

function connectBtnClick() {
    if (!port.opened()) {
        port.open('MicroPython', 115200);
    } else {
        port.close();
    }
}

function sendBtnClick() {

    port.write('A'); // Puedes cambiar esto a cualquier otro carácter, como 'B' o '♥'
}
```

¿Qué ocurre?
- Al hacer clic en "Connect to micro:bit", se abre el puerto serial.
- Al hacer clic en "Send Love", se envía el carácter 'A' al micro:bit.
- El micro:bit recibe ese carácter y lo muestra en su pantalla LED.
