**Mostrar imágenes**
```
from microbit import *
import time

# Imagen personalizada (una carita feliz)
carita_feliz = Image("09090:"
                     "09090:"
                     "00000:"
                     "90009:"
                     "09990")

while True:
    # Mostrar texto
    display.scroll("Hola Mundo")
    sleep(1000)

    # Mostrar imagen personalizada
    display.show(carita_feliz)
    sleep(2000)

    # Mostrar imagen predefinida
    display.show(Image.HEART)
    sleep(2000)
```

Ahora, como lo  hice funcionar? Es algo simple :P
1. display.scroll("Hola Mundo")
Muestra el texto "Hola Mundo" desplazándose horizontalmente en la pantalla LED.

2. Imagen personalizada con Image()
Se define una imagen en forma de string con números que representan el brillo de cada LED (de 0 a 9).
En este caso, se representa una carita feliz.

3. display.show(Image.HEART)
Se muestra una imagen predefinida (un corazón) incluida en la biblioteca del micro:bit.
