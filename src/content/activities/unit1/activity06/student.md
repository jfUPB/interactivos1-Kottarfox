#### Solución 

- Describe qué pasa en el punto 15 y cómo crees que esto se logre.
Al presionar A en p5.js se muestra un círculo rojo que dice A y con B pasa lo mismo solo que se vuelve un círculo amarillo que dice B.
En parte con el código sería gracias a esta parte:
```
    if(port.availableBytes() > 0){
      let dataRx = port.read(1);
        if(dataRx == 'A'){
            fill('red');
        }
        else if(dataRx == 'B'){
            fill('yellow');
        }
  ```

- Describe qué pasa en el punto 16 y cómo crees que esto se logre.
El círculo se vuelve verde y dice C ahora. Diría yo que para que funcione tiene que reconocerse que se está haciendo otro tipo de movimiento además de los botones, supongo que a través de un sensor que tiene el mismo micro:bit.
Y bueno en código sería esta parte creo:
 ```
  }
        else{
            fill('green');
        }
   ```

- Describe qué pasa en el punto 17 y cómo crees que esto se logre.
Cuando se presiona el botón de Send Love se muestra por un poco tiempo un corazón en el display y después queda una carita feliz bastante bonita en realidad :).
Y creo que funciona mediante el script con el hace que funcione el micro:bit o también por parte del nombre.. no estoy muy segura.
