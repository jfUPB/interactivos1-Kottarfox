Código para que haga BOOOOOM:
```
from microbit import *
import music
import utime

# Estados
CONFIG = 0
ARMED = 1

estado = CONFIG
countdown_ms = 20000  # 20 segundos en milisegundos
min_time = 10000      # 10 segundos
max_time = 60000      # 60 segundos
tick = 1000           # 1 segundo

def mostrar_tiempo(ms):
    display.scroll(str(ms // 1000), delay=100, wait=False, loop=False)

while True:
    if estado == CONFIG:
        mostrar_tiempo(countdown_ms)
        
        if button_a.was_pressed():
            countdown_ms += tick
            if countdown_ms > max_time:
                countdown_ms = max_time

        if button_b.was_pressed():
            countdown_ms -= tick
            if countdown_ms < min_time:
                countdown_ms = min_time

        if accelerometer.was_gesture("shake"):
            estado = ARMED
            start_time = utime.ticks_ms()

    elif estado == ARMED:
        tiempo_restante = countdown_ms
        while tiempo_restante > 0:
            mostrar_tiempo(tiempo_restante)
            sleep(tick)
            tiempo_restante -= tick

            if pin_logo.is_touched():
                estado = CONFIG
                break

        if tiempo_restante <= 0:
            display.scroll("BOOM")
            music.play(music.WAWAWAWAA)
            sleep(2000)
            estado = CONFIG  # Reinicia luego de explotar

```

Link del vídeo: https://youtu.be/LKNdbVONwwU
