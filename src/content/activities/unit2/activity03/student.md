como el micropython controla el microbit

#### entrada y salida microbit
##### entradas 

1. los botones del microbit funcionan como la forma de interaccion mas basica, estos puedes ser usados de forma separada o junta, en la que al presionarlos este manda una informacion al codigo,
 el cual tendra una respueta programada
2. el sensor de movimiento el cual detecta movimientos del dispositivo, como inclinarlo, girarlo o sacudirlo, y tiene un acelerómetro incorporado
3. el sensor de temperatura que mide la temperatura ambiente en grados Celsius.
4. Entradas Analógicas (Pines 0, 1 y 2) que permiten leer valores de sensores externos como potenciómetros, fotocélulas o sensores de luz. Estos pines leen una señal que puede variar de 0 a 1023,
 lo que ofrece una mayor precisión que las entradas digitales.
5. niveles de luz que miden la luz que cae en el microbit
6. touch logo el cual funciona como una pantalla tactil que tambien recive la informacion de presion

##### salida 

1. pantalla LED que permite mostrar texto, números o patrones gráficos. cuya funcion es mostrar mensajes, indicaciones o valores simples.
2. salida de audio que puede ser utilizada para emitir sonidos o tonos.
3. la funcion radio o Bluetooth lo que permite enviar y recibir datos de otros dispositivos o entre las mismas microbit
4. Pines de Salida Digital (Pines 0, 1 y 2) los cuales pueden enviar señales de encendido/apagado (alto o bajo) a dispositivos externos.

#### Descripción de al menos 3 funciones de Micropython para entradas y 3 para salidas, con ejemplos de código sencillos.

##### entrada

1. la mas sencilla es la funcion input() que recolecta datos del usuario

   ```py
   nombre = input("¿Cómo te llamas? ")
   print(f"Hola, {nombre}!")
   ```
2. Utilice was_pressed() para comprobar si se presionó un botón desde que se encendió el micro:bit o desde la última vez que se utilizó was_pressed().

```py
while True:
    if button_a.was_pressed():
        display.scroll('A')
```
3. Puedes usar gestos en el programa para detectar cuando tla micro:bit se mueve de diferentes maneras.

```py
while True:
    if accelerometer.was_gesture('shake'):
        display.show(Image.CONFUSED)
```
4. la funcion de touch logo que funciona como un boton extra

```py
while True:
    if pin_logo.is_touched():
        display.show(Image.HAPPY)
```

5. temperature; El micro:bit tiene un sensor de temperatura dentro de su procesador. Puedes tomar lecturas de temperatura en grados centígrados de esta manera:

```py
display.scroll(temperature())
```

##### salida 

1. display, el cual puede mostrar imagenes en la pantalla

```py
display.show(Image.HEART)
sleep(400)
```
o mostrar una serie de palabras que formen un mensaje 

```py
display.scroll('score')    
display.scroll(23)
```

2. sound, El micro:bit puede reproducir melodías e incluso hablar. El micro:bit V2 tiene un altavoz incorporado y puedes conectar auriculares o un altavoz amplificado a los pines 0 y GND de cualquier micro:bit.

```py
import music

music.play(music.BA_DING)
```

3. Enviar datos a la consola, La función print() se utiliza para mostrar información en la consola:

```py
valor = 42
print(f"El valor es: {valor}")

```

   
