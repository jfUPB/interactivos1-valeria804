### descripcion del funcionamiento 

#### estados 

configuracion 
armada/cuenta regresiva 
explotada 

#### eventos de entrada 

UP (Botón A): Incrementa el tiempo (solo en Configuración)

DOWN (Botón B): Decrementa el tiempo (solo en Configuración)

ARMED (Shake): Arma la bomba (inicia cuenta regresiva)

TOUCH: Reinicia a Configuración (desde cualquier estado)

Timeout: El tiempo de cuenta regresiva llegó a 0

#### acciones 

Actualizar tiempo en display

Contar tiempo usando utime.ticks_ms()

Reproducir sonido al explotar (speaker)

### explicacion detallada 

#### Estado 1: Configuración

-Descripción: Se permite ajustar el tiempo inicial de la bomba

-Transiciones:

UP (Botón A): Aumenta el tiempo en 1s (hasta 60s)

DOWN (Botón B): Disminuye el tiempo en 1s (mínimo 10s)

ARMED (Shake): Transición al estado “Armada”

-Acciones:

Mostrar tiempo configurado en el display

#### Estado 2: Armada (Cuenta regresiva)
- Descripción: La bomba está activa. El tiempo empieza a disminuir

- Transiciones:

Timeout (tiempo = 0): Transición al estado “Explotada”

TOUCH: Reinicio al estado “Configuración”

- Acciones:

Decrementar y mostrar el tiempo cada segundo

Verificar si se alcanzó cero usando utime.ticks_ms()

#### Estado 3: Explotada
- Descripción: El tiempo llegó a 0. La bomba "explota"

- Transiciones:

TOUCH: Reinicio al estado “Configuración”

- Acciones:

Activar sonido con el speaker

Mostrar animación de explosión (si aplica) en el display

### diagrama 

+-------------------+          Shake           +-----------------------+
|   Configuración   | -----------------------> |    Armada (Contando)  |
| (Tiempo ajustable)|           Touch          |   (Cuenta regresiva)  |
+-------------------+ <----------------------- +-----------------------+
 ^      ^      |                                          |
 |      |      | UP / DOWN                                | Timeout
 |      |      | (Modificar tiempo)                       v
 |      |      +------------------------------+    +-------------+
 |      |                                     |     |  Explotada  |
 |      |              Touch                  |     | (Fin del    |
 |      +-------------------------------------+     |  juego)     |
 |                                                 +------+------+
 |                           Touch                        |
 +--------------------------------------------------------+
                                           






