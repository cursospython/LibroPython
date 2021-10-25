Decoradores
----------

Los decoradores son una funcionalidad relativamente importante en Python. Se podría decir que son funciones que modifican la funcionalidad de otras funciones, y ayudan a hacer nuestro código más corto y Pytónico o Pythonic. A continuación veremos lo que son, cómo se crean y cómo podemos usarlos.

Todo es un objeto en Python:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Antes de entrar en materia con los decoradores, vamos a entender bien las funciones.

.. code:: python

    def hola(nombre="Covadonga"):
        return "Hola " + nombre

    print(hola())
    # Salida: 'Hola Covadonga'

    # Podemos asignar una función a una variable
    saluda = hola
    # No usamos () porque no la estamos llamando, sino que la estamos
    # asignado a una variable

    print(saluda())
    # Salida: 'Hola Covadonga'

    # También podemos eliminar la función asignada a la variable con del
    del hola
    print(hola())
    #Salida: NameError

    print(saluda())
    #Salida: 'Hola Covadonga'

Definir funciones dentro de funciones:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Vamos a ir un paso más allá. En Python podemos definir funciones dentro de otras funciones. Veamos un ejemplo:

.. code:: python

    def hola(nombre="Covadonga"):
        print("Estás dentro de la función hola()")

        def saluda():
            return "Estás dentro de la función saluda()"

        def bienvenida():
            return "Estás dentro de la función bienvenida()"

        print(saluda())
        print(bienvenida())
        print("De vuelta a la función hola()")

    hola()
    #Salida:Estas dentro de la función hola()
    #       Estás dentro de la función saluda()
    #       Estás dentro de la función bienvenida()
    #       De vuelta a la función hola()

    # Esto muestra como cada vez que llamas a la función hola()
    # se llama en realidad también a saluda() y bienvenida()
    # Sin embargo estas dos últimas funciones no están accesibles
    # fuera de hola(). Si lo intentamos, tendremos un error.

    saluda()
    #Saluda: NameError: name 'saluda' is not defined

Ya hemos visto entonces como podemos definir funciones dentro de otras funciones. En otras palabras, podemos crear funciones anidadas. Pero para entender bien los decoradores, necesitamos ir un paso más allá. Las funciones también pueden devolver otras funciones.

Devolviendo funciones desde funciones:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

No es necesario ejecutar una función dentro de otra. Simplemente podemos devolverla como salida:

.. code:: python

    def hola(nombre="Covadonga"):
        def saluda():
            return "Estás dentro de la función saluda()"

        def bienvenida():
            return "Estás dentro de la función bienvenida()"

        if nombre == "Covadonga":
            return saluda
        else:
            return bienvenida

    a = hola()
    print(a)
    #Salida: <function saluda at 0x7f2143c01500>

    #Es decir, la variable 'a' ahora apunta a la función
    # saluda() declarada dentro de hola(). Por lo tanto podemos llamarla.

    print(a())
    #Salida: Estás dentro de la función saluda()

Echa un vistazo otra vez al código. Si te fijas en el if/else, estamos devolviendo ``saluda`` y ``bienvenida`` y no ``saluda()`` y ``bienvenida()``. ¿A qué se debe esto? Se debe a que cuando usas paréntesis ``()`` la función se ejecuta. Por lo contrario, si no los usas la función es pasada y puede ser asignada a una variable sin ser ejecutada.

Vamos a analizar el código paso por paso. Al principio usamos ``a = hola()``, por lo que el parámetro para ``nombre`` que se toma es Covadonga ya que es el que hemos asignado por defecto. Esto hará que en el ``if`` se entre en ``nombre == "Covadonga"``, lo que hará que se devuelva la función saluda. Si por lo contrario hacemos la llamada a la función con ``a = hola(nombre="Pelayo")``, la función devuelta será ``bienvenida``.

Usando funciones como argumento de otras:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Por último, podemos hacer que una función tenga a otra como entrada y que además la ejecute dentro de sí misma. En el siguiente ejemplo podemos ver como ``hazEstoAntesDeHola()`` es una función que de alguna forma encapsula a la función que se le pase como parámetro, añadiendo una determinada funcionalidad. En este ejemplo simplemente imprimimos algo por pantalla antes de llamar a la función.

.. code:: python

    def hola():
        return "¡Hola!"

    def hazEstoAntesDeHola(func):
        print("Hacer algo antes de llamar a func")
        print(func())

    hazEstoAntesDeHola(hola)
    #Salida: Hacer algo antes de llamar a func
    #        ¡Hola!


