Uso de \*args y \*\*kwargs
---------------------

La mayoría de los programadores nuevos en Python tienen dificultades para entender el uso de \*args y \*\*kwargs. ¿Para qué se usan? Lo primero de todo es que en realidad no tienes porque usar los nombres args o kwargs, ya que se trata de una mera convención entre programadores. Sin embargo lo que si que tienes que usar es el asterisco simple ``*`` o doble ``**``. Es decir, podrías escribir \*variable y \*\*variables. Empecemos viendo el uso de \*args.

Uso de \*args
^^^^^^^^^^^^^^^

El principal uso de \*args y \*\*kwargs es en la definición de funciones. Ambos permiten pasar un número variable de argumentos a una función, por lo que si quieres definir una función cuyo número de parámetros de entrada puede ser variable, considera el uso de \*args o \**kwargs como una opción. De hecho, el nombre de args viene de argumentos, que es como se denominan en programación a los parámetros de entrada de una función.

A continuación te mostramos un ejemplo para que veas el uso de \*args:

.. code:: python

    def test_var_args(f_arg, *argv):
        print("primer argumento normal:", f_arg)
        for arg in argv:
            print("argumentos de *argv:", arg)

    test_var_args('python', 'foo', 'bar')

Y la salida que produce el código anterior al llamarlo con 3 parámetros es la siguiente:

.. code:: python

    primer argumento normal: python
    argumentos de *argv: foo
    argumentos de *argv: bar

Espero que esto haya aclarado el uso de \*args, continuemos con \*\*kwargs.

Uso de \*\*kwargs
^^^^^^^^^^^^^^^^

\*\*kwargs permite pasar argumentos de longitud variable asociados con un nombre o **key** a una función. Deberías usar \*\*kwargs si quieres manejar argumentos con nombre como entrada a una función. Aquí tienes un ejemplo de su uso.


.. code:: python

    def saludame(**kwargs):
        for key, value in kwargs.items():
            print("{0} = {1}".format(key, value))

    >>> saludame(nombre="Covadonga")
    nombre = Covadonga

Es decir, dentro de la función no solo tenemos acceso a la variable como con \*args, sino que también tenemos acceso a un nombre o key asociado. A continuación veremos como se puede usar \*args y \*\*kwargs para llamar a una función con una lista o diccionario como argumentos.

Usando \*args y \*\*kwargs para llamar a una función
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Ahora veremos como se puede llamar a una función usando \*args y \*\*kwargs. Consideremos la siguiente.

.. code:: python

    def test_args_kwargs(arg1, arg2, arg3):
        print("arg1:", arg1)
        print("arg2:", arg2)
        print("arg3:", arg3)

Ahora puedes usar \*args o \*\*kwargs para pasarle argumentos a la función. Se puede hacer de la siguiente manera:

.. code:: python

    # Primero con *args
    >>> args = ("dos", 3, 5)
    >>> test_args_kwargs(*args)
    arg1: dos
    arg2: 3
    arg3: 5

    # Ahora con **kwargs:
    >>> kwargs = {"arg3": 3, "arg2": "dos", "arg1": 5}
    >>> test_args_kwargs(**kwargs)
    arg1: 5
    arg2: dos
    arg3: 3

Por último, si quieres usar los tres tipos de argumentos de entrada a una función: normales, \*args y \*\*kwargs, deberás hacerlo en el siguiente orden.

.. code:: python

    funcion(fargs, *args, **kwargs)

¿Cuándo usarlos?
^^^^^^^^^^^^^^^^^

Dependerá mucho de los requisitos de tu programa, pero uno de los usos más comunes es para crear decoradores para funciones (que veremos en otro capítulo). También puede ser usado para *monkey patching*, lo que significa modificar código en tiempo de ejecución. Considera por ejemplo que tienes una clase con una función llamada ``get_info`` que llama a una API que devuelve una determinada respuesta. Si quieres testearla, se puede reemplazar la llamada a la API por unos datos de test, como por ejemplo:

.. code:: python

    import someclass

    def get_info(self, *args):
        return "Test data"

    someclass.get_info = get_info

Estoy seguro de que se te ocurren otros usos.
