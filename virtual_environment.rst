Entornos virtuales
-------------------

Los entornos virtuales o *virtual environments* son una herramienta muy potente que es parte de cualquier desarrollador de Python. Entonces, ¿qué son los ``virtualenv``?

Se trata de una herramienta que permite crear entornos virtuales de Python totalmente aislados. Imagina que tienes una aplicación que requiere la versión 2 de Python, pero que también tienes otra que requiere Python 3. ¿Cómo puedes usar ambas aplicaciones? O también puedes tener diferentes aplicaciones que usan diferentes versiones de un determinado paquete. ¿Cómo podemos hacer? La respuesta son los ``virtualenv``.

Si instalas todo en ``/usr/lib/python2.7/site-packages`` (o el directorio que tengas si usas Windows o cualquier otra plataforma) cualquiera de tus aplicaciones compartirá por defecto el contenido de esa carpeta, como por ejemplo las versiones de los paquetes que usas. El problema es que es normal acabar actualizando algún paquete, y esto puede ser algo que nos interese para una aplicación que tengamos, pero no para otra.

La solución a este problema es usar ``virtualenv`` para crear entornos completamente aislados unos de otros. Por lo tanto, tus aplicaciones usarán uno determinado, y los nuevos paquetes o actualizaciones que instales en uno no afectaran a otros.

Para instalar esta herramienta, basta con ejecutar el siguiente comando en el terminal:

.. code:: python

    $ pip install virtualenv

Los comandos más importantes son los siguientes:

-  ``$ virtualenv myproject``
-  ``$ source myproject/bin/activate``

El primero crea un entorno virtual en la carpeta ``myproject``, y el segundo activa ese entorno.

Cuando creas un entorno virtual es necesario tomar una decisión sobre si quieres usar los paquetes que están por defecto en tu sistema en ``site-packages``. Es importante notar que por defecto ``virtualenv`` no tiene acceso a ``site-packages``.

Si quieres que tu entorno tenga acceso a los paquetes instalados en tu sistema en ``site-packages`` puedes usar ``--system-site-packages``.

.. code:: python

    $ virtualenv --system-site-packages mycoolproject

Por otro lado, puedes desactivar el entorno de la siguiente manera:

.. code:: python

    $ deactivate

Ejecutando `python` después de desactivarlo, se usará el conjunto de paquetes que tengas instalado en el sistema (``site-packages``).

**Extra**


Puedes usar la librería ``smartcd``, que permite que en *bash* y *zsh* puedas cambiar de entorno al hacer un cambio de directorio con ``cd``. Puede ser realmente útil si tienes varios proyectos con diferentes entornos y quieres navegar por ellos, ya que el entorno se irá activando o desactivando según el directorio en el que estés. Puedes leer más acerca de este proyecto en su `GitHub <https://github.com/cxreg/smartcd>`__.

Y hasta aquí esta rápida introducción de los ``virtualenv``. Hay mucho más que esto, por lo que si quieres saber más, te recomendamos el `siguiente enlace <http://docs.python-guide.org/en/latest/dev/virtualenvs/>`__.
