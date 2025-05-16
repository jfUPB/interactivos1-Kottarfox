jemplo 1
Enlace del ejemplo:
https://editor.p5js.org/generative-design/sketches/P_2_1_1_03

¿Qué me llamó la atención?
Lo que más me llamó la atención fue el diseño geométrico generado a partir de líneas en diferentes posiciones y ángulos. La interacción del mouse con la visualización le da una dimensión dinámica, permitiendo ver cómo pequeñas modificaciones afectan el diseño general. Me pareció una forma muy elegante de mostrar cómo una simple función puede crear un efecto visual complejo.

¿Cómo está hecho el sketch?
Este sketch utiliza principalmente line(), translate(), rotate() y ciclos for para crear una cuadrícula en la que se dibujan líneas con ángulos aleatorios. La función noise() también es empleada para generar variaciones suaves. Las funciones clave de p5.js utilizadas son setup(), draw(), stroke(), noFill() y random(). Se aplica programación generativa, uso de transformaciones geométricas y patrones visuales repetitivos.

Modificación realizada:
Reemplacé las líneas por pequeños triángulos rotados para observar cómo cambia la percepción del patrón. También añadí color utilizando stroke() con valores random() para darle una apariencia más viva.

Enlace del sketch modificado:
https://editor.p5js.org/KrowKrapp/sketches/nMhf2Bdek

Ejemplo 2
Enlace del ejemplo:
https://editor.p5js.org/generative-design/sketches/P_2_1_5_03

¿Qué me llamó la atención?
Este ejemplo genera una composición visual con líneas paralelas contenidas dentro de formas rectangulares que el usuario puede dibujar con el mouse. Me llamó la atención cómo, a partir de una interacción simple, se logra un efecto visual interesante que recuerda al moiré o a patrones interferenciales. El control que se le da al usuario para modificar parámetros como color, altura y posición de las formas lo convierte en una herramienta creativa poderosa.

¿Cómo está hecho el sketch?
El sketch utiliza una clase llamada Shape que define una figura rectangular basada en dos puntos y una altura. Dentro de esta clase se usan funciones clave de p5.js como line(), stroke(), translate(), rotate() y dist(). Las figuras se generan al hacer clic y se almacenan en un arreglo (shapes) para ser redibujadas continuamente. El usuario puede modificar el color con las teclas del 1 al 4, y la altura de las figuras con las flechas del teclado. La lógica está bien modularizada y hace uso efectivo de objetos y transformaciones geométricas.

Modificación realizada:
Modifiqué el sketch para hacerlo más dinámico y con mayor variedad visual. En lugar de limitarme a las líneas rectas, añadí tres modos visuales: líneas tradicionales, puntos y líneas onduladas (tipo wave). Estos modos se alternan presionando las teclas 1, 2 y 3. También cambié el sistema de color para utilizar el modo HSB, permitiendo colores más vivos y cambiantes: ahora cada nueva figura que se dibuja con el mouse tiene un color aleatorio en ese espacio de color. Además, en el modo “waves” implementé una oscilación animada para cada línea con sin() y frameCount, lo que genera una ilusión de movimiento. El fondo ahora es blanco para mejorar el contraste.Enlace del sketch modificado:

Enlace del sketch modificado:
https://editor.p5js.org/KrowKrapp/sketches/GTHCBJcQ9

