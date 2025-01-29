p5.js

``` js
let port;
let connectBtn;

function setup() {
    createCanvas(400, 400);
    background(220);
    port = createSerial();
    connectBtn = createButton('Connect to micro:bit');
    connectBtn.position(80, 300);
    connectBtn.mousePressed(connectBtnClick);
    let sendBtn = createButton('Send Love');
    sendBtn.position(220, 100);
    sendBtn.mousePressed(sendBtnClick);
    let sendBtn1 = createButton('Send Skull');
    sendBtn1.position(220, 150);
    sendBtn1.mousePressed(sendBtnClick1);
    let sendBtn2 = createButton('Send Sad');
    sendBtn2.position(220, 200);
    sendBtn2.mousePressed(sendBtnClick2);
    let sendBtn3 = createButton('Send Duck');
    sendBtn3.position(220, 250);
    sendBtn3.mousePressed(sendBtnClick3);
    fill('white');
    
}

function draw() {

    if(port.availableBytes() > 0){
        let dataRx = port.read(1);
        
        background(220);
        
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

function sendBtnClick() {
    port.write('h');
}

function sendBtnClick1() {
    port.write('a');
}

function sendBtnClick2() {
    port.write('b');
}

function sendBtnClick3() {
    port.write('c');
}
```
microbit

``` py
from microbit import *

uart.init(baudrate=115200)
display.show(Image.BUTTERFLY)

while True:
    if uart.any():
        data = uart.read(1)
        if data:
            if data[0] == ord('h'):
                display.show(Image.HEART)
                sleep(500)
            if data[0] == ord('a'):
                display.show(Image.SKULL)
                sleep(500)
            if data[0] == ord('b'):
                display.show(Image.SAD)
                sleep(500)
            if data[0] == ord('c'):
                display.show(Image.DUCK)
                sleep(500)
                
```

para esta actividad use como base el codigo dado anteriormente 

primero cree los cuatro botones que tengan la informacion de las imagenes usando este como base 

```
let sendBtn = createButton('Send Love');
    sendBtn.position(220, 100);
    sendBtn.mousePressed(sendBtnClick);
```

junto a la funcion 

```
function sendBtnClick() {
    port.write('h');
}
```

en este caso se crea el boton send love con una posicion, luego cuando se presiona el mouse se llama la funcion sendBtnClick guardandolo con una variable de un digito, ya para los otros botones 
cree nuevas variables y funciones. dentro de cada funcion guarde la accion con nuevos digitos (h,a,b,c)

luego en el mricrobit entendi el uart.any() el cual tiene la funcion de ver si hay algo guardado, luego de que se asegura de que lo este lo lee, verifica que variable es y dependiendo de esta
muestra la imagen que se halla especificado 

```
if data[0] == ord('h'):
                display.show(Image.HEART)
                sleep(500)
```
















