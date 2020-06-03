Mutabilidad
--------

Los tipos mutables e inmutables en Python son conceptos que causan verdaderos quebraderos de cabeza a los programadores. En otras palabras, **mutable** significa "que puede cambiar" e **inmutable** significa "que es constante". ¿Quieres ver lo enrevesado que puede parecer si no se entiende correctamente? Veamos un ejemplo:

.. code:: python

    foo = ['hola']
    print(foo)
    # Salida: ['hola']

    bar = foo
    bar += ['adios']
    print(foo)
    # Output: ['hola', 'adios']

¿Que ha ocurrido? Hemos modificado la variable ``bar``, pero la variable ``foo`` también ha sido modificada. Tal vez te esperabas algo como lo que mostramos a continuación:

.. code:: python

    foo = ['hola']
    print(foo)
    # Salida: ['hola']

    bar = foo
    bar += ['adios']

    print(foo)
    # Salida esperada: ['hola']
    # Salida: ['hola', 'adios']

    print(bar)
    # Salida: ['hola', 'adios']

Evidentemente no se trata de un fallo, sino que lo que estamos viendo es la mutabilidad en acción. Cuando asignas una variable a otra variable que es de tipo mutable, cualquier cambio que hagas sobre la segunda, afectará a la primera y viceversa. La variable nueva que hemos creado al hacer ``bar = foo`` es simplemente un *alias* de la primera. Por lo tanto cualquier modificación sobre ``bar`` afectará también a ``foo``. No obstante, esto es solo cierto para los tipos mutables.

Veamos otro ejemplo:

.. code:: python

    def agrega(num, target=[]):
        target.append(num)
        return target

    agrega(1)
    # Salida: [1]

    agrega(2)
    # Salida: [1, 2]

    agrega(3)
    # Salida: [1, 2, 3]

Tal vez te esperabas otro comportamiento, ya que en cada llamada a ``agrega`` estamos creando una lista nueva vacía. Sería razonable esperar que la salida fuera la siguiente:

.. code:: python

    def agrega(num, target=[]):
        target.append(num)
        return target

    agrega(1)
    # Salida: [1]

    agrega(2)
    # Salida: [2]

    agrega(3)
    # Salida: [3]

Otra vez, estamos viendo la mutabilidad en acción. En Python, los argumentos por defecto se evalúan una vez que la función ha sido definida, no cada vez que la función es llamada. Por lo tanto, nunca deberías definir un argumento por defecto de un tipo mutable, a menos que realmente estés seguro de lo que estas haciendo. El siguiente ejemplo sería más correcto:

.. code:: python

    def agrega(element, target=None):
        if target is None:
            target = []
        target.append(element)
        return target

Ahora cada vez que llamamos a la función sin el argumento ``target``, una nueva lista será creada. Por ejemplo:

.. code:: python

    agrega(42)
    # Salida: [42]

    agrega(42)
    # Salida: [42]

    agrega(42)
    # Salida: [42]

