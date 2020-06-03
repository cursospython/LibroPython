Funciones Lambda
-------

Las funciones lambda son funciones que se definen en una línea, y son conocidas en otros lenguajes como funciones anónimas. Uno de sus usos es cuando tienes una determinada función que sólo vas a llamar una vez. Por lo demás, su uso y comportamiento es muy similar a las funciones "normales".

**Forma**

.. code:: python

    lambda argumentos: manipular(argumentos)

**Ejemplo**

.. code:: python

    suma = lambda x, y: x + y

    print(suma(3, 5))
    # Salida: 8

A continuación mostramos otras formas de usar las funciones lambda:

**Ordenar una lista**

.. code:: python

    a = [(1, 2), (4, 1), (9, 10), (13, -3)]
    a.sort(key=lambda x: x[1])

    print(a)
    # Salida: [(13, -3), (4, 1), (1, 2), (9, 10)]

**Ordenar listas paralelamente**

.. code:: python

    datos = zip(lista1, lista2)
    datos.sort()
    lista1, lista2 = map(lambda t: list(t), zip(*datos))

Si quieres saber más acerca de las funciones lambda, puedes encontrar más información `en este post post <https://cursospython.com/lambda-python/>`__.
