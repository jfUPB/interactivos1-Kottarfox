#### Respuesta

- Código de p5.js
```
let circleX = 200; 

function setup() {
  createCanvas(400, 400); // Crea el lienzo de 400x400 píxeles
}

function draw() {
  background(220); 
  ellipse(circleX, 200, 100, 100); 
}

// Esta función es para actualizar la posición del círculo
function updatePosition(x) {
  circleX = x; // Actualiza la posición X del círculo
}

```
