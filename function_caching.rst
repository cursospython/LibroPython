Caching de Funciones
----------------

El *caching* de funciones permite almacenar el valor de retorno de una función dependiendo de los argumentos de entrada. Puede ahorrar tiempo cuando una determinada función es llamada con los mismos argumentos de entrada una y otra vez. En versiones anteriores a Python 3.2, teníamos que implementarlo a mano, pero de Python 3.2 en adelante, tenemos un decorador ``lry_cache`` que permite almacenar y eliminar el caché de retorno de una determinada función.

**Nota**: Si tienes alguna duda sobre el uso de las funciones en Python, te recomendamos `este post <https://cursospython.com/funciones-en-python/>`__

Veamos como se puede hacer en diferentes versiones de Python.

Python 3.2+
^^^^^^^^^^^

Vamos a implementar una función que calcule la sucesión de Fibonacci usando ``lru_cache``.

.. code:: python

    from functools import lru_cache

    @lru_cache(maxsize=32)
    def fib(n):
        if n < 2:
            return n
        return fib(n-1) + fib(n-2)

    >>> print([fib(n) for n in range(10)])
    # Salida: [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]

El argumento ``maxsize`` indica a ``lru_cache`` el número de valores de retorno a almacenar en caché.

Podemos fácilmente limpiar el caché usando:

.. code:: python

    fib.cache_clear()

Python 2+
^^^^^^^^^

En Python 2+, existen un par de formas de conseguir el mismo efecto. Puedes crear crear tú mismo el mecanismo de caché, que dependerá de tus necesidades. Aquí te mostramos un ejemplo genérico. Se crea un decorador que aplicado a una función hace que al llamarla se busque en ``memo`` por los argumentos de entrada. Si no se encuentran, llama a la función y los almacena.

.. code:: python

    from functools import wraps

    def memoize(function):
        memo = {}
        @wraps(function)
        def wrapper(*args):
            try:
                return memo[args]
            except KeyError:
                rv = function(*args)
                memo[args] = rv
                return rv
        return wrapper

    @memoize
    def fibonacci(n):
        if n < 2: return n
        return fibonacci(n - 1) + fibonacci(n - 2)

    fibonacci(25)

**Nota:** memoize no funcionará con tipos mutables (o *unhashable*) (diccionarios, listas, etc...). Sólo funcionará con tipos inmutables, así que ten esto en cuenta.

`En el siguiente enlace <https://www.caktusgroup.com/blog/2015/06/08/testing-client-side-applications-django-post-mortem/>`__
tienes un artículo de Caktus Group en el que encuentran un fallo en Django debido al uso de ``lru_cache``. Es bastante interesante, échale un vistazo.
