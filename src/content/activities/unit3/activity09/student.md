|estado actual|Evento|	fuente |Resultado esperado| resultado |
| --- | --- | --- | --- | --- |
|CONFIG|A|teclado|aumenta el tiempo en 1 hasta 60|[X]|
|CONFIG|A|microbit|aumenta el tiempo en 1 hasta 60|[X]|
|CONFIG|B|teclado|disminuye el tiempo en 1 hasta 10|[X]|
|CONFIG|B|microbit|disminuye el tiempo en 1 hasta 10|[X]|
|CONFIG|S|teclado o microbit|	estado = "ARMADO" y tiempo inicia conteo|[X]|
|CONFIG|T|teclado o microbit|	accion ignorada |[X]|

|estado actual|Evento|	fuente |Resultado esperado| resultado |
| --- | --- | --- | --- | --- |
|ARMADO|A|teclado o microbit|Agrega a seqPropia|[X]|
|ARMADO|B|teclado o microbit|Agrega a seqPropia|[X]|
|ARMADO|S|teclado o microbit|Agrega a seqPropia|[X]|
|ARMADO|Secuencia [“A”, “B”, “A”, “S”]|teclado o microbit|secuencia correcta, cambia a CONFIG y reinicia tiempo|[X]|
|ARMADO|Secuencia incorrecta|teclado o microbit|	sigue en estado ARMADO|[X]|
|ARMADO|T|teclado o microbit|		estado = "CONFIG" y tiempo = 20 |[X]|
|ARMADO|Tiempo llega a 0|tiempo llega a 0|sin fuente |[X]|

|estado actual|Evento|	fuente |Resultado esperado| resultado |
| --- | --- | --- | --- | --- |
|EXPLOSION|T|teclado o microbit|estado = "CONFIG" y tiempo = 20|[X]|
|EXPLOSION|cualuier otra variable|teclado o microbit|sin efecto|[X]|

|estado actual|Evento|	fuente |Resultado esperado| resultado |
| --- | --- | --- | --- | --- |
|1. ARMADO|Desconexión serial|sin fuente|sincronicidad mantenida, sigue funcionando desde el microbit|[]|
|2. CONFIG|conexion y desconexion serial|sin fuente|solo puede realizar esta accion en este estado|[]|
|3. EXPLOSION|Desconexión serial|sin fuente|sincronicidad mantenida, sigue funcionando desde el microbit|[]|

### pruebas de regresion 

antes se podia conectar y desconectar el microbit sin importar el estado ya que no habia una verificacion del estado en la funcion ¨connectBtnClick¨, para deshabilitar esta funcion en otros estados agregue un condicional para que verifique en que estado se encuentra y dependiendo si es o no CONFIG permite el cambio

```js
function connectBtnClick() {
  if (estado === "CONFIG") {
    if (!port.opened()) {
      port.open('MicroPython', 115200);
    } else {
      port.close();
    }
  } 
}
```
tambien para indicar en los estados que estada deshabilitado, agregue en la funsion draw() dentro de los estados ARMADA y EXPLOSION ¨connectBtn.attribute('disabled', ''); // Deshabilitar el botón¨ que deshabilita visualmente la funcion. y dentro del estado CONFIG ¨connectBtn.removeAttribute('disabled'); // Habilita el botón¨




