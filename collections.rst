Colecciones
-----------

Python viene con un modulo que contiene varios contenedores de datos llamados colecciones o **collections** en Inglés. Hablaremos de algunos de ellos y de sus usos.

En concreto, hablaremos de los siguientes:

-  ``defaultdict``
-  ``OrderedDict``
-  ``counter``
-  ``deque``
-  ``namedtuple``
-  ``enum.Enum`` (fuera del módulo; Python 3.4+)

``defaultdict``
^^^^^^^^^^^^^^^^^^^

Personalmente uso **defaultdict** bastante. A diferencia de ``dict`` con ``defaultdict`` no tienes que verificar que una llave o *key* este presente. Es decir, puedes hacer lo siguiente:

.. code:: python

    from collections import defaultdict

    colours = (
        ('Asturias', 'Oviedo'),
        ('Galicia', 'Ourense'),
        ('Extremadura', 'Cáceres'),
        ('Galicia', 'Pontevedra'),
        ('Asturias', 'Gijón'),
        ('Cataluña', 'Barcelona'),
    )

    ciudades = defaultdict(list)

    for name, colour in colours:
        ciudades[name].append(colour)

    print(ciudades)

    # Salida
    # defaultdict(<type 'list'>,
    #    {'Extremadura': ['Cáceres'],
    #     'Asturias': ['Oviedo', 'Gijón'],
    #     'Cataluña': ['Silver'],
    #     'Galicia': ['Ourense', 'Pontevedra']
    # })

Una de las ocasiones en las que son más útiles, es si quieres añadir elementos a listas anidadas dentro de un diccionario. Si la llave o *key* no está ya presente en el diccionario, tendrás un error tipo ``KeyError``. El uso de ``defaultdict`` permite evitar este problema. Antes de nada, vamos a ver un ejemplo con ``dict`` que daría un error ``KeyError`` como hemos mencionado, y después veremos la solución usando ``defaultdict``.

**Problema:**

.. code:: python

    some_dict = {}
    some_dict['region']['ciudad'] = "Oviedo"
    # Raises KeyError: 'region'

**Solución:**

.. code:: python

    from collections import defaultdict
    tree = lambda: defaultdict(tree)
    some_dict = tree()
    some_dict['region']['ciudad'] = "Oviedo"
    # ¡Funciona!

Ahora podrías imprimir también el diccionario ``some_dict`` usando ``json.dumps``. Aquí tienes un ejemplo:

.. code:: python

    import json
    print(json.dumps(some_dict))
    # Output: {"region": {"ciudad": "Oviedo"}}

``OrderedDict``
^^^^^^^^^^^^^^^^^^^

``OrderedDict`` es un diccionario que mantiene ordenadas sus entradas según van siendo añadidas. Es importante saber también que sobreescribir un valor existente no cambia la posición de la llave o *key*. Sin embargo, eliminar y reinsertar una entrada mueve la llave al final del diccionario. 

**Problema:**

.. code:: python

    colours =  {"Rojo" : 198, "Verde" : 170, "Azul" : 160}
    for key, value in colours.items():
        print(key, value)
    # Salida:
    #   Verde 170
    #   Azul 160
    #   Rojo 198
    # Las entradas son recuperadas en un orden no predecible.
   
**Solución:**

.. code:: python

    from collections import OrderedDict
    
    colours = OrderedDict([("Rojo", 198), ("Verde", 170), ("Azul", 160)])
    for key, value in colours.items():
        print(key, value)
    # Output:
    #   Rojo 198
    #   Verde 170
    #   Azul 160
    # El orden de inserción se mantiene.

``counter``
^^^^^^^^^^^^^^^

El uso de ``counter`` nos permite contar el número de elementos que una llave tiene. Por ejemplo, puede ser usado para contar el número de colores favoritos de diferentes personas.

.. code:: python

    from collections import Counter

    colours = (
        ('Covadonga', 'Amarillo'),
        ('Pelayo', 'Azul'),
        ('Xavier', 'Verde'),
        ('Pelayo', 'Negro'),
        ('Covadonga', 'Rojo'),
        ('Amaya', 'Plata'),
    )

    favs = Counter(name for name, colour in colours)
    print(favs)
    # Salida: Counter({
    #    'Covadonga': 2,
    #    'Pelayo': 2,
    #    'Xavier': 1,
    #    'Amaya': 1
    # })

