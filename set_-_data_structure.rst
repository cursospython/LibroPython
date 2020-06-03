Estructura de datos ``set``
----------------------

El ``set`` es una `estructura de datos muy usada <https://cursospython.com/sets-python/>`__. Los ``sets`` se comportan como las listas, con la diferencia de que no pueden contener elementos duplicados. También son inmutables, y una vez son definidos sus elementos no pueden ser modificados. Tampoco son ordenadores, por lo que no respetan el orden en el que son definidos. Son útiles si por ejemplo quieres ver si en una lista hay duplicados o no. Tienes dos opciones de hacerlo, donde la primera usa un bucle ``for``:

.. code:: python

    lista = ['a', 'b', 'c', 'b', 'd', 'm', 'n', 'n']

    duplicados = []
    for value in lista:
        if lista.count(value) > 1:
            if value not in duplicados:
                duplicados.append(value)

    print(duplicados)
    # Salida: ['b', 'n']

Pero hay una forma más simple y elegante de realizar la misma tarea usando los ``sets``. Lo vemos a continuación:

.. code:: python

    lista = ['a', 'b', 'c', 'b', 'd', 'm', 'n', 'n']
    duplicados = set([x for x in lista if lista.count(x) > 1])
    print(duplicados)
    # Salida: set(['b', 'n'])

Los sets también tienen otros métodos, vemos algunos a continuación.

**Intersección**

Podemos calcular la intersección entre dos sets de la siguiente manera.

.. code:: python

    set1 = set(['amarillo', 'rojo', 'azul', 'verde', 'negro'])
    set2 = set(['rojo', 'marrón'])
    print(set2.intersection(set1))
    # Salida: set(['rojo'])

**Diferencia**

Con el método ``difference`` podemos calcular la diferencia entre dos sets. Es importante notar que no es lo mismo la diferencia ``A-B`` que ``B-A``. En el siguiente caso se ve como la diferencia del set2 y el set1 son los elementos del set2 que no están presentes en el set1.

.. code:: python

    set1 = set(['amarillo', 'rojo', 'azul', 'verde', 'negro'])
    set2 = set(['rojo', 'marrón'])
    print(set2.difference(set1))
    # Salida: set(['marrón'])

También puedes crear sets usando ``{}`` como se muestra a continuación.

.. code:: python

    set1 = {'red', 'blue', 'green'}
    print(type(set1))
    # Salida: <type 'set'>

Existen otros métodos del ``set`` muy bien explicados en `este post <https://cursospython.com/sets-python/>`__.
