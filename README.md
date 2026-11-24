# Práctica 5. Biometría: Detección y caracterización de caras

## Filtro 1: Rayos láser

Consiste en hacer que el usuario dispare rayos láser por los ojos.

### Inspiración
Se ha tomado como inspiración el siguiente filtro:

![filtro](imgs/jonkler.gif)


### Trabajo realizado

#### Detección de la cara

Se ha usado la `face_landmarker.task` de *mediapipe* para la detección de caras.
En concreto, ha sido necesario detectar las siguientes *landmarks*:
- Las pupilas, para marcar el punto de inicio de los rayos.
- La punta de la nariz, para calcular la inclinación de la cabeza.

La función `create_detector()` se encarga de crear el detector a partir de una configuración.
Esta configuración es detectar una única cara en un vídeo
(`running_mode = vision.RunningMode.VIDEO`, `num_faces = 1`).

Por último, la posición de los *landmarks* detectados se normaliza a las proporciones de la imagen con la función `get_face_points()`.

Una vez detectada la cara, se procede al dibujado de los rayos.

### Dibujado de los rayos
Para dibujar los rayos láser, es necesario calcular la dirección en la que irán.
Esta dependerá de la dirección en la que apunte la cabeza.

La dirección de los rayos será la combinación del vector normal al que une los ojos y al que une el punto medio de este último con la nariz, sumada a un ángulo de abertura.

Al cálculo de la dirección del rayo se le aplica también un suavizado,
de modo que el movimiento de los mismos sea más natural,
evitando saltos y parpadeos.

Es necesario calibrar el filtro a la cara detectada para el correcto cálculo de la dirección.
Para esto, una vez detectada la cara, se debe presionar la tecla `c`
mirando hacia la cámara.


El dibujado se compone de:
- un rayo láser partiendo de cada ojo
- un punto brillante en cada ojo
- un haz de luz difusa alrededor de cada elemento

La intensidad de la luz difusa varía con el tiempo.

Una vez calculada la posición y la intensidad de los rayos,
estos se dibujan con la función `draw_beams()`.
Esta función se encarga de calcular el color de los rayos y la luz difusa, que sufren una ligera variación en el tiempo;
así como el tamaño final de los mismos.

Los rayos y la luz difusa se dibujan sobre matrices de ceros,
que luego se suma a la imagen captada por la cámara.

