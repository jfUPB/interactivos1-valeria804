p5.js

```js
function setup() {
    createCanvas(400, 400);
    background(220);
    port = createSerial();
    connectBtn = createButton('Connect to micro:bit');
    connectBtn.position(130, 260);
    connectBtn.mousePressed(connectBtnClick);

    let sendBtn = createButton('key');
    sendBtn.position(40, 300);
    sendBtn.mousePressed(() => sendData(key));



  
}

function draw() {
    if (port.availableBytes() > 0) {
        let dataRx = port.read(1);
        background(220);
        fill('black');
        textSize(32);
        textAlign(CENTER, CENTER);
        text(dataRx, width / 2, height / 2);
    }

    if (!port.opened()) {
        connectBtn.html('Connect to micro:bit');
    } else {
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

function sendData(key) {
    port.write(key);
}
```

microbit

```py
from microbit import *
import utime

while True:
    if uart.any():
        data = uart.read(1)
        if data:
            char = data.decode('utf-8')
            display.show(char)
            utime.sleep(1)
            display.clear()
```

en este programa use la variable key para que guardara la variable del teclado que se haya presionado y lo mandara al microbit 

Se realizo la función sendData para manejar diferentes caracteres de forma dinámica.

cuando inicia la aplicacion hay un boton que va a almacenar la informacion de la tecla y en el microbit se va a mandar la variable char. entonces la aplicacion funciona de tal forma en que presiona una tecla y para que esta se muestre en la pantalla LED se presiona el boton key para mandar la informacion 
