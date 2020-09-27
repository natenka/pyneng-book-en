Class creation
---------------

.. note::

    Note that the basis is explained here given that the reader has no experience with OOP. Some examples are not very correct from Python’s ideology point of view, but they help to better understand how it works. At the end, an explanation is given of how this should be done in proper way.

Keyword ``class`` is used in python to create classes. The easiest class you can create in Python:

.. code:: python

    In [1]: class Switch:
       ...:     pass
       ...:

.. note::

    Class names: usually class names in Python are written in CamelCase format.

To create a class instance, call class:

.. code:: python

    In [2]: sw1 = Switch()

    In [3]: print(sw1)
    <__main__.Switch object at 0xb44963ac>

Using dot notation, it is possible to derive values of instance variables, create new variables and assign a new value to existing ones:

.. code:: python

    In [5]: sw1.hostname = 'sw1'

    In [6]: sw1.model = 'Cisco 3850'

In another instance of Switch class, the variables may be different:

.. code:: python

    In [7]: sw2 = Switch()

    In [8]: sw2.hostname = 'sw2'

    In [9]: sw2.model = 'Cisco 3750'

You can see value of instance variables using the same dot notation:

.. code:: python

    In [10]: sw1.model
    Out[10]: 'Cisco 3850'

    In [11]: sw2.model
    Out[11]: 'Cisco 3750'

