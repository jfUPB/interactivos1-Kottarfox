El código logra la concurrencia mediante el uso de condicionales if y else, que funcionan como semáforos para determinar qué camino debe seguir el código. Es crucial implementarlos para asegurar que el flujo del programa esté bien definido. Además, cada estado debe ser llamado correctamente, y el estado anterior debe limpiarse al entrar en el nuevo. Para lograr un flujo adecuado, se debe evitar que el código se quede esperando a que otro estado termine, asegurando que siempre se pueda acceder a un estado de manera concurrente. Finalmente, un bucle while que se ejecute de manera continua garantiza que el código siga siendo cíclico, manteniendo la máquina de estados activa si esa es la intención del diseño.

Vectores de prueba:
1. Si el programa pasa tres estados seguidos → Presionar A → Presionar A → Dejar correr durante un estado. Resultados esperados: Happy, Smile, Sad, Smile, Happy, Smile.
2. Si el programa pasa dos estados seguidos → Presionar A → Presionar A → Presionar A → Presionar A. Resultados esperados: Happy, Smile, Happy, Sad, Smile, Happy.
3. Si el programa pasa dos estados seguidos → Presionar A → Dejar correr durante cuatro estados → Presionar A. Resultados esperados: Happy, Smile, Happy, Smile, Sad, Happy, Sad.

Resultados obtenidos:
Los tres vectores logran pasar la prueba.
