en un principio podriamos pensar que el usuario solo puede crear los trazos con el click, pero revisando mas a profundidad el codigo me di cuenta
que el tamaño del trazo, la velocidad de rotación, el color, y el tipo de trazo (línea o SVG) puede ser modificados atraves del teclado y el mouse.
También permite guardar el lienzo como imagen y borrar el contenido.

#### variables globales 

- c: color actual de trazo o imagen (color).

- lineModuleSize: tamaño del módulo que se dibuja (imagen o línea).

- angle: ángulo actual de rotación.

- angleSpeed: velocidad de rotación.

- lineModule: arreglo que almacena imágenes SVG como módulos visuales.

- lineModuleIndex: índice del módulo visual a usar (0 = línea simple).

- clickPosX, clickPosY: coordenadas del último clic, útiles para restringir movimiento con SHIFT.

#### presenta 7 funciones principales:

- preload(): se ejecuta antes del setup(), carga las imagenes SVG (modulos graficos) antes de que inicie el programa. esto para evitar
 que se dibujen elementos sin haberse cargado completamente. se carga con ¨loadImage()¨, y se guardan en el arreglo ¨lineModule[]¨

- setup(): configura el entorno grafico solo al inicio. crea el lienzo ¨createCanvas(windowWidth, windowHeight)¨, declara el color del fondo
  ¨background(255)¨, oculta el cursor ¨noCursor()¨, establece el grosor de la linea ¨strokeWeight(0.75)¨ y el color inicial ¨c = color(181, 157, 0¨

- windowResized(): rediomensiona el lienzo si se cambia el tamaño de la ventana del navegador. se ajusta al nuevo tamaño con ¨resizeCanvas(windowWidth, windowHeight)¨

- draw(): se llama en cada fotograma, solo dibuja mientras se presiona el botón izquierdo del mouse. Si se mantiene SHIFT al hacer clic,
  restringe el trazo a línea vertical u horizontal según el movimiento predominante, es decir, el usuario puede dibujar en línea recta en
  una dirección específica.

  - el shift hace que el programa compara qué tanto te moviste en X frente a Y. Si el movimiento horizontal (abs(clickPosX - x))
    es mayor que el vertical, entonces fija ¨y = clickPosY¨, bloqueando el trazo en horizontal.Si el movimiento vertical es mayor, entonces
    fija ¨x = clickPosX¨, bloqueando el trazo en vertical.

  - si el mouse esta presionado (mouseIsPressed) se dibujan los elementos en la posicion del mouse. Si lineModuleIndex es diferente de 0, 
    significa que el usuario presionó una tecla del 6 al 9 → se carga una imagen SVG. usa ¨image()¨ para dibujar la imagen como pincel.
    
    Si lineModuleIndex es 0, significa que se eligió la opción de línea (con tecla 5) → se dibuja una línea diagonal como pincel.
    Usa line() para trazar una línea desde (0, 0) hasta (lineModuleSize, lineModuleSize).

  - Usa push() para guardar el estado actual de transformación del canvas (posición, rotación, escala, etc.). luego translate(x, y): Mueve
    el origen al punto donde está el mouse; rotate(): Rota el sistema de coordenadas para dibujar girado en función del valor de angle y
    angleSpeed. y luego dibuja el modulo.
    
  - Después de dibujar cada módulo (imagen o línea), el programa incrementa el ángulo de rotación ¨angle += angleSpeed¨. Esto hace 
    que el próximo dibujo esté un poco más rotado que el anterior, hace que el elemento gire mientras se dibuja.

  - pop() restaura el estado anterior, para que el siguiente dibujo no esté afectado por la rotación o traslación hecha aquí.

- mousePressed(): cuando se hace clic se establece un tamaño aleatorio para el modulo ¨(lineModuleSize)¨ y se guarda la posicion del clic
  para determinar la dirección en la que se dibujará el siguiente elemento.

- keyPressed(): se ejecuta cuando se presiona una tecla. puede cambiar el tamaño del modulo (UP, DOWN), ¨lineModuleSize¨ aumenta o disminuye
  tambien cambia la velocidad de rotacion ¨angleSpeed¨ con (LEFT, RIGHT)

- keyReleased(): se ejecuta cuando la tecla se suelta. sus funciones son:

  S: guarda la imagen como un png con ¨saveCanvas()¨

  DELETE o BACKSPACE: limpia el lienzo con ¨background(255)¨

  D: invierte la dirección del giro y rota 180°, cambia la direccion de ¨angleSpeed *= -1¨

  ESPACIO: cambia a un color aleatorio con la variable ¨c¨ agregandole valores random()

  1–4: cambia entre colores predefinidos.

  5–9: selecciona el tipo de módulo (línea o imagen SVG), es el que asigna un valor diferente a ¨lineModuleIndex¨




