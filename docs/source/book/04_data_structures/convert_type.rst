Types conversion
--------------------

Python has several useful built-in features that allow data to be converted from one type to another.

``int``
~~~~~~~~~

``int`` converts a string to int:

.. code:: python

    In [1]: int("10")
    Out[1]: 10

Using ``int`` function you can convert a binary number into a decimal number (binary number must be written as a string)

.. code:: python

    In [2]: int("11111111", 2)
    Out[2]: 255

``bin``
~~~~~~~~~

You can convert a decimal number to binary format with ``bin``:

.. code:: python

    In [3]: bin(10)
    Out[3]: '0b1010'

    In [4]: bin(255)
    Out[4]: '0b11111111'

``hex``
~~~~~~~~~

A similar function exists for conversion to hexadecimal format:

.. code:: python

    In [5]: hex(10)
    Out[5]: '0xa'

    In [6]: hex(255)
    Out[6]: '0xff'

``list``
~~~~~~~~~~

Function ``list`` converts an argument to a list:

.. code:: python

    In [7]: list("string")
    Out[7]: ['s', 't', 'r', 'i', 'n', 'g']

    In [8]: list({1, 2, 3})
    Out[8]: [1, 2, 3]

    In [9]: list((1, 2, 3, 4))
    Out[9]: [1, 2, 3, 4]

``set``
~~~~~~~~~

Function ``set`` converts an argument into a set:

.. code:: python

    In [10]: set([1, 2, 3, 3, 4, 4, 4, 4])
    Out[10]: {1, 2, 3, 4}

    In [11]: set((1, 2, 3, 3, 4, 4, 4, 4))
    Out[11]: {1, 2, 3, 4}

    In [12]: set("string string")
    Out[12]: {' ', 'g', 'i', 'n', 'r', 's', 't'}

This function is very useful when you need to get unique elements in a sequence.

``tuple``
~~~~~~~~~~~

Function ``tuple`` converts argument into a tuple:

.. code:: python

    In [13]: tuple([1, 2, 3, 4])
    Out[13]: (1, 2, 3, 4)

    In [14]: tuple({1, 2, 3, 4})
    Out[14]: (1, 2, 3, 4)

    In [15]: tuple("string")
    Out[15]: ('s', 't', 'r', 'i', 'n', 'g')

This can be useful if you want an immutable object.

``str``
~~~~~~~~~

Function ``str`` converts an argument into a string:

.. code:: python

    In [16]: str(10)
    Out[16]: '10'
