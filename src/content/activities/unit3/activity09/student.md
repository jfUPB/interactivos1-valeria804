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
|ARMADO|Desconexión serial|sin fuente|Sincronicidad mantenida, sigue funcionando con teclado|[X]|
|CONFIG|Reconexión serial|sin fuente|Retoma comunicación sin reiniciar estado|[X]|




