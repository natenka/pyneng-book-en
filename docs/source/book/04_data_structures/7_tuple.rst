Tuple
--------------


Tuple in Python is:

* a sequence of elements separated by a comma and enclosed in brackets
* immutable ordered data type

Roughly speaking, a tuple is a list that can’t be changed. I mean, tuple only has reading rights. It could be a defense against accidental change.

Create an empty tuple:

.. code:: python

    In [1]: tuple1 = tuple()

    In [2]: print(tuple1)
    ()

Tuple with one element (note the comma):

.. code:: python

    In [3]: tuple2 = ('password',)

Tuple from list:

.. code:: python

    In [4]: list_keys = ['hostname', 'location', 'vendor', 'model', 'ios', 'ip']

    In [5]: tuple_keys = tuple(list_keys)

    In [6]: tuple_keys
    Out[6]: ('hostname', 'location', 'vendor', 'model', 'ios', 'ip')

Objects in tuple can be accessed as well as objects in list, by order number:

.. code:: python

    In [7]: tuple_keys[0]
    Out[7]: 'hostname'

But since tuple is immutable you cannot assign a new value:

.. code:: python

    In [8]: tuple_keys[1] = 'test'
    ---------------------------------------------------------------------------
    TypeError                                 Traceback (most recent call last)
    <ipython-input-9-1c7162cdefa3> in <module>()
    ----> 1 tuple_keys[1] = 'test'

    TypeError: 'tuple' object does not support item assignment

Function sorted() sorts tuple elements in ascending order and returns a new list with sorted elements:

.. code:: python


    In [2]: tuple_keys = ('hostname', 'location', 'vendor', 'model', 'ios', 'ip')

    In [3]: sorted(tuple_keys)
    Out[3]: ['hostname', 'ios', 'ip', 'location', 'model', 'vendor']

