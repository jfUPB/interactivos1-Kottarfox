🧐🧪✍️ 1. Resultado al ver los datos binarios como texto en SerialTerminal
Observación:
Cuando seleccioné "Texto" como modo de visualización, los datos se veían como caracteres raros o símbolos sin sentido. Algunos eran como cuadraditos, símbolos extraños o simplemente líneas vacías.

¿Por qué se ve así?
Esto ocurre porque estamos enviando datos en binario y no en formato de texto (ASCII). Los bytes enviados no siempre se corresponden con caracteres imprimibles, así que al intentar mostrarlos como texto, se interpretan mal o no se muestran correctamente.

🧐🧪✍️ 2. Resultado al ver los datos como "Todo en Hex"
Observación:
Cuando cambié la vista a "Todo en Hex", pude ver los bytes claramente. Por ejemplo, algo como:
FF 38 00 C2 01 00

Relación con la línea de código:
```
data = struct.pack('>2h2B', xValue, yValue, int(aState), int(bState))
```
Cada par de caracteres hexadecimales representa un byte. Los dos primeros pares (FF 38) corresponden a xValue, los siguientes dos (00 C2) a yValue, y los últimos dos (01 00) a aState y bState (valores booleanos convertidos en 1 o 0). Esto confirma que se están enviando 6 bytes en total (2h = 4 bytes, 2B = 2 bytes).

🧐🧪✍️ 3. Ventajas y desventajas del formato binario vs. ASCII
Ventajas del formato binario:
Más compacto: ocupa menos bytes.
Más eficiente: al tener tamaño fijo, se puede leer más rápido.
Ideal para sistemas embebidos o de baja potencia.
Desventajas del formato binario:
Difícil de leer e interpretar manualmente.
Requiere conocer exactamente el formato de los datos para decodificarlos.
Ventajas del formato ASCII:


Fácil de leer por humanos.
Más simple para pruebas rápidas o debugging.
Desventajas del formato ASCII:
Más lento y pesado: requiere convertir números a texto y viceversa.
Los paquetes varían en tamaño, lo que complica la lectura estructurada.

🧐🧪✍️ 4. Resultado del experimento con evento shake (envío binario controlado)
Observación:
Ahora los datos solo se envían cuando se detecta un "shake". En el modo hexadecimal, cada vez que agito el micro:bit aparecen 6 bytes nuevos.
¿Cuántos bytes se envían?
Se envían 6 bytes:
2 bytes para xValue (entero corto)
2 bytes para yValue (entero corto)
1 byte para aState
1 byte para bState

Relación con '>2h2B':
2h = 2 short integers (2 bytes cada uno) = 4 bytes
2B = 2 unsigned chars (1 byte cada uno) = 2 bytes
Total = 6 bytes por paquete.

¿Qué representa cada byte?
Bytes 0-1: valor X del acelerómetro (en formato de 2 bytes, puede ser negativo).
Bytes 2-3: valor Y del acelerómetro.
Byte 4: botón A (0 o 1).
Byte 5: botón B (0 o 1).

🧐🧪✍️ 5. ¿Cómo se verían valores negativos en '>2h2B'?
Respuesta:
Los valores negativos se representan en complemento a dos. Por ejemplo, si xValue = -100, en binario se vería como FF 9C en hexadecimal. Esto es importante porque si no se interpreta correctamente, el valor parecería un número grande positivo en lugar de uno negativo.

🧐🧪✍️ 6. Comparación entre envío ASCII y binario (último experimento)
Observación:
En el terminal, después de una agitación, aparecen primero los 6 bytes en hexadecimal (binario) y luego la palabra ASCII: seguida por los datos como texto, por ejemplo:
```
ASCII:  
-232,148,1,0

```







