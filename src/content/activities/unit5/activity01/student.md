- Describe cómo se están comunicando el micro:bit y el sketch de p5.js. ¿Qué datos envía el micro:bit?

  p5.js y el microbit se comunican atravez del puerto serial y se estable con el comando uart.init(115200), enviando  xValue, yValue,
  aState, bState,  como una cadena de texto ASCII separada por comas y terminada con un salto de linea

- ¿Cómo es la estructura del protocolo ASCII usado?

  <xValue>,<yValue>,<aState>,<bState>\n. en este hay valores enteros y valores booleanos

- Muestra y explica la parte del código de p5.js donde lee los datos del micro:bit y los transforma en coordenadas de la pantalla.

  ````
  if (port.availableBytes() > 0) { // Verifica si hay datos disponibles
  let data = port.readUntil("\n"); // lee una linea completa
  if (data) {
    data = data.trim(); // elimina espacios y saltos de linea 
    let values = data.split(","); // divide la linea por valores individuales
    if (values.length == 4) {

      // Convierte los valores a los tipos adecuados: xValue y yValue se convierten a enteros. aState y bState se convierten a booleanos (true o false)
      microBitX = int(values[0]) + windowWidth / 2;
      microBitY = int(values[1]) + windowHeight / 2;
      microBitAState = values[2].toLowerCase() === "true";
      microBitBState = values[3].toLowerCase() === "true";
      updateButtonStates(microBitAState, microBitBState);
    } else {
      print("No se están recibiendo 4 datos del micro:bit");
    }
  }
  }

  ````
  microBitX y microBitY se usan para posicionar elementos en la pantalla con base en los datos del acelerómetro.
  
  microBitX y microBitY son las coordenadas donde se dibujara en la pantalla, ajustando el origen para que este en el centro del canvas.

- ¿Cómo se generan los eventos A pressed y B released que se generan en p5.js a partir de los datos que envía el micro:bit?

  se genera en la funcion updateButtonStates(), comparando el estado actual del boton con su estado anterior, permitiendo detectar cuando
  un boton ha sido presionado o soltado. permitiendo acciones especificas solo cuando hay transición de estado

  ```
  function updateButtonStates(newAState, newBState) {
  // Generar eventos de keypressed
  if (newAState === true && prevmicroBitAState === false) {
    // create a new random color and line length
    lineModuleSize = random(50, 160);
    // remember click position
    clickPosX = microBitX;
    clickPosY = microBitY;
    print("A pressed");
  }
  // Generar eventos de key released
  if (newBState === false && prevmicroBitBState === true) {
    c = color(random(255), random(255), random(255), random(80, 100));
    print("B released");
  }

  prevmicroBitAState = newAState;
  prevmicroBitBState = newBState;
  }
  ```

- Capturas de pantalla de los algunos dibujos que hayas hecho con el sketch.
