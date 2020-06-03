Extensiones C de Python
===================

Una característica muy interesante de la que disponemos en Python es tener un interfaz para interactuar con código escrito en el lenguaje de programación C. Existen varios métodos, pero los que cubriremos en este capítulo serán tres: ``ctypes``, ``SWIG`` y ``Python/C API``. Veremos las ventajas y desventajas de cada uno con algunos ejemplos de como pueden ser usados.

Pero antes tal vez te preguntes ¿y para qué quiero yo usar código C en Python? Pues bien, existen varias razones:

-   Si necesitas velocidad, C es mucho más rápido que Python. En determinadas ocasiones puede ser del orden de decenas de veces más rápido, por lo que si buscamos velocidad, esta puede ser una buena opción.
-   Algunas librerías antiguas de C funcionan perfectamente, por lo que podría ser conveniente poder usarlas. Escribirlas otra vez en Python costaría tiempo y dinero.
-   Tal vez quieras tener ciertos accesos a recursos muy a bajo nivel.
-   O tal vez simplemente porque quieres hacerlo.

Sabido esto, vamos a ver tres formas de usar código C en Python.

CTypes
---------

El módulo `ctypes <https://docs.python.org/2/library/ctypes.html>`__ de Python es la forma más sencilla de llamar a funciones escritas en C desde Python. Este módulo nos proporciona tipos de datos compatibles con C, y funciones para cargar las librerías DLL de tal forma que puedan ser llamadas. La principal ventaja es que el código escrito en C **no necesita ser modificado**.

**Ejemplo**

Tenemos un código muy sencillo escrito en C que realiza la suma de dos números. Lo guardamos en ``suma.c``.

.. code:: c

    //Código C para sumar dos números, enteros y floats.

    #include <stdio.h>

    int suma_int(int, int);
    float suma_float(float, float);

    int suma_int(int num1, int num2){
        return num1 + num2;
    }

    float suma_float(float num1, float num2){
        return num1 + num2;
    }

Ahora compilamos el código C en un archivo ``.so`` (DLL para Windows). Esto generará el fichero ``adder.so``.

.. code:: bash

    #Para Linux
    $ gcc -shared -Wl,-soname,adder -o adder.so -fPIC suma.c

    #Para macOS
    $ gcc -shared -Wl,-install_name,adder.so -o adder.so -fPIC suma.c

Ahora en el código Python, podemos hacer lo siguiente

.. code:: python

    from ctypes import *

    #Cargamos la librería
    adder = CDLL('./adder.so')

    #Realizamos la suma entera
    res_int = adder.suma_int(4,5)
    print "La suma de 4 y 5 es = " + str(res_int)

    #Realizamos la suma float
    a = c_float(5.5)
    b = c_float(4.1)

    suma_float = adder.suma_float
    suma_float.restype = c_float
    print "La suma de 5.5 y 4.1 es = ", str(suma_float(a, b))

Y tras ejecutarlo nos encontraríamos con la siguiente salida:

::

    La suma de 4 y 5 es = 9
    La suma de 5.5 y 4.1 es = 9.60000038147

Vamos a explicar el ejemplo paso por paso. Por un lado tenemos el fichero de C, que tampoco necesita explicación. Simplemente tenemos un par de funciones que suman dos valores, la primera enteros y la segunda float.

Por otro lado, en el fichero de Python importamos el módulo ``ctypes``. Después importamos la *shared library* que hemos creado usando la función ``CDLL``. Una vez hayamos hecho esto, las funciones definidas en la librería de C estarán disponibles en Python a través de la variable ``adder``. Por lo tanto, podemos por ejemplo llamar a ``suma_int`` usando ``adder.suma_int()`` y pasando dos enteros como entrada. Esto producirá que la función de C sea llamada con esos parámetros que hemos proporcionado y se nos devuelva la salida. Es importante notar que podemos usar por defectos los tipos enteros y cadenas.

Para otros tipos como booleanos o float, tenemos que especificarlo nosotros. Esto es por lo que cuando pasamos los parámetros a la otra función que hemos definido que usaba floats ``adder.suma_float()``, tenemos que especificar el tipo con ``c_float``. Como se puede ver esta forma es relativamente sencilla de implementar, pero tiene limitaciones. Por ejemplo, no sería posible manipular objetos en C.

SWIG
-------

Otra de las formas que existen para usar código C desde Python, es usando SWIG, que viene de *Simplified Wrapper and Interface Generator*. En este método, es necesario crear un nuevo interfaz (un fichero), que es usado como entrada para SWIG.

Es un método no muy conocido ya que en la mayoría de los casos es innecesariamente complejo. Sin embargo, es un método bastante útil cuando tenemos cierto código en C/C++ y queremos usarlo en diferentes lenguajes (no sólo en Python).

**Ejemplo** (De la `web de SWIG <http://www.swig.org/tutorial.html>`__ )

Por un lado tenemos el código en C guardado en ``ejemplo.c``, que tiene diferentes funciones y variables.

