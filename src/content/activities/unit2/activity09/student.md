Siguiendo la ayuda de varios códigos realizados aquí más la ayuda de chat gpt logré hacer este código:
```
from microbit import *
import utime

class Semaforo:
    def __init__(self):
        self.state = "ROJO"
        self.startTime = utime.ticks_ms()

    def update(self):
        currentTime = utime.ticks_ms()
        elapsed = utime.ticks_diff(currentTime, self.startTime)

        if self.state == "ROJO":
            # Mostrar rojo (fila 0 encendida)
            self.mostrar_luz(0)
            if elapsed >= 3000:
                self.state = "VERDE"
                self.startTime = currentTime


        elif self.state == "VERDE":
            # Mostrar verde (fila 2 encendida)
            self.mostrar_luz(2)
            if elapsed >= 3000:
                self.state = "AMARILLO"
                self.startTime = currentTime

        elif self.state == "AMARILLO":
            # Mostrar amarillo (fila 1 encendida)
            self.mostrar_luz(1)
            if elapsed >= 2000:
                self.state = "ROJO"
                self.startTime = currentTime

    def mostrar_luz(self, fila):
        display.clear()
        for x in range(5):
            display.set_pixel(x, fila, 9)

semaforo = Semaforo()

while True:
    semaforo.update()
    sleep(100)

```

Ahora, este código tiene ttres estados los cuales funcionan igual a un semáforo, osea, rojo, amarillo y verde y termina funcionando así:
- Tiempo transcurrido desde que entró a un estado.
En ROJO, si han pasado 3 segundos, pasa a VERDE.
En VERDE, si han pasado 3 segundos, pasa a AMARILLO.
En AMARILLO, si han pasado 2 segundos, vuelve a ROJO.
