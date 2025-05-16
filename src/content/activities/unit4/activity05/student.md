Código OG: https://editor.p5js.org/KrowKrapp/sketches/GTHCBJcQ9

Nuevo códgio: 
```
'use strict';

let shapes = [];
let density = 2.5;
let shapeHeight = 64;
let shapeColor;
let newShape;
let mode = "lines"; // Puede ser "lines", "dots", "waves"

// Serial
let port, reader;
let yAccel = 0;
let aState = false;
let bState = false;
let lastAState = false;
let lastBState = false;

function setup() {
  createCanvas(800, 800);
  colorMode(HSB, 360, 100, 100);
  noFill();
  shapeColor = color(random(360), 80, 80);

  // Iniciar comunicación serial
  if ("serial" in navigator) {
    navigator.serial.requestPort().then(selectedPort => {
      port = selectedPort;
      return port.open({ baudRate: 115200 });
    }).then(() => {
      readSerial();
    }).catch(err => {
      console.error("Error abriendo el puerto serial:", err);
    });
  } else {
    alert("Tu navegador no soporta Web Serial API.");
  }
}

async function readSerial() {
  while (port.readable) {
    reader = port.readable.getReader();
    let buffer = '';
    try {
      while (true) {
        const { value, done } = await reader.read();
        if (done) break;
        if (value) {
          buffer += new TextDecoder().decode(value);
          let lines = buffer.split("\n");
          buffer = lines.pop();
          for (let line of lines) {
            processData(line.trim());
          }
        }
      }
    } catch (error) {
      console.error("Error leyendo serial:", error);
    } finally {
      reader.releaseLock();
    }
  }
}

function processData(data) {
  let parts = data.split(",");
  if (parts.length === 4) {
    let x = parseInt(parts[0]);
    let y = parseInt(parts[1]);
    aState = parts[2] === "1";
    bState = parts[3] === "1";

    // Control con acelerómetro (eje Y)
    if (y > 300) shapeHeight += density;
    if (y < -300) shapeHeight = max(density, shapeHeight - density);

    // Botón A: cambiar modo (solo si el estado cambia)
    if (aState && !lastAState) {
      if (mode === "lines") mode = "dots";
      else if (mode === "dots") mode = "waves";
      else mode = "lines";
    }

    // Botón B: nuevo color
    if (bState && !lastBState) {
      shapeColor = color(random(360), 100, 100);
    }

    lastAState = aState;
    lastBState = bState;
  }
}

function draw() {
  background(0, 0, 95);

  for (let shape of shapes) {
    shape.draw();
  }

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
      if (mode === "lines") {
        line(0, i, w, i);
      } else if (mode === "dots") {
        point(w * noise(i * 0.1, frameCount * 0.01), i);
      } else if (mode === "waves") {
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
  if (key == 's' || key == 'S') saveCanvas('modified-moire', 'png');

  if (key == '1') mode = "lines";
  if (key == '2') mode = "dots";
  if (key == '3') mode = "waves";

  if (keyCode == UP_ARROW) shapeHeight += density;
  if (keyCode == DOWN_ARROW) shapeHeight = max(density, shapeHeight - density);
}
```

Link: https://editor.p5js.org/KrowKrapp/sketches/A4Hm3Y_6s

