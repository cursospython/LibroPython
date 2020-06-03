Depurando
---------

Depurar es una de las herramientas que mas nos pueden ayudar si tenemos un *bug* o fallo que necesitamos resolver. Mucha gente olvida la importancia del depurador de Python ``pdb``. En esta sección veremos algunos de los comandos más importantes, por lo que si quieres entrar en detalle, no olvides entrar en la documentación oficial.

**Desde línea de comandos**

Puedes ejecutar un *script* desde la línea de comandos usando el depurador de Python. Se hace de la siguiente manera:

.. code:: python

    $ python -m pdb my_script.py

Esto hará que el depurador pare la ejecución del programa en la primera sentencia que encuentre. Su uso es muy útil cuando el *script* es corto. Puedes inspeccionar las variables y continuar con la ejecución línea por línea.

**Desde dentro del script**

También puedes asignar diferentes *break points* o puntos de ruptura para poder inspeccionar el contenido de las variables en determinados puntos del código. Esto se puede hacer con el método ``pdb.set_trace()``. Vemos un ejemplo:

.. code:: python

    import pdb

    def haz_algo():
        pdb.set_trace()
        return "No quiero"

    print(haz_algo())

Intenta ejecutar el código anterior una vez guardado. Entrarás en el depurador en cuanto empieces a ejecutarlo. Visto esto, vamos a ver algunos de los comandos más útiles del depurador.

**Comandos:**

-  ``c``: continúa la ejecución
-  ``w``: muestra el contexto de la línea que se esta ejecutando.
-  ``a``: imprime la lista de argumentos para la función actual.
-  ``s``: ejecuta la primera línea y para en cuanto sea posible.
-  ``n``: continúa la ejecución hasta la siguiente línea en la función actual o hasta que se retorna.

La diferencia entre ``n`` y ``s`` se ve muy fácil en Inglés, ya que viene de **n**\ext y **s**\top. El uso de next ejecuta la función llamada prácticamente a velocidad normal, tan solo parando en la siguiente línea. Por lo contrario, stop para dentro de la función llamada.

Estos son sólo unos pocos comandos. ``pdb`` también soporta análisis *post mortem*, una de las características que te recomendamos que investigues un poco más a fondo.

**Nota:**

Puede no ser muy intuitivo usar ``pdb.set_trace()`` si eres nuevo en esto. Afortunadamente si usas Python 3.7+ puedes usar simplemente ``breakpoint()`` https://docs.python.org/3/library/functions.html#breakpoint. Automáticamente importa ``pdb`` y llama a ``pdb.set_trace()``.
