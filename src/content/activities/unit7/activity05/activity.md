#### Aplica lo aprendido

:::note[🎯 Enunciado]
Ahora que comprendes cómo funciona el sistema base, es tu turno de modificarlo. Diseña e implementa una variación creativa de la interacción. Debes seguir usando el input táctil del móvil como control principal, pero el resultado en la aplicación de escritorio debe ser diferente al simple movimiento de un círculo.
:::

:::tip[Recursos]
*   El código completo y funcional del caso de estudio de la Unidad 7.
*   [Referencia de p5.js](https://p5js.org/reference/) (para ideas visuales y de interacción).
*   Tus notas y comprensión de las actividades SEEK.
:::

👣 **Pasos**:

1.  **Lluvia de ideas (Brainstorming):** piensa en diferentes formas en que la entrada táctil del móvil podría controlar algo en el escritorio. Algunas ideas para inspirarte (¡pero sé original!):
    *   ¿Controlar el **color** o el **tamaño** de una forma en el escritorio con la posición X/Y del toque?
    *   ¿Usar el toque para **dibujar líneas o formas** en el escritorio, dejando un rastro?
    *   ¿Usar la **velocidad** del movimiento del dedo en el móvil para afectar algo en el escritorio (requeriría calcularla)?
    *   ¿Crear un "instrumento musical" simple donde diferentes zonas del móvil activen sonidos o cambios visuales en el escritorio?
    *   ¿Controlar un personaje simple o una pequeña animación en el escritorio?
2.  **Elige un concepto:** selecciona una idea que te parezca interesante y factible de implementar modificando el código existente.
3.  **Planifica los cambios:**
    *   **¿Qué datos necesita enviar el móvil?** ¿Son suficientes las coordenadas X/Y, ¿O necesitas enviar algo más?.Decide el formato del objeto JSON que enviará el móvil.
    *   **¿Necesita cambiar el servidor?** Si solo cambias cómo interpretan los datos los clientes, probablemente no. Pero si cambias el *nombre* del evento ('message') o necesitas que el servidor haga algún cálculo simple, sí tendrías que modificar `server.js`. Para esta actividad, intenta mantener el servidor igual si es posible.
    *   **¿Cómo reaccionará el escritorio?** ¿Qué necesita cambiar en `desktop/sketch.js`? ¿Cómo interpretará los datos recibidos (parsing del JSON)? ¿Qué funciones de p5.js usarás en `draw()` o en respuesta al mensaje para crear el nuevo efecto visual/interactivo?
4.  **Implementa los cambios:** modifica los archivos `mobile/sketch.js` y `desktop/sketch.js` (y `server.js` solo si es estrictamente necesario) según tu plan.
5.  **Prueba y depura:** usa el Dev Tunnel para probar tu nueva interacción. Utiliza `console.log` en el servidor y en los clientes para verificar que los datos fluyen como esperas. Ajusta tu código hasta que funcione correctamente.

:::note[🧐🧪✍️ Reporta en tu bitácora]
*   Describe claramente el **concepto de interacción** que elegiste implementar. ¿Qué se supone que hace?
*   Explica los **cambios clave** que realizaste en `mobile/sketch.js`: ¿Qué datos envías ahora y cómo/cuándo los envías? Pega fragmentos de código relevantes.
*   Explica los **cambios clave** que realizaste en `desktop/sketch.js`: ¿Cómo recibes e interpretas los datos? ¿Qué modificaste en `setup()` o `draw()` para lograr el nuevo efecto? Pega fragmentos de código relevantes.
*   Si modificaste `server.js`, explica por qué fue necesario y qué cambiaste.
:::

:::caution[📤 Entrega]
*   Copia cada uno de los códigos: server, cliente móvil y cliente de escritorio.
*   Incluye en tu bitácora la descripción del concepto y la explicación de los cambios (con código).
*   Añade un **ENLACE** a un video corto mostrando tu nueva interacción en funcionamiento. NO OLVIDES: un enlace al 
    video, no el video
:::

