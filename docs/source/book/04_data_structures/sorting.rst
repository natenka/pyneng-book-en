Sorting basics
==============

When sorting data like list of lists or list of tuples,
``sorted`` sorts by the first element of nested lists (tuples),
and if the first element is the same, on the second:

.. code:: python

    In [1]: data = [[1, 100, 1000], [2, 2, 2], [1, 2, 3], [4, 100, 3]]

    In [2]: sorted(data)
    Out[2]: [[1, 2, 3], [1, 100, 1000], [2, 2, 2], [4, 100, 3]]


If the sort is done for a list of numbers that are written as strings,
the sort will be lexicographic, not natural, and the order will be:

.. code:: python

    In [7]: vlans = ['1', '30', '11', '3', '10', '20', '30', '100']

    In [8]: sorted(vlans)
    Out[8]: ['1', '10', '100', '11', '20', '3', '30', '30']

For the sorting to be "correct" it is necessary to convert vlans to numbers.

The same problem appears, for example, with IP addresses:

.. code:: python

    In [2]: ip_list = ["10.1.1.1", "10.1.10.1", "10.1.2.1", "10.1.11.1"]

    In [3]: sorted(ip_list)
    Out[3]: ['10.1.1.1', '10.1.10.1', '10.1.11.1', '10.1.2.1']

How to solve the problem with sorting IP addresses is discussed in section "10. Useful functions".