.. code:: c

     #include <time.h>
     double My_variable = 3.0;

     int fact(int n) {
         if (n <= 1) return 1;
         else return n*fact(n-1);
     }

     int my_mod(int x, int y) {
         return (x%y);
     }

     char *get_time()
     {
         time_t ltime;
         time(&ltime);
         return ctime(&ltime);
     }

Por otro lado tenemos el fichero que actúa de interfaz, y que será el mismo para cualquier lenguaje de programación, por lo que se puede reusar.
::

    /* ejemplo.i */
     %module ejemplo
     %{
     /* Pon aquí las cabeceras o las declaraciones como se muestra a continuación */
     extern double My_variable;
     extern int fact(int n);
     extern int my_mod(int x, int y);
     extern char *get_time();
     %}

     extern double My_variable;
     extern int fact(int n);
     extern int my_mod(int x, int y);
     extern char *get_time();

Lo compilamos.

::

    unix % swig -python ejemplo.i
    unix % gcc -c ejemplo.c ejemplo_wrap.c \
            -I/usr/local/include/python2.1
    unix % ld -shared ejemplo.o ejemplo_wrap.o -o _ejemplo.so

Y ahora en la parte de Python.

.. code:: python

    >>> import ejemplo
    >>> ejemplo.fact(5)
    120
    >>> ejemplo.my_mod(7,3)
    1
    >>> ejemplo.get_time()
    'Sun Feb 11 23:01:07 1996'
    >>>

Como podemos ver, el resultado que conseguimos con SWIG es el mismo, pero requiere de un poco más des esfuerzo al tener que crear un fichero nuevo. Sin embargo tal vez merezca la pena si queremos compartir código C con más de un lenguaje, ya que este fichero de interfaz que hemos visto sólo necesitaría ser creado una vez.

Python/C API
---------------

Por último, la `CPython API <https://docs.python.org/2/c-api/>`__ es una de las opciones más usadas, aunque no por su simplicidad. Esto se debe aunque a pesar de ser más compleja que las anteriores vistas, nos permite manipular objetos de Python en C.

Este método requiere que el código C sea escrito de manera específica para poder ser usado desde Python. Todos los objetos de Python se representan como un ``PyObject``, y la cabecera ``Python.h`` nos proporciona diferentes funciones para manipularlos. Por ejemplo, si el ``PyObject`` es un ``PyListType`` (es decir, una lista), podemos usar ``PyList_Size()`` para calcular su longitud. Sería el equivalente a usar ``len(list)`` en Python. En general, la mayoría de las funciones de Python están disponibles en ``Python.h``.

**Ejemplo**

Vamos a ver como escribir una extensión en C, que toma una lista y suma todos sus elementos. Vamos a asumir que todos los elementos son números, como resulta evidente.

Empecemos viendo la forma en la que nos gustaría poder usar la extensión de C que vamos a crear.

.. code:: python

    #A pesar de que parece un import normal, en realidad
    #importa la extensión en C que definiremos a continuación.
    import sumaLista

    l = [1,2,3,4,5]
    print "La suma de la lista es " + str(l) + " = " +  str(sumaLista.add(l))

El ejemplo anterior podría parecer un fichero normal y corriente de Python, que importa y usa otro módulo llamado ``sumaLista``. Sin embargo este módulo no está escrito en Python sino en C. Esta es una de las ventajas principales, ya que en la parte de Python no nos tenemos que preocupar de aprender nada nuevo o usar funciones extra. Se nos abstrae la librería de C como si fuera un módulo Python normal.

Lo siguiente es escribir el código ``sumaLista`` que será usado como hemos visto antes en Python. Puede parecer un poco complicado pero ya verás como no lo es tanto.

.. code:: c

    //Fichero: adder.c

    //Python.h proporciona funciones para manipular los objetos de Python
    #include <Python.h>

    //Esta es la función que es llamada desde Python
    static PyObject* sumaLista_add(PyObject* self, PyObject* args){

      PyObject * listObj;

      //Los argumentos de entrada son proporcionados como una tupla, los parseamos para
      //obtener las variables. En este caso sólo se trata de una lista, que será 
      //referenciada por listObj.
      if (! PyArg_ParseTuple( args, "O", &listObj))
        return NULL;

      //Devuelve la longitud de la lista
      long length = PyList_Size(listObj);

      //Iteramos todos los elementos
      long i, sum =0;
      for(i = 0; i < length; i++){
        //Tomamos un elemento de la lista (también es un objeto Python)
        PyObject* temp = PyList_GetItem(listObj, i);
        //Sabemos que es un entero, por lo que lo convertimos a long
        long elem = PyInt_AsLong(temp);
        //Realizamos la suma y guardamos el resultado
        sum += elem;
      }

      //Devolvemos a Python otro objeto Python
      //Convertimos el long que teníamos en C a entero en Python
      return Py_BuildValue("i", sum);
    }

    //Este es un docstring (documentación) de nuestra función suma.
    static char sumaLista_docs[] =
    "add( ): Suma todos los elementos de la lista\n";

    /* Esta tabla relaciona la siguiente información -
      <function-name del módulo Python>, <actual-function>,
      <type-of-args que la función espera>, <docstring asociado a la función>
    */
    static PyMethodDef sumaLista_funcs[] = {
      {"add", (PyCFunction)sumaLista_add, METH_VARARGS, sumaLista_docs},
      {NULL, NULL, 0, NULL}
    };

    /*
      sumaLista es el nombre del módulo, y esto es la inicialización.
      <desired module name>, <the-info-table>, <module's-docstring>
    */
    PyMODINIT_FUNC initsumaLista(void){
      Py_InitModule3("sumaLista", sumaLista_funcs,
       "Suma todos los elementos");
    }