Ahora ya tienes todas las piezas del rompecabezas. Los decoradores son funciones que decoran a otras funciones, pudiendo ejecutar código antes y después de la función que está siendo decorada.

Tu primer decorador:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Realmente en el ejemplo anterior ya vimos como crear un decorador. Vamos a modificarlo y hacerlo un poco realista.

.. code:: python

    def nuevo_decorador(a_func):

        def envuelveLaFuncion():
            print("Haciendo algo antes de llamar a a_func()")

            a_func()

            print("Haciendo algo después de llamar a a_func()")

        return envuelveLaFuncion

    def funcion_a_decorar():
        print("Soy la función que necesita ser decorada")

    funcion_a_decorar()
    #Salida: "Soy la función que necesita ser decorada"

    funcion_a_decorar = nuevo_decorador(funcion_a_decorar)
    #Ahora funcion_a_decorar está envuelta con el decorador que hemos creado

    funcion_a_decorar()
    #Salida: Haciendo algo antes de llamar a a_func()
    #        Soy la función que necesita ser decorada
    #        Haciendo algo después de llamar a a_func()

Simplemente hemos aplicado todo lo aprendido en los apartados anteriores. Así es exactamente como funcionan los decoradores en Python. Envuelven una función para modificar su comportamiento de una manera determinada.

Tal vez te preguntes ahora porqué no hemos usado @ en el código. Esto es debido a que @ es simplemente una forma de hacerlo más corto, pero ambas opciones son perfectamente válidas.

.. code:: python

    @nuevo_decorador
    def funcion_a_decorar():
        print("Soy la función que necesita ser decorada")

    funcion_a_decorar()
    #Salida: Haciendo algo antes de llamar a a_func()
    #        Soy la función que necesita ser decorada
    #        Haciendo algo después de llamar a a_func()

    #El uso de @nuevo_decorador es simplemente una forma acortada
    #de hacer lo siguiente.
    funcion_a_decorar = nuevo_decorador(funcion_a_decorar)

Una vez visto esto, hay un pequeño problema con el código. Si ejecutamos lo siguiente:

.. code:: python

    print(funcion_a_decorar.__name__)
    # Output: envuelveLaFuncion

Nos encontramos con un comportamiento un tanto inesperado. Nuestra función es ``funcion_a_decorar`` pero al haberla envuelto con el decorador es en realidad ``envuelveLaFuncion``, por lo que sobreescribe el nombre y el *docstring* de la misma, algo que no es muy conveniente. Por suerte, Python nos da una forma de arreglar este problema usando ``functools.wraps``. Vamos a modificar nuestro ejemplo anterior haciendo uso de esta herramienta.

.. code:: python

    from functools import wraps

    def nuevo_decorador(a_func):
        @wraps(a_func)
        def envuelveLaFuncion():
            print("Haciendo algo antes de llamar a a_func()")
            a_func()
            print("Haciendo algo después de llamar a a_func()")
        return envuelveLaFuncion

    @nuevo_decorador
    def funcion_a_decorar():
        print("Soy la función que necesita ser decorada")

    print(funcion_a_decorar.__name__)
    # Salida: funcion_a_decorar

Mucho mejor ahora. Veamos también unos fragmentos de código muy usados.

**Ejemplos:**

.. code:: python

    from functools import wraps
    def nombre_decorador(f):
        @wraps(f)
        def decorada(*args, **kwargs):
            if not can_run:
                return "La función no se ejecutará"
            return f(*args, **kwargs)
        return decorada

    @nombre_decorador
    def func():
        return("La función se esta ejecutando")

    can_run = True
    print(func())
    # Salida: La función se esta ejecutando

    can_run = False
    print(func())
    # Salida: La función no se ejecutará

Nota: ``@wraps`` toma una función para ser decorada y añade la funcionalidad de copiar el nombre de la función, el *docstring*, los argumentos y otros parámetros asociados. Esto nos permite acceder a los elementos de la función a decorar una vez decorada. Es decir, resuelve el problema que vimos con anterioridad.


Casos de uso:
~~~~~~~~~~

A continuación veremos algunos áreas en las que los decoradores son realmente útiles.


Autorización
~~~~~~~~~~~~

Los decoradores permiten verificar si alguien está o no autorizado a usar una determinada función, por ejemplo en una aplicación web. Son muy usados en *frameworks* como Flask o Django. Aquí te mostramos como usar un decorador para verificar que se está autenticado.


**Ejemplo :**

.. code:: python

    from functools import wraps

    def requires_auth(f):
        @wraps(f)
        def decorated(*args, **kwargs):
            auth = request.authorization
            if not auth or not check_auth(auth.username, auth.password):
                authenticate()
            return f(*args, **kwargs)
        return decorated

