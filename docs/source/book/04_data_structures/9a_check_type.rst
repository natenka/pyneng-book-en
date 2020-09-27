Types checking
~~~~~~~~~~~~~~

This type of error can occur when converting data types:

.. code:: python

    In [1]: int('a')
    ------------------------------------------------------
    ValueError           Traceback (most recent call last)
    <ipython-input-42-b3c3f4515dd4> in <module>()
    ----> 1 int('a')

    ValueError: invalid literal for int() with base 10: 'a'

The error is perfectly logical. We’re trying to convert string 'a' into decimal format.

And if the example here is probably stupid, however, when you want to go through a list of strings and convert to a number the strings that contain numbers, you can get that error.

To avoid it, it would be nice to be able to check what we’re working with.

``isdigit()``
^^^^^^^^^^^^^

Python has such methods. For example, the ``isdigit()`` method can be used to check whether a string consists only of digits:

.. code:: python

    In [2]: "a".isdigit()
    Out[2]: False

    In [3]: "a10".isdigit()
    Out[3]: False

    In [4]: "10".isdigit()
    Out[4]: True


``isalpha()``
^^^^^^^^^^^^^

The ``isalpha()`` method makes it possible to check whether a string consists only of letters:

.. code:: python

    In [7]: "a".isalpha()
    Out[7]: True

    In [8]: "a100".isalpha()
    Out[8]: False

    In [9]: "a--  ".isalpha()
    Out[9]: False

    In [10]: "a ".isalpha()
    Out[10]: False

``isalnum()``
^^^^^^^^^^^^^

The ``isalnum()`` пethod makes it possible to check whether a string consists of letters or numbers:

.. code:: python

    In [11]: "a".isalnum()
    Out[1]: True

    In [12]: "a10".isalnum()
    Out[12]: True

``type()``
^^^^^^^^^^

Sometimes, depending on the result, a library or function can output different types of objects. For example, if an there is one object a string is returned, if several a tuple is returned.

We have to construct the program in different ways, depending on whether a string or a tuple has been returned.

The ``type()`` function can help:

.. code:: python

    In [13]: type("string")
    Out[13]: str

    In [14]: type("string") is str
    Out[14]: True

Similar to tuple (and other data types):

.. code:: python

    In [15]: type((1,2,3))
    Out[15]: tuple

    In [16]: type((1,2,3)) is tuple
    Out[16]: True

    In [17]: type((1,2,3)) is list
    Out[17]: False

