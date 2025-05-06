
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

    let sendBtn1 = createButton('Send sillyness');
    sendBtn1.position(220, 300);
    sendBtn1.mousePressed(sendBtn1Click);

    let sendBtn2 = createButton('Send Bill');
    sendBtn2.position(220, 250);
    sendBtn2.mousePressed(sendBtn2Click);

    let sendBtn3 = createButton('Send Wackwack');
    sendBtn3.position(220, 200);
    sendBtn3.mousePressed(sendBtn3Click);

    let sendBtn4 = createButton('Send BOO');
    sendBtn4.position(220, 150);
    sendBtn4.mousePressed(sendBtn4Click);
}

function draw() {
  background(220);

}

function connectBtnClick() {
    if (!port.opened()) {
        port.open('MicroPython', 115200);
    } else {
        port.close();
    }
}

function sendBtn1Click() {
    port.write('h');
}
function sendBtn2Click() {
    port.write('j');
}

function sendBtn3Click() {
    port.write('k');
}
function sendBtn4Click() {
    port.write('l');
}
```

Código en mibro bit:
```
from microbit import *

uart.init(baudrate=115200)
display.show(Image.BUTTERFLY)

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
    if uart.any():
        data = uart.read(1)
        if data:
            if data[0] == ord('h'):
                display.show(Image.SILLY)
                sleep(500)
                display.show(Image.HAPPY)
            if data[0] == ord('j'):
                display.show(Image.DIAMOND_SMALL)
                sleep(500)
                display.show(Image.TRIANGLE)
            if data[0] == ord('k'):
                display.show(Image.PACMAN)
                sleep(500)
                display.show(Image.HAPPY)
            if data[0] == ord('l'):
                display.show(Image.CONFUSED)
                sleep(500)
                display.show(Image.GHOST)
```
