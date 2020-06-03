Método mágico \_\_slots\_\_
-------------------

En Python cualquier clase tiene atributos de instancia. Por defecto se usa un diccionario para almacenar los atributos de un determinado objeto, y esto es algo muy útil que permite por ejemplo crear nuevos atributos en tiempo de ejecución.

Sin embargo, para clases pequeñas con atributos conocidos, puede llegar a resultar un cuello de botella. El uso del diccionario ``dict`` desperdicia un montón de memoria RAM y Python no puede asignar una cantidad de memoria estática para almacenar los atributos. Por lo tanto, se come un montón de RAM si creas muchos objetos (del orden de miles o millones). Por suerte hay una forma de solucionar esto, haciendo uso de ``__slots__``, que permite decirle a Python que no use un diccionario y que solo asigne memoria para una cantidad fija de atributos. Aquí mostramos un ejemplo del uso de ``__slots__``:

**Sin usar** ``__slots__``:

.. code:: python

    class MiClase(object):
        def __init__(self, nombre, identificador):
            self.nombre = nombre
            self.identificador = identificador
            self.iniciar()
        # ...

**Usando** ``__slots__``:

.. code:: python

    class MiClase(object):
        __slots__ = ['nombre', 'identificador']
        def __init__(self, nombre, identificador):
            self.nombre = nombre
            self.identificador = identificador
            self.iniciar()
        # ...

El segundo código reducirá el uso de RAM. En alguna ocasiones se han reportado reducciones de hasta un 40 o 50% usando esta técnica.

Como nota adicional, tal vez quieras echar un vistazo a PyPy, ya que hace este tipo de optimizaciones por defecto.

En el siguiente ejemplo puedes ver el uso exacto de memoria con y sin ``__slots__``hecho en IPython gracias a https://github.com/ianozsvald/ipython_memory_usage

.. code:: python

	Python 3.4.3 (default, Jun  6 2015, 13:32:34)
	Type "copyright", "credits" or "license" for more information.

	IPython 4.0.0 -- An enhanced Interactive Python.
	?         -> Introduction and overview of IPython's features.
	%quickref -> Quick reference.
	help      -> Python's own help system.
	object?   -> Details about 'object', use 'object??' for extra details.

	In [1]: import ipython_memory_usage.ipython_memory_usage as imu

	In [2]: imu.start_watching_memory()
	In [2] used 0.0000 MiB RAM in 5.31s, peaked 0.00 MiB above current, total RAM usage 15.57 MiB

	In [3]: %cat slots.py
	class MyClass(object):
		__slots__ = ['name', 'identifier']
		def __init__(self, name, identifier):
			self.name = name
			self.identifier = identifier

	num = 1024*256
	x = [MyClass(1,1) for i in range(num)]
	In [3] used 0.2305 MiB RAM in 0.12s, peaked 0.00 MiB above current, total RAM usage 15.80 MiB

	In [4]: from slots import *
	In [4] used 9.3008 MiB RAM in 0.72s, peaked 0.00 MiB above current, total RAM usage 25.10 MiB

	In [5]: %cat noslots.py
	class MyClass(object):
		def __init__(self, name, identifier):
			self.name = name
			self.identifier = identifier

	num = 1024*256
	x = [MyClass(1,1) for i in range(num)]
	In [5] used 0.1758 MiB RAM in 0.12s, peaked 0.00 MiB above current, total RAM usage 25.28 MiB

	In [6]: from noslots import *
	In [6] used 22.6680 MiB RAM in 0.80s, peaked 0.00 MiB above current, total RAM usage 47.95 MiB

Se puede ver una clara reducción en el uso de RAM 9.3008 MiB vs 22.6680 MiB.
