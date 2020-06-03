Usando Python 2+3
--------------------

En algunas ocasiones puede ser normal querer desarrollar programas que puedan funcionar en Python 2+ y Python 3+. Imagínate por ejemplo que has credo un módulo de Python usado por cientos de personas, pero no todos tienen Python 2 o 3. En este caso podrías tener dos opciones. La primera sería distribuir dos módulos, uno para Python 2 y otro para Python 3. La segunda sería modificar tu código para que funcionara con ambas versiones.

En esta sección vamos a ver algunos de los trucos que puedes usar para que tus programas sean compatibles con ambas versiones.

**Imports del futuro**

El primer y más importante método es usar los *imports* con ``__future__``. Permite importar funcionalidades de Python 3 en Python 2. Veamos un par de ejemplos:

Por ejemplo, los gestores de contexto o *context managers* se introdujeron en Python 2.6+. Para usarlos en Python 2.5 podrías hacer lo siguiente.

.. code:: python

    from __future__ import with_statement

Por otro lado, ``print`` fue cambiado a una función en Python 3. Si quieres usarlo en Python 2, podrías hacer lo siguiente haciendo uso de ``__future__``:

.. code:: python

    print
    # Salida:

    from __future__ import print_function
    print(print)
    # Salida: <built-in function print>

**Gestionando cambios de nombre en los módulos**

Antes de nada, veamos como podemos importar módulos en Python. Es muy común hacerlo de la siguiente manera.

.. code:: python

    import foo
    # o también
    from foo import bar

¿Sabes que otra cosa puedes hacer? Es posible también realizar lo siguiente:

.. code:: python

    import foo as foo

La funcionalidad es la misma que la anterior, pero es vital para hacer tu programa compatible con Python 2 y Python 3. Ahora examinemos el siguiente código:

.. code:: python

    try:
        import urllib.request as urllib_request  # Para Python 3
    except ImportError:
        import urllib2 as urllib_request  # Para Python 2

Lo primero, estamos realizando los *import* dentro de un ``try/excep`` `lee este post si tienes dudas sobre try o except <https://cursospython.com/excepciones-try-except-finally/>`__.

Hacemos esto ya que en Python 2 no existe el módulo ``urllib.request``, por lo que si intentamos importar tendremos un ``ImportError``. La funcionalidad de ``urllib.request`` es proporcionada por ``urllib2`` en Python 2. Por lo tanto, si usamos Python 2 intentaremos importar ``urllib.request`` y como dará un error, importaremos ``urllib2``.

También es importante mencionar el uso de la palabra clase ``as``. Es una forma de asignar un nombre al módulo importado, en nuestro caso ``urllib_request``. Por lo tanto si realizamos esto, todas las clases y métodos de ``urllib2`` estarán disponibles con el alias ``urllib_request``.

**Funciones obsoletas de Python 2**

Otra cosa muy importante a tener en cuenta es que hay un total de 12 funciones de Python 2 que han sido eliminadas de Python 3. Es importante asegurarse de que no se usan en Python 2, para hacer que el código sea compatible con Python 3. A continuación mostramos una forma que nos permite asegurarnos de que estas 12 funciones no son usadas.

.. code:: python

    from future.builtins.disabled import *

Ahora cada vez que usas una de las funciones que han sido eliminadas de Python 3, tendrás un error ``NameError`` como el que se muestra a continuación.

.. code:: python

    from future.builtins.disabled import *

    apply()
    # Salida: NameError: obsolete Python 2 builtin apply is disabled

**Librerías externas (backports)**

Existen algunos paquetes que proporcionan determinadas funcionalidades de Python 3 en Python 2. Tenemos por ejemplo las siguientes:

-  enum ``pip install enum34``
-  singledispatch ``pip install singledispatch``
-  pathlib ``pip install pathlib``

Para más información, te recomendamos `la documentación oficial de Python
<https://docs.python.org/3/howto/pyporting.html>`_ con los pasos que tienes que seguir para hacer tu código Python compatible entre las versiones 2 y 3.
