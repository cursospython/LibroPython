Introspección de objetos
--------------------

En el mundo de la programación, la **instrospección** es la habilidad para determinar el tipo de un objeto en tiempo de ejecución, y se trata de una de las mejores características de Python. En Python todo es un objeto, y podemos examinarlos de manera muy sencilla con las funciones por defecto que se nos proporcionan.

``dir``
^^^^^^^^^^^

A continuación explicaremos el uso de ``dir`` y como podemos usarla. Se trata de una de las funciones clave para la introspección de objetos en Python. Nos devuelve una lista con todos los atributos y métodos que un determinado objeto tiene.

.. code:: python

    mi_lista = [1, 2, 3]
    dir(mi_lista)
    # Salida: ['__add__', '__class__', '__contains__', '__delattr__', '__delitem__',
    # '__delslice__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__',
    # '__getitem__', '__getslice__', '__gt__', '__hash__', '__iadd__', '__imul__',
    # '__init__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__',
    # '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__rmul__',
    # '__setattr__', '__setitem__', '__setslice__', '__sizeof__', '__str__',
    # '__subclasshook__', 'append', 'count', 'extend', 'index', 'insert', 'pop',
    # 'remove', 'reverse', 'sort']

Como podemos ver, se nos devuelven todos los atributos y métodos en una lista. Esto puede ser útil si no recuerdas el nombre de un método y no tienes la documentación a mano. Si ejecutamos ``dir()`` sin ningún argumento, se nos devolverá todos los nombres en el *scope* actual.


``type`` y ``id``
^^^^^^^^^^^^^^^^^^^^^^^

La función ``type`` nos devuelve el tipo de un objeto:

.. code:: python

    print(type(''))
    # Salida: <type 'str'>

    print(type([]))
    # Salida: <type 'list'>

    print(type({}))
    # Salida: <type 'dict'>

    print(type(dict))
    # Salida: <type 'type'>

    print(type(3))
    # Salida: <type 'int'>

Por otro lado, ``id`` devuelve un *id* o identificador único para cada objeto.

.. code:: python

    nombre = "Pelayo"
    print(id(nombre))
    # Salida: 139972439030304

Módulo ``inspect``
^^^^^^^^^^^^^^^^^^^^^^

El módulo ``inspect`` nos proporciona diferentes funciones para consultar información de objetos. Por ejemplo, puedes consultar los miembros de un objeto ejecutando el siguiente código:

.. code:: python

    import inspect
    print(inspect.getmembers(str))
    # Salida: [('__add__', <slot wrapper '__add__' of ... ...

Existen también otros métodos para realizar introspección sobre objetos. Te recomendamos que consultes la documentación oficial y leas sobre ellos.