```
function setup() {
  createCanvas(400, 400);
  noFill();
  stroke(0);
}
function draw() {
  background(255);
  let time = millis() / 1000;

  // Colores dinámicos
  let r = map(sin(time), -1, 1, 0, 255);
  let g = map(cos(time), -1, 1, 0, 255);
  let b = map(sin(time * 2), -1, 1, 0, 255);
  stroke(r, g, b);

  let radius = 100;
  let numShapes = 10;

  // Generar patrón de círculos
  for (let i = 0; i < numShapes; i++) {
    let angle = TWO_PI / numShapes * i;
    let x = width / 2 + cos(angle + time) * radius;
    let y = height / 2 + sin(angle + time) * radius;
    let s = map(sin(time + i), -1, 1, 10, 50);
    ellipse(x, y, s, s);
  }

  // Generar líneas aleatorias
  stroke(100, 150, 255);
  for (let i = 0; i < 50; i++) {
    let x1 = random(width);
    let y1 = random(height);
    let x2 = random(width);
    let y2 = random(height);
    line(x1, y1, x2, y2);
  }
}
```

el codigo genera patrones visuales que se modifican con el tiempo en base a funciones random() sen() y cos(), visualmente muestra lineas aleatorias que cambian de tamaño y posicion, a demas de 10 circulos 
que giran al rededor del centro, mientras van cambiando su tamaño y color

##### mostrando esto

![image](../../../../assets/circulos.png)

##### sen() cos()

```
let x = width / 2 + cos(angle + time) * radius;
let y = height / 2 + sin(angle + time) * radius;
```

Aquí, sin() y cos() se usan para calcular las coordenadas de los círculos a lo largo de un camino circular

La función cos() devuelve un valor entre -1 y 1, que se utiliza para calcular la posición en el eje X. El ángulo angle determina la dirección en la que se mueve el punto, y el time hace que la posición cambie con el paso del tiempo, haciendo que el círculo se mueva en el eje X de manera dinámica.

sin() devuelve un valor entre -1 y 1 y se usa para calcular la posición en el eje Y. El ángulo y el time son los factores que afectan el movimiento en el eje Y, haciendo que el círculo se desplace en un patrón circular.

width / 2 y height / 2: Estos valores colocan el centro del círculo en el centro de la pantalla, ya que el sistema de coordenadas de p5.js tiene el origen (0, 0) en la esquina superior izquierda.

radius: Este valor multiplica el resultado de cos() y sin(), lo que define el radio del círculo, es decir, qué tan lejos estará el punto del centro del lienzo.

```
let s = map(sin(time + i), -1, 1, 10, 50);
```

sin(time + i) se usa para generar un valor que varía de -1 a 1 en función del tiempo (time) y el índice del ciclo (i). Esto se mapea con la función map() para cambiar el tamaño de los círculos. el índice i agrega una ligera variación a cada forma, de modo que no todos los círculos cambian de tamaño de la misma manera en un solo ciclo de la animación.

La función map() transforma el valor de sin(time + i) de su rango original [-1, 1] a un rango de tamaño [10, 50]

 ##### para los colores 

```
let r = map(sin(time), -1, 1, 0, 255);
let g = map(cos(time), -1, 1, 0, 255);
let b = map(sin(time * 2), -1, 1, 0, 255);
stroke(r, g, b);
```
sin(time) crea una oscilación en el valor de rojo (r).

cos(time) crea una oscilación en el valor de verde (g).

sin(time * 2) aumenta la velocidad de la oscilación para el valor azul (b), creando un cambio de color más dinámico.

##### para las lineas 

```
stroke(100, 150, 255);
for (let i = 0; i < 50; i++) {
  let x1 = random(width);
  let y1 = random(height);
  let x2 = random(width);
  let y2 = random(height);
  line(x1, y1, x2, y2);
}
```

El código genera 50 líneas en posiciones aleatorias dentro del lienzo. Las coordenadas de los puntos de inicio (x1, y1) y de finalización (x2, y2) son seleccionadas aleatoriamente utilizando random(width) y random(height).


























