```JS
function setup() {
  createCanvas(400, 400);
  textAlign(CENTER, CENTER);
  textSize(32);
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
    text("BOOOOOM!!!!", width / 2, height / 2);
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
  if (estado === "CONFIG") {
    manejarConfig(key);
  } else if (estado === "ARMADO") {
    manejarArmado(key);
  } else if (estado === "EXPLOSION") {
    manejarExplosion(key);
  }
}
```

para realizar este codigo cree los estados CONFIG, ARMADO y EXPLOSION, ademas de las funciones manejarConfig, ManejarArmado, ManejarExplosion. 
tiene las mismas funciones y acciones de la bomba original, con la modificacion del tiempo: 

```js
if (key === 'A') {
    tiempo = min(60, tiempo + 1);
  } else if (key === 'B') {
    tiempo = max(10, tiempo - 1);
```

la cuenta regresiva: 

```js
if (millis() - lastFrameTime >= 1000 && tiempo > 0) {
      tiempo--;
      lastFrameTime = millis();
```
 y tambien la secuencia de desactivacion, para eso cree las variables ¨seqDesac¨ que contiene la secuencia de desactivacion correcta, y 
 ¨seqPropia¨ que alamenara la secuencia que uno ponga. esta se comprobara en la funcion manejarArmado, verificando que la cantidad de variables
 que uno ponga sea igual a cuatro; si esta cantidad de variables es correcta entonces verifica si son las mismas variables en el orden correcto

```js
if (key === 'A' || key === 'B' || key === 'S') {
    if (seqPropia.length >= 4) {
      seqPropia.shift(); // Elimina el primer elemento si ya hay 4
    }
    seqPropia.push(key);

    if (seqPropia.length === seqDesac.length) {
      if (JSON.stringify(seqPropia) === JSON.stringify(seqDesac))
```
 y para hacer los cambios de estado se hacian condiciones que dependiendo de la variable saltaba a otro estado



