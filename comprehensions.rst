Comprensión
--------------

La comprensión o *comprehensions* en Python son una de las características que una vez sabes usarlas, echarias mucho de menos si las quitaran. Se trata de un tipo de construcción que permite crear secuencias a partir de otras secuencias. Existen diferentes *comprehensions* soportadas en Python 2 y Python 3:

-  Comprensión de listas
-  Comprensión de diccionarios
-  Comprensión de sets
-  Comprensión de generadores

A continuación las explicaremos una por una. Una vez que entiendes el uso con las listas, cualquiera de las otras será entendia my fácilmente.

Comprensión de ``list``
^^^^^^^^^^^^^^^^^^^^^^^

Las comprensiones de listas nos proporcionan una forma corta y concisa de crear listas. Se usan con corchetes ``[]`` y en su interior contienen una expresión seguida de un bucle ``for`` y cero o más sentencias ``for`` o ``if``. La expresión puede ser cualquier cosa que se te ocurra, lo que significa que puedes usar cualquier tipo de objetos en la lista. El resultado es una nueva lista creada tras evaluar las expresiones que haya dentro.

**Uso**

.. code:: python

    variable = [out_exp for out_exp in input_list if out_exp == 2]

Aquí mostramos un ejemplo:

.. code:: python

    multiples = [i for i in range(30) if i % 3 == 0]
    print(multiples)
    # Salida: [0, 3, 6, 9, 12, 15, 18, 21, 24, 27]

Esto puede ser realmente útil para crear listas de manera rápida. De hecho hay gente que las prefiere sobre el uso de la función ``filter``. Las comprensiones de listas son la mejor opción si por ejemplo quieres añadir elementos a una lista fruto de un bucle ``for``. Si queremos hacer algo como lo siguiente:

.. code:: python

    squared = []
    for x in range(10):
        squared.append(x**2)

Se podría simplificar en una línea de código con el uso de las comprensiones de listas:

.. code:: python

    squared = [x**2 for x in range(10)]

``dict`` comprehensions
^^^^^^^^^^^^^^^^^^^^^^^

Los diccionarios se usan de una manera muy similar. Aquí vemos un ejemplo:

.. code:: python

    mcase = {'a': 10, 'b': 34, 'A': 7, 'Z': 3}

    mcase_frequency = {
        k.lower(): mcase.get(k.lower(), 0) + mcase.get(k.upper(), 0)
        for k in mcase.keys()
    }

    # mcase_frequency == {'a': 17, 'z': 3, 'b': 34}

En el ejemplo anterior combinamos los valores de las llaves o *keys* del diccionario que sean las mismas pero en mayúsculas o minúsculas. Es decir, el contenido de 'a' y 'A' se juntaría.

Otro ejemplo podría ser invertir las llaves y valores de un diccionario como se muestra a continuación:

.. code:: python

    {v: k for k, v in some_dict.items()}

``set`` comprehensions
^^^^^^^^^^^^^^^^^^^^^^

Las comprensiones en los sets son muy similares a las listas. La única diferencia es que es necesario hacer uso de llaves ``{}`` en vez de corchetes.

.. code:: python

    squared = {x**2 for x in [1, 1, 2]}
    print(squared)
    # Output: {1, 4}

``generator`` comprehensions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Por último, tenemos los generadores. La única diferencia es que no asignan memoria para toda la lista, sino que la asignan elemento a elemento, lo que las hace mas eficientes desde el punto de vista del uso de la memoria.

.. code:: python

    multiples_gen = (i for i in range(30) if i % 3 == 0)
    print(multiples_gen)
    # Salida: <generator object <genexpr> at 0x7fdaa8e407d8>
    for x in multiples_gen:
      print(x)
      # Salida numbers
