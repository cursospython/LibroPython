Global & Return
---------------

Estoy seguro de que por poco código en Python que hayas visto, te habrás encontrado la sentencia ``return`` al final de una función alguna vez. Al igual que en muchos lenguajes de programación, nos permite devolver valores a quien llama a la función. Veamos un ejemplo:

.. code:: python

    def suma(valor1, valor2):
        return valor1 + valor2

    resultado = suma(3, 5)
    print(resultado)
    # Salida: 8

La función anterior toma dos argumentos de entrada y como salida devuelve su suma. Otra forma de conseguir el mismo resultado podría haber sido:

.. code:: python

    def suma(valor1, valor2):
        global resultado
        result = valor1 + valor2

    suma(3,5)
    print(resultado)
    # Salida: 8

Vamos a analizar ambos ejemplos. Empecemos por el primero, en el que usábamos la sentencia ``return``. Lo que esa función hace, es devolver el resultado, y ese resultado puede ser asignado como hemos visto a una variable con ``=``. Esta es una de las formas más frecuentes de "sacar" contenido de la función, ya que de no hacerlo, la variable ``resultado`` se perdería.

Por otro lado, hemos visto como se puede hacer uso de ``global``. Por norma general se podría decir que salvo que estemos muy seguros de lo que estamos haciendo, no es muy común hacer uso de variables globales. Lo que hace el modificador ``global`` es crear una variable global. Por lo tanto, dado que esa variable es global seguirá existiendo una vez se salga de la función, y por consiguiente puede ser accedida una vez la función ha terminado de ejecutarse. Vamos a ver un ejemplo.

.. code:: python

    # Sin usar global
    def suma(valor1, valor2):
        resultado = valor1 + valor2

    suma(2, 4)
    print(resultado)

    # Si intentamos acceder a la variable resultado
    # tendremos un error ya que ha sido declarada dentro
    # de la función, y por lo tanto no es accesible desde
    # fuera
    Traceback (most recent call last):
      File "", line 1, in
        result
    NameError: name 'resultado' is not defined

    # Ahora vamos a hacer lo mismo pero declarando la función
    # como global.
    def suma(valor1, valor2):
        global resultado
        resultado = valor1 + valor2

    suma(2, 4)
    print(resultado)
    # Salida
    # 6

Y como hemos explicado la segunda forma se ejecutará sin problemas. Sin embargo ten cuidado con el uso de ``global``, ya que suele ser una buena práctica evitar su uso. No es muy recomendable tener variables globales salvo casos muy excepcionales.


Devolviendo múltiples valores
^^^^^^^^^^^^^^^^^^^^^^^

Tal vez quieras devolver más de una variable desde una función. Una primera forma de hacerlo sería la siguiente, pero de verdad, no te recomendamos que lo hagas así. Huye de esto y no mires atrás:

.. code:: python

    def perfil():
        global nombre
        global edad
        name = "Pelayo"
        age = 30

    perfil()
    print(nombre)
    # Salida: Pelayo

    print(edad)
    # Salida: 30

**Nota:** No hagas esto. Tal vez te preguntes porqué mostramos código que no está bien. Pues bien, nos gusta mostrar también ejemplos de lo que está mal, ya que ayudan a entender lo que no se debe hacer.

Otra forma mucho mejor de hacer esto, es devolviendo los datos dentro de una estructura tipo ``tuple``, ``list`` o ``dict``. Una forma de hacerlo sería la siguiente:

.. code:: python

    def perfil():
        nombre = "Pelayo"
        edad = 30
        return (nombre, edad)

    datos_perfil = perfil()
    print(datos_perfil[0])
    # Salida: Pelayo

    print(datos_perfil[1])
    # Salida: 30

Y otra forma prácticamente igual pero más usada por convención sería la siguiente.

.. code:: python

    def perfil():
        nombre = "Pelayo"
        edad = 30
        return nombre, edad

    nombre_perfil, edad_perfil = perfil()
    print(nombre_perfil)
    # Salida: Pelayo
    print(edad_perfil)
    # Salida: 30

Ten en cuenta que en el ejemplo anterior también se está devolviendo una `tupla <https://cursospython.com/tuplas-python/>`__ (aunque no haya paréntesis). Vistas estas formas, se podría decir que hay otra forma un poco más completa que tal vez te sea útil. Se trata del uso de `namedtuple <https://docs.python.org/3/library/collections.html#collections.namedtuple>`_. Veamos un ejemplo:

.. code:: python

    from collections import namedtuple                                                                                     
    def perfil():
        Persona = namedtuple('Persona', 'nombre edad')
        return Persona(nombre="Pelayo", edad=31)

    # Usando el namedtuple
    p = perfil()
    print(p, type(p))
    # Persona(nombre='Pelayo', edad=31) <class '__main__.Persona'>
    print(p.nombre)
    # Pelayo
    print(p.edad)
    #31

    # Otra forma de usar la namedtuple
    p = perfil()
    print(p[0])
    # Pelayo
    print(p[1])
    #31

    # También se puede hacer el unpacking
    nombre, edad = profile()
    print(nombre)
    # Pelayo
    print(edad)
    #31

Esta forma es bastante útil sobre todo debido a que podemos acceder a los elementos de forma muy sencilla usando ``.`` y el argumento. Como hemos mencionado, otra forma de hacerlo sería con ``lists`` y ``dicts``, pero como ya hemos comentado, intenta evitar ``global`` en la medida de lo posible.