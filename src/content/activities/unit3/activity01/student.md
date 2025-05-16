Hice un cambio un poco fuerte al ponerle las clases aunque creo que funcionó lo suficientemente bien :)

```
from microbit import *
import utime

class Semaforo:
    def __init__(self, x, tiempos):
        self.x = x  # columna donde se dibujará el semáforo (0 a 4)
        self.tiempos = tiempos  # diccionario con duración por color
        self.state = "ROJO"
        self.startTime = utime.ticks_ms()

    def update(self):
        now = utime.ticks_ms()
        elapsed = utime.ticks_diff(now, self.startTime)

        if self.state == "ROJO":
            self.mostrar_color(9)  # intensidad 9 = rojo (máxima)
            if elapsed >= self.tiempos["ROJO"]:
                self.state = "VERDE"
                self.startTime = now

        elif self.state == "VERDE":
            self.mostrar_color(3)  # intensidad baja para representar verde
            if elapsed >= self.tiempos["VERDE"]:
                self.state = "AMARILLO"
                self.startTime = now

        elif self.state == "AMARILLO":
            self.mostrar_color(6)  # intensidad media para representar amarillo
            if elapsed >= self.tiempos["AMARILLO"]:
                self.state = "ROJO"
                self.startTime = now

    def mostrar_color(self, intensidad):
        # El semáforo se muestra como una columna (x) completa
        for y in range(5):
            display.set_pixel(self.x, y, intensidad)

# Definición de los tiempos en milisegundos
semaforo1 = Semaforo(0, {"ROJO": 5000, "AMARILLO": 2000, "VERDE": 3000})
semaforo2 = Semaforo(2, {"ROJO": 3000, "AMARILLO": 1000, "VERDE": 2000})
semaforo3 = Semaforo(4, {"ROJO": 4000, "AMARILLO": 3000, "VERDE": 2000})

while True:
    display.clear()  # borrar antes de dibujar todos los semáforos
    semaforo1.update()
    semaforo2.update()
    semaforo3.update()
    sleep(100)
```
#### ¿Por qué usar una clase?
Las ventajas de codificar por clases y además que sean concurrentes es que puedo instanciar varios objetos que reproduzcan a la vez señales en un solo dispositivo y que cada uno tenga características diferentes sin tener que reescribir el código para cada uno.
Siempre que tengan la misma construcción y funciones o formas de actuar similares, puedes ahorrar mucho espacio de código con una clase.Además, el uso de clases permite tener una programación más organizada, reutilizable y escalable. En combinación con el uso de máquinas de estados, se logra un control claro y eficiente sobre comportamientos complejos como los del semáforo. Cada objeto puede manejar su propio tiempo y lógica de cambio de estado de forma independiente, pero compartiendo una misma estructura de código.
