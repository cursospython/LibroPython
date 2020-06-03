Operadores ternarios
-----------------

Los operadores ternarios son más conocidos en Python como expresiones condicionales. Estos operadores evalúan si una expresión es verdadera o no. Se añadieron a Python en la versión 2.4.

**Forma:**

.. code:: python

    condition_if_true if condition else condition_if_false

**Ejemplo:**

.. code:: python

    es_bonito = True
    estado = "Es bonito" if es_bonito else "No es bonito"

Si te quedas con dudas `te recomendamos este post <https://cursospython.com/if-python/>`__ donde se explican con más ejemplos.

Como se puede ver, permiten verificar de manera rápida una condición, y lo mejor de todo es que se puede hacer en una sola línea de código. Por lo general hacen que el código sea más compacto y fácil de leer.

Otra forma un tanto extraña y no demasiado usada es la siguiente:

**Forma:**

.. code:: python

    (if_test_is_false, if_test_is_true)[test]

**Example:**

.. code:: python

    es_bonito = True
    apariencia = ("Feo", "Bonito")[es_bonito]
    print("El gato es ", apariencia)
    # Salida: El gato es bonito

Este ejemplo funciona ya que ``True=1`` y ``False=0``, y puede ser usado también con listas. Es importante decir también que este ejemplo no es muy usado, y por lo general no gusta a los *Pythonistas*.

Otro de los motivos por los que no resulta del todo correcto su uso, es que ambos elementos son evaluados, mientras que en operador ternario if-else no.

**Ejemplo:**

.. code:: python

    condicion = True
    print(2 if condition else 1/0)
    #Salida is 2

    print((1/0, 2)[condicion])
    #Se lanza ZeroDivisionError

En este ejemplo la ``condicion`` es verdadera, por lo que tomaremos el segundo elemento (índice 1) de la lista. Sin embargo como podemos ver, el ``1/0`` es también evaluado, ya que se lanza una excepción. Esto sucede ya que primero la `tupla <https://cursospython.com/tuplas-python/>`__ es creada, y después se toma el elemento con el índice ``[]``. Sin embargo el if-else ternario es igual que un if-else normal, por lo que sólo se evalúa una rama.

**Abreviación ternaria**

En Python existe también una forma acortada del operador ternario normal que hemos visto antes. Esta sintaxis fue introducida en Python 2.5, por lo que puede ser usada de ahí en adelante.

**Ejemplo**

.. code:: python

    >>> True or "Valor"
    True
    >>>
    >>> False or "Valor"
    'Some'

En el primer ejemplo `True or "Some"` devuelve ``True``, mientras que en el segundo se devuelve ``"Valor"``. Es una herramienta bastante útil cuando quieres verificar rápidamente el contenido de una variable, y mostrar un mensaje alternativo si está vacío.

.. code:: python

    >>> salida = None
    >>> msg = salida or "No se devolvió nada"
    >>> print(msg)
    No se devolvió nada

O también es una forma muy simple de definir parámetros con valores por defecto dinámicos. En el siguiente ejemplo vemos como se imprime el ``nombre_real`` por defecto, pero si se proporciona también un ``nombre_opcional`` se imprimirá este por pantalla en vez del anterior.


.. code:: python

    >>> def mi_funcion(nombre_real, nombre_opcional=None):
    >>>     nombre_opcional = optional_display_name or nombre_real
    >>>     print(nombre_opcional)
    >>> mi_funcion("Pelayo")
    Pelayo
    >>> mi_funcion("Covadonga", "Cova")
    Cova
