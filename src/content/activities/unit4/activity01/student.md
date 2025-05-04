### opneprocessing
https://openprocessing.org/sketch/2615334 

me llamo la atencion como se genera una distribucion de puntos que generan lineas aleatoria en todo el lienzo, 
creando patrones de formas y colores muy interesantes, me parecio interesante como el proceso fue gradual y no automatico

#### funciones y tecnicas principales

- random(): Genera números aleatorios. Se usa para el radio del círculo, las posiciones de los puntos y para seleccionar índices aleatorios.
- p5.Vector.random2D(): Genera un vector 2D aleatorio, que se usa para calcular una dirección aleatoria desde un punto específico.
- p5.Vector.setMag(): Establece la magnitud de un vector. Esto se usa para ajustar la distancia de los puntos generados.
- floor(): Redondea un número hacia abajo al entero más cercano. Se utiliza para convertir las posiciones en las celdas de la cuadrícula
- dist(): Calcula la distancia entre dos puntos. Se usa para comprobar si dos puntos generados están demasiado cerca.


- Muestro de Poisson: Se implementa el algoritmo de muestreo de Poisson para colocar los puntos de manera que no estén demasiado cerca
 unos de otros, respetando un radio mínimo entre ellos.
- Estructura de cuadrícula: El lienzo se divide en una cuadrícula para facilitar el cálculo de las posiciones de los puntos generados.
  Cada celda de la cuadrícula almacena información sobre la presencia de un punto.

#### https://editor.p5js.org/valeria804/sketches/_--PGZ-QF

lo modifique para que en vez de hacer lineas hechas atra ves de puntos conectados, ahora se usa una ellipse() para dibujar un circulo en 
cada punto generado. no se dibujan lineas entre los puntos 

elimine la parte donde las lineas conectaban los puntos (line(sample.x, sample.y, pos.x, pos.y)), y en su lugar, solo se genera un
círculo. para que los circulos no tuvieran bordes y mantener la estetica se establecio ¨strokeWeight(0)¨. 
fill(color) se usa para darle color al círculo con el mismo color que antes se usaba para las líneas.

y quite la funcion point() ya que los puntos ahora son representados por circulos generados con ellipse().

### Generative Design
http://www.generative-gestaltung.de/2/sketches/?01_P/P_2_1_1_03

este proyecto me llamo la atencion debido a como su forma cambiaba dependiendo de la direccion del mause no solo vertical si no tambien horizontal,
y como los patrones cambian tambien al clikear

#### funciones y tecnicas principales

  - Interacción en tiempo real: el número de celdas y el grosor de las líneas se controlan con la posición del mouse.
  - Aleatoriedad controlada: se usa randomSeed() para que el patrón cambie al hacer clic, pero sea repetible hasta que se haga clic de nuevo.
  - Uso de arreglos 2D implícitos: el patrón se construye recorriendo una cuadrícula con for anidados.
  - Transparencia condicional: los valores alpha cambian dinámicamente si se activan las opciones con teclas (3 y 4).
  - Abstracción con funciones auxiliares como colorsEqual() para comparar colores correctamente.
    
  - draw(): se ejecuta continuamente en bucle. Aquí se dibuja el patrón gráfico en cada cuadro.
  - keyReleased(): reacciona a teclas específicas para cambiar colores o transparencia.
  - clear(): limpia el canvas sin eliminar el fondo.
  - strokeWeight(): cambia el grosor de las líneas.
  - stroke(): define el color del trazo.
  - line(x1, y1, x2, y2): dibuja líneas

#### https://editor.p5js.org/valeria804/sketches/rzcRXzEnq

para el nuevo codigo agregue un cambio de tonalidad colores a medida que pasa el tiempo 

```js
// Dentro de draw()
let hueLeft = (frameCount * 0.5) % 360;
let hueRight = (360 - frameCount * 0.5) % 360;

colorLeft = color(hueLeft, 100, 100, alphaLeft);
colorRight = color(hueRight, 100, 100, alphaRight);
```
quite la funcion keyReleased() y mousePressed(), o sea que las teclas ya no tienen funcion 

hice que se haga solo una de las dos posibles lineas diagonales por celda, antes en cada celda del grid se dibujaban dos líneas por 
diagonal dividida, es decir, se formaban "V" o "Λ" en cada celda con dos trazos por celda dependiendo de un valor aleatorio.

```js
if (int(random(2)) === 0) {
  line(posX, posY, posX + (width / tileCount), posY + (height / tileCount));
} else {
  line(posX + (width / tileCount), posY, posX, posY + (height / tileCount));
}
```
    
### p5.js examples
https://p5js.org/examples/animation-and-variables-conditions/

me llamo la atencio el cambio de colores y la relacion directa con el mouse, ademas de como rebota y siempre sabe a donde dirigirse 
para crear una forma definida 

#### funciones y tecnicas principales

- setup(): se ejecuta una vez al principio. Aquí se crea el canvas, se definen los modos de dibujo (ellipseMode, colorMode),
  se establecen colores de fondo y bordes, y se dibujan áreas fijas en gris en los extremos del lienzo.

- draw(): se ejecuta continuamente en bucle. Aquí ocurre toda la lógica de dibujo del círculo, su movimiento, la animación de color
  y la detección de colisiones con los bordes.

- ellipseMode(RADIUS): define que los valores del radio serán usados para los círculos.

- fill(), stroke(), strokeWeight(): controlan los colores y contornos.

- colorMode(HSB): permite que los colores se manejen con matiz, saturación y brillo.
- mouseIsPressed: una variable booleana que indica si el mouse está presionado.
- Condicionales (if) para controlar el color de relleno y las colisiones del círculo.

- Interacción con el usuario mediante la variable mouseIsPressed.

#### https://editor.p5js.org/valeria804/sketches/fqegsSHVyW

ahora el tamaño cambia cada vez que el círculo toca un borde, mediante la función cambiarTamaño(),
que lo redefine con un valor aleatorio entre 15 y 40.

```js
function cambiarTamaño() {
  circleRadius = random(15, 40);
}
```

si el círculo se encuentra en las zonas grises (≤100 o ≥300 en x), su relleno se vuelve rojo, de lo contrario negro.

```js
if (circlePositionX <= 100 || circlePositionX >= 300) {
  fill(0, 100, 100); // rojo
} else {
  fill(0); // negro
}
```
