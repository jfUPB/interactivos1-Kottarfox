#### Pruebas Exhaustivas – Bomba Temporizada
Vector de Prueba 1: Aumentar tiempo
Acciones: Iniciar → Presionar botón A tres veces
Esperado: El tiempo pasa de 20 a 23 segundos
Resultado: Funciona correctamente
Error: Ninguno

- Vector de Prueba 2: Disminuir tiempo por debajo del mínimo
Acciones: Presionar botón B muchas veces hasta intentar bajar de 10 segundos
Esperado: El tiempo no baja de 10 segundos
Resultado: Funciona correctamente
Error: Ninguno

- Vector de Prueba 3: Armar bomba y dejarla detonar
Acciones: Ajustar tiempo a 5 segundos → Shake → esperar
Esperado: Cuenta regresiva → al llegar a 0 suena el speaker y muestra "BOOM"
Resultado: Cumple comportamiento esperado
Error: Ninguno

- Vector de Prueba 4: Interrumpir cuenta regresiva con botón touch
Acciones: Ajustar tiempo → Shake → durante cuenta regresiva, tocar botón touch
Esperado: Vuelve al estado de configuración
Resultado:  Funciona correctamente
Error: Ninguno

- Vector de Prueba 5: Sacudir varias veces rápidamente
Acciones: Shake varias veces seguidas antes y después de armado
Esperado: Solo un cambio de estado
Resultado: Doble activación, la bomba vuelve a armar después de desactivarse
Error encontrado: El gesto “shake” sigue activo unos milisegundos y provoca múltiples transiciones
Corrección: Se agregó una pausa (debounce) después de detectar el shake

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
            sleep(500)  # debounce para evitar múltiples shakes

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
            estado = CONFIG
```
