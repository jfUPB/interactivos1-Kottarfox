Entradas del micro:bit (físicas):
- Botón A: Botón físico en el lado izquierdo. Detecta si se presiona para ejecutar acciones específicas.
- Botón B: Botón físico en el lado derecho. Funciona igual que el botón A y permite otra entrada manual.
- Pin 0 (P0): Entrada digital o analógica; se usa comúnmente para sensores táctiles o potenciómetros.
- Pin 1 (P1): Similar al P0, permite leer datos de sensores como temperatura o luz.

Salidas del micro:bit (físicas):
- Matriz de LEDs (5x5): Pantalla integrada que permite mostrar texto, imágenes o animaciones.
- Pin 0 (P0): Además de entrada, puede usarse como salida para encender un LED, por ejemplo.
- Pin 1 (P1): Permite enviar señal digital o analógica para controlar dispositivos externos como zumbadores.
- Pin 2 (P2): Otra salida útil para conectar motores o luces.

Ahora, que puedo hacer con estas entradas?
1. Botones: Puedes controlar luces, menús o valores. Por ejemplo, puedes usar el botón A para prender un LED y el botón B para apagarlo, o bien llevar un conteo simple
```
from microbit import *

count = 0
display.show(count)

while True:
    if button_a.is_pressed() and button_b.is_pressed():
        count = 0
        display.scroll(count)
    elif button_b.is_pressed():
        count += 1
        display.scroll(count)
    elif button_a.is_pressed():
        display.scroll(count)
    sleep(100)
```

2. Micrófono:
Puedes hacer que el microbit reaccione a sonidos fuertes como aplausos. Por ejemplo, muestra un corazón grande al detectar un sonido fuerte y uno pequeño cuando está en silencio
```
from microbit import *
import microphone

microphone.listen()

while True:
    if microphone.current_event() == SoundEvent.LOUD:
        display.show(Image.HEART)
        sleep(200)
    if microphone.current_event() == SoundEvent.QUIET:
        display.show(Image.HEART_SMALL)
```

3. Acelerómetro: se pueden detectar movimientos como sacudidas. Aquí un ejemplo de un juego de piedra, papel o tijeras usando gestos

```
from microbit import *
import random

while True:
    if accelerometer.was_gesture('shake'):
        tool = random.randint(0, 2)
        if tool == 0:
            display.show(Image.SQUARE_SMALL)  # Piedra
        elif tool == 1:
            display.show(Image.SQUARE)       # Papel
        else:
            display.show(Image.SCISSORS)     # Tijeras
```

4. Pin de entrada (P0): se puede conectar un botón físico externo y usarlo para encender un LED interno o mostrar un mensaje cuando se presione
```
from microbit import *

while True:
    if pin0.is_touched():
        display.show("H")
    else:
        display.clear()
```

Y que con las salidas?
1. Altavoz: Puedes hacer sonidos o música. Este ejemplo crea un metrónomo básico donde puedes cambiar el tempo con los botones
```   
from microbit import *
import music

tempo = 100

while True:
    music.set_tempo(bpm=tempo)
    music.play(['C4:1', 'r:3'])
    if button_a.was_pressed():
        tempo -= 5
    if button_b.was_pressed():
        tempo += 5
  ```    

2. Luces LED: Puedes mostrar emociones, símbolos o animaciones en la matriz de LEDs.
from microbit import *
```
while True:
    if button_a.is_pressed():
        display.show(Image.HAPPY)
    if button_b.is_pressed():
        display.show(Image.SAD)
```

3. Radio: Puedes comunicarte entre dos micro:bits.
   ```
    from microbit import *
    import radio

    radio.config(group=23)
    radio.on()

    while True:
    message = radio.receive()
    if message == 'duck':
        display.show(Image.DUCK)
    if accelerometer.was_gesture('shake'):
        radio.send('duck')
        display.clear()
   ```
   


4. Pin de salida (P1):Puedes encender un LED externo conectado al pin 1.
```
from microbit import *
import radio

radio.config(group=23)
radio.on()

while True:
    message = radio.receive()
    if message == 'duck':
        display.show(Image.DUCK)
    if accelerometer.was_gesture('shake'):
        radio.send('duck')
        display.clear()

```
