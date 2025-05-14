escogi la aplicacion de Generative Design https://editor.p5js.org/generative-design/sketches/P_2_1_1_03

realiza las modificaciones necesarias a este codigo

```js
let tileCount = 1;
let actRandomSeed = 0;
let colorLeft;
let colorRight;
let alphaLeft = 0;
let alphaRight = 100;
let transparentLeft = false;
let transparentRight = false;

let port;
let connectBtn;
let microBitConnected = false;

const STATES = {
  WAIT_MICROBIT_CONNECTION: "WAIT_MICROBIT_CONNECTION",
  RUNNING: "RUNNING",
};

let appState = STATES.WAIT_MICROBIT_CONNECTION;
let microBitX = 0;
let microBitY = 0;
let microBitAState = false;
let microBitBState = false;
let prevAState = false;
let prevBState = false;

function setup() {
  createCanvas(600, 600);
  colorMode(HSB, 360, 100, 100, 100);

  colorRight = color(0, 0, 0, alphaRight);
  colorLeft = color(323, 100, 77, alphaLeft);
  
  port = createSerial();
  connectBtn = createButton("Connect to micro:bit");
  connectBtn.position(0, 0);
  connectBtn.mousePressed(connectBtnClick);
}

function connectBtnClick() {
  if (!port.opened()) {
    port.open("MicroPython", 115200);
  } else {
    port.close();
  }
}

function updateButtonStates(newAState, newBState) {
  // Detectar si se soltó A (pasó de true a false)
  if (newAState === false && prevAState === true) {
    transparentLeft = !transparentLeft;
    print("Botón A soltado: transparentLeft = " + transparentLeft);
  }

  // Detectar si se soltó B (pasó de true a false)
  if (newBState === false && prevBState === true) {
    transparentRight = !transparentRight;
    print("Botón B soltado: transparentRight = " + transparentRight);
  }

  // Actualizar estados anteriores después de revisar
  prevAState = newAState;
  prevBState = newBState;
}

function draw() {
  clear();

  // COMUNICACIÓN SERIAL CON micro:bit
  if (!port.opened()) {
    connectBtn.html("Connect to micro:bit");
    microBitConnected = false;
  } else {
    connectBtn.html("Disconnect");
    microBitConnected = true;

    if (port.availableBytes() > 0) {
      let data = port.readUntil("\n");
      if (data) {
        data = data.trim();
        let values = data.split(",");
        if (values.length == 4) {
          microBitX = int(values[0]);
          microBitY = int(values[1]);
          microBitAState = values[2].toLowerCase() === "true";
          microBitBState = values[3].toLowerCase() === "true";
          updateButtonStates(microBitAState, microBitBState);
        }
      }
    }
  }
  switch (appState) {
    case STATES.WAIT_MICROBIT_CONNECTION:
      if (microBitConnected === true) {
        print("Microbit ready to draw");
        strokeWeight(0.75);
        noCursor();
        appState = STATES.RUNNING;
      }
      break;

    case STATES.RUNNING:
      if (microBitConnected === false) {
        print("Waiting microbit connection");
        cursor();
        appState = STATES.WAIT_MICROBIT_CONNECTION;
      }

      clear();
      strokeWeight(microBitX / 15);
      randomSeed(actRandomSeed);
      tileCount = microBitY / 15;

      for (let gridY = 0; gridY < tileCount; gridY++) {
        for (let gridX = 0; gridX < tileCount; gridX++) {
          let posX = width / tileCount * gridX;
          let posY = height / tileCount * gridY;

          alphaLeft = transparentLeft ? gridY * 10 : 100;
          colorLeft = color(hue(colorLeft), saturation(colorLeft), brightness(colorLeft), alphaLeft);

          alphaRight = transparentRight ? 100 - gridY * 10 : 100;
          colorRight = color(hue(colorRight), saturation(colorRight), brightness(colorRight), alphaRight);

          let toggle = int(random(0, 2));

          if (toggle === 0) {
            stroke(colorLeft);
            line(posX, posY, posX + (width / tileCount) / 2, posY + height / tileCount);
            line(posX + (width / tileCount) / 2, posY, posX + (width / tileCount), posY + height / tileCount);
          } else {
            stroke(colorRight);
            line(posX, posY + width / tileCount, posX + (height / tileCount) / 2, posY);
            line(posX + (height / tileCount) / 2, posY + width / tileCount, posX + (height / tileCount), posY);
          }
        }
      }
      break;
  }
}

function mousePressed() {
  actRandomSeed = random(100000);
}

function keyReleased() {
  if (key === 's' || key === 'S') saveCanvas(gd.timestamp(), 'png');

  if (key === '1') {
    if (colorsEqual(colorLeft, color(273, 73, 51, alphaLeft))) {
      colorLeft = color(323, 100, 77, alphaLeft);
    } else {
      colorLeft = color(273, 73, 51, alphaLeft);
    }
  }

  if (key === '2') {
    if (colorsEqual(colorRight, color(0, 0, 0, alphaRight))) {
      colorRight = color(192, 100, 64, alphaRight);
    } else {
      colorRight = color(0, 0, 0, alphaRight);
    }
  }

  if (key === '0') {
    transparentLeft = false;
    transparentRight = false;
    colorLeft = color(323, 100, 77, alphaLeft);
    colorRight = color(0, 0, 0, alphaRight);
  }
}

function colorsEqual(col1, col2) {
  return col1.toString() === col2.toString();
}
```

```py
# Imports go at the top
from microbit import *

uart.init(115200)
display.set_pixel(0,0,9)

while True:
    xValue = accelerometer.get_x()
    yValue = accelerometer.get_y()
    aState = button_a.is_pressed()
    bState = button_b.is_pressed()
    data = "{},{},{},{}\n".format(xValue, yValue, aState,bState)
    uart.write(data)
    sleep(100) # Envia datos a 10 Hz
```

codigo de la aplicacion modificada: 
