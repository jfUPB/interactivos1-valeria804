para el diseño de la maquina de estados se implemento un diseño basado en transiciones de estado y eventos de usuario. primero que nada 
identifique los estados principales: configuración, armado y explosión, sus transiciones son controladas por eventos como pulsadores, movimiento
y toque de logo (touch). tambien se uso la funcion utime.ticks_ms() para medir el tiempo sin bloquear la ejecucion, esto para la cuenta regresiva.
el hecho de que el programa permita configurar la bomba en cualquier momento usando el touch facilita mucho su uso. tuve dificultades al momento 
de implementar la cuenta regresiva ya que presente dificultades en su comprension. Control de rebote de botones (debounce): El código no filtra 
pulsaciones accidentales rápidas (rebotes) de los botones físicos. Se podría implementar un pequeño retraso o verificación del tiempo entre 
pulsaciones para evitar múltiples cambios de tiempo no deseados.
