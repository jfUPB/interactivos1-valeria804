### codigo 

```py
from microbit import *
import music
import utime

# Estados
CONFIGURANDO = 0
ARMADA = 1
EXPLOTADA = 2
estado = CONFIGURANDO

# Variables de tiempo
tiempo_inicial = 20  # segundos
tiempo_actual = tiempo_inicial

# Variables para medir tiempo
ultimo_tiempo = utime.ticks_ms()

while True:
    if estado == CONFIGURANDO:
        display.scroll(str(tiempo_inicial))

        if button_a.was_pressed():  # Botón UP
            if tiempo_inicial < 60:
                tiempo_inicial += 1
            display.show(tiempo_inicial)

        if button_b.was_pressed():  # Botón DOWN
            if tiempo_inicial > 10:
                tiempo_inicial -= 1
            display.show(tiempo_inicial)

        if accelerometer.was_gesture('shake'):
            estado = ARMADA
            tiempo_actual = tiempo_inicial
            ultimo_tiempo = utime.ticks_ms()
            display.show(tiempo_actual)

    elif estado == ARMADA:
        ahora = utime.ticks_ms()
        if utime.ticks_diff(ahora, ultimo_tiempo) >= 1000:
            ultimo_tiempo = ahora
            tiempo_actual -= 1
            if tiempo_actual >= 0:
                display.show(tiempo_actual)
        
        if tiempo_actual <= 0:
            # Bomba explota
            display.show(Image.SKULL)
            music.play(music.WAWAWAWAA)
            estado = EXPLOTADA  # Cambia a estado EXPLOTADA

        # Permite volver a configuración durante la cuenta regresiva
        if pin_logo.is_touched():
            estado = CONFIGURANDO
            tiempo_inicial = 20
            tiempo_actual = tiempo_inicial
            display.clear()

    elif estado == EXPLOTADA:
        display.show(Image.SKULL)

        if pin_logo.is_touched():
            estado = CONFIGURANDO
            tiempo_inicial = 20
            tiempo_actual = tiempo_inicial
            display.clear()

```
#### explicacion 

- primero se crearon los estados y se determino en que estado comenzaria el programa, en el estado configuracion se determianron las 
funciones de a y b, haciendo que disminuya o aumente el tiempo y actualizando ese dato en tiempo_inicial

```py
 if button_a.was_pressed():
            if tiempo_inicial < 60:
                tiempo_inicial += 1

if button_b.was_pressed():
            if tiempo_inicial > 10:
                tiempo_inicial -= 1
```

- luego se implemento la funcion shake, en esta se actualizo la variable de tiempo_actual con los valores que se habian guardado en tiempo_inicial

```py
if accelerometer.was_gesture('shake'):
            estado = ARMADA
            tiempo_actual = tiempo_inicial
            display.show(tiempo_actual)
```
- para la cuenta regresiva se uso utime.ticks_ms()

```py
elif estado == ARMADA:
        ultimo_tiempo = utime.ticks_ms()

        while estado == ARMADA:
            ahora = utime.ticks_ms()
            if utime.ticks_diff(ahora, ultimo_tiempo) >= 1000:
                ultimo_tiempo = ahora
                tiempo_actual -= 1
                display.show(tiempo_actual)
```
se guardo el tiempo actual cuando empieza la cuenta "ultimo_tiempo = utime.ticks_ms()" en el estado armada. Este valor será 
utilizado como referencia para calcular el tiempo que ha pasado

se tomo un nuevo valor de tiempo actual con "ahora = utime.ticks_ms()". Esto nos da el tiempo en milisegundos desde que se encendio
el dispositivo.

para calcular la diferencia de tiempo "if utime.ticks_diff(ahora, ultimo_tiempo) >= 1000:"  se calcula la diferencia entre el tiempo actual
(ahora) y el ultimo tiempo registrado (ultimo_tiempo)

para actualizar el tiempo en la cuenta regresiva. se actualiza el tiempo de referencia (ultimo_tiempo) al valor del tiempo actual. "ultimo_tiempo = ahora".
 luego se resta 1 segundo de la variable tiempo_actual "tiempo_actual -= 1". y por ultimo se muestra el nuevo valor de tiempo_actual en la pantalla de LEDs
 del micro:bit usando display.show(tiempo_actual) "display.show(tiempo_actual)"

 ##### ¿Qué hace utime.ticks_diff(ahora, ultimo_tiempo)?
utime.ticks_diff(a, b) calcula cuántos milisegundos han pasado entre los valores a y b.

En este caso, ahora es el tiempo actual (cuando llega a este punto en el código) y ultimo_tiempo es el tiempo registrado previamente (cuando comenzó la cuenta regresiva).
Si la diferencia entre ambos es al menos 1000 milisegundos (1 segundo), significa que ha pasado un segundo y debemos restar 1 segundo de la cuenta regresiva (tiempo_actual -= 1).

ultimo_tiempo es la referencia temporal de cuando comenzamos a medir la cuenta regresiva, y nos permite saber cuándo ha pasado 1 segundo.

tiempo_actual es la variable principal que almacena el valor de la cuenta regresiva y disminuye cada vez que pasa 1 segundo.

 - se agrega el estado de explotada y se muestra en la pantalla led el simbolo de explosion al llegar al tiempo 0, y a demas se crea la funcion
para que se pueda volver al estado de configuracion

```py
EXPLOTADA = 2

# Al llegar a 0
if tiempo_actual <= 0:
    display.show(Image.SKULL)
    music.play(music.WAWAWAWAA)
    estado = EXPLOTADA

# Nuevo bloque para EXPLOTADA
elif estado == EXPLOTADA:
    display.show(Image.SKULL)

    if pin_logo.is_touched():
        estado = CONFIGURANDO
        tiempo_inicial = 20
        tiempo_actual = tiempo_inicial
        display.clear()
```

- y para hacer que vuelva al estado de configuracion mientras que este la cuenta regresiva identificando si touch esta activada

```py
 if pin_logo.is_touched():
            estado = CONFIGURANDO
            tiempo_inicial = 20
            tiempo_actual = tiempo_inicial
            display.clear()
```




