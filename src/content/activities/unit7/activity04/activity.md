#### Análisis del cliente móvil (`mobile/sketch.js`)

:::note[🎯 Enunciado]
Ahora nos centraremos en el código que se ejecuta en tu celular. Analizaremos cómo `mobile/sketch.js` captura los eventos táctiles, los formatea y los envía al servidor Node.js a través de la conexión Socket.IO establecida por Dev Tunnels.
:::

:::tip[Recursos]
- El archivo `mobile/sketch.js` y `mobile/index.html` del caso de estudio.
- La explicación sobre JSON y eventos táctiles de la actividad 02.
- La URL de Dev Tunnels que configuraste.
- Referencia de p5.js: `touchMoved()`, `mouseX`, `mouseY`, `abs()`.
- Referencia de Socket.IO Client API: `io()`, `socket.on()`, `socket.emit()`, `socket.connected`.
:::

👣 **Pasos**: (Análisis del código)

```html
<!-- mobile/index.html (fragmento relevante) -->
<script src="https://cdn.jsdelivr.net/npm/p5@1.11.0/lib/p5.min.js"></script>
<script src="https://cdn.socket.io/4.7.5/socket.io.min.js"></script>
<script src="sketch.js"></script>
```	

:::note[🧩 Explicación (HTML Setup)]

El index.html incluye las librerías necesarias: p5.js para dibujar y manejar eventos, la librería cliente de Socket.IO para la comunicación en red, y nuestro propio sketch.js.
:::

``` js  
// mobile/sketch.js
let socket; // Variable para guardar la conexión Socket.IO
let lastTouchX = null; // Última coordenada X enviada
let lastTouchY = null; // Última coordenada Y enviada
const threshold = 5;   // Umbral de movimiento mínimo para enviar
```

:::note[🧩 Explicación (Variables globales)]

socket: almacenará el objeto de conexión Socket.IO.

lastTouchX, lastTouchY: guardan la posición del último toque enviado al servidor. Se usan para calcular el desplazamiento.

threshold: define cuántos píxeles debe moverse el dedo (en x o y) antes de que consideremos que es un movimiento "significativo" y enviemos una actualización.
:::

``` js	
function setup() {
    createCanvas(windowWidth, windowHeight);
    background(220);

    // Conectar al servidor de Socket.IO
    //let socketUrl = 'http://localhost:3000';
    socket = io();

    socket.on('connect', () => {
        console.log('Connected to server');
    });

    socket.on('message', (data) => {
        console.log(`Received message: ${data}`);
    });

    socket.on('disconnect', () => {
        console.log('Disconnected from server');
    });

    socket.on('connect_error', (error) => {
        console.error('Socket.IO error:', error);
    });
}
```

:::note[🧩 Explicación (setup)]

* createCanvas(windowWidth, windowHeight): Crea un canvas p5.js que llena la pantalla del móvil.

* socket = io();: inicia la conexión al servidor Socket.IO en la URL especificada.

* socket.on(...): se configuran oyentes (event listener) para eventos estándar de Socket.IO:

    * connect: se dispara cuando la conexión es exitosa. Imprime el ID asignado por el servidor.

    * message: aunque este cliente no espera recibir mensajes del tipo 'message' del servidor en esta aplicación, es buena práctica tener un oyente por si acaso o para depuración.

    * disconnect: se dispara si la conexión se pierde.

    * connect_error: se dispara si hay problemas al intentar conectar (URL incorrecta, servidor caído, problema de red, etc.).
:::

``` js
function draw() {
    background(220);
    fill(255, 128, 0);
    textAlign(CENTER, CENTER);
    textSize(24);
    text('Touch to move the circle', width / 2, height / 2);
}
```

:::note[🧩 Explicación (draw)]

Una función draw muy simple. Limpia el fondo y muestra un texto instructivo. No realiza ninguna animación o dibujo complejo, ya que su función principal es capturar la entrada táctil.
:::

``` js

function touchMoved() {
    // Solo intentar enviar si la conexión está activa
    if (socket && socket.connected) { 
        // Calcular cuánto se movió desde la última vez que enviamos
        let dx = abs(mouseX - lastTouchX);
        let dy = abs(mouseY - lastTouchY);

        // Si el movimiento (en x O en y) supera el umbral...
        if (dx > threshold || dy > threshold) {
            // 1. Crear el objeto de datos
            let touchData = {
                type: 'touch', // Identificador del tipo de mensaje
                x: mouseX,     // Coordenada X actual del toque
                y: mouseY      // Coordenada Y actual del toque
            };

            // 2. Convertir el objeto a una cadena JSON
            let messageString = JSON.stringify(touchData);
            
            // 3. Enviar la cadena JSON al servidor con el nombre de evento 'message'
            socket.emit('message', messageString);
            // console.log('Mobile sent:', messageString); // Descomentar para depuración

            // 4. Actualizar las últimas coordenadas enviadas
            lastTouchX = mouseX;
            lastTouchY = mouseY;
        }
    }
    // Prevenir comportamientos táctiles por defecto del navegador (scroll, zoom)
    return false; 
}
```
:::note[🧩 Explicación (touchMoved)]

Esta función se ejecuta automáticamente por p5.js cada vez que el dedo se mueve sobre el canvas mientras está presionado.

``if (socket && socket.connected)``: verifica que la conexión Socket.IO esté establecida antes de intentar enviar algo.

``let dx = ...; let dy = ...;``: calcula la distancia absoluta (abs()) recorrida en X e Y desde la última posición enviada (lastTouchX, lastTouchY).

``if (dx > threshold || dy > threshold)``: comprueba si el movimiento superó el umbral en cualquier dirección.

Si se superó el umbral:

``let touchData = {...}``: crea un objeto JavaScript limpio con la información relevante (type, x, y). mouseX y mouseY contienen las coordenadas del toque dentro de touchMoved.

``let messageString = JSON.stringify(touchData)``: convierte el objeto a formato JSON string.

``socket.emit('message', messageString)``: envía el mensaje al servidor. El nombre del evento es 'message', y los datos son la cadena JSON.

``lastTouchX = mouseX; lastTouchY = mouseY;``: actualiza las variables para la próxima comparación en touchMoved.

``return false;``: es importante para indicarle al navegador que no realice sus acciones táctiles por defecto (como intentar hacer scroll en la página) cuando tocamos el canvas de p5.js.
:::

:::note[🧐🧪✍️ Reporta en tu bitácora]

¿Por qué es importante verificar socket && socket.connected antes de llamar a socket.emit()?

Explica la lógica del threshold y las variables lastTouchX, lastTouchY. ¿Qué problema soluciona esta lógica? ¿Qué pasaría si enviáramos un mensaje en cada frame de touchMoved sin este umbral?

Describe los 4 pasos clave que ocurren dentro del if de touchMoved cuando se detecta un movimiento significativo.

¿Qué hace JSON.stringify() y por qué es necesario antes de socket.emit()?

¿Cuál es el propósito de return false; al final de touchMoved()? Intenta comentarlo y observa qué pasa en el navegador de tu móvil al tocar y arrastrar.
:::

:::caution[📤 Entrega]
Documenta en tu bitácora el análisis del cliente móvil (mobile/sketch.js), explicando las funciones setup, draw y especialmente touchMoved. Responde a las preguntas de reflexión y documenta el experimento con return false;.
:::

