Pregunta 1: ¿Qué información se está enviando?
Se están enviando cuatro valores en cada línea, por ejemplo: 0,0,False,False repetidamente.
Los dos primeros valores corresponden a las lecturas del acelerómetro en los ejes X e Y, mientras que los dos últimos indican si los botones A y B están presionados (True o False). Esto representa datos numéricos y booleanos, enviados por el micro:bit a través del puerto serial.

Pregunta 2: ¿Cómo se está enviando? ¿Qué significa esta línea?
```
"{},{},{},{}\n".format(xValue, yValue, aState, bState)
```

Esto es una cadena formateada. Se crea una sola línea de texto que incluye los cuatro valores separados por comas. Las llaves {} indican los lugares donde se insertan los datos. El \n al final representa un salto de línea, que marca el final del paquete de datos. Esto facilita su lectura en el receptor (como p5.js o un monitor serial).

Pregunta 3: ¿Qué se puede inferir de la estructura de los datos?
Que cada línea contiene un paquete completo de datos: aceleración X, aceleración Y, estado del botón A y estado del botón B.
Separar los valores con comas permite que otros programas puedan identificarlos fácilmente usando un split(','). Cada línea representa una "foto" del estado del micro:bit cada 100 ms.

Pregunta 4: ¿Qué pasa si no hay comas ni salto de línea?
Realicé el experimento eliminando las comas y el salto de línea. El resultado fue una secuencia caótica de caracteres en el monitor serial. Aunque el código sigue corriendo, la información ya no es legible ni interpretable fácilmente. Por eso, el uso de comas y \n es esencial para estructurar los datos correctamente.

Pregunta 5: ¿Para qué sirve sleep(100)?
La función sleep(100) limita la velocidad de envío a 10 lecturas por segundo (10 Hz).
Sin esta pausa, el micro:bit enviaría datos sin control, lo que sobrecargaría la consola serial, dificultaría su lectura y agotaría rápidamente la batería. Esta técnica es común en dispositivos IoT para equilibrar rendimiento y eficiencia energética.

Pregunta 6: ¿Cómo varían xValue y yValue al inclinar el micro:bit?
Inclinando hacia mí: yValue disminuye (valores negativos.
Inclinando hacia adelante: yValue aumenta (positivos).
Inclinando a la izquierda: xValue disminuye.
Inclinando a la derecha: xValue aumenta.
Esto demuestra cómo el micro:bit puede actuar como un sensor de orientacion espacial.

Pregunta 7: ¿Qué valores toman aState y bState al presionar los botones?
Cuando se presiona un botón: El estado pasa de False a True.
Esto se refleja de inmediato en el monitor serial mientras se mantiene presionado.

Pregunta 8: Diferencia entre is_pressed() y was_pressed()
is_pressed(): retorna True mientras el botón esté presionado.
was_pressed(): solo retorna True una vez cuando se detecta que se presionó y soltó el botón desde la última lectura.
Con was_pressed(), la lectura parece "llegar después" porque espera que se suelte el botón, ideal para detectar eventos únicos y no mantener estados.

Pregunta 9: ¿Qué bytes se envían por el puerto serial para 969,652,True,False\n?
Cada carácter tiene una representación en la tabla ASCII. Si convertimos esa cadena a sus valores decimales en ASCII, obtenemos:
```
"969,652,True,False\n" →  
[57, 54, 57, 44, 54, 53, 50, 44, 84, 114, 117, 101, 44, 70, 97, 108, 115, 101, 10]
```

Estos son los bytes exactos que viajan por el puerto serial, uno por cada carácter.
Este es el mismo principio que se utiliza en muchos videojuegos como Minecraft, donde las semillas ingresadas como texto se traducen internamente en números mediante su codificación.






