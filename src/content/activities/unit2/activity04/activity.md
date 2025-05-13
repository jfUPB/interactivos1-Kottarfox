**Experimento 1: Botón A**
- ¿Qué querías comprobar?
Quería comprobar si al presionar el botón A del micro:bit, el dispositivo podía detectar la acción y ejecutar una respuesta (mostrar un mensaje en pantalla).
- ¿Cuál fue tu hipótesis?
Si presiono el botón A, entonces se mostrará el mensaje programado en la pantalla del micro:bit.

- Código involucrado en el experimento:
```  
from microbit import *

while True:
    if button_a.is_pressed():
        display.scroll("Boton A")
```

- Descripción de los resultados
Al presionar el botón A, la pantalla del micro:bit mostró el mensaje "Boton A".
- Análisis de esos resultados
El botón A fue correctamente detectado por el código con button_a.is_pressed(). La pantalla reaccionó solo mientras el botón estuvo presionado, cumpliendo con la lógica del programa.
- Conclusiones
El micro:bit puede detectar el estado del botón A correctamente. Este botón funciona como un interruptor digital que activa un evento específico cuando se presiona.

**Experimento 2: Botón B**
- ¿Qué querías comprobar?
Quería comprobar si el micro:bit podía diferenciar entre el botón A y el botón B y ejecutar una respuesta diferente.
- ¿Cuál fue tu hipótesis?
Si presiono el botón B, se mostrará un mensaje distinto al del botón A.

Código involucrado en el experimento
```  
from microbit import *

while True:
    if button_a.is_pressed():
        display.scroll("Boton A")
    elif button_b.is_pressed():
        display.scroll("Boton B")
```

- Descripción de los resultados
El micro:bit mostró "Boton A" cuando presioné A y "Boton B" cuando presioné B. No hubo interferencia entre botones.
- Análisis de esos resultados
El programa distingue correctamente entre las entradas de los botones. La función elif asegura que solo uno de los dos botones se evalúe por ciclo, evitando que ambos mensajes se muestren si se presionan simultáneamente.
- Conclusiones
Los botones A y B son independientes, y el micro:bit puede manejar múltiples entradas digitales con respuestas distintas.

**Experimento 3: Botón del logo táctil**
- ¿Qué querías comprobar?
Quería comprobar si el botón virtual del logo del micro:bit V2 responde al tacto como un botón físico.
- ¿Cuál fue tu hipótesis?
Si toco el logo, el micro:bit reconocerá la acción y mostrará un mensaje.

Código involucrado en el experimento
```  
from microbit import *

while True:
    if pin_logo.is_touched():
        display.scroll("Logo!")
```

- Descripción de los resultados
Al tocar el logo del micro:bit, la pantalla mostró "Logo!". Si dejaba de tocarlo, el mensaje ya no se mostraba.
- Análisis de esos resultados
El sensor capacitivo del logo detecta el tacto correctamente usando pin_logo.is_touched(). Esto demuestra que el micro:bit puede detectar tanto botones físicos como táctiles.
- Conclusiones
El botón del logo se comporta como un sensor táctil funcional. Esto amplía las capacidades de entrada del micro:bit, permitiendo experiencias más interactivas.
