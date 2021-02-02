Anonymous function (lambda expression)
------------------------------------

In Python, ``lambda`` expression allows creation of anonymous functions - functions that are not tied to a name.

Anonymous function:

    * may contain only one expression
    * can pass as many arguments as you want

Standard function:

.. code:: python

    In [1]: def sum_arg(a, b): return a + b

    In [2]: sum_arg(1,2)
    Out[2]: 3

Similar anonymous function or ``lambda`` function:

.. code:: python

    In [3]: sum_arg = lambda a, b: a + b

    In [4]: sum_arg(1,2)
    Out[4]: 3

Note that there is no **return** operator in ``lambda`` function definition
because there can only be one expression in this function that always returns
a value and closes the function.

Function ``lambda`` is convenient to use in expressions where you need
to write a small function for data processing.
For example, in ``sorted`` function you can use lambda expression to specify sorting key:

.. code:: python

    In [5]: list_of_tuples = [('IT_VLAN', 320),
        ...:  ('Mngmt_VLAN', 99),
        ...:  ('User_VLAN', 1010),
        ...:  ('DB_VLAN', 11)]

    In [6]: sorted(list_of_tuples, key=lambda x: x[1])
    Out[6]: [('DB_VLAN', 11), ('Mngmt_VLAN', 99), ('IT_VLAN', 320), ('User_VLAN', 1010)]

Function ``lambda`` is also useful in ``map`` and ``filter`` functions which will be discussed in the following sections.
