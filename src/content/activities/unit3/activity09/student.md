Vectores de prueba
VP1 – Ajuste máximo de tiempo
Desde: CONFIG
Evento: A (muchas veces)
Esperado: Límite de 60 segundos, no aumenta más.
Fuente: botón A físico y botón A desde p5.js.

VP2 – Ajuste mínimo de tiempo
Desde: CONFIG
Evento: B (muchas veces)
Esperado: Límite mínimo de 1 segundo, no disminuye más.
Fuente: botón B físico y botón B desde p5.js.

VP3 – Ingreso normal a COUNT
Desde: CONFIG
Evento: A x3 → T
Esperado: Se pasa a COUNT. (contraseña parcial: "AAA")
Fuente: A y T desde p5.js.
VP4 – Salida desde COUNT con shake
Desde: COUNT
Evento: S
Esperado: Vuelve a CONFIG con tiempo reiniciado (20 s).
Fuente: agitar micro:bit o botón S en p5.js.

VP5 – Contraseña incorrecta 3 veces
Desde: COUNT
Evento: Ingresar secuencias incorrectas
Esperado: Si no acierta antes del timeout, pasa a LOOSE.
Fuente: A/B desde micro:bit o p5.js

VP6 – Contraseña correcta, después spam de A/B
Desde: COUNT
Evento: A, B, A → luego muchos A y B → luego S
Esperado: Llega a WIN por contraseña correcta, luego con S vuelve a CONFIG.
Fuente: desde p5.js.

VP7 – No hacer nada en COUNT
Desde: COUNT
Evento: esperar sin tocar nada, luego justo antes de 0 presionar S
Esperado: vuelve a CONFIG antes de llegar a LOOSE.
Fuente: shake o S desde p5.js.

VP8 – Entrada por INIT correcta
Desde: INIT
Esperado: Solo se ejecuta una vez, pasa a CONFIG.
Validación: bandera en consola y display.

VP9 – Ir a WIN y luego volver a COUNT
Desde: COUNT → WIN
Evento: A, B, A → S → volver a CONFIG → T → COUNT
Esperado: Todo correcto, no salta INIT otra vez.

VP10 – Ir a LOOSE y luego volver a COUNT
Desde: COUNT → esperar timeout → S → volver a CONFIG → T → COUNT
Esperado: funciona normal.

Pruebas de regresión (hechas por el problema original en el vector 6) ahora re-ejecutadas:
VP1 a VP7: todos pasaron correctamente después de la corrección.
VP8 a VP10: agregados como pruebas extendidas, también pasaron.


