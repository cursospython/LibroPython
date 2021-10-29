Generadores
----------

Antes de nada, veamos un repaso de los iteradores. De acuerdo con Wikipedia, un iterador es un objeto que permite a un programador recorrer un contenedor, como podría ser una lista. Sin embargo, los iteradores recorren el contenedor y proveen acceso a los elementos del mismo, pero no realizan la iteración propiamente dicha. Hay tres conceptos que es necesario entender:

-  Iterable
-  Iterador
-  Iteración

Todos ellos están relacionados entre sí. A continuación los explicaremos unos por uno.

Iterable
^^^^^^^^

Un ``iterable`` es cualquier objeto en Python que implementa el método ``__iter__`` o ``__getitem__``, es decir, que devuelve un iterador que puede ser indexado. Puedes leer más acerca de esto `aquí <https://stackoverflow.com/a/20551346>`_. En otras palabras, un ``iterable`` es un objeto que nos puede proporcionar un **iterador**. ¿Pero qué es un **iterador**?


Iterador
^^^^^^^^
Un iterador es cualquier objeto en Python que tenga definidos los métodos ``next`` (Python 2) o ``__next__``. Sabido esto, vamos a ver ahora que es una **iteración**.


Iteración
^^^^^^^^^

La iteración es el proceso que seguimos cuando vamos tomando diferentes elementos de una lista, es decir, la vamos iterando. Cuando usamos un bucle para iterar sobre un elemento determinado, esto es una iteración. Es el nombre que se le da al proceso.

Una vez sabido esto, vamos a ver su relación con los **generadores**.


Generadores
^^^^^^^^^^

Los generadores son en realidad iteradores, pero sólo permiten ser iterados una vez. Esto se debe a que no almacenan todos los valores en memoria, sino que los van generando al vuelo. Pueden ser usados de dos formas diferentes, iterándolos con un bucle ``for`` o pasándolos a una función como veremos a continuación.

La mayoría de las veces, los ``generadores`` son implementados como funciones. Sin embargo no devuelven los valores con ``return`` sino que lo hacen usando ``yield``. Veamos un ejemplo sencillo de una función generadora.

.. code:: python

    def funcion_generadora():
        for i in range(10):
            yield i

    for item in funcion_generadora():
        print(item)

    # Salida: 0
    # 1
    # 2
    # 3
    # 4
    # 5
    # 6
    # 7
    # 8
    # 9

No se trata de un ejemplo demasiado práctico, pero sirve para ilustrar su funcionamiento. Los generadores son más útiles cuando es necesario realizar cálculos para un número muy elevado de elementos. Es decir, son útiles cuando no quieres tener en memoria todos los elementos a la vez, ya que sería demasiado. De hecho, muchas de las funciones de la librería estándar que devolvían listas en Python 2 han sido modificadas para devolver **generadores** en Python 3, porque se requiere de menos recursos.

Otro ejemplo con generadores para calcular la serie de fibonacci:

.. code:: python

    # Usando generadores
    def fibon(n):
        a = b = 1
        for i in range(n):
            yield a
            a, b = b, a + b

Ahora podemos realizar lo siguiente:

.. code:: python

    for x in fibon(1000000):
        print(x)

De esta manera no nos tenemos que preocupar si usaremos demasiados recursos. Sin embargo, implementado de la siguiente forma, podríamos llegar a tener problemas:

.. code:: python

    def fibon(n):
        a = b = 1
        resultado = []
        for i in range(n):
            resultado.append(a)
            a, b = b, a + b
        return resultado

Si con el ejemplo anterior usáramos como entrada un número muy elevado, podríamos llegar a tener problemas.

Hasta ahora hemos explicado el uso de los ``generators`` pero no hemos llegado a probarlos. Antes de probarlos, es necesario saber un poco más acerca de la función ``next()`` de Python. Esta función nos permite acceder al siguiente elemento de una secuencia:

.. code:: python

    def funcion_generadora():
        for i in range(3):
            yield i

    gen = funcion_generadora()
    print(next(gen))
    # Salida: 0
    print(next(gen))
    # Salida: 1
    print(next(gen))
    # Salida: 2
    print(next(gen))
    # Salida: Traceback (most recent call last):
    #            File "<stdin>", line 1, in <module>
    #         StopIteration

Como podemos ver, cuando se llega al final de la función, si se intenta llamar otra vez al ``next()`` tendremos un error ``StopIteration``, ya que no hay más valores. Esto se debe a que la función no tiene más valores de los que hacer ``yield``, es decir se ha llegado al final.


Tal vez te preguntes porque no pasa esto cuando usamos un bucle ``for``. La respuesta es muy sencilla, el bucle ``for`` se encarga automáticamente de capturar este error y de no llamar más a ``next``. ¿Sabías que algunas funciones que vienen por defecto también soportan ser iteradas? Vamos a verlo:

.. code:: python

    cadena = "Pelayo"
    next(cadena)
    # Salida: cadena (most recent call last):
    #      File "<stdin>", line 1, in <module>
    #    TypeError: str object is not an iterator

Tal vez no era eso lo que nos esperábamos. El error dice que ``str`` (la cadena) no es un elemento iterador. Bueno, eso es cierto, ya que se trata de un elemento iterable pero no es un iterador. Esto significa que soporta ser iterado pero que no puede ser iterado directamente. Entonces, ¿cómo lo iteramos? Veamos como usar la función ``iter``, que devuelve un objeto iterador (``iterator``) de una clase iterable.

Entonces, el tipo numérico entero ``int`` no es iterable, pero una cadena si que lo es. Veamos el ejemplo para el ``int``:

.. code:: python

    int_var = 1779
    iter(int_var)
    # Salida: Traceback (most recent call last):
    #   File "<stdin>", line 1, in <module>
    # TypeError: 'int' object is not iterable
    # Sucede ya que no es iterable
    
    my_string = "Pelayo"
    my_iter = iter(my_string)
    print(next(my_iter))
    # Salida: 'P'

Podemos ver entonces como si llamamos a ``iter`` sobre un tipo entero, tendremos un error, ya que los enteros no son iterables. Sin embargo, si realizamos lo mismo con una cadena, nos devolverá un iterador sobre el que podemos usar ``next()`` para ir accediendo secuencialmente a sus valores hasta llegar al final.

Una vez explicado esto, esperamos que hayas entendido los ``generators`` y los conceptos asociados como el iterador o que una clase sea iterable. Los generadores son sin duda una herramienta muy potente, por lo que te recomendamos que tengas los ojos abiertos porque seguramente encontrarás alguna aplicación donde te ayuden a resolver un problema.
