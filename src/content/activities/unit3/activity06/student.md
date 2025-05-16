- Vectores de Prueba
En config presionaré demasiadas veces A. El resultado esperado es que llegue a 60 y no aumente.
En config presionaré demasiadas veces B. El resultado esperado es que llegue a 1 y no disminuya más.
De config presionaré A 3 veces y luego T para ir a COUNT, luego presionaré S para que me lleve a INIT y de ahí a CONFIG. El contador debería resetearse y volver a 20.
De config iré a COUNT, ingresaré cuantas contraseñas erradas sean posibles (por ejemplo: A-B-B repetidamente). El resultado esperado es entrar en el estado LOOSE cuando el contador llegue a cero.
De config iré a COUNT, ingresaré la contraseña correcta (A-B-A) y después spamearé los botones A y B. Luego de un rato, presionaré S. El resultado esperado es que entre en el estado WIN y, después de S, regrese a INIT.
De config iré a COUNT, no haré nada, y justo antes de que acabe la cuenta regresiva oprimiré S. El resultado debería ser regresar a INIT sin pasar por LOOSE.

- Vectores que Pasaron
Pasaron la prueba los vectores 1, 2, 3, 4 y 6.

- Vectores que Fallaron
El vector que no funcionó fue el 5. No se logra registrar correctamente la contraseña aunque sea la correcta. La validación no se completa y no cambia al estado WIN. Además, al volver a CONFIG después de pasar por WIN o LOOSE, el evento T no funciona de inmediato. Es necesario presionarlo dos veces para que lleve a COUNT. Esto no pasa si se accede a CONFIG desde COUNT directamente.

- Correcciones
El evento T se reseteaba antes de poder usarse, por lo que se movió el reseteo de eventos (eventA, eventB, eventT, eventS) al final del ciclo while, después de llamar a tareaBomba().
También se ajustó la lógica para que el estado INIT se ejecute solo una vez y no cada vez que se vuelve desde WIN o LOOSE.
Con estas correcciones, todos los vectores pasaron correctamente en pruebas posteriores.

from microbit import *
import utime
import music

# Estados de la máquina
STATE_INIT = 0
STATE_CONFIG = 1
STATE_COUNT = 2
STATE_WIN = 3
STATE_LOOSE = 4

# Intervalo de cuenta regresiva
COUNT_INTERVAL = 1000
waitingTime = utime.ticks_ms()

# Contraseña esperada y lista para capturar intentos
Password = ["A", "B", "A"]
solution = []

# Estado inicial y variables de control
current_state = STATE_INIT
countdown = 20000
music_playing = False
init_executed = False  # Para ejecutar INIT solo una vez

# Eventos globales
eventA = False
eventB = False
eventT = False
eventS = False

def tareaBomba():
    global current_state, countdown, music_playing
    global eventA, eventB, eventT, eventS, waitingTime
    global solution, Password, init_executed

    # INIT solo una vez
    if current_state == STATE_INIT and not init_executed:
        countdown = 20000
        solution = []
        current_state = STATE_CONFIG
        init_executed = True

    if current_state == STATE_CONFIG:
        music.stop()
        music_playing = False

        if eventT:
            current_state = STATE_COUNT
            display.clear()
        elif eventA:
            countdown = min(countdown + 1000, 60000)
            display.show(int(countdown / 1000))
        elif eventB:
            countdown = max(countdown - 1000, 1000)
            display.show(int(countdown / 1000))
        else:
            display.show(int(countdown / 1000))

    elif current_state == STATE_COUNT:
        display.clear()

        # Cuenta regresiva
        if utime.ticks_diff(utime.ticks_ms(), waitingTime) > COUNT_INTERVAL:
            countdown -= 1000
            waitingTime = utime.ticks_ms()
            display.set_pixel(2, 2, 9)
            sleep(100)
            display.clear()

        # Captura de contraseña
        if eventA and len(solution) < len(Password):
            solution.append("A")
        if eventB and len(solution) < len(Password):
            solution.append("B")

        if len(solution) == len(Password):
            if solution == Password:
                current_state = STATE_WIN
            else:
                solution = []

        if countdown <= 0:
            current_state = STATE_LOOSE

        if eventS:
            countdown = 20000
            solution = []
            current_state = STATE_CONFIG

    elif current_state == STATE_WIN:
        display.show(Image.HAPPY)
        if not music_playing:
            music_playing = True
            music.play(music.BIRTHDAY, wait=False)

        if eventS:
            countdown = 20000
            solution = []
            current_state = STATE_CONFIG

    elif current_state == STATE_LOOSE:
        display.show(Image.SAD)
        if not music_playing:
            music_playing = True
            music.play(music.DADADADUM, wait=False)

        if eventS:
            countdown = 20000
            solution = []
            current_state = STATE_CONFIG

def tareaEventos():
    global eventA, eventB, eventT, eventS

    if uart.any():
        data = uart.read(1)
        if data:
            if data[0] == ord('A'):
                eventA = True
            elif data[0] == ord('B'):
                eventB = True
            elif data[0] == ord('T'):
                eventT = True
            elif data[0] == ord('S'):
                eventS = True

    if button_a.was_pressed():
        eventA = True
    if button_b.was_pressed():
        eventB = True
    if pin_logo.is_touched():
        eventT = True
    if accelerometer.was_gesture('shake'):
        eventS = True

while True:
    tareaEventos()
    tareaBomba()

    # Resetear eventos después de usarlos
    eventT = False
    eventA = False
    eventB = False
    eventS = False
