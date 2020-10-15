Set
===============

Set is a mutable unordered data type. Set always contains only unique elements.

Set in Python is a sequence of elements that are separated by a comma and placed in curly brackets.

Set can easily remove repetitive elements:

.. code:: python

    In [1]: vlans = [10, 20, 30, 40, 100, 10]

    In [2]: set(vlans)
    Out[2]: {10, 20, 30, 40, 100}

    In [3]: set1 = set(vlans)

    In [4]: print(set1)
    {40, 100, 10, 20, 30}

.. toctree::
   :maxdepth: 1

   8a_set_methods
   8b_set_operations
   8c_create_set
