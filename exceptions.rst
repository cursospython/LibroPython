Excepciones
----------

El manejo de excepciones es un arte que una vez que entiendes resulta de lo mas útil. Vamos a ver como pueden ser manejadas en Python.

**Nota**: Si aún no sabes muy bien lo que son las excepciones, te recomendamos empezar por `este post <https://cursospython.com/excepciones-try-except-finally/>`__ `y este otro <https://cursospython.com/definir-excepcion/>`__, donde se explican de manera muy sencilla y didáctica.

Empecemos con el uso de ``try/except``. El código que puede causar una excepción se pone en el ``try`` y el código que maneja esa excepción se ubica en el bloque ``except``. Veamos un ejemplo:

.. code:: python

    try:
        file = open('test.txt', 'rb')
    except IOError as e:
        print('Ocurrió un IOError {}'.format(e.args[-1]))

En ejemplo anterior estamos manejando la excepción ``IOError``. Otra cosa que veremos a continuación es que en realidad podemos manejar varias excepciones.

Manejando múltiples excepciones:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Podemos manejar las excepciones de tres maneras distintas. La primera consiste en poner todas las excepciones que puedan ocurrir separadas por coma, en una tupla. Se muestra a continuación:

.. code:: python

    try:
        file = open('test.txt', 'rb')
    except (IOError, EOFError) as e:
        print("Ocurrió un error. {}".format(e.args[-1]))

Otra forma es manejar las excepciones de manera individual, creando un bloque ``except`` para cada una. Veamos un ejemplo:


.. code:: python

    try:
        file = open('test.txt', 'rb')
    except EOFError as e:
        print("Ocurrió un EOFError")
    except IOError as e:
        print("Ocurrió un IOError")

De esta manera, si la excepción no es manejada en el primer bloque, lo será en el segundo o en alguno de los sucesivos. Aunque también puede pasar que no llegue a manejarse en ninguno.

Y por último, el siguiente método permite manejar todas las excepciones con un solo bloque.


.. code:: python

    try:
        file = open('test.txt', 'rb')
    except Exception as e:
        # Puedes añadir algún tipo de información extra
        pass

Esto puede ser útil cuando no se sabe con certeza que excepciones pueden ser lanzadas por el programa.

Uso de ``finally``
~~~~~~~~~~~~~~~~~~

Ya hemos visto que debemos ubicar el código que pueda causar una excepción en el ``try``, y que en el ``except`` podemos tratar lo que hacer en el caso de que se produzca una excepción determinada. A continuación veremos el uso del ``finally``, que permite ejecutar un determinado bloque de código siempre, se haya producido o no una excepción. Se trata de un bloque muy importante, y que suele ser usado para ejecutar alguna tarea de limpieza. Veamos un ejemplo:

.. code:: python

    try:
        file = open('test.txt', 'rb')
    except IOError as e:
        print('Ocurrió un IOError. {}'.format(e.args[-1]))
    finally:
        print("Se entra aquí siempre, haya o no haya excepción")
        
    # Salida: Ocurrió un IOError. No such file or directory
    # Se entra aquí siempre, haya o no haya excepción

Uso de ``try/else``
~~~~~~~~~~~~~~~~~~~

Puede ser también útil tener una determinada sección de código que sea ejecutada si **no** se ha producido ninguna excepción. Esto se puede realizar con el uso de ``else``. Se trata de algo bastante útil porque puede haber determinadas secciones de código que sólo tengan sentido ejecutar si el bloque completo ``try`` se ha ejecutado correctamente. Si bien es cierto que no es muy habitual ver su uso, es una herramienta a tener en cuenta.

.. code:: python

    try:
        print('Estoy seguro de que no ocurrirá ninguna excepción')
    except Exception:
        print('Excepción')
    else:
        # El código de esta sección se ejecutará si no se produce
        # ninguna excepción. Las excepciones producidas aquí
        # tampoco serán capturadas.
        print('Esto se ejecuta si no ocurre ninguna excepción')
    finally:
        print('Esto se imprimirá siempre')

    # Salida: Estoy seguro de que no ocurrirá ninguna excepción
    #         Esto se ejecuta si no ocurre ninguna excepción
    #.        Esto se imprimirá siempre

El contenido del ``else`` sólo se ejecutará si no se ha producido ninguna excepción, y será ejecutada antes del ``finally``.