|estado actual|Evento|	fuente |Resultado esperado| resultado |
| --- | --- | --- | --- | --- |
|CONFIG|A|teclado|aumenta el tiempo en 1 hasta 60|[X]|
|CONFIG|A|microbit|aumenta el tiempo en 1 hasta 60|[X]|
|CONFIG|B|teclado|disminuye el tiempo en 1 hasta 10|[X]|
|CONFIG|B|microbit|disminuye el tiempo en 1 hasta 10|[X]|
|CONFIG|S|teclado o microbit|	estado = "ARMADO" y tiempo inicia conteo|[X]|
|CONFIG|T|teclado o microbit|	accion ignorada |[X]|

|estado actual|Evento|	fuente |Resultado esperado| resultado |
| --- | --- | --- | --- | --- |
|ARMADO|A|teclado o microbit|Agrega a seqPropia|[X]|
|ARMADO|B|teclado o microbit|Agrega a seqPropia|[X]|
|ARMADO|S|teclado o microbit|Agrega a seqPropia|[X]|
|ARMADO|Secuencia [“A”, “B”, “A”, “S”]|teclado o microbit|secuencia correcta, cambia a CONFIG y reinicia tiempo|[X]|
|ARMADO|Secuencia incorrecta|teclado o microbit|	sigue en estado ARMADO|[X]|
|ARMADO|T|teclado o microbit|		estado = "CONFIG" y tiempo = 20 |[X]|
|ARMADO|Tiempo llega a 0|tiempo llega a 0|sin fuente |[X]|

|estado actual|Evento|	fuente |Resultado esperado| resultado |
| --- | --- | --- | --- | --- |
|EXPLOSION|T|teclado o microbit|estado = "CONFIG" y tiempo = 20|[X]|
|EXPLOSION|cualuier otra variable|teclado o microbit|sin efecto|[X]|

|estado actual|Evento|	fuente |Resultado esperado| resultado |
| --- | --- | --- | --- | --- |
|1. ARMADO|Desconexión serial|sin fuente|sincronicidad mantenida, sigue funcionando desde el microbit|[]|
|2. CONFIG|conexion y desconexion serial|sin fuente|solo puede realizar esta accion en este estado|[]|
|3. EXPLOSION|Desconexión serial|sin fuente|sincronicidad mantenida, sigue funcionando desde el microbit|[]|

### pruebas de regresion 

antes se podia conectar y desconectar el microbit sin importar el estado ya que no habia una verificacion del estado en la funcion ¨connectBtnClick¨, para deshabilitar esta funcion en otros estados agregue un condicional para que verifique en que estado se encuentra y dependiendo si es o no CONFIG permite el cambio

```js
function connectBtnClick() {
  if (estado === "CONFIG") {
    if (!port.opened()) {
      port.open('MicroPython', 115200);
    } else {
      port.close();
    }
  } 
}
```
tambien para indicar en los estados que estada deshabilitado, agregue en la funsion draw() dentro de los estados ARMADA y EXPLOSION ¨connectBtn.attribute('disabled', ''); // Deshabilitar el botón¨ que deshabilita visualmente la funcion. y dentro del estado CONFIG ¨connectBtn.removeAttribute('disabled'); // Habilita el botón¨

### codigo en p5.js modificado }

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
    
    connectBtn.removeAttribute('disabled'); // Habilitar el botón
    
    if (!port.opened()) {
        connectBtn.html('Connect to micro:bit');
    }
    else {
        connectBtn.html('Disconnect');
    }
    
  } else if (estado === "ARMADO") {
    text("ARMADO", width / 2, height / 2 - 50);
    text("Tiempo: " + tiempo, width / 2, height / 2);
    
    connectBtn.attribute('disabled', ''); // Deshabilitar el botón

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
    
    connectBtn.attribute('disabled', ''); // Deshabilitar el botón
    
  }
  
  recibirDato()
  
}

function connectBtnClick() {
  if (estado === "CONFIG") {
    if (!port.opened()) {
      port.open('MicroPython', 115200);
    } else {
      port.close();
    }
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


