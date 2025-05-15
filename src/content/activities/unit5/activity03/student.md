- el codigo en el que se envian los mismos datos, son enviados:
xValue = 500 
yValue = 524 
aState = True 
bState = False

el paquete seria 01 f4 02 0c 01 00

Campo	   Valor	   Tamaño   	Representación hexadecimal (big-endian)
xValue	   500   	2B	   01 f4 (500 en hex: 0x01F4)
yValue	   524   	2B	   02 0c (524 en hex: 0x020C)
aState	   True	   1B	   01 (1 en decimal)
bState	   False   	1B	   00 (0 en decimal)

 big-endian (los bytes más significativos primero)

- Explica por qué en la unidad anterior teníamos que enviar la información delimitada y además marcada con un salto de línea y
  ahora no es necesario.

  porque gracias a '>2h2B' se determina el tamaño y el formato de cada envio, es diferente ya que en el formato anterior los valores podian
  tener tamaños variables

- Compara el código de la unidad anterior con este. ¿Qué cambios ves?

  los cambios son principalmente en el principalmente en el formato de los datos que se están recibiendo desde el micro:bit por el puerto
  serial y cómo se procesan en p5.js

  ```
  if (port.availableBytes() >= 6) {
  let data = port.readBytes(6);
  if (data) {
    const buffer = new Uint8Array(data).buffer;
    const view = new DataView(buffer);
    microBitX = view.getInt16(0);
    microBitY = view.getInt16(2);
    microBitAState = view.getUint8(4) === 1;
    microBitBState = view.getUint8(5) === 1;
    updateButtonStates(microBitAState, microBitBState);

  ```
  este codigo lee solo 6 bytes del puerto serial, codificados en formato binario

  Usa DataView para interpretar los primeros 4 bytes como dos enteros de 2 bytes (x, y) y los siguientes 2 bytes como estados de
  botones (aState, bState) de 1 byte cada uno.

  mientras que el codigo de la unidad anterior: Lee una línea de texto terminada en \n (ASCII). Divide la línea por comas para obtener
  4 valores: x, y, aState, bState. Convierte esos valores desde texto (por ejemplo, "524", "true") a tipos de datos numéricos o booleanos.

- ¿Qué ves en la consola? ¿Por qué crees que se produce este error?

  en consola y en como se ejecuta el codigo se nota como la lectura de datos va cambiando lo que se recibe a pesar de que el microbit
   siempre envia lo mismo

  Al leer datos por puerto serial en binario, si no comienzas a leer exactamente desde el primer byte de un paquete de 6 bytes,
  entonces los valores quedan desalineados, y el código interpreta mal los datos.

  el puerto serial no garantiza que readBytes(6) comience en un paquete alineado. esto provoca que el microbit envia los datos de forma
  continua pero p5.js los empieza a leer en cualquier momento, lo cual provoca una inchoerencia

- en el nuevo codigo modificado para evitar desincronizacion al momento de recivir los datos implemento en el microbit:

  Estructura del paquete (8 bytes):

  Byte 0: Header fijo 0xAA (indica inicio de paquete)

  Bytes 1–6: Datos → dos enteros de 16 bits (x, y) y dos bytes para los estados de botones A y B
 
  Byte 7: Checksum → suma de los 6 bytes de datos módulo 256 (verificación)

  Esta solución crea un paquete estructurado y verificable que p5.js puede identificar fácilmente

   - Checksum:
  
   ``checksum = sum(data) % 256``
  
     Suma todos los valores byte a byte del arreglo data. Luego toma el módulo 256 → esto lo reduce a 1 byte. Si algún byte se corrompe
     o no llega, el checksum cambiará

   -  ``packet = b'\xAA' + data + bytes([checksum])``
      b'\xAA': encabezado de sincronización (1 byte). data: datos empaquetados (6 bytes). checksum: verificador (1 byte)

  por ultimo se envía todo el paquete de 8 bytes

  - la modificacion en el codigo de p5.js

    se agrego function readSerialData() que detecta los paquetes válidos de 8 bytes, verifica su integridad con un checksum, y extrae
    los datos binarios correctos

    ```
    let available = port.availableBytes(); //Pregunta cuántos bytes nuevos han llegado por el puerto serial.

    // Si hay bytes disponibles, los lee y los agrega al serialBuffer, que almacena los bytes en orden hasta que se forme un paquete completo.
    if (available > 0) {
      let newData = port.readBytes(available);
      serialBuffer = serialBuffer.concat(newData);
    }
    ```
    ``while (serialBuffer.length >= 8) `` Mientras haya al menos 8 bytes en el buffer (el tamaño de un paquete completo), intenta procesar
     uno.
  
    ```
    if (serialBuffer[0] !== 0xaa) {
      serialBuffer.shift(); // descarta bytes hasta encontrar el header correcto (0xAA)
      continue;
    }
    ```
    si el primer byte del buffer no es el header 0xAA, entonces descarta ese byte y sigue buscando.

    Si por alguna razón el buffer ya no tiene 8 bytes, sale del ciclo ``  if (serialBuffer.length < 8) break;``
  
   ```
     let packet = serialBuffer.slice(0, 8);
    serialBuffer.splice(0, 8);
   ```
   Extrae los primeros 8 bytes del buffer como un "paquete" y los elimina del buffer (para no procesarlos de nuevo).

   depues se separa los 6 bytes de datos (índices 1 a 6), Toma el último byte como checksum recibido. Calcula un checksum sumando 
   los 6 bytes de datos y haciendo módulo 256.

   Si el checksum no coincide, muestra un mensaje de error y descarta el paquete. 
   ```
   if (computedChecksum !== receivedChecksum) {
      console.log("Checksum error in packet");
       continue; // descarta el paquete si está dañado
   }
   ```
   - Se usa DataView para leer los valores binarios correctamente:

     getInt16(0) → extrae el primer entero (2 bytes): x

     getInt16(2) → segundo entero (2 bytes): y

     getUint8(4) → byte 5: estado del botón A

     getUint8(5) → byte 6: estado del botón B


- ¿Qué cambios tienen los programas y ¿Qué puedes observar en la consola del editor de p5.js?

  se reemplazaron los valores predeterminados del acelerometro del microbit por accelerometer.get_x() y accelerometer.get_y(), lo que
   hace que ahora el dibujo en p5.js ahora reaccione a la inclinación del micro:bit

  lo mismo con los valores fijos de los botones por button_a.is_pressed() y button_b.is_pressed()

  en el codigo de p5.js Se eliminó el console.log() de la función readSerialData(), lo que puede hacer que la consola se
  vea menos saturada con mensajes, se centro las inclinaciones

  ahora lo que se ve en la consola del editor es Microbit ready to draw, A pressed, B released y Checksum error in packet








