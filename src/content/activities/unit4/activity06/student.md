- ¿Qué es un protocolo de comunicación y por qué es importante en la comunicación serial?
  
  es importante ya que determina las reglas de intercambio y recivimiento de datos, sin esto podria no haber una interpretacion correcta
  del los datos transmitidos 
  
- ¿Por qué se separan los datos con comas en el protocolo ASCII que exploramos?

  funcionan como delimitadores y separadores de los datos que recibimos, sin estos la interpretacion de los datos seria mas compleja

- ¿Por qué es necesario terminar los datos con un carácter que marque el fin del mensaje?
  
  con este dato se puede marcar el fin de un paquete completo de datos (readUntil()), y de este modo se puedan procesarlos correctamnete
  
- ¿Por qué fue necesario usar una máquina de estados en la aplicación modificada de p5.js?

  permite gestionar los diferentes eventos segun el estado que determinan si esta o no conectado el microbit

- ¿Cómo se formatean los datos en el micro:bit para ser enviados por el puerto serial?

  se convierte en una cadena de datos separados por comas y terminado en /n para representar el salto de linea. "{},{},{},{}\n"

- ¿Qué significa que los datos enviados por el micro:bit están codificados en ASCII?

  que cada caracter se representa por un numero, esto facilita su interpretacion en distintos dispositivos, p5.js lo convierte a su estado original

- ¿Por qué es necesario en la aplicación de p5.js preguntar si hay bytes disponibles en el puerto serial antes de leerlos?
  
  ```
  if (port.availableBytes() > 0) {
    let data = port.readUntil("\n");
  ```
  ¿Qué pasa si esto no se hace?

  se pregunta si hay datos, para evitar leerlos si no los hay, ya que puede generar errores o bloqueos
  
- ¿Cómo se elimina el retorno de carro o salto de línea de un string en p5.js?

  Usando .trim() para eliminar espacios y caracteres de nueva línea al inicio y al final de la cadena.

- Si una cadena tiene información separada por espacios y quieres dividir dicha información en varias cadenas individuales ¿Qué función de p5.js usarías?

  La función .split(” ”). para extraer cada dato enviado en la cadena 

- Por qué es necesario en la aplicación del caso de estudio convertir las cadenas a números enteros antes de usarlas en el sketch de p5.js?

  Porque los valores recibidos son texto y deben convertirse en números para realizar cálculos y evaluar estados booleanos correctamente

- Si el micro:bit tiene los siguientes datos xValue: 123, yValue: 756, aState: False, bState: True ¿Qué bytes se enviarían por el puerto serial?

  123,756,False,True\n

  la secuencia en bytes seria [49, 50, 51, 44, 55, 53, 54, 44, 48, 44, 49, 10]  en HEX seria [31 32 33 2C 37 35 36 2C 30 2C 31 0A]


- ¿Qué aprendiste nuevo del micro:bit que no sabías antes?

  antes no entendia mucho como crear la conexion seria con p5.js y microbit. y tambien como representar ciertos estados o eventos y como deben
  enviarse para que p5.js lo pueda leer 

- ¿Qué aprendiste nuevo de p5.js que no sabías antes?

  como representar una maquina de estados en p5.js, y una forma de indicar cuando una accion o boton tenga una transicion con updateButtonStates.
  y entendi mejor como funciona la comunicación serial con createSerial() y procesar datos del micro:bit en un sketch interactivo.
