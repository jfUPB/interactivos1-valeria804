### ¿Por qué esta técnica es poderosa para la escalabilidad de tu aplicación en términos de concurrencia y de manejo de eventos?

los beneficios de usar una maquina de estados finitos se ven en el desarrollo de aplicaciones interactivas, mostrando su capacidad de manejar 
eventos y comportamientos. esto permite estructurar una aplicación como una serie de estados claramente definidos. sus ventajas son:

- Facilidad para escalar: se puede agregar nuevos estados o eventos sin afectar su funcionalidad ya establecida. Esto hace que el sistema
  sea flexible y escalable para futuras versiones.

- organizacion de comportamiento: cada estado tiene una funcion especifica y bien definida. esto permite manejar logicas complejas en bloques
  manejables

- Manejo estructurado de eventos: al definir como se comporta el sistema en cada estado, se evita que eventos inesperados afecten el
  comportamiento general.

- Concurrencia controlada: Se puede integrar información proveniente tanto del teclado como del micro:bit, y se sabe con certeza cómo
  deberá reaccionar el programa según el estado actual.

### ¿Qué ventajas y desventajas tiene el tipo de pruebas que realizaste en esta unidad?

para determinar el funcionamiento adecuado del codigo se implementaron vectores de prueba que incluyeron combinaciones de entradas desde
teclado y desde el micro:bit para cada estado posible. estas pruebas nos permiten validar que los eventos son reconocidos correctamente, 
las transiciones de estado ocurran cuando deban ocurrir y que las acciones implementadas solo se ejecuten en el estado correspondiente.
este tipo de pruebas tambien nos permiten identificar que la aplicación reacciona correctamente ante cualquier entrada, nos permite detectar
errores de logica de forma mas eficiente y nos permite tener una mejor organizacion con el codigo.

en cuanto a las desventajas puede requerir mucho tiempo y organizacion para documentar cada caso, y ademas debe actualizarse 
constantemente si la aplicación se modifica o crece en complejidad.

### Importancia de las pruebas de regresión

con las pruebas de regresion podemos volver a ejecutar todos los vectores de prueba cada vez que se realiza un cambio en el código, gracias
a esto los cambios nuevos no afectaran las funciones que ya estaban bien. su importancia esta presente en que:

- Garantizan que no se introduzcan errores al modificar el codigo

- Son fundamentales en proyectos colaborativos o en crecimiento

- Aseguran estabilidad y coherencia a largo plazo

si no se realizan puede ser posible que el programa tenga comportamientos inesperados, que en un futuro sea dificil encontrar errores y la
posibilidad de perder funcionalidades previamente implementadas





