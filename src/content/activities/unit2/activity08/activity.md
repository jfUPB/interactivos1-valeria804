####  Analizando un Programa con Máquinas de Estados

**Enunciado**: analiza el siguiente código identificando estados, eventos y acciones. Responde las preguntas planteadas.

``` py

from microbit import *
import utime

class Pixel:
    def __init__(self,pixelX,pixelY,initState,interval):
        self.state = "Init"
        self.startTime = 0
        self.interval = interval
        self.pixelX = pixelX
        self.pixelY = pixelY
        self.pixelState = initState

    def update(self):

        if self.state == "Init":
            self.startTime = utime.ticks_ms()
            self.state = "WaitTimeout"
            display.set_pixel(self.pixelX,self.pixelY,self.pixelState)

        elif self.state == "WaitTimeout":
            currentTime = utime.ticks_ms()
            if utime.ticks_diff(currentTime,self.startTime) > self.interval:
                self.startTime = currentTime
                if self.pixelState == 9:
                    self.pixelState = 0
                else:
                    self.pixelState = 9
                display.set_pixel(self.pixelX,self.pixelY,self.pixelState)

pixel1 = Pixel(0,0,0,1000)
pixel2 = Pixel(4,4,0,500)

while True:
    pixel1.update()
    pixel2.update()

```

- Describe detalladamente cómo funciona este ejemplo. Usa chatGPT para indagar y profundizar en todas las dudas que 
puedas tener.
- ¿Puedes identificar algunos estados?
- Del contexto del ejemplo ¿Qué son los estados?
- Del contexto del ejemplo ¿Qué son los eventos?
- Del contexto del ejemplo ¿Qué son las acciones?

**Entrega**: respuesta a las preguntas.
