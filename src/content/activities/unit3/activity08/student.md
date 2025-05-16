En p5.js
```
let port;
let connectBtn;
let countdown = 20;
let lastTime = 0;
let timerRunning = false;
let bombaState = "INIT";

function setup() {
  createCanvas(400, 400);
  textAlign(CENTER, CENTER);
  textSize(32);

  port = createSerial();
  connectBtn = createButton('Conectar a micro:bit');
  connectBtn.position(10, 10);
  connectBtn.mousePressed(connectBtnClick);

  let btnA = createButton('A');
  btnA.position(10, 50);
  btnA.mousePressed(() => port.write('A'));

  let btnB = createButton('B');
  btnB.position(10, 90);
  btnB.mousePressed(() => port.write('B'));

  let btnT = createButton('T (Touch)');
  btnT.position(10, 130);
  btnT.mousePressed(() => port.write('T'));

  let btnS = createButton('S (Shake)');
  btnS.position(10, 170);
  btnS.mousePressed(() => port.write('S'));

  lastTime = millis();
}

function draw() {
  background(30);

  // Dibujo de la bomba
  fill(100);
  ellipse(width / 2, height / 2, 150, 150);
  fill(255);
  text(countdown, width / 2, height / 2);

  // Estado visual
  fill(200);
  textSize(16);
  text("Estado: " + bombaState, width / 2, 30);

  // Recibe datos del micro:bit
  if (port.available() > 0) {
    let val = port.read();
    if (val === 'W') {
      bombaState = "WIN";
    } else if (val === 'L') {
      bombaState = "LOOSE";
    } else if (val === 'C') {
      bombaState = "COUNT";
      timerRunning = true;
    } else if (val === 'R') {
      bombaState = "CONFIG";
      countdown = 20;
    }
  }

  // Cuenta regresiva local solo si está activo
  if (timerRunning && millis() - lastTime > 1000) {
    countdown--;
    lastTime = millis();
    if (countdown <= 0) {
      timerRunning = false;
      bombaState = "EXPLODE";
    }
  }

  if (bombaState === "WIN") {
    fill(0, 255, 0);
    textSize(40);
    text("¡Ganaste!", width / 2, height - 40);
  } else if (bombaState === "LOOSE" || bombaState === "EXPLODE") {
    fill(255, 0, 0);
    textSize(40);
    text("¡BOOM!", width / 2, height - 40);
  }
}

function connectBtnClick() {
  if (!port.opened()) {
    port.open('MicroPython', 115200);
  } else {
    port.close();
  }
}
```

En micro:bit
```
from microbit import *
import music
import utime

# Estados
INIT = 0
CONFIG = 1
COUNT = 2
WIN = 3
LOOSE = 4

state = INIT
countdown = 20000
interval = 1000
last_tick = utime.ticks_ms()
solution = []
password = ['A', 'B', 'A']
eventA = False
eventB = False
eventT = False
eventS = False
music_playing = False

def handle_events():
    global eventA, eventB, eventT, eventS
    if uart.any():
        data = uart.read(1)
        if data:
            if data == b'A': eventA = True
            if data == b'B': eventB = True
            if data == b'T': eventT = True
            if data == b'S': eventS = True

    if button_a.was_pressed(): eventA = True
    if button_b.was_pressed(): eventB = True
    if pin_logo.is_touched(): eventT = True
    if accelerometer.was_gesture('shake'): eventS = True

def bomba():
    global state, countdown, solution, eventA, eventB, eventT, eventS, music_playing, last_tick

    if state == INIT:
        state = CONFIG
        uart.write('R')

    elif state == CONFIG:
        display.show(int(countdown / 1000))
        if eventA:
            countdown = min(countdown + 1000, 60000)
        elif eventB:
            countdown = max(countdown - 1000, 1000)
        elif eventT:
            state = COUNT
            solution = []
            last_tick = utime.ticks_ms()
            uart.write('C')
            display.clear()

    elif state == COUNT:
        if utime.ticks_diff(utime.ticks_ms(), last_tick) > interval:
            countdown -= 1000
            last_tick = utime.ticks_ms()
            display.set_pixel(2, 2, 9)
            sleep(100)
            display.clear()

        if eventA and len(solution) < 3: solution.append('A')
        if eventB and len(solution) < 3: solution.append('B')

        if len(solution) == 3:
            if solution == password:
                state = WIN
                uart.write('W')
                display.show(Image.HAPPY)
                music.play(music.POWER_UP)
            else:
                solution = []

        if countdown <= 0:
            state = LOOSE
            uart.write('L')
            display.show(Image.SAD)
            music.play(music.BA_DING)

        if eventS:
            state = CONFIG
            countdown = 20000
            uart.write('R')
            display.clear()

    elif state == WIN or state == LOOSE:
        if eventS:
            state = CONFIG
            countdown = 20000
            uart.write('R')
            display.clear()

    # Reset eventos
    eventA = False
    eventB = False
    eventT = False
    eventS = False

# Main loop
uart.init(baudrate=115200)
while True:
    handle_events()
    bomba()
```
