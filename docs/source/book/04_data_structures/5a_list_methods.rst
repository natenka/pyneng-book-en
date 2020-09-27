Useful methods for working with lists
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The list is a mutable  data type, so it is important to note that most methods for working with lists change the list on the spot without returning anything.

``join()``
^^^^^^^^^^

The **join()** method collects a list of strings into one string with the separator specified before join:

.. code:: python

    In [16]: vlans = ['10', '20', '30']

    In [17]: ','.join(vlans)
    Out[17]: '10,20,30'

.. note::
    The join method actually relates to strings but since the value must be given as a list, it is considered in this section.

``append()``
^^^^^^^^^^^^

The **append()** method adds the specified item to the end of the list:

.. code:: python

    In [18]: vlans = ['10', '20', '30', '100-200']

    In [19]: vlans.append('300')

    In [20]: vlans
    Out[20]: ['10', '20', '30', '100-200', '300']

The append() method changes the list on the spot and does not return anything.

``extend()``
^^^^^^^^^^^^

If you want to combine two lists you can use two methods: the **extend()** method and the addition operation.

These methods have an important difference: extend() changes the list to which the method is applied and addition returns a new list that consists of two.

The extend() method:

.. code:: python

    In [21]: vlans = ['10', '20', '30', '100-200']

    In [22]: vlans2 = ['300', '400', '500']

    In [23]: vlans.extend(vlans2)

    In [24]: vlans
    Out[24]: ['10', '20', '30', '100-200', '300', '400', '500']

Addition operation:

.. code:: python

    In [27]: vlans = ['10', '20', '30', '100-200']

    In [28]: vlans2 = ['300', '400', '500']

    In [29]: vlans + vlans2
    Out[29]: ['10', '20', '30', '100-200', '300', '400', '500']

Note that when adding lists in IPython the line Out appeared. This means that the result of the summation can be assigned to the variable:

.. code:: python

    In [30]: result = vlans + vlans2

    In [31]: result
    Out[31]: ['10', '20', '30', '100-200', '300', '400', '500']

``pop()``
^^^^^^^^^

The **pop()** method removes the item that corresponds to the specified number. But, importantly, the method returns this item:

.. code:: python

    In [28]: vlans = ['10', '20', '30', '100-200']

    In [29]: vlans.pop(-1)
    Out[29]: '100-200'

    In [30]: vlans
    Out[30]: ['10', '20', '30']

Without number specified the last item in the list is deleted.

``remove()``
^^^^^^^^^^^^

The **remove()** method removes the specified item.

remove() does not return the deleted item:

.. code:: python

    In [31]: vlans = ['10', '20', '30', '100-200']

    In [32]: vlans.remove('20')

    In [33]: vlans
    Out[33]: ['10', '30', '100-200']

In remove() method you must specify the item to be deleted, not its number in the list. If item number is specified, error occurs:

.. code:: python

    In [34]: vlans.remove(-1)
    -------------------------------------------------
    ValueError      Traceback (most recent call last)
    <ipython-input-32-f4ee38810cb7> in <module>()
    ----> 1 vlans.remove(-1)

    ValueError: list.remove(x): x not in list

``index()``
^^^^^^^^^^^

The **index()** method is used to check under which number the item is stored in the list:

.. code:: python

    In [35]: vlans = ['10', '20', '30', '100-200']

    In [36]: vlans.index('30')
    Out[36]: 2

``insert()``
^^^^^^^^^^^^

The **insert()** method allows you to insert an item into a specific place in the list:

.. code:: python

    In [37]: vlans = ['10', '20', '30', '100-200']

    In [38]: vlans.insert(1, '15')

    In [39]: vlans
    Out[39]: ['10', '15', '20', '30', '100-200']

``sort()``
^^^^^^^^^^

The sort() method sorts on the spot:

.. code:: python

    In [40]: vlans = [1, 50, 10, 15]

    In [41]: vlans.sort()

    In [42]: vlans
    Out[42]: [1, 10, 15, 50]

