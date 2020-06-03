Map, Filter y Reduce
------------
Estas tres funciones proporcionan un enfoque funcional a la programación. Si no sabes que es la programación funcional, te recomendamos que leas acerca de ello, ya que es un mundo muy interesante. A continuación explicamos ``map``, ``reduce`` y ``filter`` con varios ejemplos.

Map
^^^^^^

El uso de ``map`` aplica una determinada función a todos los elementos de una entrada o lista. Esta es su forma:

**Forma**

.. code:: python

    map(funcion_a_aplicar, lista_de_entradas)

Se trata de un caso de uso bastante recurrente. Imaginemos por ejemplo que tenemos una lista y queremos crear otra lista con todos sus elementos elevados al cuadrado. La primera forma que tal vez se nos ocurra, sería la siguiente:

.. code:: python

    lista = [1, 2, 3, 4, 5]
    al_cuadrado = []
    for i in lista:
        al_cuadrado.append(i**2)

Sin embargo, existe una forma más fácil de hacerlo con ``map``. Es mucho más sencilla y corta:

.. code:: python

    lista = [1, 2, 3, 4, 5]
    al_cuadrado = list(map(lambda x: x**2, lista))

La mayoría de las veces ``map`` es usado conjuntamente con funciones lambda. Si no sabes lo que son, las explicamos en otro capítulo.

Otra forma de usar ``map`` es teniendo una lista de funciones en vez de una en concreto. Veamos un ejemplo:

.. code:: python

    def multiplicar(x):
        return (x*x)
    def sumar(x):
        return (x+x)

    funcs = [multiplicar, sumar]
    for i in range(5):
        valor = list(map(lambda x: x(i), funcs))
        print(valor)

    # Salida:
    # [0, 0]
    # [1, 2]
    # [4, 4]
    # [9, 6]
    # [16, 8]

Se puede ver como ahora para cada elemento (del 0 al 4) tenemos dos salida, la primera aplica la función ``multiplicar`` y la segunda ``sumar``.

Filter
^^^^^^^^^
Como su nombre indica, ``filter`` crea una lista de elementos si usados en la llamada a una función devuelven ``True``. Es decir, filtra los elementos de una lista usando un determinado criterio. Veamos un ejemplo:

.. code:: python

    lista = range(-5, 5)
    menor_cero = list(filter(lambda x: x < 0, lista))
    print(menor_cero)

    # Salida: [-5, -4, -3, -2, -1]

La función ``filter`` es similar a un bucle, y de hecho podríamos conseguir lo mismo con un bucle y un ``if``, pero su uso es más rápido.

**Nota:** Si no te gusta el uso de map y filter, echa un vistazo a las *list comprehensions* de las que hablamos en otro capítulo.

Reduce
^^^^^^^^^

Por último, ``reduce`` es muy útil cuando queremos realizar ciertas operaciones sobre una lista y devolver su resultado. Por ejemplo, si queremos calcular el producto de todos los elementos de una lista, y devolver un único valor, podríamos hacerlo de la siguiente forma sin usar ``reduce``.

.. code:: python

    producto = 1
    lista = [1, 2, 3, 4]
    for num in lista: 
        producto = producto * num
    
    # producto = 24


Ahora vamos a hacerlo con ``reduce``.

.. code:: python

    from functools import reduce
    producto = reduce((lambda x, y: x * y), [1, 2, 3, 4])

    # Salida: 24
