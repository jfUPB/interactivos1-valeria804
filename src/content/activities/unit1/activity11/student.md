 p5.js
 
```js
let port;
let connectBtn;
let xPos;

function setup() {
    createCanvas(400, 400);
    background(220);
    port = createSerial();
    connectBtn = createButton('Connect to micro:bit');
    connectBtn.position(80, 300);
    connectBtn.mousePressed(connectBtnClick);
    xPos = width / 2;
    fill('white');
    ellipse(xPos, height / 2, 100, 100);
}

function draw() {

    if(port.availableBytes() > 0){
        let dataRx = port.read(1);
        if(dataRx == 'A'){
             xPos -= 10;
        }
        else if(dataRx == 'B'){
            xPos += 10;
        
        }
      
      xPos = constrain(xPos, 50, width - 50);
      
        background(220);
        ellipse(xPos, height / 2, 100, 100);
        fill('black');
        text(dataRx, width / 2, height / 2);
    }


    if (!port.opened()) {
        connectBtn.html('Connect to micro:bit');
    }
    else {
        connectBtn.html('Disconnect');
    }
}

function connectBtnClick() {
    if (!port.opened()) {
        port.open('MicroPython', 115200);
    } else {
        port.close();
    }
}
```
microbit

```py
from microbit import *

uart.init(baudrate=115200)
display.show(Image.BUTTERFLY)

while True:
    if button_a.is_pressed():
        uart.write('A')
        sleep(500)
    if button_b.is_pressed():
        uart.write('B')
        sleep(500)
```

para esto primero cree la variable xPos para almacenar la posicion horizontal; y se inicializo la posicion en el centro. se cambio las caracteristicas del circulo quitando "width / 2;" dejandolo como "ellipse(xPos, height / 2, 100, 100);" 
deste modo se puede modificar la posicion en x.

Cuando se recibe la letra 'A', la posición del círculo en el eje x se mueve 10 píxeles a la izquierda (xPos -= 10), y cuando se recibe 'B', se mueve 10 píxeles a la derecha (xPos += 10).

y por ultimo se agrego la función constrain(xPos, 50, width - 50) para asegurarse de que el círculo no se salga de los limites 






