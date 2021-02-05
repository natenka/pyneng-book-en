filter
--------------

Function ``filter()`` applies function to all sequence elements and returns
an iterator with those objects for which function has returned True.

For example, return only those strings that contain numbers:

.. code:: python

    In [1]: list_of_strings = ['one', 'two', 'list', '', 'dict', '100', '1', '50']

    In [2]: filter(str.isdigit, list_of_strings)
    Out[2]: <filter at 0xb45eb1cc>

    In [3]: list(filter(str.isdigit, list_of_strings))
    Out[3]: ['100', '1', '50']

From the list of numbers leave only odd:

.. code:: python

    In [3]: list(filter(lambda x: x % 2, [10, 111, 102, 213, 314, 515]))
    Out[3]: [111, 213, 515]

Similarly, only even ones:

.. code:: python

    In [4]: list(filter(lambda x: not x % 2, [10, 111, 102, 213, 314, 515]))
    Out[4]: [10, 102, 314]

From the list of words leave only those with more than two letters:

.. code:: python

    In [5]: list_of_words = ['one', 'two', 'list', '', 'dict']

    In [6]: list(filter(lambda x: len(x) > 2, list_of_words))
    Out[6]: ['one', 'two', 'list', 'dict']

List comprehension instead of filter
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

As a rule, you can use list comprehension instead of ``filter``.

Examples similar to those above in list comprehension version.

Return only those strings that contain numbers:

.. code:: python

    In [7]: list_of_strings = ['one', 'two', 'list', '', 'dict', '100', '1', '50']

    In [8]: [s for s in list_of_strings if s.isdigit()]
    Out[8]: ['100', '1', '50']

Odd/even numbers:

.. code:: python

    In [9]: nums = [10, 111, 102, 213, 314, 515]

    In [10]: [n for n in nums if n % 2]
    Out[10]: [111, 213, 515]

    In [11]: [n for n in nums if not n % 2]
    Out[11]: [10, 102, 314]

From the list of words leave only those with more than two letters:

.. code:: python

    In [12]: list_of_words = ['one', 'two', 'list', '', 'dict']

    In [13]: [word for word in list_of_words if len(word) > 2]
    Out[13]: ['one', 'two', 'list', 'dict']

