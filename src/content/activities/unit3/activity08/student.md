p5.js

```js
let connectBtn;
let serial;
let port;

function setup() {
  createCanvas(400, 400);
  textAlign(CENTER, CENTER);
  textSize(32);
  
  port = createSerial();
  
  // Botón para conectar al puerto serial
  connectBtn = createButton('Connect to micro:bit');
  connectBtn.position(30, 100);
  connectBtn.mousePressed(connectBtnClick);
}

let tiempo = 20;
let estado = "CONFIG";
let seqDesac = ["A", "B", "A", "S"];
let seqPropia = [];
let lastFrameTime = 0;

function draw() {
  background(220);

  if (estado === "CONFIG") {
    text("CONFIGURACIÓN", width / 2, height / 2 - 50);
    text("Tiempo: " + tiempo, width / 2, height / 2);
    
    if (!port.opened()) {
        connectBtn.html('Connect to micro:bit');
    }
    else {
        connectBtn.html('Disconnect');
    }
    
  } else if (estado === "ARMADO") {
    text("ARMADO", width / 2, height / 2 - 50);
    text("Tiempo: " + tiempo, width / 2, height / 2);

    // disminucion del tiempo
    if (millis() - lastFrameTime >= 1000 && tiempo > 0) {
      tiempo--;
      lastFrameTime = millis();
    }
    if (tiempo <= 0) {
      estado = "EXPLOSION";
    }
  } else if (estado === "EXPLOSION") {
    text("BOOOOOOM!!!!!", width / 2, height / 2);
  }
  
  recibirDato()
  
}

function connectBtnClick() {
    if (!port.opened()) {
        port.open('MicroPython', 115200);
    } else {
        port.close();
    }
}

// Recibe datos del micro:bit
function recibirDato() {
  if(port.availableBytes() > 0){
    let data = port.read(1);
    if (data.length > 0) {
      manejarEvento(data);
    }
  }
}

// Reacciona a evento serial o teclado
function manejarEvento(key) {
  if (estado === "CONFIG") {
    manejarConfig(key);
  } else if (estado === "ARMADO") {
    manejarArmado(key);
  } else if (estado === "EXPLOSION") {
    manejarExplosion(key);
  }
}

function manejarConfig(key) {
  if (key === 'A') {
    tiempo = min(60, tiempo + 1);
  } else if (key === 'B') {
    tiempo = max(10, tiempo - 1);
  } else if (key === 'S') {
    estado = "ARMADO";
    lastFrameTime = millis();
    seqPropia = [];
  }
}

function manejarArmado(key) {
  if (key === 'A' || key === 'B' || key === 'S') {
    if (seqPropia.length >= 4) {
      seqPropia.shift(); // Elimina el primer elemento si ya hay 4
    }
    seqPropia.push(key);

    if (seqPropia.length === seqDesac.length) {
      if (JSON.stringify(seqPropia) === JSON.stringify(seqDesac)) {
        estado = "CONFIG";
        tiempo = 20;
      }
    }
  }
  if (key === 'T') {
    estado = "CONFIG";
    tiempo = 20;
  }
}

function manejarExplosion(key) {
  if (key === 'T') {
    estado = "CONFIG";
    tiempo = 20;
  }
}

function keyPressed() {
  manejarEvento(key);
}
```

para lograr que este codigo tenga una conexion serial con el microbit agregue las variables presentes en las primeras tres lineas del codigo

cree el objeto de conexion serial que vimos en unidades anteriores, creando un boton para conectar. tambien mas adelnate se crea un cambio 
dinamico en el texto del boton para saber si esta conectado o no, esto dentro de ¨if (estado === "CONFIG")¨ ya que es donde el usuario
prepara o ajusta la bomba

tambien se crea la funcion ¨recibirDato()¨ para hacer una lectura de datos desd el micro:bit, el cual se llama en cada draw(), en este
se verifica si hay datos nuevos desde el micro:bit y lee la letra o comando, luego lo envia a ¨manejarEvento()¨¨. 

en la funcion ¨manejarEvento()¨ se decide que hacer con el evento recibido dependiendo del estado en que se encuentre 

micro:bit

```py
from microbit import *

while True:
    if button_a.was_pressed():
        uart.write("A\n")
    if button_b.was_pressed():
        uart.write("B\n")
    if accelerometer.was_gesture("shake"):
        uart.write("S\n")
    if pin_logo.is_touched():
        uart.write("T\n")

    display.show(Image.HEART)
    sleep(100)
```
estos se comunican estre si a traves de la comunicacion serial (UART), y se comunica con el codigo p5js a traves de la funcion ¨recibirDato()¨.
si se presiona un boton en el microbit este creara una relacion con la tecla se haya presionado



