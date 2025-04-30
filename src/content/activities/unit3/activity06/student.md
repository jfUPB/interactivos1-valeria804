|estado actual|Evento|	Acción esperada|	Estado siguiente|Resultado|
| --- | --- | --- | --- | --- |
|configurando|A (botón o serial) con tiempo < 60	|tiempo_inicial += 1, mostrar tiempo	|CONFIGURANDO	|[X]|
|configurando|A con tiempo = 60	|tiempo no cambia	|CONFIGURANDO	|[X]|
|configurando|B (botón o serial) con tiempo > 10|	tiempo_inicial -= 1, mostrar tiempo|	CONFIGURANDO|	[X]|
|configurando|B con tiempo = 10	|tiempo no cambia|	CONFIGURANDO	|[X]|
|configurando|S (shake o serial "S")|	cambia a estado ARMADA, inicia cuenta regresiva |	ARMADA	|[X]|
|configurando|T (touch o serial "T")	|sin acción (evento ignorado)|	CONFIGURANDO	|[X]|


|estado actual|Evento|	Acción esperada|	Estado siguiente|Resultado|
| --- | --- | --- | --- | --- |
|ARMADA|Cada 1000ms	|tiempo_actual -= 1, mostrar tiempo	|ARMADA o EXPLOTADA si llega a 0	|[X]|
|ARMADA|tiempo_actual llega a 0	|mostrar calavera, sonar alarma|	EXPLOTADA	|[X]|
|ARMADA|A (evento válido)|	se añade "A" a user_sequence|	ARMADA	|[X]|
|ARMADA|B (evento válido)|	se añade "B" a user_sequence	|ARMADA	|[X]|
|ARMADA|S (shake o serial "S")|	se añade "SHAKE" a user_sequence|	ARMADA|	[X]|
|ARMADA|T (touch o serial "T")	|sin efecto|	ARMADA	|[X]|
|ARMADA|Secuencia incorrecta |	user_sequence se borra	|ARMADA|	[X]|
|ARMADA|Secuencia correcta (A, B, A, SHAKE)|	mostrar HAPPY, reiniciar bomba|	CONFIGURANDO|	[X]|
|ARMADA|LOGO tocado|	reinicio manual, reset de bomba|	CONFIGURANDO	|[X]|


|estado actual|Evento|	Acción esperada|	Estado siguiente|Resultado|
| --- | --- | --- | --- | --- |
|EXPLOTADA|Mostrar SKULL continuamente|	se muestra calavera	|EXPLOTADA	|[X]|
|EXPLOTADA|T (touch o serial "T")	|reiniciar sistema	|CONFIGURANDO	|[X]|
|EXPLOTADA|Cualquier otro evento|	sin efecto|	EXPLOTADA|	[X]|
|EXPLOTADA|user_sequence se borra al entrar	|user_sequence = []	|EXPLOTADA	|[X]|
