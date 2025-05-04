### codigo 

```py
from microbit import *
import utime

class Semaforo:
    def __init__(self, rojo, amarillo, verde, posicion):
        self.tiempos = {'rojo': rojo, 'amarillo': amarillo, 'verde': verde}
        self.estado = 'rojo'
        self.estado_anterior = 'rojo'
        self.tiempo_restante = rojo
        self.posicion = posicion  # Posición en el display
        self.ultimo_cambio = utime.ticks_ms()  # Marca de tiempo inicial
        self.dibujar()

    def actualizar_estado(self):
        ahora = utime.ticks_ms()
        if utime.ticks_diff(ahora, self.ultimo_cambio) >= 1000:  # Cada 1 segundo
            self.tiempo_restante -= 1
            self.ultimo_cambio = ahora

            if self.tiempo_restante <= 0:
                self.estado_anterior = self.estado  # Guardar el estado anterior antes de cambiar

                if self.estado == 'rojo':
                    self.estado = 'verde'
                    self.tiempo_restante = self.tiempos['verde']
                elif self.estado == 'verde':
                    self.estado = 'amarillo'
                    self.tiempo_restante = self.tiempos['amarillo']
                elif self.estado == 'amarillo':
                    self.estado = 'rojo'
                    self.tiempo_restante = self.tiempos['rojo']

                self.borrar(self.estado_anterior)
                self.dibujar()

    def dibujar(self):
        if self.estado == 'rojo':
            display.set_pixel(self.posicion, 0, 9)  # Luz arriba (rojo)
        elif self.estado == 'amarillo':
            display.set_pixel(self.posicion, 2, 5)  # Luz en medio (amarillo)
        elif self.estado == 'verde':
            display.set_pixel(self.posicion, 4, 3)  # Luz abajo (verde)

    def borrar(self, estado_anterior):
        if estado_anterior == 'rojo':
            display.set_pixel(self.posicion, 0, 0)
        elif estado_anterior == 'amarillo':
            display.set_pixel(self.posicion, 2, 0)
        elif estado_anterior == 'verde':
            display.set_pixel(self.posicion, 4, 0)

# Crear los tres semáforos en columnas separadas (0, 2, 4)
semaforo1 = Semaforo(5, 2, 3, 0)
semaforo2 = Semaforo(3, 1, 2, 2)
semaforo3 = Semaforo(4, 3, 2, 4)

while True:
    semaforo1.actualizar_estado()
    semaforo2.actualizar_estado()
    semaforo3.actualizar_estado()
    sleep(50)  # Pequeña pausa para no saturar el micro:bit

```

- para este codigo se definieron la clase semaforo, esto representa un semaforo independiente; genera sus tiempo en cada color, su estado actual
su posicion en la pantalla y permite dibujarse y actualizarce a lo largo del tiempo 

- al inicializar el _init_ para el objeto y para inicializar todas las variables de este. "def __init__(self, rojo, amarillo, verde, posicion):"
aqui se definen que datos necesita el semaforo: cuanto dura cada color (rojo amarillo y verde) y en que colmna del display va a estar 
la posicion 

"self.tiempos = {'rojo': rojo, 'amarillo': amarillo, 'verde': verde}" Crea un diccionario para guardar los tiempos de cada color.

"self.estado = 'rojo'" hace que el semaforo empiece en rojo cada vez que se crea 

"self.estado_anterior = 'rojo'" Guarda el color anterior

"self.tiempo_restante = rojo"	Cuánto tiempo falta en el estado actual. Como empieza en rojo, se inicializa con el tiempo de rojo.

"self.posicion = posicion"	Columna donde aparece el semáforo

"self.ultimo_cambio = utime.ticks_ms()"	Guarda el tiempo en que inició. Funciona como un reloj interno personal para cada semáforo

"self.dibujar()"	Dibuja el primer color inmediatamente

- funcion actualizar estado

 "hora = utime.ticks_ms()"

Aquí tomas el **tiempo actual** en **milisegundos** (`ms`) desde que encendió el micro:bit.

Ejemplo: si han pasado 4 segundos, `ahora` será más o menos `4000` ms.

if utime.ticks_diff(ahora, self.ultimo_cambio) >= 1000:
Calculas la diferencia entre ahora y último cambio.

Si ha pasado más de 1000 ms (1 segundo), entonces entras al if.

Esto garantiza que solo actúes una vez por segundo.

"self.tiempo_restante -= 1 self.ultimo_cambio = ahora"

**Resta 1 segundo** al contador `self.tiempo_restante`.
**Actualiza** `self.ultimo_cambio` para registrar **cuándo fue la última actualización**.
Ahora, **el semáforo sabe** que ya pasó 1 segundo menos.

Si el tiempo llega a 0:

Guarda el estado anterior (self.estado_anterior = self.estado).

despues se decide a que color cambiar el estado

Le pones el tiempo que corresponde al color ej: self.tiempo_restante = self.tiempos['verde']

- luego se dibuja en el display

display.set_pixel(self.posicion, fila, intensidad)

- se borran despues del tiempo

def borrar(self, estado_anterior):

Antes de cambiar al siguiente color, debes apagar el LED donde estaba prendido el color anterior.
Apagas el LED anterior poniendo su intensidad en 0:

display.set_pixel(self.posicion, fila, 0)

Se apaga la luz del color anterior, no del color actual.

- luego se crean los semaforos y se modifica aqui el self.posicion

- en el bucle principal Todo el tiempo llamas actualizar_estado() de los tres semáforos.

Cada semáforo cambia independientemente según su tiempo de duración.

Se llama sleep(50) para dar un pequeño descanso al micro:bit (evitar saturarlo).

### ventajas de usar una clase 

el hecho de usar una clase en un codigo permite la reutilizacion del codigo, ya que la clase nos permite crear múltiples 
instancias sin necesidad de escribir código repetitivo.

permite la independencia de cada semaforo, siendo que cada uno lleva su propio estado, maneja su propio temporizador y se actualiza de forma
independiente 

tambien facilita la ejecucion de cambios, como en como se dibuja el semaforo, como funciona el cambio de colores, entre otros. solo se modificaria
una vez 

Si en el futuro necesitamos más semáforos o cambiar sus tiempos, solo debemos modificar los valores de las instancias sin alterar 
la lógica principal.






