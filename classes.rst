Clases
-------

Las clases son el núcleo de Python. Nos dan un montón de poder, pero es muy fácil usarlo de manera incorrecta. En esta sección compartiremos algunos de los trucos relacionados con las clases en Python. ¡Vamos a por ello!

**Nota**: Si aún no entiendes bien la Programación Orientada a Objetos, te recomendamos que empieces antes `por este post <https://cursospython.com/programacion-orientada-a-objetos/>`__ dónde se explica de manera muy fácil la POO y conceptos relacionados como la `herencia <https://cursospython.com/herencia-en-python/>`__ y `los métodos estáticos y de clase <https://cursospython.com/metodos-estaticos-clase-python/>`__.


1. Variables de instancia y clase
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

La mayoría de principiantes o incluso algunos programadores avanzados de Python, no entienden la diferencia entre instancia y clase. Dicha falta de conocimiento, les fuerza a hacer un uso incorrecto de los mismos. Vamos a explicarlos.

La diferencia es la siguiente:

- Las variables de instancia son usadas para almacenar datos que son únicos para cada objeto.
- Por lo contrario, las variables de clase son compartidas entre diferentes instancias de la clase.

Vamos a ver un ejemplo:

.. code:: python

    class Cal(object):
        # pi es una variable de clase
        pi = 3.142

        def __init__(self, radio):
            # self.radio es una variable de instancia
            self.radio = radio

        def area(self):
            return self.pi * (self.radio ** 2)

    a = Cal(32)
    a.area()
    # Salida: 3217.408
    a.pi
    # Salida: 3.142
    a.pi = 43
    a.pi
    # Salida: 43

    b = Cal(44)
    b.area()
    # Salida: 6082.912
    b.pi
    # Salida: 3.142
    b.pi = 50
    b.pi
    # Salida: 50

En el ejemplo anterior no hay demasiados problemas al estar usando variables de clase que son inmutables, es decir que no son modificadas. Esta es una de las principales razones por la que ciertos programadores no intentan aprender mas acerca de ellas, ya que no se suelen enfrentar a ningún problema. En el siguiente ejemplo vemos como un mal uso de las variables de clase e instancia pueden causar problemas.

.. code:: python

    class SuperClass(object):
        superpowers = []

        def __init__(self, name):
            self.name = name

        def add_superpower(self, power):
            self.superpowers.append(power)

    foo = SuperClass('foo')
    bar = SuperClass('bar')
    foo.name
    # Salida: 'foo'

    bar.name
    # Salida: 'bar'

    foo.add_superpower('fly')
    bar.superpowers
    # Salida: ['fly']

    foo.superpowers
    # Salida: ['fly']

Esto es un mal uso de las variables de clase. Si te das cuenta la llamada ``add_superpower`` sobre el objeto ``foo`` modifica la variable de clase ``superpowers``, y dado que es compartida por todos los objetos de la clase, hace que ``bar`` también cambie. Por lo tanto es importante tener cuidado con esto, y salvo que realmente sepas lo que estás haciendo, no es muy recomendable usar variables de clase mutables.

2. Nuevo estilo de clases
^^^^^^^^^^^^^^^^^^^^

Un nuevo estilo de clases fue introducido en Python 2.1, pero mucha gente aún no sabe de ello. Puede ser en parte porque Python sigue manteniendo el antiguo estilo para mantener lo que se llama compatibilidad hacia atrás o *backward compatibility*. Veamos las diferencias:

- En el estilo antiguo, las clases no heredan de nada.
- En el nuevo estilo las clases heredan de ``object``.


Un ejemplo muy sencillo podría ser:

.. code:: python

    class OldClass():
        def __init__(self):
            print('I am an old class')

    class NewClass(object):
        def __init__(self):
            print('I am a jazzy new class')

    old = OldClass()
    # Salida: I am an old class

    new = NewClass()
    # Salida: I am a jazzy new class

Esta herencia de ``object`` permite que las clases pueden utilizar cierta *magia*. Una de las principales ventajas es que puedes hacer uso de diferentes optimizaciones como ``__slots__``. También puedes hacer uso de ``super()`` o de descriptores. ¿Conclusión? Intenta usar el nuevo estilo de clases.

**Nota:** Python 3 solo tiene el estilo nuevo de clases. No importa si heredas de ``object`` o no. Sin embargo es recomendable que heredes de ``object``, aunque tal vez en la práctica tampoco se hace.

3. Métodos mágicos
^^^^^^^^^^^^^^^^

Las clases en Python son famosas por sus métodos mágicos, comúnmente referidos con **dunder** que viene del Inglés y significa *double underscore*. Es decir, son métodos definidos con doble barra baja, tanto al principio con al final del nombre del mismo. Vamos a explicar algunos de ellos.

-  ``__init__``

Se trata de un inicializador de clase o también conocido como constructor. Cuando una instancia de una clase es creada, el método ``__init__`` es llamado. Por ejemplo:

.. code:: python

    class GetTest(object):
        def __init__(self):
            print('Saludos!!')
        def another_method(self):
            print('Soy otro método que no es llamado'
                  ' automáticamente')

    a = GetTest()
    # Salida: Saludos!!

    a.another_method()
    # Salida: Soy otro método que no es llamado automáticamente
    # called

Puedes ver como ``__init__`` es llamado inmediatamente después de que la instancia haya sido creada. También puedes pasar argumentos en la inicialización, como se muestra a continuación.

.. code:: python

    class GetTest(object):
        def __init__(self, name):
            print('Saludos!! {0}'.format(name))
        def another_method(self):
            print('Soy otro método que no es llamado'
                  ' automáticamente')

    a = GetTest('Pelayo')
    # Salida: Saludos!! Pelayo

    # Si intentas crear el objeto sin ningún argumento, da error.
    b = GetTest()
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: __init__() takes exactly 2 arguments (1 given)

Estoy seguro de que con esto ya entiendes perfectamente el método ``__init__``.

-  ``__getitem__``

Implementar el método ``__getitem__`` en una clase permite a la instancia usar ``[]`` para indexar sus elementos. Veamos un ejemplo:

.. code:: python

    class GetTest(object):
        def __init__(self):
            self.info = {
                'name':'Covadonga',
                'country':'Asturias',
                'number':12345812
            }

        def __getitem__(self,i):
            return self.info[i]

    foo = GetTest()

    foo['name']
    # Output: 'Covadonga'

    foo['number']
    # Output: 12345812

Sin implementar el método ``__getitem__`` tendríamos un error si intentamos hacerlo:

.. code:: python

    >>> foo['name']

    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: 'GetTest' object has no attribute '__getitem__'

.. Métodos abstractos, estáticos y de clase
.. ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

