#### Bomba con desactivación

```
from microbit import *
import music
import utime

CONFIG = 0
ARMED = 1
estado = CONFIG
countdown_ms = 20000  # 20 segundos
min_time = 10000
max_time = 60000
tick = 1000

# Secuencia para desactivar la bomba: A, B, A, shake
secuencia_esperada = ['A', 'B', 'A', 'shake']
indice_secuencia = 0

def mostrar_tiempo(ms):
    display.show(str(ms // 1000))

def reiniciar_secuencia():
    global indice_secuencia
    indice_secuencia = 0

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
            reiniciar_secuencia()
            sleep(500)  # debounce

    elif estado == ARMED:
        tiempo_restante = countdown_ms

        while tiempo_restante > 0:
            mostrar_tiempo(tiempo_restante)
            sleep(tick)
            tiempo_restante -= tick

            # Verifica si se toca el logo para salir manualmente (modo prueba)
            if pin_logo.is_touched():
                estado = CONFIG
                break

            # Verifica secuencia de desactivación
            if button_a.was_pressed():
                if secuencia_esperada[indice_secuencia] == 'A':
                    indice_secuencia += 1
                else:
                    reiniciar_secuencia()

            elif button_b.was_pressed():
                if secuencia_esperada[indice_secuencia] == 'B':
                    indice_secuencia += 1
                else:
                    reiniciar_secuencia()

            elif accelerometer.was_gesture("shake"):
                if secuencia_esperada[indice_secuencia] == 'shake':
                    indice_secuencia += 1
                else:
                    reiniciar_secuencia()
                sleep(500)  # debounce

            # Si la secuencia completa fue ingresada correctamente
            if indice_secuencia == len(secuencia_esperada):
                display.scroll("Desactivada")
                estado = CONFIG
                reiniciar_secuencia()
                break

        if tiempo_restante <= 0:
            display.scroll("BOOM")
            music.play(music.WAWAWAWAA)
            sleep(2000)
            estado = CONFIG
            reiniciar_secuencia()

```
**Explicación:**
Para permitir la desactivación de la bomba una vez armada, se creó una lista (secuencia_esperada) con los eventos que deben cumplirse en orden. Cada vez que se detecta uno de los eventos (botón A, botón B o shake), se compara con el elemento actual de la secuencia. Si el evento es correcto, se avanza al siguiente paso. Si se falla en el orden, se reinicia la secuencia. Si se completa la secuencia correctamente, se desactiva la bomba y se vuelve al estado de configuración.
