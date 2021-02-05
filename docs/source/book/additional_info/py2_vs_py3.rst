Python 2.7 and Python 3.6 distinctions
-------------------------------

`Unicode <../16_unicode/>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Python 2.7 has two string types: ``str`` and ``unicode``:

.. code:: python

    In [1]: line = 'test'

    In [2]: line2 = u'test'

In Python 3, string is ``str`` type but in addition ``bytes`` type appeared in Python 3:

.. code:: python

    In [3]: line = 'test'

    In [4]: line.encode('utf-8')
    Out[4]: b'\xd1\x82\xd0\xb5\xd1\x81\xd1\x82'

    In [5]: byte_str = b'test'

`print function <../10_useful_functions/print.html>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In Python 2.7 ``print`` was an operator:

.. code:: python

    In [6]: print 1, 'test'
    1 test

In Python 3 `print - function <../10_useful_functions/print.md>`__:

.. code:: python

    In [7]: print(1, 'test')
    1 test

In Python 2.7 it is possible to put arguments in parentheses, but it doesn't make
``print`` a function and ``print`` returns another result (tuple):

.. code:: python

    In [8]: print(1, 'test')
    (1, 'test')

In Python 3, using Python 2.7 syntax will result in an error:

.. code:: python

    In [9]: print 1, 'test'
      File "<ipython-input-2-328abb6b105d>", line 1
        print 1, 'test'
              ^
    SyntaxError: Missing parentheses in call to 'print'

`input instead of raw_input <../05_basic_scripts/2_user_input.html>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In Python 2.7, ``raw_input`` function was used to get information from
user as a string:

.. code:: python

    In [10]: number = raw_input('Number: ')
    Number: 55

    In [11]: number
    Out[11]: '55'

Python 3 uses ``input``:

.. code:: python

    In [12]: number = input('Number: ')
    Number: 55

    In [13]: number
    Out[13]: '55'

`range instead of xrange <../10_useful_functions/range.html>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Python 2.7 had two functions

* ``range`` - returns list
* ``xrange`` - returns iterator

Example ``range`` and ``xrange`` in Python 2.7:

.. code:: python

    In [14]: range(5)
    Out[14]: [0, 1, 2, 3, 4]

    In [15]: xrange(5)
    Out[15]: xrange(5)

    In [16]: list(xrange(5))
    Out[16]: [0, 1, 2, 3, 4]

Python 3 has only a ``range`` function and it returns an iterator:

.. code:: python

    In [17]: range(5)
    Out[17]: range(0, 5)

    In [18]: list(range(5))
    Out[18]: [0, 1, 2, 3, 4]

`Dictionary methods <../04_data_structures/6a_dict_methods.html>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Several changes have occurred in dictionary methods.

``dict.keys``, ``values``, ``items``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Methods ``keys``, ``values``, ``items`` in Python 3 return ``views`` instead of
lists. The peculiarity of view is that they change with the change of
dictionary. And in fact, they just give you a way to look at corresponding
objects but they don't make a copy of them.

Python 3 has no methods:

* viewitems, viewkeys, viewvalues
* iteritems, iterkeys, itervalues

For comparison, dictionary methods in Python 2.7:

.. code:: python

    In [19]: d = {1:100, 2:200, 3:300}

    In [20]: d.
        d.clear      d.get        d.iteritems  d.keys       d.setdefault d.viewitems
        d.copy       d.has_key    d.iterkeys   d.pop        d.update     d.viewkeys
        d.fromkeys   d.items      d.itervalues d.popitem    d.values     d.viewvalues

And in Python 3:

.. code:: python

    In [21]: d = {1:100, 2:200, 3:300}

    In [22]: d.
               clear()      get()        pop()        update()
               copy()       items()      popitem()    values()
               fromkeys()   keys()       setdefault()

`Variables unpacking <../08_python_basic_examples/variable_unpacking.html>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In Python 3 it is possible to use ``*`` when unpacking variables:

.. code:: python

    In [23]: a, *b, c = [1,2,3,4,5]

    In [24]: a
    Out[24]: 1

    In [25]: b
    Out[25]: [2, 3, 4]

    In [26]: c
    Out[26]: 5

Python 2.7 does not support this syntax:

.. code:: python

    In [27]: a, *b, c = [1,2,3,4,5]
      File "<ipython-input-10-e3f57143ffb4>", line 1
        a, *b, c = [1,2,3,4,5]
           ^
    SyntaxError: invalid syntax

`Iterator instead of list <../10_useful_functions/>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In Python 2.7 map, filter and zip returned a list:

.. code:: python

    In [28]: map(str, [1,2,3,4,5])
    Out[28]: ['1', '2', '3', '4', '5']

    In [29]: filter(lambda x: x>3, [1,2,3,4,5])
    Out[29]: [4, 5]

    In [30]: zip([1,2,3], [100,200,300])
    Out[30]: [(1, 100), (2, 200), (3, 300)]

In Python 3, they return an iterator:

.. code:: python

    In [31]: map(str, [1,2,3,4,5])
    Out[31]: <map at 0xb4ee3fec>

    In [32]: filter(lambda x: x>3, [1,2,3,4,5])
    Out[32]: <filter at 0xb448c68c>

    In [33]: zip([1,2,3], [100,200,300])
    Out[33]: <zip at 0xb4efc1ec>

`subprocess.run <../12_useful_modules/subprocess.html>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Python 3.5 introduced the new ``run`` function in subprocess module.
It provides a more user-friendly interface for working with module
and getting output of commands.

Accordingly, ``run`` function is used instead of ``call`` and ``check_output``
functions. But ``call`` and ``check_output`` functions remain.

Jinja2
~~~~~~

In Jinja2 module it is no longer necessary to use such code, since the default
encoding is utf-8:

.. code:: python

    import sys
    reload(sys)
    sys.setdefaultencoding('utf-8')

In the templates themselves as in Python, dictionary methods have changed. Here,
you should use ``items`` instead of ``iteritems``.

Modules pexpect, telnetlib, paramiko
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Modules pexpect, telnetlib, paramiko send and receive bytes, so you have to
make encode/decode accordingly.

In netmiko this conversion is performed automatically.

Trivia
~~~~~~

-  Name of Queue module changed to queue
-  Starting from Python 3.6, csv.DictReader returns OrderedDict instead of a regular dictionary.

Additional information
~~~~~~~~~~~~~~~~~~~~~~~~~

Below are links to resources with information about changes in Python 3.

Documentation:

-  `What's New In Python
   3.0 <https://docs.python.org/3.0/whatsnew/3.0.html>`__
-  `Should I use Python 2 or Python 3 for my development
   activity? <https://wiki.python.org/moin/Python2orPython3>`__

Articles:

-  `The key differences between Python 2.7.x and Python 3.x with
   examples <http://sebastianraschka.com/Articles/2014_python_2_3_key_diff.html>`__
-  `Supporting Python 3: An in-depth
   guide <http://python3porting.com/>`__

