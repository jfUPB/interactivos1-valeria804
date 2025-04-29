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

# Secuencia de desactivación esperada
deactivation_sequence = ["A", "B", "A", "SHAKE"]
user_sequence = []

def check_deactivation():
    """ Verifica si la secuencia ingresada es correcta """
    return user_sequence == deactivation_sequence

while True:
    if estado == CONFIGURANDO:
        display.scroll(str(tiempo_inicial))

        if button_a.was_pressed():
            if tiempo_inicial < 60:
                tiempo_inicial += 1
                display.show(tiempo_inicial)

        if button_b.was_pressed():
            if tiempo_inicial > 10:
                tiempo_inicial -= 1
                display.show(tiempo_inicial)

        # Ahora control para mantener presionado
        if button_a.is_pressed():
            utime.sleep_ms(300)  # espera para distinguir si es toque corto o presionado
            while button_a.is_pressed():
                if tiempo_inicial < 60:
                    tiempo_inicial += 1
                    display.show(tiempo_inicial)
                    utime.sleep_ms(150)  # velocidad de incremento manteniendo presionado

        if button_b.is_pressed():
            utime.sleep_ms(300)
            while button_b.is_pressed():
                if tiempo_inicial > 10:
                    tiempo_inicial -= 1
                    display.show(tiempo_inicial)
                    utime.sleep_ms(150)

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
            display.show(Image.SKULL)
            music.play(music.WAWAWAWAA)
            estado = EXPLOTADA

        if pin_logo.is_touched():
            estado = CONFIGURANDO
            tiempo_inicial = 20
            tiempo_actual = tiempo_inicial
            display.clear()

          # Capturar secuencia de desactivación
        if button_a.was_pressed():
            user_sequence.append("A")
        if button_b.was_pressed():
            user_sequence.append("B")
        if accelerometer.was_gesture("shake"):
            user_sequence.append("SHAKE")

        # Si se ingresa una secuencia errónea, se borra
        if len(user_sequence) > len(deactivation_sequence) or user_sequence != deactivation_sequence[:len(user_sequence)]:
            user_sequence = []

        # Si la secuencia es correcta, se desactiva la bomba
        if check_deactivation():
            display.show(Image.HAPPY)
            utime.sleep_ms(2000)
            estado = CONFIGURANDO
            tiempo_inicial = 20
            user_sequence = []  # Reiniciar secuencia

    elif estado == EXPLOTADA:
        display.show(Image.SKULL)

        user_sequence = []
        if pin_logo.is_touched():
            estado = CONFIGURANDO
            tiempo_inicial = 20
            tiempo_actual = tiempo_inicial
            display.clear()
```

para la creacion de este codigo se implemento:

Lógica de serialización de eventos (secuencia de desactivación).

Control de concurrencia de eventos (cronómetro mientras capturas entrada de usuario).

user_sequence: Guarda los botones presionados en orden.

para esto defini la secuencia de desactivacion (deactivation_sequence = ["A", "B", "A", "SHAKE"]). para verificar que el usuario esta 
realizando el codigo de desactivacion el programa escucha constantemente si el usuario hace alguna de estas acciones:

```py
if button_a.was_pressed():
    user_sequence.append("A")
if button_b.was_pressed():
    user_sequence.append("B")
if accelerometer.was_gesture("shake"):
    user_sequence.append("SHAKE")
```

estas acciones se agregan a la lista user_sequence

- si el usuario se equivoca el programa verifica si los pasos que lleva el usuario coinciden con los primeros elementos
  de la secuencia esperada. cada error hace que la comprobacion se reinicie 

```
if len(user_sequence) > len(deactivation_sequence) or user_sequence != deactivation_sequence[:len(user_sequence)]:
    user_sequence = []
```

- cuando la secuencia del usuario coincide

Cuando user_sequence coincide exactamente con deactivation_sequence

```py
if check_deactivation():
    display.show(Image.HAPPY)
    utime.sleep_ms(2000)
    estado = CONFIGURANDO
    tiempo_inicial = 20
    user_sequence = []
```

- - para evitar datos basura en configuracion, por no limpiar user_sequence cuando explota se puede agregar user_sequence = [] en estado==explotada

- ```py
  elif estado == EXPLOTADA:
        display.show(Image.SKULL)

        user_sequence = [] # <--esto
        if pin_logo.is_touched():
            estado = CONFIGURANDO
            tiempo_inicial = 20
            tiempo_actual = tiempo_inicial
            display.clear()
  ```
  user_sequence = [] crea o reinicia la memoria temporal de las acciones del usuario. esto si el usuario interactúa de nuevo tras
  reiniciar la bomba




