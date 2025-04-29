codigo en p5.js

```js
let port;
let botonA, botonB, botonS, botonT;

function setup() {
    createCanvas(400, 400);
    background(220);
  
    port = createSerial();
  
    // Botón para conectar al puerto serial
    connectBtn = createButton('Connect to micro:bit');
    connectBtn.position(30, 100);
    connectBtn.mousePressed(connectBtnClick);
  
    botonA = createButton('Botón A');
    botonA.position(30, 30);
    botonA.mousePressed(() => enviarEvento('A'));

    botonB = createButton('Botón B');
    botonB.position(120, 30);
    botonB.mousePressed(() => enviarEvento('B'));

    botonS = createButton('Shake');
    botonS.position(210, 30);
    botonS.mousePressed(() => enviarEvento('S'));

    botonT = createButton('Touch');
    botonT.position(300, 30);
    botonT.mousePressed(() => enviarEvento('T'));
  
    fill('white');

}

function draw() {
  
    background(220);
  
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

function enviarEvento(valor) {
    port.write(valor + '\n');
    console.log('Enviado: ' + valor);
}
```

para este codigo se crearon las variables principales ``let botonA, botonB, botonS, botonT;`` y para hacer que este se conecte con las 
condiciones del microbit se crean cuatro botones virtuales (A, B, S, T), que al presionarse llaman a la funcion enviarEvento() con un 
caracter especifico

en el microbit esta el fragmento 

```
if uart.any():
    data = uart.read(1)
    if data:
        event_occurred = True
        event_value = chr(data[0])
```
gracias a este los caracteres son enviados al micro:bit a traves de UART, linea por linea (port.write(valor + '\n');), 
lo que implica que el micro:bit puede leer uno por uno esos caracteres desde el puerto serial. envia el evento al microbit 

```
botonT.mousePressed(() => enviarEvento('T'));
```

```
function enviarEvento(valor) {
    port.write(valor + '\n');
    console.log('Enviado: ' + valor);
}
```
en este se escribe (envía) un carácter como "A", "B", "S" o "T" al micro:bit



