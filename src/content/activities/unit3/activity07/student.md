let countdown = 20; // tiempo inicial en segundos
let lastTime = 0;
let timerRunning = true;

function setup() {
  createCanvas(400, 400);
  textAlign(CENTER, CENTER);
  textSize(32);
  lastTime = millis();
}

function draw() {
  background(30);
  
  // Dibujo de la bomba
  fill(100);
  ellipse(width / 2, height / 2, 150, 150); // cuerpo de la bomba
  
  // Mecha
  stroke(255, 100, 0);
  strokeWeight(4);
  line(width / 2, height / 2 - 75, width / 2, height / 2 - 100);
  noStroke();
  
  // Llama
  if (frameCount % 20 < 10) {
    fill(255, 150, 0);
    ellipse(width / 2, height / 2 - 110, 20, 20);
  }

  // Temporizador
  fill(255);
  text(countdown, width / 2, height / 2);

  // Cuenta regresiva
  if (timerRunning && millis() - lastTime >= 1000) {
    countdown--;
    lastTime = millis();
  }

  // Explosión al llegar a cero
  if (countdown <= 0) {
    timerRunning = false;
    fill(255, 0, 0);
    textSize(48);
    text("¡BOOM!", width / 2, height / 2 + 100);
  }
}
