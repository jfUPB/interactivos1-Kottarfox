**Código para que reproduzca música**
```
from microbit import *
import music

while True:
    if pin_logo.is_touched():
        music.play(music.CHASE, wait=False, loop=True)
    
   elif button_b.is_pressed():
        music.play(music.WAWAWAWAA, wait=False, loop=True)

 else:
        music.stop() 

    sleep(100)
```

**¿Qué hace este código?**
- Si se toca el logo del micro:bit, se reproduce la melodía music.CHASE en bucle.

- Si se presiona el botón B, se reproduce la melodía music.WAWAWAWAA en bucle.

- Si ninguna de las condiciones anteriores se cumple, el micro:bit detiene la música (music.stop()).
