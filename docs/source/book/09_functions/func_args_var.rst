Variable length arguments
--------------------------

Sometimes it is necessary to make function accept not a fixed number of arguments, but any number. For such a case, in Python it is possible to create a function with a special parameter that accepts variable length arguments. This parameter can be both keyword and positional.

.. note::
    Even if you don’t use it in your scripts there’s a good chance you’ll find it in someone else’s code.

Variable length positional arguments
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Parameter that takes positional variable length arguments is created by adding an asterisk before parameter name. Parameter can have any name but by agreement ``*args`` is the most common name.

Example of a function:

.. code:: python

    In [1]: def sum_arg(a, *args):
      ....:     print(a, args)
      ....:     return a + sum(args)
      ....: 

Function ``sum_arg`` is created with two parameters:

* parameter ``a`` 

  * if passed as positional argument, should be first
  * if passed as a keyword argument, the order does not matter

* parameter ``*args`` - expects variable length arguments

  * all other arguments as a tuple
  * these arguments may be missed

Call a function with different number of arguments:

.. code:: python

    In [2]: sum_arg(1, 10, 20, 30)
    1 (10, 20, 30)
    Out[2]: 61

    In [3]: sum_arg(1, 10)
    1 (10,)
    Out[3]: 11

    In [4]: sum_arg(1)
    1 ()
    Out[4]: 1

You can also create such a function:

.. code:: python

    In [5]: def sum_arg(*args):
      ....:     print(args)
      ....:     return sum(args)
      ....: 

    In [6]: sum_arg(1, 10, 20, 30)
    (1, 10, 20, 30)
    Out[6]: 61

    In [7]: sum_arg()
    ()
    Out[7]: 0

Keyword variable length arguments
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Parameter that accepts keyword variable length arguments is created by adding two asterisk in front of parameter name. Name of parameter can be any but by agreement most commonly use name ``**kwargs`` (from keyword arguments).

Example of a function:

.. code:: python

    In [8]: def sum_arg(a, **kwargs):
      ....:     print(a, kwargs)
      ....:     return a + sum(kwargs.values())
      ....: 

Function ``sum_arg`` is created with two parameters:

* parameter ``a``
  
  * if passed as positional argument, should be first
  * if passed as a keyword argument, the order does not matter

* parameter ``**kwargs`` - expects keyword variable length arguments
  
  * all other keyword arguments as a dictionary
  * these arguments may be missed

Calling a function with different number of keyword arguments:

.. code:: python

    In [9]: sum_arg(a=10, b=10, c=20, d=30)
    10 {'c': 20, 'b': 10, 'd': 30}
    Out[9]: 70

    In [10]: sum_arg(b=10, c=20, d=30, a=10)
    10 {'c': 20, 'b': 10, 'd': 30}
    Out[10]: 70

