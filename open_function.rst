Función ``open``
----------------

La función `open <http://docs.python.org/dev/library/functions.html#open>`__ simplemente abre un determinado fichero. Puede parecer sencillo pero en gran cantidad de ocasiones es usada de manera incorrecta. Por ejemplo:

.. code:: python

    f = open('foto.jpg', 'r+')
    jpgdata = f.read()
    f.close()

Una de las razones por las que creemos conveniente explicar ``open()`` es por que es habitual encontrarse el código anterior. Pues bien, hay un total de tres errores (o más bien malas practicas). Al final de este capítulo entenderás porqué. Empecemos por lo básico.

La función ``open`` devuelve lo que se conoce como *file handle*, y es dado por el sistema operativo a tu aplicación de Python. Una vez has terminado de usar este *file handle* (que te permite acceder al fichero) es importante devolverlo y cerrarlo. Esto se debe en parte a que el sistema operativo tiene un número máximo de ficheros que puede tener abiertos, y no tendría mucho sentido mantener uno abierto si ya no se está usando.

En el código anterior podemos ver como existe la llamada ``close()``. La intención de este código es buena, porque se cierra el fichero abierto, pero el problema es que sólo se cerrará si ``f.read()`` funciona correctamente. Es decir, si existe un error en la función ``f.read()``, el programa terminará y el cierre del fichero no se producirá.

Por lo tanto, una de las mejores formas de asegurarnos de que el fichero se cierra correctamente, pase lo que pase, es la siguiente haciendo uso de ``with``.

.. code:: python

    with open('foto.jpg', 'r+') as f:
        jpgdata = f.read()

El primer argumento de ``open`` es el nombre del fichero. El segundo es el modo de apertura, que indica cómo se abrirá el fichero:

-  ``r``: Abre el fichero en modo lectura.
-  ``r+``: Si quieres leer y escribir en el fichero.
-  ``w``: Para sobreescribir el contenido.
-  ``a``: Para añadir al final del fichero en el caso de que ya exista.

Existe algún otro modo de apertura, pero estos son los más comunes. El modo es muy importante ya que cambia el comportamiento, y podríamos llegar a encontrarnos con un error si abrimos un fichero con ``w`` del que no tenemos permiso de escritura.

Existe un modo más de apertura que merece una mención especial. Se trata del **modo binario** ``b``. Es un modo muy usado cuando abrimos ficheros que realmente no tienen contenido legible por los humanos, como podría ser una imagen. Una imagen puede ser vista una vez interpretada y representada por el ordenador, pero si abrimos su contenido con Python, no veremos ninguna información útil. Este tipo de ficheros es común abrirlos en modo binario con ``b``.

Por otro lado, un fichero abierto en **modo texto** necesita saber de su *encoding*. Es decir, en que forma está almacenado el texto. Esto es muy importante ya que al final y al cabo, para Python todos los ficheros tienen contenido binario, solo que si lo abrimos en modo texto, lo interpreta de una manera determinada para mostrárnoslo.

Por desgracia, ``open()`` no soporta especificar el *encoding* en Python 2.x. Sin embargo, la función `io.open <http://docs.python.org/2/library/io.html#io.open>`__ está disponible tanto en Python 2.x como 3.x y nos lo permite hacer. Puedes pasar el tipo de *encoding* con la palabra ``encoding``. Si no pasas ningún argumento, se tomará el *encoding* por defecto. Suele ser una buena práctica indicar un *encoding* específico. El ``utf-8`` es uno de los más usados y con mayor soporte en navegadores y lenguajes de programación. Por último, de la misma manera que se elige *encoding* para leer, también se puede seleccionar para escribir un fichero.

**Nota**: Si usas ``utf-8`` no deberías tener ningún problema con las ``ñ`` u otras letras como ``á`` o ``ó``. Sin embargo con otros *encodings* podrías tenerlos.


Llegados a este punto, tal vez te preguntes ¿y cómo se yo el *enconding* de un fichero?. Bueno, existen varias maneras de hacerlo. Algunos editores de texto como Sublime Text te lo suelen decir. Muchas veces los ficheros vienen con unos metadatos que indican el *encoding* que es usado (como por ejemplo en las cabeceras HTTP).

Una vez sabido esto, vamos a escribir un programa que lee un fichero, y determina si es una imagen JPG o no. (pista: Los ficheros JPG empiezan con la siguiente secuencia de bits ``FF D8``).

.. code:: python

    import io

    with open('foto.jpg', 'rb') as inf:
        jpgdata = inf.read()

    if jpgdata.startswith(b'\xff\xd8'):
        text = u'Es una imagen JPEG (%d bytes long)\n'
    else:
        text = u'No es una imagen JPEG (%d bytes long)\n'

    #Escribimos también el resultado en un fichero
    with io.open('resumen.txt', 'w', encoding='utf-8') as outf:
        outf.write(text % len(jpgdata))

Con esto ya hemos visto como abrir ficheros en diferentes modos, asegurándonos de que son cerrados al terminar con ellos con ``with open``. Hemos visto también el uso del *encoding* y como podemos usar los metadatos de un fichero para saber si un archivo contiene o no una imagen en JPEG.

Si te quedas con dudas, en estos post puedes leer más acerca de `escribir ficheros <https://cursospython.com/escribir-archivos-python/>`__ y `leer ficheros en Python <https://cursospython.com/leer-archivos-python/>`__.
