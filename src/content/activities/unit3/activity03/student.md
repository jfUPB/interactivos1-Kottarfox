#### Código que propongo:
```
from microbit import *
import music

# Estados de la bomba
CONFIG = 0
ARMED = 1
estado = CONFIG

# Temporización
countdown_ms = 20000
min_time = 10000
max_time = 60000
tick = 1000

# Variables globales de eventos
evento_disponible = False
evento = None

# Secuencia de desactivación
secuencia_esperada = ['A', 'B', 'A', 'S']
indice_secuencia = 0

def reiniciar_secuencia():
    global indice_secuencia
    indice_secuencia = 0

def mostrar_tiempo(ms):
    display.show(str(ms // 1000))

def tareaEventos():
    global evento_disponible, evento

    # Sensores físicos
    if button_a.was_pressed():
        evento = 'A'
        evento_disponible = True
    elif button_b.was_pressed():
        evento = 'B'
        evento_disponible = True
    elif accelerometer.was_gesture("shake"):
        evento = 'S'
        evento_disponible = True
    elif pin_logo.is_touched():
        evento = 'T'
        evento_disponible = True

    # Evento desde puerto serial
    if uart.any():
        recibido = uart.read().decode('utf-8').strip().upper()
        if recibido in ['A', 'B', 'S', 'T']:
            evento = recibido
            evento_disponible = True

def tareaBomba():
    global estado, countdown_ms, evento_disponible, evento, indice_secuencia

    if estado == CONFIG:
        mostrar_tiempo(countdown_ms)

        if evento_disponible:
            if evento == 'A':
                countdown_ms += tick
                if countdown_ms > max_time:
                    countdown_ms = max_time
            elif evento == 'B':
                countdown_ms -= tick
                if countdown_ms < min_time:
                    countdown_ms = min_time
            elif evento == 'S':
                estado = ARMED
                reiniciar_secuencia()
            # Consumir evento
            evento_disponible = False

    elif estado == ARMED:
        tiempo_restante = countdown_ms

        while tiempo_restante > 0:
            mostrar_tiempo(tiempo_restante)
            sleep(tick)
            tiempo_restante -= tick

            tareaEventos()  # Revisa si hay nuevos eventos

            if evento_disponible:
                if evento == secuencia_esperada[indice_secuencia]:
                    indice_secuencia += 1
                else:
                    reiniciar_secuencia()

                if indice_secuencia == len(secuencia_esperada):
                    display.scroll("Desactivada")
                    estado = CONFIG
                    reiniciar_secuencia()
                    break

                evento_disponible = False  # Consumir evento

            if evento == 'T':  # Tocar botón de escape
                estado = CONFIG
                reiniciar_secuencia()
                evento_disponible = False
                break

        if tiempo_restante <= 0:
            display.scroll("BOOM")
            music.play(music.WAWAWAWAA)
            sleep(2000)
            estado = CONFIG
            reiniciar_secuencia()

# Configura UART para comunicación serial
uart.init(baudrate=9600)

# Loop principal
while True:
    tareaEventos()
    tareaBomba()
```

**Explicación:**
En esta versión 3.0 de la bomba temporizada, el objetivo fue hacer una refactorización del código para permitir que los eventos que controlan la máquina de estados provengan tanto de los sensores físicos del micro:bit (botones A, B, gesto de agitar y touch logo) como del puerto serial. Para lograr esto, se reorganizó el código en dos funciones principales: tareaEventos() y tareaBomba(). La función tareaEventos() se encarga exclusivamente de leer las entradas desde los sensores y el puerto serial, y cuando detecta un evento, lo almacena en una variable llamada evento y activa una bandera booleana evento_disponible, indicando que hay un nuevo evento pendiente por procesar. Esta función unifica la fuente de eventos, haciendo que la máquina de estados no distinga si el evento viene de un botón físico o de un mensaje enviado por el usuario por serial.
Por otro lado, la función tareaBomba() contiene la lógica de la máquina de estados. Esta función únicamente revisa si evento_disponible está activa, y si es así, consulta el valor de evento para decidir qué acción tomar. Cuando el estado es de configuración (CONFIG), los eventos ‘A’ y ‘B’ incrementan o disminuyen el tiempo de cuenta regresiva, mientras que el evento ‘S’ (shake) cambia el estado a armado (ARMED). Cuando el estado es ARMED, se inicia la cuenta regresiva, la cual se visualiza en el display LED. Durante esta cuenta regresiva, también se revisan continuamente los eventos entrantes. Si se recibe la secuencia correcta de desactivación (‘A’, ‘B’, ‘A’, ‘S’) en orden, la bomba se desactiva y vuelve al estado de configuración. Si se toca el botón touch, también se cancela la bomba regresando al estado CONFIG. Si el tiempo llega a cero y la bomba no fue desactivada, se muestra “BOOM” y suena un tono de explosión con el speaker del micro:bit.
Una vez que la función tareaBomba() procesa un evento, debe "consumirlo", es decir, poner evento_disponible en False para indicar que ya fue manejado, permitiendo que tareaEventos() pueda capturar un nuevo evento. Esto garantiza que cada evento sea procesado solo una vez y evita que la máquina de estados reaccione varias veces al mismo estímulo. Esta separación clara entre recolección de eventos y lógica de estados hace el código más modular y fácil de extender en el futuro. Además, permite aplicar esta estructura a otros proyectos que requieran entrada desde múltiples fuentes de eventos, como sensores, botones, internet o comunicación remota, manteniendo una arquitectura limpia y escalable.
