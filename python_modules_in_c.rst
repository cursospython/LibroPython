Módulos de Python en C
----------------------

Todos sabemos que Python puede ser algo lento en ciertas ocasiones. Si lo quieres usar para realizar alguna tarea muy intensa en cálculos, puede que llegue a ser un cuello de botella. Entonces ¿qué opciones hay a parte de simplemente optimizar el código?

La primera solución es evidentemente optimizar el código que ya tenemos, aunque puede no ser suficiente. Afortunadamente Python tiene una solución perfecta para nosotros. Nos permite interactuar a través de un interfaz con C, por lo que las partes más críticas de nuestro código podrían ser escritas en este lenguaje más eficiente. Esto nos permite dar un extra de velocidad a nuestro código.

Existen varios módulos que han sido escritos en C para optimizar su velocidad, como cPicke, cProfile y c.