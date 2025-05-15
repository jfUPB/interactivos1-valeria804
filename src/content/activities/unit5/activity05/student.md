| | formato ASCII |formato binario | justificacion |
| --- | --- | --- | --- |
| eficiencia | baja |alta | en ASCII se requieren mas bytes para representar un mismo valor, mientras que en binario en uno o dos bytes cada uno, dependiendo como se desarrolle. serial.read() lee directamente los bytes, serial.readLine() lee los textos pero para eso necesita .split(',') y int() |
| velocidad | lento |rapido | en ASCII al enviar mas bytes, el procesamiento necesita mas tiempo, a demas deben convertirse de textos a numeros, y los separa let values = inString.trim().split(',');. mientras que el binario se transmite menos informacion para los mismos datos, serial.read() solo recibe los bytes directos y son solo 8 bytes |
| facilidad | mas facil | compleja | el formato ASCII usa un texto legible, lo cual facilita su analisis, por lo tanto es mas facil de implementar y depurar. el binario requiere trabajar con buffers, marcas de inicio y conversion manual de bytes, esto puede ser complicado para alguien que no tiene mucha experiencia en programacion; se necesita comprobar los marcadores de inicio y final manualmente, y convertir manualmente los bytes a valores  |
| uso de recursos | mayor | menor | en ASCII se necesita un procesamiento de texto y separacion de cadenas, serial.readLine(), trim(), split(',') y int(). el binario es mucho mas directo ya que el serial.read() solo lee un byte a la vez, buffer.push(b) guarda los bytes crudos sin convertir |

- ¿Por qué fue necesario introducir framing en el protocolo binario?

  porque a diferencia del formato de texto donde los datos tienen una forma de separarse. en un flujo binario no hay una separacion
  clara entre paquetes de datos. por eso es necesario un sistema de delimitacion, que permita al receptor identificar donde empieza y termina
  cada mensaje, ese sistema es el framing

- ¿Cómo funciona el framing?

  en el framing se usa algo que delimite el inicio y el fin de los paquetes, para el inicio se usa el byte header (0xAA) que indica el
  comienzo de un paquete. los datos se almacenan en el medio del paquete, Incluyen valores del acelerómetro (x, y), estados de los
  botones (A, B). y se marca el final con el checksum, byte final que permite verificar si los datos llegaron correctamente.
  
- ¿Qué es un carácter de sincronización?

  es un byte especial que identifica el inicio del paquete. en el protocolo binario que usamos el byte 0xAA cumple esa funcion

- ¿Qué es el checksum y para qué sirve?

  se usa para detectar errores, y asi identificar si los datos fueron recibidos correctamente. en el protocolo usado se calcula sumando todos
  los bytes del paquete y tomando el modulo 256

- En la función readSerialData() del programa en p5.js: ¿Qué hace la función concat? ¿Por qué?

  Une los nuevos datos al buffer existente para acumularlos. porque la lectura desde el puerto serial puede no traer un paquete completo
  en una sola llamada.

- En la función readSerialData() tenemos un bucle que recorre el buffer solo si este tiene 8 o más bytes ¿Por qué?

  porque cada paquete de codigo binario tiene 8 bytes (el primero que delimita el inicio, los seis datos, y el ultimo que muestra el final)

- En el código anterior qué significa 0xaa?

  0xAA es un número hexadecimal que equivale a 170 en decimal y a 10101010 en binario. Este valor se usa como carácter de sincronización
  o “header” para marcar el inicio de un nuevo paquete.

  en esa parte del codigo, si el header no era correcto descarta bytes hasta encontrar el header correcto

- En el código anterior qué hace la función shift y la instrucción continue? ¿Por qué?

  shift() elimina y devuelve el primer elemento del arreglo serialBuffer, eliminando el primer byte. continue salta al siguiente ciclo si
  no es 0xAA

- Si hay menos de 8 bytes qué hace la instrucción break? ¿Por qué?

  break se usa para detener el bucle inmediatamente, ya que no hay los datos suficientes para el paquete completo

- ¿Cuál es la diferencia entre slice y splice? ¿Por qué se usa splice justo después de slice?

  slice() copia y extrae los bytes del buffer como un paquete. splace() Elimina elementos del arreglo original para no procesarlos de nuevo.
  Se usan juntos para procesar y limpiar.

- A la siguiente parte del código se le conoce como programación funcional ¿Cómo opera la función reduce?

  recorre un arreglo y acumula un solo resultado. suma los valores del array para calcular el checksum

- ¿Por qué se compara el checksum enviado con el calculado? ¿Para qué sirve esto?

  para detectar errores en la transmicion computedChecksum es lo que calcula el programa con lo recibido y receivedChecksum es lo que el microbit
  ya envio dentro del paquete

- En el código anterior qué hace la instrucción continue? ¿Por qué?

  salta el ciclo actual si hay un error y pasa al siguiente bucle

- ¿Qué es un DataView? ¿Para qué se usa?

  de cierto modo transforma los datos binarios crudos y permite leerlos como enteros o booleanos desde un buffer. sirve para interpretar los
  bytes como enteros 

- ¿Por qué es necesario hacer estas conversiones y no simplemente se toman tal cual los datos del buffer?

  los datos binarios no son direvctamente legibles, como son secuencias de bytes deben ser interpretadas con el tipo de dato que representan











  
