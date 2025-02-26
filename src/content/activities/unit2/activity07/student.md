en esta actividad busque realizar un speech y luego un sonido, a demas de que ese sonido aumente o disminuya su velocidad progresivamente se haga shake o touch logo al microbit

```py
from microbit import *
import music
import speech
tempo=120
music.set_tempo(bpm=tempo)


uart.init(baudrate=115200)
display.show(Image.BUTTERFLY)

while True:
     if button_b.was_pressed():
        speech.say('Hello, world. How are you?')
         
     if button_a.was_pressed():
        music.play(music.BIRTHDAY)
        sleep

     if accelerometer.was_gesture('shake'):
        tempo-=50
        music.set_tempo(bpm=tempo)
        music.play(music.BIRTHDAY)
        sleep
        
     if pin_logo.is_touched():
        tempo+=100
        music.set_tempo(bpm=tempo)
        music.play(music.BIRTHDAY)
        sleep
```

para esto modifique y cree la variable tempo, que esta pueda aumentar y disminuir dependiendo de la condicion 
