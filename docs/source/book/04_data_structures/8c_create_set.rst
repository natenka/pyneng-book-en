Options for set creation 
~~~~~~~~~~~~~~~~~~~~~~~~~~~

You cannot create an empty set using a literal set (in this case it will not be a set but a dictionary):

.. code:: python

    In [1]: set1 = {}

    In [2]: type(set1)
    Out[2]: dict

But an empty set can be created in this way:

.. code:: python

    In [3]: set2 = set()

    In [4]: type(set2)
    Out[4]: set

Set from string:

.. code:: python

    In [5]: set('long long long long string')
    Out[5]: {' ', 'g', 'i', 'l', 'n', 'o', 'r', 's', 't'}

Set from list:

.. code:: python

    In [6]: set([10,20,30,10,10,30])
    Out[6]: {10, 20, 30}

