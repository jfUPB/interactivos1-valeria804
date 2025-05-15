enlace a la aplicacion original: https://editor.p5js.org/generative-design/sketches/P_2_1_1_03 

código de p5.js de la version modificada: 

```js
let serialBuffer = []; // Buffer para almacenar bytes recibidos

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

function readSerialData() {
  // Acumula los bytes recibidos en el buffer
  let available = port.availableBytes();
  if (available > 0) {
    let newData = port.readBytes(available);
    serialBuffer = serialBuffer.concat(newData);
  }
  
  // Procesa el buffer mientras tenga al menos 8 bytes (tamaño de un paquete)
  while (serialBuffer.length >= 8) {
    // Busca el header (0xAA)
    if (serialBuffer[0] !== 0xaa) {
      serialBuffer.shift(); // Descarta bytes hasta encontrar el header
      continue;
    }
    
   // Si hay menos de 8 bytes, espera a que llegue el paquete completo
    if (serialBuffer.length < 8) break;

    // Extrae los 8 bytes del paquete
    let packet = serialBuffer.slice(0, 8);
    serialBuffer.splice(0, 8); // Elimina el paquete procesado del buffer
    
    // Separa datos y checksum
    let dataBytes = packet.slice(1, 7);
    let receivedChecksum = packet[7];
    // Calcula el checksum sumando los datos y aplicando módulo 256
    let computedChecksum = dataBytes.reduce((acc, val) => acc + val, 0) % 256;

    if (computedChecksum !== receivedChecksum) {
      console.log("Checksum error in packet");
      continue; // Descarta el paquete si el checksum no es válido
    }
    
    // Si el paquete es válido, extrae los valores
    let buffer = new Uint8Array(dataBytes).buffer;
    let view = new DataView(buffer);
    microBitX = view.getInt16(0) + windowWidth / 2;
    microBitY = view.getInt16(2) + windowHeight / 2;
    microBitAState = view.getUint8(4) === 1;
    microBitBState = view.getUint8(5) === 1;
    updateButtonStates(microBitAState, microBitBState);

  }
}

function draw() {
  clear();

  // COMUNICACIÓN SERIAL CON micro:bit
  if (!port.opened()) {
    connectBtn.html("Connect to micro:bit");
    microBitConnected = false;
  } else {
    microBitConnected = true;
    connectBtn.html("Disconnect");
  }
  
  
  switch (appState) {
    case STATES.WAIT_MICROBIT_CONNECTION:
      if (microBitConnected === true) {
        print("Microbit ready to draw");
        strokeWeight(0.75);
        noCursor();
        port.clear();
        prevmicroBitAState = false;
        prevmicroBitBState = false;
        appState = STATES.RUNNING;
      }
      break;

    case STATES.RUNNING:
      if (microBitConnected === false) {
        print("Waiting microbit connection");
        cursor();
        appState = STATES.WAIT_MICROBIT_CONNECTION;
        break;
      }
      
      //EVENTO: recepción de datos seriales del micro:bit

      readSerialData();

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
  if (key === 's' || key === 'S') {
    let ts =
      year() +
      nf(month(), 2) +
      nf(day(), 2) +
      "_" +
      nf(hour(), 2) +
      nf(minute(), 2) +
      nf(second(), 2);
    saveCanvas(ts, "png");
  }


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
codigo microbit 

```py
from microbit import *
import struct

uart.init(115200)
display.set_pixel(0, 0, 9)

while True:
    xValue = accelerometer.get_x()
    yValue = accelerometer.get_y()
    aState = button_a.is_pressed()
    bState = button_b.is_pressed()
    # Empaqueta los datos: 2 enteros (16 bits) y 2 bytes para estados
    data = struct.pack('>2h2B', xValue, yValue, int(aState), int(bState))
    # Calcula un checksum simple: suma de los bytes de data módulo 256
    checksum = sum(data) % 256
    # Crea el paquete con header, datos y checksum
    packet = b'\xAA' + data + bytes([checksum])
    uart.write(packet)
    sleep(100)  # Envía datos a 10 Hz
```
en lace al codigo modificado: https://editor.p5js.org/valeria804/sketches/F7i72IMZz 

