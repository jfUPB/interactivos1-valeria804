microbit 
```py
from microbit import *

class Semaforo:
    def __init__(self):
        self.state = "ROJO"
        self.startTime = running_time()
        self.interval = 4000

    def update(self):
        currentTime = running_time()

        if self.state == "ROJO":
            self.mostrar_display()
            if currentTime - self.startTime > self.interval:
                self.state = "AMARILLO1"
                self.startTime = currentTime
                self.interval = 800

        elif self.state == "VERDE":
            self.mostrar_display()
            if currentTime - self.startTime > self.interval:
                self.state = "ROJO"
                self.startTime = currentTime
                self.interval = 4000

        elif self.state == "AMARILLO1":
            self.mostrar_display()
            if currentTime - self.startTime > self.interval:
                self.state = "VERDE"
                self.startTime = currentTime
                self.interval =3000


    def mostrar_display(self):
        display.clear()
        if self.state == "ROJO":
            display.set_pixel(2,0,9)

        elif self.state == "AMARILLO1":
            display.set_pixel(2,1,9)
        
        elif self.state == "VERDE":
            display.set_pixel(2,2,9)


semaforo = Semaforo()

while True:
    semaforo.update()
    sleep(100)
```

las acciones son las funciones update y display. en el update se ejecutan los eventos que permiten el cambio de estados, los que permiten verificar el paso del tiempo y le dan acceso a los estados es el
"if currentTime - self.startTime > self.interval:"

los estados son rojo, amarillo y verde

el funcionamiento del codigo es que al inicio se llama al objeto semaforo y esto lo pasa a la funcion update, en la cual se inicializa el estado rojo "self.startTime" toma el valor de "running_time" 
e inicializa el "self.interva" en 4000; y con currentTime = running_time(). despues se ejecuta el evento "if currentTime - self.startTime > self.interval:" que verifica si paso el tiempo determinado para pasar de estado
y cuando pasa de estado se actualiza el self.startTime y self.interval, y despues ejecuta la funcion del estado; y asi con los demas.

en display se pinta los puntos de luces "if self.state == "ROJO": display.set_pixel(2,0,9)". esta variable "self.mostrar_display()" es llamada en los estados para pintar el punto cuando se este ejecutando




