- Captura el resultado del experimento anterior, Mostrar datos como: texto. ¿Por qué se ve este resultado?

  se ven caracteres ilegibles en la pantalla, esto porque el metodo ``struct.pack('>2h2B', ...)`` codifica los datos en formato binario, no
  como texto. lo que pasa es que los datos son enviados como bytes crudos, no como una cadena de texto o numero, entonces se envian bytes binarios
  
- Captura el resultado del experimento anterior, mostrar datos como: HEX. Lo que ves ¿Cómo está relacionado con esta línea de código? ``data = struct.pack('>2h2B', xValue, yValue, int(aState), int(bState))``

  se ve una secuencia de bytes expresada en valores hexadecimales, cada par de digitos representa un byte en hexadecimal. con el codigo se
  relaciona con lo que se ve en el monitor serial, ya que la parte de 2h empaqueta dos enteros, xValue y yValue, cada uno
  ocupa 2 bytes. y 2B dos enteros, int(aState) y int(bState), cada uno ocupa 1 byte. o sea que en total se enviarian 6 bytes por paquete
  enviado

  este es mas dificil que leer el ASCII ya que puede resultar difícil de interpretar datos binarios sin herramientas o conocimiento del formato.

- ¿Qué ventajas y desventajas ves en usar un formato binario en lugar de texto en ASCII?

  - ventajas
    usa menos bytes para representar los mismos datos, lo cual permite que ahorre el ancho de banda. al ocupar menos espacio, los datos
    viajan mas rapido, es util cuando en una aplicacion se envian altas tasas de envios. ademas permite enviar datos sin ambigüedad, o sea
    que no es necesario separadores

  - desventaja
    son un poco mas dificles de leer si no se tienen herramientas especiales. Puede ser menos compatible con herramientas genéricas
    de depuración

- la diferencia del nuevo codigo es que este solo envia datos cuando detecta el gesto shake, o sea que es eventual. el otro codigo tiene una
  frecuencia de envio constante sin importar si el microbit se esta moviendo o no 

- Captura el resultado del experimento. ¿Cuántos bytes se están enviando por mensaje? ¿Cómo se relaciona esto con el formato '>2h2B'?
  ¿Qué significa cada uno de los bytes que se envían?

  ya que la parte de 2h empaqueta dos enteros, xValue y yValue, cada uno
  ocupa 2 bytes. y 2B dos enteros, int(aState) y int(bState), cada uno ocupa 1 byte. o sea que en total se enviarian 6 bytes por paquete
  enviado. esto cada vez que se detecta que se sacudio.

  2h: dos enteros con signo de 16 bits (2 * 2 = 4 bytes)  representan xValue y yValue. 2B: dos enteros sin signo de 8 bits
  (2 * 1 = 2 bytes) representan los estados de los botones aState y bState. siendo un total de 6 bytes por mensaje

  Bytes 0-1: xValue (acelerómetro eje X)

  Bytes 2-3: yValue (acelerómetro eje Y)

  Byte 4: aState (estado del botón A)

  Byte 5: bState (estado del botón B)

- ¿Cómo se verían esos números en el formato '>2h2B'?

  se puede mandar numeros positivos o negativos en este formato gracias al uso del tipo h, que representa un entero con signo de 16 bits.

  Positivo (ej. 1000) → 0x03E8 → Bytes: 03 e8

  Negativo (ej. -1000) → 2’s complement: 0xFC18 → Bytes: fc 18

- la nueva modificacion al codigo envia los datos en dos formas diferentes por el puerto serial cada vez que se sacude

- ¿Qué diferencias ves entre los datos en ASCII y en binario? ¿Qué ventajas y desventajas ves en usar un formato binario
  en lugar de texto en ASCII? ¿Qué ventajas y desventajas ves en usar un formato ASCII en lugar de binario?

  la diferencia entre ambos formatos es la representacion de sus datos, el ASCII es un texto legible y el binario son bytes codificados.
  el tamaño de los bytes es fijo mientras que el del ASCII depende del numero o dato. el binario es mas compato, eficiente y rapido, por
  tanto su velocidad de procesamiento es mayor, mientras que la velocidad del ASCII es mas lenta ya que requiere una conversion de texto.
  por ultimo el formato ASCII es mas facil de depurar que el binario

  - ventajas del formato binario
    - es mas compacto
    - mas rapido de transmitir y procesar
    - es consistente con su tamaño
  - desventajas del formato binario
    - es mas dificil de leer si no se tiene una herramienta especializada
    - su depuracion es mas compleja
    - requiere de un codigo para decodificar
  - ventajas del formato ASCII
    - es compatible con muchas aplicaciones
    - como su depuracion es mas rapida es mejor para pruebas y prototipado
    - es mas facil de entender
  - desventajas del formato ASCII
    - como cada numero puede ocupar varios caracteres, es mas pesado
    - Menos eficiente para comunicación frecuente o continua.
   


¿Cuál usar?
Usa ASCII si estás probando, depurando o enseñando.

Usa binario si necesitas velocidad, precisión y rendimiento (como en visualizaciones interactivas o comunicación eficiente).

VENTAJAS DEL FORMATO BINARIO
 Rápido de transmitir y procesar: ideal para tiempo real o animaciones.









    
