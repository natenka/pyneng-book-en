.. _range:

Range
-------------

Function range() returns an immutable sequence of numbers as a **range** object.

Function syntax:

.. code:: python

    range(stop)
    range(start, stop[, step])

Parameters of function:

* **start** - from what number the sequence begins. By default - 0
* **stop** - on which number the sequence of numbers ends. Mentioned number is not included in range
* **step** - with what step numbers increase. By default 1

Function range() stores only **start**, **stop** and **step** values and calculates values as necessary. This means that regardless of the size of range that describes range() function, it will always occupy a fixed amount of memory.

The easiest range() option is to pass only **stop** value:

.. code:: python

    In [1]: range(5)
    Out[1]: range(0, 5)

    In [2]: list(range(5))
    Out[2]: [0, 1, 2, 3, 4]

If two arguments are passed, the first is used as **start** and the second as **stop**:

.. code:: python

    In [3]: list(range(1, 5))
    Out[3]: [1, 2, 3, 4]

And in order to indicate sequence step, you have to pass three arguments:

.. code:: python

    In [4]: list(range(0, 10, 2))
    Out[4]: [0, 2, 4, 6, 8]

    In [5]: list(range(0, 10, 3))
    Out[5]: [0, 3, 6, 9]

Function range() can also generate descending sequences of numbers:

.. code:: python

    In [6]: list(range(10, 0, -1))
    Out[6]: [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]

    In [7]: list(range(5, -1, -1))
    Out[7]: [5, 4, 3, 2, 1, 0]

To obtain a descending sequence use a negative step and specify **start** by a greater number and **stop** by a smaller number.

In descending sequence the steps can also be different:

.. code:: python

    In [8]: list(range(10, 0, -2))
    Out[8]: [10, 8, 6, 4, 2]

Function supports negative **start** and **stop** values:

.. code:: python

    In [9]: list(range(-10, 0, 1))
    Out[9]: [-10, -9, -8, -7, -6, -5, -4, -3, -2, -1]

    In [10]: list(range(0, -10, -1))
    Out[10]: [0, -1, -2, -3, -4, -5, -6, -7, -8, -9]

The **range** object supports all `operations <https://docs.python.org/3.6/library/stdtypes.html#sequence-types-list-tuple-range>`__ that support sequences in Python, except addition and multiplication.

Check whether a number falls within a range:

.. code:: python

    In [11]: nums = range(5)

    In [12]: nums
    Out[12]: range(0, 5)

    In [13]: 3 in nums
    Out[13]: True

    In [14]: 7 in nums
    Out[14]: False

.. note::
    Starting with Python 3.2 this check is performed in constant time (O(1)).

You can get a specific range element:

.. code:: python

    In [15]: nums = range(5)

    In [16]: nums[0]
    Out[16]: 0

    In [17]: nums[-1]
    Out[17]: 4

Range supports slices:

.. code:: python

    In [18]: nums = range(5)

    In [19]: nums[1:]
    Out[19]: range(1, 5)

    In [20]: nums[:3]
    Out[20]: range(0, 3)

You can get range length:

.. code:: python

    In [21]: nums = range(5)

    In [22]: len(nums)
    Out[22]: 5

And a minimum and maximum element:

.. code:: python

    In [23]: nums = range(5)

    In [24]: min(nums)
    Out[24]: 0

    In [25]: max(nums)
    Out[25]: 4

In addition, **range** object supports index() method:

.. code:: python

    In [26]: nums = range(1, 7)

    In [27]: nums.index(3)
    Out[27]: 2

