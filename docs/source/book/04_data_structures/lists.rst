List
=============

List in Python is:

* sequence of elements separated by comma and enclosed in square brackets
* mutable ordered data type

Examples of lists:

.. code:: python

    In [1]: list1 = [10,20,30,77]
    In [2]: list2 = ['one', 'dog', 'seven']
    In [3]: list3 = [1, 20, 4.0, 'word']

Creating a list using a literal:

.. code:: python

    In [1]: vlans = [10, 20, 30, 50]

.. note::

    Literal is an expression that creates an object.

Create a list using ``list`` function:

.. code:: python

    In [2]: list1 = list('router')

    In [3]: print(list1)
    ['r', 'o', 'u', 't', 'e', 'r']

Since a list is an ordered data type just like a string, in lists you can
refer to an item by number, make slices:

.. code:: python

    In [4]: list3 = [1, 20, 4.0, 'word']

    In [5]: list3[1]
    Out[5]: 20

    In [6]: list3[1::]
    Out[6]: [20, 4.0, 'word']

    In [7]: list3[-1]
    Out[7]: 'word'

    In [8]: list3[::-1]
    Out[8]: ['word', 4.0, 20, 1]

You can reverse list by ``reverse`` method:

.. code:: python

    In [10]: vlans = ['10', '15', '20', '30', '100-200']

    In [11]: vlans.reverse()

    In [12]: vlans
    Out[12]: ['100-200', '30', '20', '15', '10']

Since lists are mutable, list elements can be changed:

.. code:: python

    In [13]: list3
    Out[13]: [1, 20, 4.0, 'word']

    In [14]: list3[0] = 'test'

    In [15]: list3
    Out[15]: ['test', 20, 4.0, 'word']

You can also create a list of lists. As in a regular list you can refer to
items in nested lists:

.. code:: python

    In [16]: interfaces = [['FastEthernet0/0', '15.0.15.1', 'YES', 'manual', 'up', 'up'],
       ....: ['FastEthernet0/1', '10.0.1.1', 'YES', 'manual', 'up', 'up'],
       ....: ['FastEthernet0/2', '10.0.2.1', 'YES', 'manual', 'up', 'down']]

    In [17]: interfaces[0][0]
    Out[17]: 'FastEthernet0/0'

    In [18]: interfaces[2][0]
    Out[18]: 'FastEthernet0/2'

    In [19]: interfaces[2][1]
    Out[19]: '10.0.2.1'

The ``len`` function returns number of items in list:

.. code:: python

    In [1]: items = [1, 2, 3]

    In [2]: len(items)
    Out[2]: 3

And ``sorted`` function sorts list items in ascending order and returns
a new list with sorted items:

.. code:: python

    In [1]: names = ['John', 'Michael', 'Antony']

    In [2]: sorted(names)
    Out[2]: ['Antony', 'John', 'Michael']



.. toctree::
   :maxdepth: 1
   :hidden:

   list_methods
