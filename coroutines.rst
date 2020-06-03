Corrutinas
----------

Las corrutinas son similares a los generadores pero tienen ciertas diferencias. Las principales son las siguientes:

- Los generadores son productores de datos
- Las corrutinas son consumidores de datos


Antes de nada, vamos a revisar como se creaba un generador. Podemos hacerlo de la siguiente manera:

.. code:: python

    def fib():
        a, b = 0, 1
        while True:
            yield a
            a, b = b, a+b

A modo de breve recordatorio, el uso de ``yield`` retorna de la función a donde fue llamada, pero si es vuelta a llamar continúa su ejecución inmediatamente después del ``yield``.

Ahora podemos usarlo en un bucle ``for`` como se muestra a continuación:

.. code:: python

    for i in fib():
        print(i)

Es rápido y no consume demasiada memoria ya que genera los valores al vuelo (uno a uno) en vez de almacenarlos todos en una lista. Ahora, si usamos ``yield`` en el anterior ejemplo, tendremos una corrutina. Las corrutinas consumen los valores que le son enviados. Un ejemplo muy sencillo sería un ``grep`` en Python:

.. code:: python

    def grep(pattern):
        print("Buscando", pattern)
        while True:
            line = (yield)
            if pattern in line:
                print(line)

Pero espera, ¿qué es lo que devuelve ``yield``? Bueno, en realidad lo que hemos hecho es convertirlo en una corrutina. No contiene ningún valor inicialmente, sino que proporcionamos esos valores externamente. Los valores son proporcionados usando el método ``.send()``. Aquí podemos ver un ejemplo:

.. code:: python

    search = grep('coroutine')
    next(search)
    # Salida: Buscando coroutine
    search.send("I love you")
    search.send("Don't you love me?")
    search.send("I love coroutines instead!")
    # Salida: I love coroutines instead!

Por lo tanto cuando enviamos una línea con `.send()`, si cumple con el *pattern* o patrón que hemos indicado al llamar a la función ``grep()`` será impresa por pantalla.

Los valores enviados son accedidos por ``yield``. Tal vez te preguntes sobre el uso de ``next()``. Es requerido para empezar la corutina. Al igual que los ``generators``, las corutinas no empiezan inmediatamente, sino que se ejecutan en respuesta a los métodos ``__next__()`` y ``.send()``. Por lo tanto tienes que ejecutar ``next()`` para que la ejecución avance hasta la expresión ``yield``.

Por otro lado, podemos cerrar la corrutina llamando al método ``.close()`` como se muestra a continuación:

.. code:: python

    search = grep('coroutine')
    # ...
    search.close()

Las ``coroutines`` van mucho más allá de lo que hemos explicado, por lo que te sugerimos que eches un vistazo a estar increíble presentación http://www.dabeaz.com/coroutines/Coroutines.pdf de
David Beazley (en Inglés).
