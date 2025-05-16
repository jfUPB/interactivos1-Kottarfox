App OG:
https://editor.p5js.org/KrowKrapp/sketches/GTHCBJcQ9

Codigo:
```
'use strict';

let shapes = [];
let density = 2.5;
let shapeHeight = 64;
let shapeColor;
let newShape;
let mode = "lines";

let serial;
let serialBuffer = [];
let latestPacket = null;

function setup() {
  createCanvas(800, 800);
  colorMode(HSB, 360, 100, 100);
  noFill();
  shapeColor = color(random(360), 80, 80);

  // Inicializa la comunicación serial
  serial = new WebSerial(); // usa "WebSerial" si así está definido en el archivo
  serial.on("data", readSerialData);
  serial.open();
}

function draw() {
  background(0, 0, 95);

  if (latestPacket) {
    let hue = map(latestPacket.x, -1024, 1024, 0, 360);
    shapeColor = color(hue, 80, 80);
    if (latestPacket.a) mode = "dots";
    if (latestPacket.b) mode = "waves";
  }

  for (let shape of shapes) shape.draw();

  if (newShape) {
    newShape.x2 = mouseX;
    newShape.y2 = mouseY;
    newShape.h = shapeHeight;
    newShape.c = shapeColor;
    newShape.draw();
  }
}

function Shape(x1, y1, x2, y2, h, c) {
  this.x1 = x1;
  this.y1 = y1;
  this.x2 = x2;
  this.y2 = y2;
  this.h = h;
  this.c = c;

  this.draw = function () {
    let w = dist(this.x1, this.y1, this.x2, this.y2);
    let a = atan2(this.y2 - this.y1, this.x2 - this.x1);
    stroke(this.c);
    push();
    translate(this.x1, this.y1);
    rotate(a);
    translate(0, -this.h / 2);
    for (let i = 0; i < this.h; i += density) {
      if (mode === "lines") line(0, i, w, i);
      else if (mode === "dots") point(w * noise(i * 0.1, frameCount * 0.01), i);
      else if (mode === "waves") {
        let y = i + 5 * sin(i * 0.2 + frameCount * 0.05);
        line(0, y, w, y);
      }
    }
    pop();
  };
}

function mousePressed() {
  shapeColor = color(random(360), 100, 100);
  newShape = new Shape(pmouseX, pmouseY, mouseX, mouseY, shapeHeight, shapeColor);
}

function mouseReleased() {
  shapes.push(newShape);
  newShape = undefined;
}

function keyPressed() {
  if (key == 's' || key == 'S') saveCanvas('moire-output', 'png');
  if (key == '1') mode = "lines";
  if (key == '2') mode = "dots";
  if (key == '3') mode = "waves";
  if (keyCode == UP_ARROW) shapeHeight += density;
  if (keyCode == DOWN_ARROW) shapeHeight = max(density, shapeHeight - density);
}

// Función para leer datos binarios
function readSerialData() {
  while (serial.available()) {
    let byte = serial.read();
    if (byte !== -1) serialBuffer.push(byte);
  }

  while (serialBuffer.length >= 8) {
    if (serialBuffer[0] !== 0xAA) {
      serialBuffer.shift();
      continue;
    }

    let packet = serialBuffer.slice(0, 8);
    let dataBytes = packet.slice(1, 7);
    let checksum = packet[7];
    let sum = dataBytes.reduce((acc, val) => acc + val, 0) % 256;

    if (sum === checksum) {
      let x = getInt16(dataBytes[0], dataBytes[1]);
      let y = getInt16(dataBytes[2], dataBytes[3]);
      let a = dataBytes[4];
      let b = dataBytes[5];
      latestPacket = { x, y, a, b };
    }
    serialBuffer.splice(0, 8);
  }
}

// Convierte dos bytes a entero con signo
function getInt16(high, low) {
  let value = (high << 8) | low;
  return value > 32767 ? value - 65536 : value;
}
```

Arreglado (necesita el p5.webserial.js)
https://editor.p5js.org/KrowKrapp/sketches/Bdg_Ebr3h
