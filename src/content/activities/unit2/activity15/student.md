Durante esta unidad, profundicé en el diseño de máquinas de estados aplicadas a microcontroladores como la micro:bit,
y entendí cómo los estados permiten organizar y controlar el flujo de un programa en sistemas interactivos. Aprendí que una máquina 
de estados bien estructurada facilita el manejo de múltiples eventos, como entradas digitales provenientes de botones o sensores
(en este caso, los botones A, B y el acelerómetro), permitiendo que el programa reaccione adecuadamente en cada situación. 
También me familiaricé con la lectura de entradas digitales usando funciones como was_pressed() e is_pressed(), entendiendo sus 
diferencias para gestionar tanto eventos instantáneos como presiones continuas. La unidad me ayudó a comprender la importancia de medir 
el tiempo en sistemas embebidos usando contadores como utime.ticks_ms() en lugar de simples retardos, lo cual facilita la concurrencia: 
es decir, poder "esperar" y al mismo tiempo seguir ejecutando otras tareas. Además, pude apreciar cómo los microcontroladores manejan 
datos de forma simple, pero poderosa, como el envío de datos seriales para debug o la visualización de estados en una matriz de LEDs. 
En proyectos futuros, estos conceptos me permitirán crear sistemas que respondan en tiempo real a múltiples entradas, manejar mejor la 
comunicación serial, y diseñar lógicas concurrentes donde no se detenga todo el programa por esperar un evento. El principal desafío fue 
lograr que el programa respondiera de manera fluida a entradas simultáneas y manejar los cambios de estado sin errores, lo que resolví 
aplicando una metodología de desarrollo incremental y prueba continua.
