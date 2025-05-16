#### ¿Qué información se está enviando? ¿Cómo se está enviando? Qué significa esta parte del código: 


Se estan enviando cuatro datos: xValue: inclinación en el eje X del acelerómetro. yValue: inclinación en el eje Y del acelerómetro.
aState: estado del botón A. bState: estado del botón B.

esto se envia atraves de UART(115200)la comunicacion serial

esta parte del codigo: ```"{},{},{},{}\n".format(xValue, yValue, aState,bState)```. es una cadena de texto con valores separados por comas 
y terminados en un salto de línea. {} se remplaza con los valores de xValue, yValue, aState, y bState, y los valores son separados por comas.
El \n indica un salto de línea, para que cada paquete de datos vaya en una línea nueva. esto genera un string con los valores de los sensores
y botones en una cadena. en la estructura se muestra primero los dos valores del acelerometro, y luego muestra si los estados a y b estan 
siendo ejecutados.

#### ¿Por qué se separan los datos con comas y se termina con un salto de línea?

se usan las comas para delimitar los datos y el salto de línea organiza cada conjunto de datos en una nueva línea; esto con el objetivo de 
facilitar el analisis

#### ¿Qué crees que pasaría si no se separan los datos con comas y no terminan con un salto de línea?

sin estos elementos seria mas dificil separar los diferentes datos, lo caul haria que sean mas dificiles de interpretar 

####Para qué crees que se usa la función sleep(100)? ¿Qué pasaría si no se usara?

hace que el microbit envie los datos cada 100 ms, es decir, 10 veces por segundo, esto evita que se sature el canal UART enviando 
demasiados datos. sin eso el microbit enviaria datos a maxima velocidad y podria bloquear la conexion 

#### Observa cómo cambian los valores de xValue y yValue a medida que el micro:bit se inclina hacia la izquierda, derecha, 
adelante y atrás. ¿Qué valores toman xValue y yValue en cada caso?

Valores de xValue y yValue: Los valores cambian según la inclinación del micro:bit, con xValue variando al inclinarse
hacia los lados (positivo si se inclina hacia la derecha, negativo si se inclina a la izquierda) y yValue al inclinarse 
hacia adelante o atrás (positivo si se inclina hacia adelante, negativo si se inclina hacia atrás). ambos toman valores cercanos a cero
cuando esta plano

#### ¿Qué valores toman aState y bState cuando presionas los botones A y B?

Valores de aState y bState: Son True si los botones A o B están presionados, y False si no lo están.

#### Observa qué ocurre si en vez de is_pressed() usas was_pressed(). ¿Qué diferencias encuentras?

la principal diferencia es que is-pressed() devuelve True una sola vez si el botón fue presionado desde el ciclo anterior,
mientras que is_pressed() devuelve true si el valor esta siendo presionado

#### si el micro:bit tiene los siguientes datos xValue: 969, yValue: 652, aState: True, bState: False ¿Qué bytes se enviarían por el puerto serial? 

haciendo la transformacion de los caracteres ```"969,652,1,0\n"``` a HEX tendriamos: 39 36 39 2C 36 35 32 2C 31 2C 30 0A








