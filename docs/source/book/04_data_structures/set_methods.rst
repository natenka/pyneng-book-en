Set methods
~~~~~~~~~~~

``add``
^^^^^^^^^

Method ``add`` adds an item to set:

.. code:: python

    In [1]: set1 = {10,20,30,40}

    In [2]: set1.add(50)

    In [3]: set1
    Out[3]: {10, 20, 30, 40, 50}

``discard``
^^^^^^^^^^^^^

Method ``discard`` allows deleting elements without showing an error if there
is no element in set:

.. code:: python

    In [3]: set1
    Out[3]: {10, 20, 30, 40, 50}

    In [4]: set1.discard(55)

    In [5]: set1
    Out[5]: {10, 20, 30, 40, 50}

    In [6]: set1.discard(50)

    In [7]: set1
    Out[7]: {10, 20, 30, 40}

``clear``
^^^^^^^^^^^

Method ``clear`` empties set:

.. code:: python

    In [8]: set1 = {10,20,30,40}

    In [9]: set1.clear()

    In [10]: set1
    Out[10]: set()

