el codigo hace que se muestres dos pixeles en el microbit que son controlados, estos cambian su estado de encendido y apagado en función de un intervalo de tiempo específico.

estado: un momento en el que programa esta esperando que ocurra algo

Los eventos son aquellas cosas por las que el programa pregunta durante un estado. son condiciones o situaciones que provocan un cambio de estado o una acción

Las acciones son las actividades que realiza el programa cuando ocurre un evento.

existe la clase pixel. 

- en esta se le da las propiedades. estan las coordenadas pixelX, pixelY
- initState: Es el estado inicial del píxel (0 o 9, donde 0 es apagado y 9 es encendido, dependiendo de la intensidad de la luz).
- interval: Es el tiempo en milisegundos entre los cambios de estado del píxel.
- state: Representa el estado actual del objeto Pixel. Inicialmente se establece en "Init".
- startTime: El tiempo en el que comenzó un estado determinado.
- pixelState: El estado de luminosidad del píxel (0 o 9).

### estados (en el metodo update)

- Init: Este es el estado inicial cuando el objeto Pixel se crea. En este estado, se configura el tiempo de inicio y se cambia al estado WaitTimeout. se guarda el tiempo actual (en milisegundos)
 y se cambia el estado a "WaitTimeout". Además, se configura el píxel en las coordenadas especificadas con el estado inicial.

- WaitTimeout: En este estado, el programa espera que pase el tiempo establecido por el interval. Dependiendo de si ha transcurrido el tiempo, cambia el estado del píxel y reinicia el tiempo de espera.
  En este estado, el programa comprueba si ha transcurrido el tiempo definido por el intervalo (self.interval). Si ha pasado el tiempo, se actualiza el estado del píxel:

Si el estado del píxel es 9 (encendido), se cambia a 0 (apagado).
Si el estado del píxel es 0 (apagado), se cambia a 9 (encendido).
Después de cambiar el estado del píxel, el tiempo se restablece para esperar otro ciclo de tiempo.

pixel1: Controla el píxel en la posición (0, 0) con un intervalo de 1000 milisegundos (1 segundo).

pixel2: Controla el píxel en la posición (4, 4) con un intervalo de 500 milisegundos (0.5 segundos).

### Eventos

- Transcurrir del tiempo: La principal condición que provoca un cambio de estado es que haya pasado el tiempo definido por interval. Esto ocurre cuando el utime.ticks_diff(utime.ticks_ms(), self.startTime) es mayor 
que self.interval.

### Acciones

- Mostrar el píxel con su estado actual: Cada vez que el estado de un píxel cambia, el programa actualiza la visualización del píxel en el display con *display.set_pixel*(self.pixelX, self.pixelY, self.pixelState).
- Cambiar el estado del píxel: Si el estado del píxel es 9 (encendido), se cambia a 0 (apagado), y si es 0 (apagado), se cambia a 9 (encendido).








