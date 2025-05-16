#### Código:
```
let port;
let connectBtn;

function setup() {
    createCanvas(400, 400);
    background(220);
    
    // Crear objeto de puerto serial
    port = createSerial();
    
    // Botón para conectar al micro:bit
    connectBtn = createButton('Conectar a micro:bit');
    connectBtn.position(100, 320);
    connectBtn.mousePressed(connectBtnClick);

    // Botón para enviar 'A' (botón A)
    let sendBtn1 = createButton('Botón A');
    sendBtn1.position(140, 40);
    sendBtn1.mousePressed(() => port.write('A'));

    // Botón para enviar 'B' (botón B)
    let sendBtn2 = createButton('Botón B');
    sendBtn2.position(140, 100);
    sendBtn2.mousePressed(() => port.write('B'));

    // Botón para enviar 'S' (shake)
    let sendBtn3 = createButton('Agitar (Shake)');
    sendBtn3.position(140, 160);
    sendBtn3.mousePressed(() => port.write('S'));

    // Botón para enviar 'T' (touch)
    let sendBtn4 = createButton('Touch Logo');
    sendBtn4.position(140, 220);
    sendBtn4.mousePressed(() => port.write('T'));
}

function connectBtnClick() {
    if (!port.opened()) {
        port.open('MicroPython', 115200); // Asegúrate de que coincide con el nombre del dispositivo
    } else {
        port.close();
    }
}
```