Iniciar sesión
~~~~~~~~~~~~

El inicio de sesión es otra de las áreas donde los decoradores son muy útiles. Vamos un ejemplo:

.. code:: python

    from functools import wraps

    def logit(func):
        @wraps(func)
        def with_logging(*args, **kwargs):
            print(func.__name__ + " was called")
            return func(*args, **kwargs)
        return with_logging

    @logit
    def addition_func(x):
       """Función suma"""
       return x + x


    result = addition_func(4)
    # Salida: addition_func was called


Decoradores con argumentos
^^^^^^^^^^^^^^^^^^^^^^^^^

Hemos visto ya el uso de ``@wraps``, y tal vez te preguntes ¿pero no es también un decorador? De hecho si te fijas acepta un parámetro (que en nuestro caso es una función). A continuación te explicamos como crear un decorador que también acepta parámetros de entrada. 


Anidando un Decorador dentro de una Función
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Vayamos de vuelta al ejemplo de inicio de sesión, y creemos un *wraper* que permita especificar el fichero de salida que queremos usar para el fichero de *log*. Si te fijas, el decorador ahora acepta un parámetro de entrada.

.. code:: python

    from functools import wraps
    
    def logit(logfile='out.log'):
        def logging_decorator(func):
            @wraps(func)
            def wrapped_function(*args, **kwargs):
                log_string = func.__name__ + " fue llamada"
                print(log_string)
                # Abre el fichero y añade su contenido
                with open(logfile, 'a') as opened_file:
                    # Escribimos en el fichero el contenido
                    opened_file.write(log_string + '\n')
                return func(*args, **kwargs)
            return wrapped_function
        return logging_decorator

    @logit()
    def myfunc1():
        pass
        
    myfunc1()
    # Salida: myfunc1 fue llamada
    # Se ha creado un fichero con el nombre por defecto (out.log)
    
    @logit(logfile='func2.log')
    def myfunc2():
        pass
    
    myfunc2()
    # Salida: myfunc2  fue llamada
    # Se crea un fichero func2.log

Clases Decoradoras
~~~~~~~~~~~~~~~~~

Llegados a este punto ya tenemos el decorador *logit* creado en el apartado anterior funcionando en producción, pero algunas partes de nuestra aplicación son críticas, y si se produce un fallo este necesitará atención inmediata. Vamos a suponer que en determinadas ocasiones quieres simplemente escribir en el *log* (como hemos hecho), pero en otras quieres que se envíe un correo. En una aplicación como esta podríamos usar la herencia, pero hasta ahora sólo hemos usado decoradores.

Por suerte, las clases también pueden ser usadas para crear decoradores. Vamos a volver a definir *logit*, pero en este caso como una clase en vez de con una función.

.. code:: python

    class logit(object):
    
        _logfile = 'out.log'
    
        def __init__(self, func):
            self.func = func
        
        def __call__(self, *args):
            log_string = self.func.__name__ + " fue llamada"
            print(log_string)
            # Abre el fichero de log y escribe
            with open(self._logfile, 'a') as opened_file:
                # Escribimos el contenido
                opened_file.write(log_string + '\n')
            # Enviamos una notificación (ver método)
            self.notify()
            
            # Devuelve la función base
            return self.func(*args)
            
            
        
        def notify(self):
            # Esta clase simplemente escribe el log, nada más.
            pass
    
Esta implementación es mucho más limpia que con la función anidada. Por otro lado, la función puede ser envuelta de la misma forma que veníamos usando hasta ahora, usando ``@``.

.. code:: python
    
    logit._logfile = 'out2.log' # Si queremos usar otro nombre
    @logit
    def myfunc1():
        pass
    
    myfunc1()
    # Output: myfunc1 fue llamada

Ahora, vamos a crear una subclase de *logit* para añadir la funcionalidad de enviar un email. Enviaremos el email de manera ficticia.

.. code:: python

    class email_logit(logit):
        '''
        Implementación de logit con envío de email
        '''
        def __init__(self, email='admin@myproject.com', *args, **kwargs):
            self.email = email
            super(email_logit, self).__init__(*args, **kwargs)
            
        def notify(self):
            # Enviamos email a self.email
            # Código para enviar email
            # ...
            pass

Una vez creada la nueva clase que hereda de ``logit``, si usamos ``@email_logit`` como decorador tendrá el mismo comportamiento, pero además enviará un email.

Si quieres saber más acerca de los decoradores, en `este post <https://cursospython.com/decoradores-python/>`__ tienes más información.
