**Preguntas**
1. Describe detalladamente cómo funciona este ejemplo. Usa chatGPT para indagar y profundizar en todas las dudas que puedas tener.
   R// Este programa crea dos objetos Pixel que encienden y apagan un LED en la pantalla del micro:bit a intervalos distintos:
pixel1 parpadea en la esquina superior izquierda (0,0) cada 1 segundo.
pixel2 parpadea en la esquina inferior derecha (4,4) cada 0.5 segundos.
Cada objeto Pixel funciona como una máquina de estados con su propia lógica de tiempo y visualización.
   
2.Del contexto del programa ¿Puedes identificar algún estado? Los estados son momentos del programa en los cuales este espera a que ocurra algo.
 R// En este código hay dos estados en la clase Pixel:
- "Init": Estado inicial en el que se establece el tiempo de inicio y se muestra el estado inicial del pixel en pantalla.
- "WaitTimeout": Estado en el que se espera a que transcurra el tiempo definido por interval. Al completarse, cambia el estado del pixel (encendido ↔ apagado) y reinicia el temporizador.

3. Del contexto del programa ¿Puedes identificar algún evento? Los eventos son aquellas cosas por las que el programa pregunta durante un estado.
    R// Evento de timeout (espera terminada): Se produce cuando la diferencia entre el tiempo actual y startTime supera el valor de interval.
   
   
4.Del contexto del programa ¿Puedes identificar alguna acción? Las acciones son aquellas cosas que el programa ejecuta en respuesta a la ocurrencia de un evento.
 R// Sí, hay varias como respuesta a los eventos. 
 - En estado "Init":
Se registra el tiempo actual: self.startTime = utime.ticks_ms()
Se enciende o apaga el pixel con el valor inicial:
display.set_pixel(self.pixelX, self.pixelY, self.pixelState)
Se cambia al estado "WaitTimeout"

- En estado "WaitTimeout" cuando el evento de timeout ocurre:
Se alterna el valor del pixel entre 0 y 9 (apagado ↔ encendido)
Se actualiza el pixel en pantalla con el nuevo valor.
Se reinicia el contador de tiempo.
   
