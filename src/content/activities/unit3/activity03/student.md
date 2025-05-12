```py
from microbit import *
import music
import utime

# Estados
CONFIGURANDO = 0
ARMADA = 1
EXPLOTADA = 2
estado = CONFIGURANDO

# Variables de control
# Variables de tiempo
tiempo_inicial = 20  # segundos
tiempo_actual = tiempo_inicial

# Variables para medir tiempo
ultimo_tiempo = utime.ticks_ms()

# Secuencia de desactivación esperada
deactivation_sequence = ["A", "B", "A", "SHAKE"]
user_sequence = []

# Variables de eventos
event_occurred = False
event_value = ""

def tareaEventos():
    """ Captura eventos del micro:bit y del puerto serial """
    global event_occurred, event_value

    if button_a.was_pressed():
        event_occurred = True
        event_value = "A"

    if button_b.was_pressed():
        event_occurred = True
        event_value = "B"

    if accelerometer.was_gesture("shake"):
        event_occurred = True
        event_value = "S"

    if pin_logo.is_touched():
        event_occurred = True
        event_value = "T"

    if uart.any():
        data = uart.read(1)
        if data:
            if data[0] == ord('A'):
               event_occurred = True
               event_value = "A"
            elif data[0] == ord('B'):
               event_occurred = True
               event_value= "B"
            elif data[0] == ord('S'):
                event_occurred = True
                event_value = "S"
            elif data[0] == ord('T'):
                event_occurred = True
                event_value= "T"

def tareaBomba():
    global event_occurred, event_value, estado, tiempo_inicial, tiempo_actual, ultimo_tiempo, user_sequence
    if estado == CONFIGURANDO:
        display.scroll(str(tiempo_inicial))

        if event_occurred:
            if event_value == "A" and tiempo_inicial < 60:
                tiempo_inicial += 1
                display.show(tiempo_inicial)
    
            elif event_value == "B" and tiempo_inicial > 10:
                tiempo_inicial -= 1
                display.scroll(tiempo_inicial)
    
            elif event_value == "S":  # Activar bomba
                estado = ARMADA
                tiempo_actual = tiempo_inicial
                ultimo_tiempo = utime.ticks_ms()
                display.show(tiempo_actual)

            event_occurred = False  # Consumir el evento

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

        if event_occurred:
            if event_value == "T":
                estado = CONFIGURANDO
                tiempo_inicial = 20
                tiempo_actual = tiempo_inicial
                display.clear()
        event_occurred = False

          # Capturar secuencia de desactivación
        if event_occurred:
            if event_value == "A":
                user_sequence.append("A")
            if event_value == "B":
                user_sequence.append("B")
            if event_value == "S":
                user_sequence.append("SHAKE")

            print(user_sequence)
        event_occurred = False  # Consumir el evento

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
        if event_occurred:
            if event_value == "T":
                estado = CONFIGURANDO
                tiempo_inicial = 20
                tiempo_actual = tiempo_inicial
                display.clear()
        event_occurred = False # Consumir el evento
            
def check_deactivation():
    """ Verifica si la secuencia ingresada es correcta """
    return user_sequence == deactivation_sequence

while True:
    tareaBomba()
    tareaEventos()
```

para este ejercicio dividi las tareas en tareaEventos() y tareaBomba()

- en la ``tareaEventos()`` se capturan eventos del micro:bit y del puerto serial,como los botones a y b, shake, touch y datos
entrantes por el puerto serie (uart) almacenándolos en las variables ``event_occurred`` y``event_value``. cuando detecta que ha
ocurrido un evento es matcardo por ``true`` y cuarda que tipo de evento fue en ``event_value``  (por ejemplo, "A", "B", "S", "T" o
datos seriales).

con el uso de eventos seriales (uart): ahora se pueden enviar comandos desde un dispositivo conectado por UART 
(por ejemplo, otro microcontrolador o un PC)

- en la ``tareaBomba()`` se controla la máquina de estados de la bomba, consumiendo los eventos y ejecutando las acciones correspondientes.
gestiona la logica del sistema dependiendo de los estados de la maquina, este usa los eventos capturados por ``tareaEventos()``

- y para evitar que se consuma mucho, despues de procesar un evento la variable ``event_occurred`` vuelve a False para evitar
que se procese repetidamente.