Veamos una explicación paso por paso:

- El fichero ``<Python.h>`` proporciona todos los tipos que son usados para representar objetos en Python, además de funciones para operar con ellos, como por ejemplo la lista que hemos visto y su función para calcular la longitud.
- A continuación escribimos la función que vamos a llamar desde Python. Por convención, se usa {nombre-módulo}\_{nombre-función}. En nuestro caso es ``sumaLista_add``.
- Después añadimos a la tabla la información sobre esa función, como el nombre (tanto en C como en Python). En esta tabla hay una entrada por cada función que tengamos, y tiene que ir terminada por lo que se conoce como valor *sentinel*, que es una fila con elementos nulos.
- Finalmente, inicializamos el módulo con ``PyMODINIT_FUNC``.

Como podemos ver, la función ``sumaLista_add`` acepta argumentos de entrara que son del tipo ``PyObject`` (args es también del tipo tupla, pero como en Python todo es un objeto, usaremos la notación de PyObject). Por otro lado, los argumentos de entrada se parsean usando ``PyArg_ParseTuple()``. El primer parámetro es el argumento variable a ser parseado. El segundo es una cadena que nos dice como parsear cada elemento de la tupla. La letra en la posición ``n`` de la cadena indica el tipo del elemento en la posición ``n``de la tupla. Por ejemplo, 'i' significa entero (*integer*), 's' cadena (*string*) y '0' significa objeto Python.

Por otro lado tenemos la función ``PyArg_ParseTuple()`` que merece una explicación por separado. Esta función permite almacenar los elementos que se han parseado en variables separadas. Su número de argumentos es igual al número de argumentos que la función espera recibir. Veamos un ejemplo. Si nuestra función recibiera una cadena, un entero y una lista de Python en ese orden, la función se llamaría de la siguiente forma. Una vez llamada, tendríamos en las variables ``n``, ``s`` y ``list`` los valores ya parseados y listos para ser usados.

.. code:: c

    int n;
    char *s;
    PyObject* list;
    PyArg_ParseTuple(args, "siO", &s, &n, &list);

Sin embargo en nuestro ejemplo simplemente extraemos la lista, y almacenamos su contenido en ``listObj``. Por otro lado, se puede ver como hacemos uso de la función ``PyList_Size()``, lo que nos devuelve la longitud de la lista, el equivalente a ``len(list)`` que conocemos de Python.

Más adelante, iteramos la lista y tomamos cada elemento con la función ``PyList_GetItem(list, index)``. Esto nos devuelve un PyObject\*, pero como sabemos que ese elemento es en realidad un entero ``PyIntType``, podemos usar la función ``PyInt_AsLong(PyObj *)`` para obtener el valor. Como se puede ver, realizamos esto para cada elemento calculando la suma.

La suma es por lo tanto convertida a un objeto de Python y devuelta, con ayuda de la función ``Py_BuildValue()``. Como podemos ver, usamos 'i', lo que indica que queremos convertir un valor que es un entero (*integer*).

Una vez entendido esto, podemos compilar el módulo C. Guarda el siguiente fichero como ``setup.py``.

.. code:: python

    #Compila los módulos

    from distutils.core import setup, Extension

    setup(name='sumaLista', version='1.0',  \
          ext_modules=[Extension('sumaLista', ['adder.c'])])

Y ejecuta el siguiente comando en el terminal.

.. code:: sh

    python setup.py install

Una vez realizado esto, ya podríamos usar el módulo que hemos creado en Python como si de un módulo normal se tratase. Veamos como funciona:

.. code:: python

    #Importamos el módulo que "habla" con C
    import sumaLista

    l = [1,2,3,4,5]
    print "La suma dela lista - " + str(l) + " = " +  str(sumaLista.add(l))

Y aquí tenemos la salida.

::

    La suma de la lista - [1, 2, 3, 4, 5] = 15


Hemos explicado como crear tu primera extensión de C para Python usando la API ``Python.h``. Se trata de un método que puede parecer un poco complejo inicialmente, pero una vez te acostumbras a el, puede ser realmente útil.

Existen otras formas de usar código C desde Python, como puede ser usar `Cython <http://cython.org/>`__, pero se trata de un lenguaje un tanto diferente al típico Python, por lo que no lo cubriremos aquí. No obstante te recomendamos que le eches un vistazo.
