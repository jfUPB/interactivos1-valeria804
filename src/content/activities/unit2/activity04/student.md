#### ¿Qué querías comprobar?

queria comprobar como se comportan los diferentes botones de la microbit y como se detecta su estado, si esta siendo presionado o no, usando el MicroPython

#### ¿Cuál fue tu hipótesis?

Cuando presiono el botón A, se debería registrar un cambio de estado (de no presionado a presionado) y el micro:bit mostrara un caracter en la pantalla. Cuando presiono el botón B, 
debería suceder algo diferente a cuando presiono el botón A, como mostrar otro carácter en la pantalla, indicando que el estado del botón B fue detectado. y por ultimo Cuando toco el logo de la micro:bit,
debería registrar un cambio de estado (de no tocado a tocado) y el micro:bit debería reaccionar de otra manera 

#### Los códigos involucrados en el experimento

```py
from microbit import *

while True:
    if button_a.is_pressed(): 
        display.show("A")  
    elif button_b.is_pressed(): 
        display.show("B")  
    elif pin_logo.is_touched(): 
        display.show("L") 
    else:
        display.clear()
```

#### Descripción de lo resultados

 Cuando presiono el botón A, aparece la letra "A"; Cuando presiono el botón B, aparece la letra "B"; Cuando toco el logo de la micro:bit, aparece la letra "L". Cuando dejo de presionar alguno, la pantalla se borra.

#### Análisis de esos resultados

las funciones son leidas correctamente gracias a la documentacion de los botones. con la funcion is_pressed() se verifica de que esta constantemente presionado, si esa condicion if se cumple hace que el microbit dibuje 
en la pantalla A o B dependiendo de la funcion. mientras que con la funcion is_touched() detecta el contacto en el logo,  Cuando toco el logo, se activa la condición if, lo que hace que se muestre la letra "L".
Si nada esta siendo presionado, el micro:bit borra la pantalla con la funcion display.clear().

#### Conclusiones

Los botones A y B, así como el logo táctil, pueden ser fácilmente leídos en MicroPython utilizando las funciones is_pressed() y is_touched(), respectivamente.

El comportamiento observado del experimento fue el esperado: cada botón y el logo reaccionan de manera diferente cuando son presionados o tocados, lo que permite al usuario interactuar
con el dispositivo de diversas formas.

