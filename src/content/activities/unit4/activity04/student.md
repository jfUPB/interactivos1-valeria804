- Compara el código original del caso de estudio con el anterior. ¿Qué notas de diferente?
  El código original esta configurado para un proyecto de diseño generativo más amplio, con varias librerias de: tipografia, sonido, color,
  lenguaje natural, interaccion visual.

  el nuevo codigo esta enfocado en la comunicacion con el microbit, simplificandose eliminando dependencias. añade la libreria p5.webserial.js
  que permite recibir datos del micro:bit por puerto serial.

- ¿Para qué se usan estas imágenes? ¿Qué representan? Revisa de nuevo el sketch original y analiza.

  en la palicacion original son usados como pinceles o modulos de linea, usando imagenes SVG, los cuales permiten dibujar diferentes patrones
  con el microbit

- ¿Recuerdas que en las unidades anteriores teníamos un pseudoestado llamado INIT? donde esta

  La función setup() cumple el rol de INIT, realizando la configuración inicial.

- hasta esta parte se definen dos estados, cuyas acciones cambian dependiendo de este, el cambio se permite por la funcion switch().

  Se define una variable appState que indica en qué estado está la aplicación actualmente, en esta parte espera que se conecta el microbit.
  esto organiza el flujo de ejecucion. por el momento ambos casos estan vacios.

  en la siguiente parte se crean variables globales: port, connectBtn y microBitConnected. par que se puedan usar en todo el programa.

  en la siguiente parte se crea la adicion con el puerto serial. primero se crea el objeto que representa la conexion serial; despues se
  crea el objeto boton, la variable connectBtn guardara su direccion. se manipula la posicion del boton. se manipula su comportamiento,
  indicandole que cuando se presione el mause, se llevara a la funcion connectBtnClick()

  en esta funcion se verifica si el puerto serial no esta abierto, si no lo esta se abre con el metodo open(“MicroPython”, 115200),
  si esta abierto y se ejecuta el metodo se cierra 

- ¿Qué pasaría al hacer clic en el botón?

  con el metodo anterior alterna entre abrir (si está cerrado) y cerrar el puerto serial.
  
- ¿Para qué abres el puerto serial?

  abrirlo permite recibir los datos del microbit, los que se vieron en la actividad anterior (posición y estado de botones).

- en la siguiente parte se busca dar a entender al usuario si el microbit esta conectado o no, para esto en la funcion draw(), para
  comprobar que en cada frame se esta comprobando el estado del purto, permitiendo cambiar el texto e informale a la aplicacion el estado
  del puerto a traves del evento microBitConnected.

  al inicio de la aplicacion microBitConnected es falso, pero esto permite que cambie de estado entre false y true. en la funcion draw()
  se pregunta si el puerto esta abirto, si no lo esta muestra el mensaje para conectar el microbit, y si  microBitConnected = true da la
  opcion de desconectar

- solo se leen datos del micro:bit si el puerto está abierto ¿Por qué? ¿Podrías leer datos si el puerto está cerrado? ¿Qué pasaría
 si el puerto está cerrado y el micro:bit envía datos?

 La comunicación solo es posible con una conexión activa, por la tanto no se pueden leer los datos si el puerto esta cerrado, estos datos
 enviados en este estado se pierden porque la computadora no tiene un canal abierto para resivirlos, y si el puerto se abre despues esos 
 datos antiguos no se recuperaran 

- en esta nueva parte, para leer los datos del microbit, la aplicacion verifica primero si hay datos disponibles y espera que llegue una
  linea completa, la función readUntil("\n") sabe que la línea está completa cuando detecta el carácter de fin de línea \n, haciendo esto
  con cada paquete de datos que se envie

- ¿Qué pasaría si el micro:bit no envía el "\n"?

  generaria un error en la lectura: readUntil() esperará indefinidamente, y los datos no se procesarán.

- en la siguiente parte se explica detalladamente el funcionamiento de cada parte agregada. primero se verifica si la linea de datos no esta vacia y
  es valida y se elimina el caracter de salto de linea /n, y los datos separados por comas se dividen en un arreglo, ya que el microbit
  envia los datos de esa forma. este formato (comas para separar y \n para finalizar) constituye un protocolo que ambas aplicaciones
  (micro:bit y p5.js) deben respetar para comunicarse correctamente, por eso se virifica que hallan llegado los cuatro datos.
  luego se convierten los datos codificados en ASCII en numeros enteros, convirtiendo las coordenadas a numeros enteros y
  los estados de los botones A y B a valores booleanos (true o false).

