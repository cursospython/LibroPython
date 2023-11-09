``for/else``
----------

Los *loops* o bucles son una parte muy importante de cualquier lenguaje de programación, y por supuesto también existen en Python. Sin embargo, tienen algunas particularidades que mucha gente no conoce. A continuación las explicaremos.

**Nota:** Si buscas una explicación más completa de los bucles ``for`` en Python te recomendamos `este post sobre el uso del for <https://cursospython.com/for-python/>`__ y `este otro para el while <https://cursospython.com/while-python/>`__.

Empecemos con un ejemplo básico de ``for``, nada nuevo:

.. code:: python

    frutas = ['manzana', 'plátano', 'mango']
    for fruta in frutas:
        print(fruta.capitalize())

    # Output: Manzana
    #         Plátano
    #         Mango

Un ejemplo sencillo en el que iteramos una lista que almacena diferentes cadenas con for, y cambiamos su primera letra con una mayúscula. Veamos ahora otras de las funcionalidades que tal vez no sean tan conocidas.

Uso del ``else``
^^^^^^^^^^^^^^^^^^^^

Los bucles ``for`` también tienen una cláusula ``else``, y puede ser usada para ejecutar un determinado fragmento de código cuando el bucle termina de manera natural. Por manera natural se entiende que el bucle ha sido ejecutado tantas veces como había sido planeado en su definición, y no termina por la sentencia ``break``. Por lo tanto, si un ``break`` rompe la ejecución del bucle, la cláusula ``else`` no será ejecutada.

Un ejemplo muy clásico en el uso de bucles, es iterar una determinada lista buscando un elemento concreto. Si el elemento se encuentra, es habitual usar ``break`` para dejar de buscar, ya que una vez hayamos encontrado lo que buscábamos, no tendría mucho sentido seguir buscando.

Por otro lado, podría ocurrir también que se acabara de iterar la lista y que no se hubiera encontrado nada. En este caso, el bucle terminaría sin pasar por la sentencia ``break``. Por lo tanto, una vez sabidos estos dos posibles escenarios, uno podría querer saber cual ha sido la causa por la que el bucle ha terminado, si ha sido porque se ha encontrado el elemento que se buscaba, o si por lo contrario se ha terminado sin encontrar nada.

Veamos un ejemplo de la estructura del ``for/else``:

.. code:: python

    for item in container:
        if busca_algo(item):
            # Se ha encontrado
            procesa(item)
            break
    else:
        # No se encontró nada
        no_encontrado()

Veamos un ejemplo en concreto, tomado de la documentación oficial.

.. code:: python

    for n in range(2, 10):
        for x in range(2, n):
            if n % x == 0:
                print(n, 'igual', x, '*', n/x)
                break

Este ejemplo itera números de 2 a 10, y para cada uno busca un número que divida de manera entera a cada uno de ellos. Si se encuentra, se rompe el primer bucle y se continúa con el siguiente número.

Al ejemplo anterior podemos añadir un bloque ``else``, para mostrar determinada información al usuario. Por ejemplo, si el número no es divisible por ninguno de sus antecesores, significará que es un número primo.

.. code:: python

    for n in range(2, 10):
        for x in range(2, n):
            if n % x == 0:
                print( n, 'igual', x, '*', n/x)
                break
        else:
            # Si no se llama a break, se entra al else
            print(n, 'es un número primo')
