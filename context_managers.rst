Gestores de Contexto
----------------

Los gestores de contexto o *context managers* permiten asignar o liberar recursos de una forma expresa. El ejemplo más usado es el ``with``. Imagínate que tienes dos operaciones relacionadas que te gustaría ejecutar con un determinado código de por medio. Los gestores de contexto te permiten hacer precisamente esto. Veamos un ejemplo:

.. code:: python

    with open('fichero', 'w') as opened_file:
        opened_file.write('Hola!')

En el ejemplo anterior se abre el fichero, se escriben unos datos y se cierra automáticamente. Si se produce un error al intentar abrir el fichero o al intentar escribir contenido en el, el fichero se cierra al final. El siguiente código sería el equivalente con manejo de excepciones.

.. code:: python

    file = open('fichero', 'w')
    try:
        file.write('Hola!')
    finally:
        file.close()

Al comparar los ejemplos anteriores podemos ver que gran cantidad de código repetido es eliminado al usar ``with``. La principal ventaja del uso de ``with`` es que se asegura que el fichero se cierra, sin importar lo que hay en el bloque de código.

En general, los usos más comunes de los gestores de contexto son bloquear y liberar recursos, como en el ejemplo que acabamos de ver con un fichero.

Vamos a ver como podemos implementar nuestro propio gestor de contexto. Esto sin duda te permitirá entender que es lo que pasa por debajo.


Implementando un Gestor de Contexto I
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Todo gestor de contextos tiene que tener al menos unos métodos ``__enter__`` y un ``__exit__`` definidos. Vamos a crear nuestro propio gestor de contextos para abrir un fichero:

.. code:: python

    class File(object):
        def __init__(self, file_name, method):
            self.file_obj = open(file_name, method)
        def __enter__(self):
            return self.file_obj
        def __exit__(self, type, value, traceback):
            self.file_obj.close()

Una vez definidos los métodos ``__enter__`` y ``__exit__`` en nuestra clase ya podemos hacer uso del ``with`` de la misma forma que vimos anteriormente. Vamos a probarlo:

.. code:: python

    with File('demo.txt', 'w') as opened_file:
        opened_file.write('Hola!')

Nuestro método ``__exit__`` acepta tres argumentos, más adelante veremos porqué.

Pero antes, analicemos lo que pasa por debajo:

1. La sentencia ``with`` almacena el método ``__exit__`` de la clase ``File``.
2. Llama al método ``__enter__`` de la clase.
3. El método ``__enter__`` abre el fichero y lo devuelve.
4. El fichero abierto es pasado a ``opened_file``.
5. Escribimos en él usando ``.write()``.
6. La sentencia ``with`` llama al método ``__exit__``.
7. Por último el método ``__exit__`` cierra el fichero.


Manejando Excepciones
^^^^^^^^^^^^^^^^^^^

En el ejemplo anterior no hemos hablado sobre los argumentos ``type``, ``value`` y ``traceback`` que tenía el método ``__exit__``. Entre los pasos 4 y 6 anteriores, si ocurre una excepción, Python pasa estas tres variables al método ``__exit__``. Esto es lo que permite a ``__exit__`` decidir como cerrar el fichero y si realizar algún otro tipo de acción.

¿Que pasaría si tuviéramos una excepción? Por ejemplo, tal vez podríamos estar accediendo a a un método que no existe:

.. code:: python

    with File('demo.txt', 'w') as opened_file:
        # Este método no existe.
        opened_file.undefined_function('Hola!')


Veamos ahora todo lo que ocurre cuando ``with`` se encuentra con una excepción.

1. Se pasa el type, value y traceback del error al método ``__exit__``.
2. Se delega en el ``__exit__`` la gestión de la excepción.
3. Si ``__exit__`` devuelve ``True``, significa que la excepción ha sido manejada correctamente.
4. Si algo diferente a `True` es devuelto, una excepción es lanzada por la sentencia `with`.

En nuestro caso el método ``__exit__`` devuelve ``None`` (ya que no hemos especificado ningún valor de retorno). Por lo tanto y como hemos explicado, ``with`` lanzará la siguiente excepción:

.. code:: python

    Traceback (most recent call last):
      File "<stdin>", line 2, in <module>
    AttributeError: 'file' object has no attribute 'undefined_function'

Vamos a dar un paso más y manejar la excepción en el método ``__exit__``, ademas de devolver ``True``:

.. code:: python

    class File(object):
        def __init__(self, file_name, method):
            self.file_obj = open(file_name, method)
        def __enter__(self):
            return self.file_obj
        def __exit__(self, type, value, traceback):
            print("La excepción fue manejada")
            self.file_obj.close()
            return True

    with File('demo.txt', 'w') as opened_file:
        opened_file.undefined_function()

    # Output: La excepción fue manejada

Podemos ver ahora como ``__exit__`` devuelve `True`, por lo tanto ``with`` ya no lanza ninguna excepción.

Esta no es la única forma de implementar Gestor de Contexto. Existe otra forma que explicaremos en la siguiente sección.

Implementando un Gestor de Contexto II
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

También podemos implementar un gestor de contexto usando decoradores y generadores. Python viene con un módulo llamado ``contextlib`` para este propósito. En vez de crear una clase, podemos usar una función genérica. Veamos un ejemplo sencillo, aunque tal vez no muy útil.

.. code:: python

    from contextlib import contextmanager

    @contextmanager
    def open_file(name):
        f = open(name, 'w')
        try:
            yield f
        finally:
            f.close()

La verdad que esta forma de implementar el gestor de contexto parece mucho más fácil e intuitiva. Sin embargo esta forma requiere de algo de conocimiento previo acerca de los generadores, decoradores y la sentencia `yield`. En este ejemplo no hemos capturado ninguna excepción que pueda ocurrir.

Vamos a verlo parte por parte:

1. Python se encuentra con la palabra ``yield``, por lo que crea un generador en vez de una función normal.
2. Debido al uso del decorador, ``contexmanager`` es llamado con la función ``open_file`` como argumento.
3. El decorador ``contextmanager`` devuelve el generador envuelto con el objeto ``GeneratorContextManager``.
4. El ``GeneratorContextManager`` es asignado a la función ``open_file``. Por lo tanto, cuando llamamos a la función ``open_file`` estamos en realidad usando un objeto de la clase ``GeneratorContextManager``.


Ahora que ya sabemos esto, podemos usar nuestro nuevo gestor de contexto de la siguiente forma:

.. code:: python

    with open_file('some_file') as f:
        f.write('hola!')
