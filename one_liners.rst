Ejemplos en 1 línea
----------------

En este capítulo veremos algunos ejemplos en Python que pueden ser escritos en una sola línea de código.

**Servidor Web**

¿Alguna vez has querido enviar un fichero a través de la red? En Python se puede hacer de manera muy fácil de la siguiente forma. Vete al directorio donde tengas el fichero, y escribe el siguiente código.

.. code:: python

    # Python 2
    python -m SimpleHTTPServer

    # Python 3
    python -m http.server

**Prints Organizados**

Algo muy común a lo que a veces nos enfrentamos, es tener que imprimir un determinado tipo con ``print()``, pero a veces nos encontramos con un contenido que es prácticamente imposible de leer. Supongamos que tenemos un diccionario. A continuación mostramos como imprimirlo de una manera más organizada. Para ello usamos ``pprint()`` que viene de *pretty* (bonito).

.. code:: python

    from pprint import pprint

    my_dict = {'name': 'Pelayo', 'age': 'undefined', 'personality': 'collaciu'}
    print(dir(my_dict))
    # ['__add__', '__class__', '__contains__', '__delattr__', '__delitem__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__rmul__', '__setattr__', '__setitem__', '__sizeof__', '__str__', '__subclasshook__', 'append', 'clear', 'copy', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort']
    
    pprint(dir(my_dict))
    # ['__add__',
    #  '__class__',
    #  '__contains__',
    #  '__delattr__',
    #  '__delitem__',
    #  '__dir__',
    #  '__doc__',
    #  '__eq__',
    #  '__format__',
    #  '__ge__',
    #  '__getattribute__',
    #  '__getitem__',
    #  '__gt__',
    #  '__hash__',
    #  '__iadd__',
    #  '__imul__',
    #  '__init__',
    #  '__init_subclass__',
    #  '__iter__',
    #  '__le__',
    #  '__len__',
    #  '__lt__',
    #  '__mul__',
    #  '__ne__',
    #  '__new__',
    #  '__reduce__',
    #  '__reduce_ex__',
    #  '__repr__',
    #  '__reversed__',
    #  '__rmul__',
    #  '__setattr__',
    #  '__setitem__',
    #  '__sizeof__',
    #  '__str__',
    #  '__subclasshook__',
    #  'append',
    #  'clear',
    #  'copy',
    #  'count',
    #  'extend',
    #  'index',
    #  'insert',
    #  'pop',
    #  'remove',
    #  'reverse',
    #  'sort']

Usado en diccionarios anidados, resulta incluso más efectivo. Por otro lado, también puedes imprimir un fichero *json* con el siguiente comando.

.. code:: python

    cat file.json | python -m json.tool

**Profiling de un script**

Esto puede ser realmente útil para ver donde se producen los cuellos de botella de nuestro código. Se entiende por hacer *profiling* de un código, al analizar los tiempos de ejecución de sus diferentes partes, para saber dónde se pierde más tiempo y actuar en consecuencia.

.. code:: python

    python -m cProfile mi_script.py

Nota: ``cProfile`` es una implementación más rápida que ``profile`` ya que está escrito en C.

**Convertir CSV a json**

Si ejecutas esto en el terminal, puedes convertir un CSV a json.

.. code:: python

    python -c "import csv,json;print json.dumps(list(csv.reader(open('csv_file.csv'))))"

Asegúrate de que cambias ``csv_file.csv`` por tu fichero.

**Convertir una Lista anidada**

Puedes convertir una lista con elemento anidados a una única lista de una dimensión con ``itertools.chain.from_iterable`` del paquete ``itertools``. Veamos un ejemplo:

.. code:: python

    lista = [[1, 2], [3, 4], [5, 6]]
    print(list(itertools.chain.from_iterable(lista)))
    # Salida: [1, 2, 3, 4, 5, 6]
    
    # Otra forma 
    print(list(itertools.chain(*lista)))
    # Salida: [1, 2, 3, 4, 5, 6]


**Construcciones en 1 línea**

Otro código bastante interesante y que nos puede ahorrar varias líneas es el siguiente. Tenemos el constructor de una clase con un determinado número de parámetros. En vez de hacer ``self.nombre = nombre`` uno a uno, podemos reemplazarlo por la siguiente línea.

.. code:: python

    class A(object):
        def __init__(self, a, b, c, d, e, f):
            self.__dict__.update({k: v for k, v in locals().items() if k != 'self'})


Si quieres ver más construcciones de una línea, te recomendamos que leas el `siguiente enlace <https://wiki.python.org/moin/Powerful%20Python%20One-Liners>`__.
