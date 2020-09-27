Map
-----------

The map() function applies function to each element of sequence and returns iterator with  result.

For example, map() can be used to perform element transformations. Convert all strings to uppercase:

.. code:: python

    In [1]: list_of_words = ['one', 'two', 'list', '', 'dict']

    In [2]: map(str.upper, list_of_words)
    Out[2]: <map at 0xb45eb7ec>

    In [3]: list(map(str.upper, list_of_words))
    Out[3]: ['ONE', 'TWO', 'LIST', '', 'DICT']

Converting to numbers:

.. code:: python

    In [3]: list_of_str = ['1', '2', '5', '10']

    In [4]: list(map(int, list_of_str))
    Out[4]: [1, 2, 5, 10]

With map() it is convenient to use lambda expressions:

.. code:: python

    In [5]: vlans = [100, 110, 150, 200, 201, 202]

    In [6]: list(map(lambda x: 'vlan {}'.format(x), vlans))
    Out[6]: ['vlan 100', 'vlan 110', 'vlan 150', 'vlan 200', 'vlan 201', 'vlan 202']

If map() function expects two arguments, two lists are passed:

.. code:: python

    In [7]: nums = [1, 2, 3, 4, 5]

    In [8]: nums2 = [100, 200, 300, 400, 500]

    In [9]: list(map(lambda x, y: x*y, nums, nums2))
    Out[9]: [100, 400, 900, 1600, 2500]

List comprehension instead of map
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

As a rule, you can use list comprehension instead of map(). Most often, list comprehension option is more understandable and in some cases even faster.

    `Alex Martelli response with comparison of map and list
    comprehension <https://stackoverflow.com/a/1247490>`__

But map() can be more effective when you have to generate a large number of elements because map() is an iterator and list comprehension generates a list.

Examples similar to those above in the list comprehension variant.

Convert all strings to uppercase:

.. code:: python

    In [48]: list_of_words = ['one', 'two', 'list', '', 'dict']

    In [49]: [ str.upper(word) for word in list_of_words ]
    Out[49]: ['ONE', 'TWO', 'LIST', '', 'DICT']

Converting to numbers:

.. code:: python

    In [50]: list_of_str = ['1', '2', '5', '10']

    In [51]: [ int(i) for i in list_of_str ]
    Out[51]: [1, 2, 5, 10]

String formatting:

.. code:: python

    In [52]:  vlans = [100, 110, 150, 200, 201, 202]

    In [53]: [ 'vlan {}'.format(x) for x in vlans ]
    Out[53]: ['vlan 100', 'vlan 110', 'vlan 150', 'vlan 200', 'vlan 201', 'vlan 202']

Use zip() to get pairs of elements:

.. code:: python

    In [54]: nums = [1, 2, 3, 4, 5]

    In [55]: nums2 = [100, 200, 300, 400, 500]

    In [56]: [ x*y for x, y in zip(nums,nums2) ]
    Out[56]: [100, 400, 900, 1600, 2500]

