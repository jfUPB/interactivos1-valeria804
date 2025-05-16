### enunciado 

#### ¿Cómo es posible estructurar una aplicación usando una máquina de estados para poder atender varios eventos de manera concurrente?

la maquina de estados divide el comportamiento del programa en estados bien definidos. esta la accion de entrada, en la que se muestra la imagen y se reinicia y se restablece el tiempo. una respuesta a eventos, en la que se pulsan los botones.
y una transicion por tiempo, cuando se excede cierto intervalo 

tambien ayuda la existencia de estados definidos 

- STATE_INIT: Inicializa el primer estado mostrando la cara feliz.

- STATE_HAPPY: Muestra la cara feliz, espera 1.5s o una pulsación de botón A.

- STATE_SMILE: Muestra la cara sonriente, espera 1s o una pulsación.

- STATE_SAD: Muestra la cara triste, espera 2s o una pulsación.

la concurrencia ocurre con la evaluacion constante de las acciones que se pueden realñizar dentro de un mismo estado luego de un determinado timepo, la ejecucion de varias acciones en difeententes momentos dentro de un estado:

- Se pregunta todo el tiempo si se presionó el botón A.

- Se mide cuánto tiempo ha pasado desde que se entró al estado.

#### ¿Cómo haces para probar que el programa está correcto?

para comprobar que el programa es correcto se realizan diferente pruebas de ejecucion del programa, realizando todas las acciones que se quiera que se ejecuten,formas de hacer esto es: prueba de ciclo basico, Prueba de eventos (interacción), Pruebas límite
en la que se ejecuta una accion varioas veces para verificar que el programa no se rompa 

### entrega 

#### Explicación donde muestres cómo este programa logra hacer varias cosas a las vez (a esto lo llamamos concurrencia).

Este programa utiliza una máquina de estados finitos (FSM) dentro de un bucle infinito para simular concurrencia. Aunque el micro:bit no tiene múltiples hilos de ejecución, logra hacer múltiples tareas "al mismo tiempo" 
gracias a que en cada iteración del bucle:

Verifica si el botón A fue presionado.

Calcula si ha pasado el tiempo correspondiente en el estado actual.

Cambia de estado según lo que ocurra primero (evento o tiempo).

#### Describe y aplica al menos 3 vectores de prueba para el programa.

- Estructura del vector de prueba

Condición inicial: Estado y visualización actual del sistema.

Evento(s): Pulsación de botón o paso del tiempo.

Resultado esperado: Imagen mostrada, estado siguiente, tiempo de espera.

Resultado obtenido: Observado manualmente en el micro:bit.

Resultado de la prueba: ¿Pasa o falla?

- Vector de prueba 1: Transición automática (sin pulsaciones)

Condición inicial: Estado = STATE_HAPPY (cara feliz), tiempo = 0 ms.

Evento: Esperar 1500 ms sin presionar ningún botón.

Resultado esperado:

Imagen = (SMILE)

Estado siguiente = STATE_SMILE

Tiempo de espera para siguiente transición = 1000 ms

Resultado obtenido: Coincide con lo esperado.

Resultado de la prueba: PASA

- Vector de prueba 2: Interrupción por botón A en estado feliz

Condición inicial: Estado = STATE_HAPPY (cara feliz), dentro del intervalo de 1500 ms.

Evento: Pulsar botón A antes de los 1500 ms.

Resultado esperado:

Imagen cambia inmediatamente a (SAD)

Estado siguiente = STATE_SAD

Tiempo de espera en estado triste = 2000 ms

Resultado obtenido: Coincide con lo esperado.

Resultado de la prueba: PASA

- Vector de prueba 3: Interrupción por botón A en estado triste
Condición inicial: Estado = STATE_SAD, cara triste mostrada.

Evento: Pulsar botón A durante los primeros 2000 ms.

Resultado esperado:

Imagen cambia inmediatamente a (SMILE)

Estado siguiente = STATE_SMILE

Tiempo de espera en sonrisa = 1000 ms

Resultado obtenido: Coincide con lo esperado.

Resultado de la prueba: PASA
















