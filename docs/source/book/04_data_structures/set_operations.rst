Operations with sets
~~~~~~~~~~~~~~~~~~~~~~

Sets are useful in performing different operations such us finding union of sets, intersection and so on. 

Union of sets can be obtained by ``union()`` or
operator ``|``:

.. code:: python

    In [1]: vlans1 = {10,20,30,50,100}
    In [2]: vlans2 = {100,101,102,102,200}

    In [3]: vlans1.union(vlans2)
    Out[3]: {10, 20, 30, 50, 100, 101, 102, 200}

    In [4]: vlans1 | vlans2
    Out[4]: {10, 20, 30, 50, 100, 101, 102, 200}

Intersection of sets can be obtained by
``intersection()`` or operator ``&``:

.. code:: python

    In [5]: vlans1 = {10,20,30,50,100}
    In [6]: vlans2 = {100,101,102,102,200}

    In [7]: vlans1.intersection(vlans2)
    Out[7]: {100}

    In [8]: vlans1 & vlans2
    Out[8]: {100}