- ¿Por qué se suma windowWidth/2 y windowHeight/2 a los valores de x e y?

  Al sumar windowWidth / 2 y windowHeight / 2, estamos trasladando el punto de referencia al centro de la pantalla. para cuando se envie
  xValue = 0 y yValue = 0 esto este en el centro

- ¿Cómo puedes verificar que los eventos de keypressed y keyreleased se están generando?

  Observar mensajes en la consola (print) y el comportamiento visual de la aplicación, esto se puede lograr agregando algo
  visual en pantalla para cada evento

-  analices el algoritmo updateButtonStates. ¿Qué hace? ¿Por qué es necesario almacenar el estado anterior de los botones?
  ¿Qué pasaría si no se almacenara el estado anterior?
  
  - la funcion updateButtonStates(newAState, newBState) sirve para detectar cambios en los botones A y B del micro:bit y generar
    acciones en respuesta a esos eventos

    Recibe como parámetros los estados actuales (true o false) de los botones A y B leídos desde el micro:bit.

    se crea una condicion donde se detecta un nuevo click, newAState === true, pero que ese click no lo estaba antes
    prevmicroBitAState === false, cuando el boton A es presionado se define un tamaño aleatorio para algo que se va a dibujar (lineModuleSize),
    se guarda la posición del micro:bit en ese instante (clickPosX, clickPosY) y se imprime "A pressed" en la consola

    se crea otra condicion que detecta que el botón B fue soltado newBState === false (estaba presionado en el ciclo anterior y ahora no
    prevmicroBitBState === true). luego se genera un nuevo color aleatorio y se imprime "B released" en la consola.

    por ultimo se actualizan los estados anteriores para que en el próximo ciclo se pueda detectar si hubo un cambio
    
  - almacenar el estado anterior permite detectar cambios de estado y no solo el estado actual, permite detectar el momento excato
    en que cambia y la accion se ejecuta una sola vez. si no se hiciera esto se se sabria
    si el boton acaba de ser presionado o ya llevaba presionado un rato, o sea que solo se sabe si esta presionado o no y puede hacer
    que el evento se ejecute varias veces mientras que este presionado el boton. podria funcionar como un waspressed.

    Por eso, guardar prevmicroBitAState y prevmicroBitBState es clave para una interaccion fluida y precisa con el microbit.

- ¿Qué diferencias encuentras? ¿Qué pasó con algunos eventos del mouse? ¿Qué paso con la función relacionada con la barra de
  espacio del teclado?

  El nuevo código se centra en el control del micro:bit. Eventos del ratón originales fueron reemplazados por botones
  y acelerometro del micro:bit. en este nuevo codigo ya no se usa el mause para dibujar, el evento de dibujo si el botón A del micro:bit
  está presionado (microBitAState === true). las coordenadas se guardan cuando A del micro:bit se presiona.

  ```
  if (microBitAState === true) {
  let x = microBitX;
  let y = microBitY;

  ```

  la funcion de la barra espaciadora fue reemplazada por una logica que asigna un color aleatorio cuando se suelta el boton B del microbit

  mousePressed y keyReleased es reemplazado por updateButtonStates()

  las demas funciones se mantuvieron iguales, como save, deleate, las opciones de numeros y el shift

- Análisis del mensaje en la consola: “No se están recibiendo 4 datos del micro:bit”

Este mensaje significa que la aplicación esperaba recibir cuatro valores separados por comas desde el micro:bit (x, y, a, b), 
pero no está recibiendo los cuatro valores correctamente. esto es porque la función port.readUntil(“\n”) puede estar recibiendo 
líneas de datos incompletas en algunos frames. Aparece ocasionalmente,  indica que en algunos momentos el micro:bit no está enviando
datos completos. para solucionar esto se podria modificar el codigo del microbit y revisar si el microbit está enviando datos 
correctamente sin cortes ni valores faltantes.




  
