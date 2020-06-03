Enumerados
---------

Python viene con un tipo por defecto denominado Enumerate. Permite asignar índices a elementos de, por ejemplo una lista. Veamos un ejemplo:


.. code:: python

    for contador, valor in enumerate(lista):
        print(contador, valor)

También acepta un parametro opcional que que lo hace aún más útil.

.. code:: python

    mi_lista = ['Ibias', 'Pesoz', 'Tineo', 'Boal']
    for c, valor in enumerate(mi_lista, 1):
        print(c, valor)

    # Salida:
    # 1 Ibias
    # 2 Pesoz
    # 3 Tinero
    # 4 Boal

Este argumento opcional nos permite decirle al ``enumerate`` el primer elemento del índice. También puedes creas tuplas que contengan el índice y la lista. Por ejemplo:

.. code:: python

    mi_lista = ['Ibias', 'Pesoz', 'Tineo', 'Boal']
    lista_contador = list(enumerate(mi_lista, 1))
    print(lista_contador)
    # Salida: [(1, 'Ibias'), (2, 'Pesoz'), (3, 'Tineo'), (4, 'Boal')]

