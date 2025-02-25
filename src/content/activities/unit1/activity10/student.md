#### Solución

- ¿Cómo funciona?
El micro:bit se conecta al ordenador por USB y usa el puerto serial para enviar datos a p5.js. En el código de p5.js, se abre el puerto serial para recibir estos datos. Cuando el micro:bit detecta que el botón A se presiona cambia el color del cuadrado en la pantalla al recibir este mensaje. La comunicación se mantiene activa mientras ambos dispositivos están conectados.

- Código de p5.js
```
let port;
let connectBtn;

let buttonState = 0;

function setup() {
  createCanvas(400, 400);
  background(220);
  port = createSerial();
    connectBtn = createButton('Connect to micro:bit');
    connectBtn.position(80, 300);
    connectBtn.mousePressed(connectBtnClick);

}

function draw() {
   if(port.availableBytes() > 0){
      let dataRx = port.read(1);
        if(dataRx == 'A'){
            fill('red');
        }
        else if(dataRx == 'B'){
            fill('yellow');
        }
        else{
            fill('green');
        }
   }
  rect(width / 2, height / 2, 100, 100);

}


function connectBtnClick() {
    if (!port.opened()) {
        port.open('MicroPython', 115200);
    } else {
        port.close();
    }
}

function sendBtnClick() {
    port.write('h');
}
```

- Código para microbit
```
from microbit import *

while True:
    if button_a.is_pressed():
        uart.write('A')
        sleep(500)
    if button_b.is_pressed():
        uart.write('B')
        sleep(500)
    if accelerometer.was_gesture('shake'):
        uart.write('C')
        sleep(500)
```