También podemos contar las líneas más comunes de un fichero, como por ejemplo:

.. code:: python

    with open('nombre_fichero', 'rb') as f:
        line_count = Counter(f)
    print(line_count)

``deque``
^^^^^^^^^^^^^

``deque`` proporciona una cola con dos lados, lo que significa que puedes añadir y eliminar elementos de cualquiera de los lados de la cola. Primero debes importar el módulo de la librería de colecciones o *collections*:


.. code:: python

    from collections import deque

Una vez importado ya podemos crear el objeto:

.. code:: python

    d = deque()

Tienen un comportamiento relativamente similar a las conocidas listas de Python, y sus métodos son también similares. Puedes hacer lo siguiente:

.. code:: python

    d = deque()
    d.append('1')
    d.append('2')
    d.append('3')

    print(len(d))
    # Salida: 3

    print(d[0])
    # Salida: '1'

    print(d[-1])
    # Salida: '3'

También puedes tomar elementos de los dos lados de la cola, una funcionalidad conocida como *pop*. Es importante notar que *pop* devuelve el elemento eliminado.

.. code:: python

    d = deque(range(5))
    print(len(d))
    # Salida: 5

    d.popleft()
    # Salida: 0

    d.pop()
    # Salida: 4

    print(d)
    # Salida: deque([1, 2, 3])

También podemos limitar la cantidad de elementos que la cola ``deque`` puede almacenar. Al hacer esto, simplemente quitará elementos del otro lado de la cola si el límite es superado. Se ve mejor con un ejemplo como se muestra a continuación:

.. code:: python

    d = deque([0, 1, 2, 3, 5], maxlen=5)
    print(d)
    # Salida: deque([0, 1, 2, 3, 5], maxlen=5)
    
    d.extend([6])
    print(d)
    #Salida: deque([1, 2, 3, 5, 6], maxlen=5)

Ahora cuando insertamos valores después del 5, la parte más a la izquierda será eliminada de la lista. También puedes expandir la lista en cualquier dirección con valores nuevos.

.. code:: python

    d = deque([1,2,3,4,5])
    d.extendleft([0])
    d.extend([6,7,8])
    print(d)
    # Salida: deque([0, 1, 2, 3, 4, 5, 6, 7, 8])

``namedtuple``
^^^^^^^^^^^^^^^^^^

Tal vez conozcas ya las tuplas, que son listas inmutables que permiten almacenar una secuencia de valores separados por coma. Son simplemente como las listas pero con algunas diferencias importantes. La principal es que a diferencia de las listas **no puedes reasignar el valor de un elemento** una vez inicializada. Para acceder a un índice de la tupla se hace de la siguiente manera:

.. code:: python

    man = ('Pelayo', 30)
    print(man[0])
    # Output: Pelayo

Sabido esto, ¿qué son las ``namedtuples``?. Se trata de un tipo que convierte las tuplas en contenedores bastante útiles para tareas simples. Con ellas, no necesitas usar índices enteros para acceder a los miembros de la misma. Puedes pensar en ellas como si fuesen diccionarios, con la salvedad de que son inmutables. Veamos un ejemplo.

.. code:: python

    from collections import namedtuple

    Animal = namedtuple('Animal', 'nombre edad tipo')
    perry = Animal(nombre="perry", edad=31, tipo="cat")

    print(perry)
    # Salida: Animal(nombre='perry', edad=31, tipo='cat')

    print(perry.nombre)
    # Salida: 'perry'

Puedes ver como es posible acceder a los elementos a través de su nombre, simplemente haciendo uso de ``.``. Vamos a verlo con más detalle. Una ``namedtuple`` requiere de dos argumentos. Estos son, el nombre de la tupla y los campos de la misma. En el ejemplo anterior hemos visto como el nombre de la tupla era 'Animal' y tenía tres atributos: 'nombre', 'edad' y 'tipo'.

