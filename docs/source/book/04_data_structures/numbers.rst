Numbers
=====

With numbers it is possible to perform various mathematical operations.

.. code:: python

    In [1]: 1 + 2
    Out[1]: 3

    In [2]: 1.0 + 2
    Out[2]: 3.0

    In [3]: 10 - 4
    Out[3]: 6

    In [4]: 2**3
    Out[4]: 8

Division int and float:

.. code:: python

    In [5]: 10/3
    Out[5]: 3.3333333333333335

    In [6]: 10/3.0
    Out[6]: 3.3333333333333335

The round() function - round a number to a given precision in decimal digits:

.. code:: python

    In [9]: round(10/3.0, 2)
    Out[9]: 3.33

    In [10]: round(10/3.0, 4)
    Out[10]: 3.3333

Remainder of division:

.. code:: python

    In [11]: 10 % 3
    Out[11]: 1

Comparison operators

.. code:: python

    In [12]: 10 > 3.0
    Out[12]: True

    In [13]: 10 < 3
    Out[13]: False

    In [14]: 10 == 3
    Out[14]: False

    In [15]: 10 == 10
    Out[15]: True

    In [16]: 10 <= 10
    Out[16]: True

    In [17]: 10.0 == 10
    Out[17]: True

The int() function allows converting to int type. The second argument can specify number system:

.. code:: python

    In [18]: a = '11'

    In [19]: int(a)
    Out[19]: 11

If you specify that string should be read as a binary number, the result is:

.. code:: python

    In [20]: int(a, 2)
    Out[20]: 3

Convert to int from float:

.. code:: python

    In [21]: int(3.333)
    Out[21]: 3

    In [22]: int(3.9)
    Out[22]: 3

The bin() function produces a binary representation of a number (note that the result is a string):

.. code:: python

    In [23]: bin(8)
    Out[23]: '0b1000'

    In [24]: bin(255)
    Out[24]: '0b11111111'

Similarly, function hex() produces a hexadecimal value:

.. code:: python

    In [25]: hex(10)
    Out[25]: '0xa'

And, of course, you can do several changes at the same time:

.. code:: python

    In [26]: int('ff', 16)
    Out[26]: 255

    In [27]: bin(int('ff', 16))
    Out[27]: '0b11111111'

For more complex mathematical functions, Python has a ``math`` module:

.. code:: python

    In [28]: import  math

    In [29]: math.sqrt(9)
    Out[29]: 3.0

    In [30]: math.sqrt(10)
    Out[30]: 3.1622776601683795

    In [31]: math.factorial(3)
    Out[31]: 6

    In [32]: math.pi
    Out[32]: 3.141592653589793

