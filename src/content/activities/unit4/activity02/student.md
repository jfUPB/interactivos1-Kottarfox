- ¿Qué hace este sketch?
Este sketch te permite dibujar elementos visuales rotados con el mouse. Estos pueden ser líneas o imágenes SVG cargadas dinámicamente, y tú controlas su rotación, color, tamaño y dirección. Es como un pincel digital que se transforma con teclas.

- Explicación del código:
Variables principales
El sketch comienza declarando un conjunto de variables globales que controlan los elementos visuales del dibujo. La variable c representa el color actual con el que se dibujan las líneas o imágenes; este color puede cambiar dinámicamente durante la ejecución del programa. La variable lineModuleSize define el tamaño que tendrá cada módulo visual al colocarse en el lienzo; este valor se modifica aleatoriamente cuando se presiona el mouse y también puede ajustarse con las flechas del teclado. La variable angle lleva el ángulo de rotación acumulado que se aplicará a cada nuevo módulo, mientras que angleSpeed define la velocidad con la que este ángulo se incrementa, controlando así el giro progresivo de las figuras a medida que se dibuja.
El arreglo lineModule contiene las imágenes SVG que se pueden usar como “pinceles” o módulos gráficos. Estos se cargan desde archivos externos y se indexan del 1 al 4, mientras que el índice 0 indica que se usará una simple línea en lugar de una imagen. lineModuleIndex nos dice cuál de estos módulos está activo. Finalmente, clickPosX y clickPosY almacenan la posición del mouse cuando se hace clic, lo cual se utiliza especialmente si se desea bloquear el movimiento horizontal o vertical del trazo al mantener presionada la tecla Shift.

- Función preload()
En la función preload() se cargan los archivos SVG que se usarán como módulos visuales. Esto es necesario porque el método loadImage() es asíncrono, y preload() garantiza que los recursos se encuentren completamente cargados antes de iniciar el programa. Las imágenes cargadas se almacenan en las posiciones del arreglo lineModule, del índice 1 al 4. Estas imágenes funcionan como plantillas gráficas que serán colocadas en pantalla cuando se dibuje.

- Función setup()
La función setup() se ejecuta una sola vez al inicio. Aquí se configura el lienzo con createCanvas() usando el tamaño completo de la ventana del navegador. Se establece un fondo blanco con background(255) y se desactiva el cursor usando noCursor(), lo que mejora la experiencia visual al eliminar elementos de interfaz innecesarios. También se define un grosor de línea muy fino con strokeWeight(0.75), lo cual da un trazo elegante y preciso. Finalmente, se define el color inicial del pincel como un tono dorado usando color(181, 157, 0).

- Función windowResized()
Este sketch está diseñado para ocupar siempre toda la ventana del navegador. Por eso, si el usuario cambia el tamaño de la ventana, la función windowResized() redimensiona automáticamente el lienzo con resizeCanvas(windowWidth, windowHeight), asegurando una experiencia fluida y responsiva.

-  Función draw()
La función draw() es el núcleo del comportamiento interactivo. En cada frame, primero se verifica si el mouse está presionado y si el botón presionado es el izquierdo. Si es así, se obtiene la posición actual del mouse (mouseX, mouseY) y se evalúa si el usuario también está presionando la tecla Shift. Si Shift está activo, se fuerza el trazo a alinearse horizontal o verticalmente, dependiendo de qué desplazamiento es mayor. Esto permite dibujar figuras rectas en una sola dirección, agregando control al proceso.

Luego se aplica una transformación para mover el sistema de coordenadas al punto actual del mouse usando translate(x, y) y se gira con rotate() según el ángulo actual. Después, si hay un módulo SVG seleccionado (índice diferente de 0), se dibuja usando image(), aplicando un color de tinte (tint(c)) y el tamaño definido. Si no se ha seleccionado un módulo, se dibuja una simple línea diagonal. Cada vez que se ejecuta el bloque, el ángulo se incrementa según la velocidad definida, generando un efecto de rotación progresiva entre cada módulo.

- Función mousePressed()
Cuando se hace clic con el mouse, esta función se ejecuta y genera un nuevo tamaño aleatorio para el módulo usando random(50, 160). También guarda la posición del clic (clickPosX, clickPosY) para que, si se activa el modo de trazo recto con Shift, el punto inicial se mantenga fijo.

- Función keyPressed()
Esta función permite modificar parámetros del sketch usando el teclado. Las flechas ↑ y ↓ aumentan o disminuyen el tamaño del módulo en 5 unidades. Las flechas ← y → modifican la velocidad de rotación del ángulo, permitiendo al usuario controlar si las figuras giran más rápido o más lento. Estos controles son útiles para afinar detalles mientras se dibuja.

-  Función keyReleased()
Esta función permite realizar acciones adicionales al soltar ciertas teclas. Por ejemplo, presionar s guarda la imagen generada como un archivo .png. Al presionar Backspace o Delete, el fondo se limpia y se borra todo el dibujo. La tecla d invierte la dirección de rotación, lo cual puede generar composiciones más complejas y simétricas.
También es posible cambiar el color actual presionando la barra espaciadora ( ), lo que genera un nuevo color aleatorio con cierto nivel de transparencia. Además, las teclas del 1 al 4 permiten seleccionar colores predefinidos: dorado, azul, púrpura y fucsia. Finalmente, las teclas del 5 al 9 permiten elegir cuál imagen SVG se usará como pincel (o volver a usar una línea simple con el 5).

