Las ``namedtuple`` son muy útiles ya que hacen que las tuplas tengan una especie de documentación propia, y apenas sea necesaria una explicación de como usarlas, ya que puedes verlo con un simple vistazo al código. Además, dado que no es necesario usar índices, hace que sea más fácil de mantener.

Otra de las ventajas es que son bastante ligeras, y no necesitan mas memoria que las tuplas normales. Esto hace que sean mas rápidas que los diccionarios. Sin embargo, recuerda que los atributos de las tuplas son inmutables, por lo que no pueden ser modificados. El siguiente ejemplo no funcionaría:

.. code:: python

    from collections import namedtuple

    Animal = namedtuple('Animal', 'nombre edad tipo')
    perry = Animal(nombre="perry", edad=31, tipo="cat")
    perry.edad = 42

    # Salida: Traceback (most recent call last):
    #            File "", line 1, in
    #         AttributeError: can't set attribute

Deberías usar las ``namedtuple`` si quieres que tu código sea autodocumentado. Lo mejor de todo es que ofrecen compatibilidad con las tuplas, por lo que **puedes indexarlas como si de una tupla normal se tratase**. Veamos un ejemplo:

.. code:: python

    from collections import namedtuple

    Animal = namedtuple('Animal', 'nombre edad tipo')
    perry = Animal(nombre="perry", edad=31, tipo="cat")
    print(perry[0])
    # Salida: perry

Por último, aunque no por ello menos importante, puedes convertir una namedtuple en un diccionario. Se puede hacer de la siguiente manera:

.. code:: python

    from collections import namedtuple

    Animal = namedtuple('Animal', 'nombre edad tipo')
    perry = Animal(nombre="Perry", edad=31, tipo="cat")
    print(perry._asdict())
    # Salida: OrderedDict([('nombre', 'Perry'), ('edad', 31), ...

``enum.Enum`` (Python 3.4+)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Otra de las colecciones más útiles de Python es el tipo **enum**, que se encuentra disponible en el módulo ``enum`` desde Python 3.4 en adelante (también está disponible como *backport* en PyPI bajo el nombre ``enum32``). Los enums (`enumerated type <https://en.wikipedia.org/wiki/Enumerated_type>`_) son básicamente una forma de organizar aquellos nombres que puedan tomar un determinado número de estados limitados y claramente definidos.

Vamos a considerar el ejemplo anterior en namedtuples del Animal. Si recuerdas, había un campo denominado ``tipo``. El problema de este tipo es que era una cadena. ¿Qué pasaría si escribimos ``Gato`` o ``GATO``?

El uso de enum nos puede ayudar a resolver este problema, evitando por lo tanto usar cadenas. Veamos el siguiente ejemplo:

.. code:: python

    from collections import namedtuple
    from enum import Enum

    class Especies(Enum):
        gato = 1
        perro = 2
        caballo = 3
        lobo = 4
        mariposa = 5
        buho = 6
        # ¡Y muchos más!

        # Se pueden usar también alias
        gatito = 1
        perrito = 2

    Animal = namedtuple('Animal', 'name age type')
    perry = Animal(name="Perry", age=31, type=Especies.gato)
    caballo = Animal(name="HorseLuis", age=4, type=Especies.caballo)
    tom = Animal(name="Tom", age=75, type=Especies.lobo)
    luna = Animal(name="Luna", age=35, type=Especies.gatito)

    # Y un ejemplo
    >>> perry.type == luna.type
    True
    >>> luna.type
    <Especies.gato: 1>

Un código así es mucho menos propenso a tener fallos. Si necesitamos ser específicos, deberíamos usar sólo los tipos enumerados.

Por último, existen tres formas de acceder a los enum. Sigamos con el ejemplo anterior de las especies. Vamos a acceder a **gato**:

.. code:: python

    Especies(1)
    Especies['gato']
    Especies.gato

Con esto finalizamos una breve introducción al módulo de ``collections`` de Python. Si quieres saber más, te recomendamos que leas la documentación oficial de Python, que aunque pueda ser un poco más técnica y menos didáctica, con esta introducción ya deberías estar list@ para entenderla.
