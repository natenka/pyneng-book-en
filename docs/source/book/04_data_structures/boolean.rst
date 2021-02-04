Boolean values
===============

Boolean values in Python are two constants ``True`` and ``False``.

In Python, not only True and False are considered True and False values.

* True value:

  * any non-zero number
  * any non-empty string
  * any non-empty object

* False value:

  * 0
  * None
  * empty string
  * empty object

Other True and False values tend to follow the condition logically.

To check boolean value of object you can use ``bool``:

.. code:: python

    In [2]: items = [1, 2, 3]

    In [3]: empty_list = []

    In [4]: bool(empty_list)
    Out[4]: False

    In [5]: bool(items)
    Out[5]: True

    In [6]: bool(0)
    Out[6]: False

    In [7]: bool(1)
    Out[7]: True

