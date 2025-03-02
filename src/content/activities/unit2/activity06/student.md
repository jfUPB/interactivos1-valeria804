para hacer esta actividad use el codigo hecho en la actividad 12 de la actividad 1 haciendole cambio de imagenes y agregandole scroll

en p5.js

```js
let port;
let connectBtn;

function setup() {
    createCanvas(400, 400);
    background(220);
    port = createSerial();
    connectBtn = createButton('Connect to micro:bit');
    connectBtn.position(80, 300);
    connectBtn.mousePressed(connectBtnClick);
    let sendBtn = createButton('a');
    sendBtn.position(220, 100);
    sendBtn.mousePressed(sendBtnClick);
    let sendBtn1 = createButton('b');
    sendBtn1.position(220, 150);
    sendBtn1.mousePressed(sendBtnClick1);
    let sendBtn2 = createButton('c');
    sendBtn2.position(220, 200);
    sendBtn2.mousePressed(sendBtnClick2);
    let sendBtn3 = createButton('d');
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

microbit

}

function sendBtnClick3() {
    port.write('c');
}
```

microbit

```py
from microbit import *

uart.init(baudrate=115200)
display.show(Image.BUTTERFLY)

while True:
    if uart.any():
        data = uart.read(1)
        if data:
            if data[0] == ord('h'):
                display.show(Image.PACMAN)
                sleep(500)
            if data[0] == ord('a'):
                display.show(Image.FABULOUS)
                sleep(500)
            if data[0] == ord('b'):
                display.scroll('PUTO xd')                  
            if data[0] == ord('c'):
                display.scroll('ZORRA')    
                
```

en este caso se crea el boton (a, b, c, d) con una posicion, luego cuando se presiona el mouse se llama la funcion sendBtnClick guardandolo con una variable de un digito, ya para los otros botones cree nuevas 
variables y funciones. dentro de cada funcion guarde la accion con nuevos digitos (h,a,b,c)

luego en el mricrobit entendi el uart.any() el cual tiene la funcion de ver si hay algo guardado, luego de que se asegura de que lo este lo lee, verifica que variable es y dependiendo de esta muestra la imagen o scroll
que se halla especificado
