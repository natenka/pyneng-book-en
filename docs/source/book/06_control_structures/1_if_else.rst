if/elif/else
============

The ``if/elif/else`` construction allows make branches during program implementation. The program goes into the branch when a certain condition is met.

In this construction only **if** is mandatory, **elif** and **else**
are optional:

* **If** condition is always checked first.
* After **If** operator there must be some condition: if this condition is met (returns true), then the actions in block **if** are executed.
* **elif** can be used to make multiple branches, that is, to check incoming data for different conditions.
* **elif** block is the same as **if** but it checked next. Roughly speaking, it is "what if ..."
* There can be many **elif** blocks.
* **else** block is executed if none of the conditions **if** or **elif** were true.



Example of construction:

.. code:: python

    In [1]: a = 9

    In [2]: if a == 10:
       ...:     print('a equal to 10')
       ...: elif a < 10:
       ...:     print('a less than 10')
       ...: else:
       ...:     print('a less than 10')
       ...:
    a less than 10


Condition
-------

**If** construction is based on conditions: conditions are always written after **if** and **elif**.
Blocks if/elif are executed only when the condition returns True, so the first thing to deal with is what is true and what is false in Python.


True and False
~~~~~~~~~~~~

In Python, apart from the obvious True and False values, all other objects also have false or true value:

* True value:

  * any non-zero number
  * any non-empty string
  * any non-empty object

* False value:

  * 0
  * None
  * empty string
  * empty object


For example, since an empty list is a false value, it is possible to check whether the list is empty:

.. code:: python

    In [12]: list_to_test = [1, 2, 3]

    In [13]: if list_to_test:
       ....:     print("The list has objects")
       ....:
    The list has objects

The same result could have been achieved somewhat differently:

.. code:: python

    In [14]: if len(list_to_test) != 0:
       ....:     print("The list has objects")
       ....:
    The list has objects


Comparison operators
~~~~~~~~~~~~~~~~~~~

**Comparison operators** can be used in conditions like:

.. code:: python

    In [3]: 5 > 6
    Out[3]: False

    In [4]: 5 > 2
    Out[4]: True

    In [5]: 5 < 2
    Out[5]: False

    In [6]: 5 == 2
    Out[6]: False

    In [7]: 5 == 5
    Out[7]: True

    In [8]: 5 >= 5
    Out[8]: True

    In [9]: 5 <= 10
    Out[9]: True

    In [10]: 8 != 10
    Out[10]: True

.. note::
    Note that the equality is checked by double ``==``.

Example of the use of comparison operators:

.. code:: python

    In [1]: a = 9

    In [2]: if a == 10:
       ...:     print('a equal to 10')
       ...: elif a < 10:
       ...:     print('a less than 10')
       ...: else:
       ...:     print('a greater than 10')
       ...:
    a less than 10

Operator in
~~~~~~~~~~~

Operator ``in`` allows checking for the presence of an element in a sequence (for example, an element in a list or substrings in a string):

.. code:: python

    In [8]: 'Fast' in 'FastEthernet'
    Out[8]: True

    In [9]: 'Gigabit' in 'FastEthernet'
    Out[9]: False

    In [10]: vlan = [10, 20, 30, 40]

    In [11]: 10 in vlan
    Out[11]: True

    In [12]: 50 in vlan
    Out[12]: False

When used with dictionaries the **in** condition performs check by dictionary keys:

.. code:: python

    In [15]: r1 = {
       ....:  'IOS': '15.4',
       ....:  'IP': '10.255.0.1',
       ....:  'hostname': 'london_r1',
       ....:  'location': '21 New Globe Walk',
       ....:  'model': '4451',
       ....:  'vendor': 'Cisco'}

    In [16]: 'IOS' in r1
    Out[16]: True

    In [17]: '4451' in r1
    Out[17]: False

Operators  and, or, not
~~~~~~~~~~~~~~~~~~~~~~

The conditions can also use **logical operators**
``and``, ``or``, ``not``:

.. code:: python

    In [15]: r1 = {
       ....:  'IOS': '15.4',
       ....:  'IP': '10.255.0.1',
       ....:  'hostname': 'london_r1',
       ....:  'location': '21 New Globe Walk',
       ....:  'model': '4451',
       ....:  'vendor': 'Cisco'}

    In [18]: vlan = [10, 20, 30, 40]

    In [19]: 'IOS' in r1 and 10 in vlan
    Out[19]: True

    In [20]: '4451' in r1 and 10 in vlan
    Out[20]: False

    In [21]: '4451' in r1 or 10 in vlan
    Out[21]: True

    In [22]: not '4451' in r1
    Out[22]: True

    In [23]: '4451' not in r1
    Out[23]: True

Operator and
^^^^^^^^^^^^

In Python the ``and`` operator returns not a boolean value but a value of one of the operands.

If both operands are true, the result is a last value:

.. code:: python

    In [24]: 'string1' and 'string2'
    Out[24]: 'string2'

    In [25]: 'string1' and 'string2' and 'string3'
    Out[25]: 'string3'

If one of the operators is a false, the result of the expression will be the first false value:

.. code:: python

    In [26]: '' and 'string1'
    Out[26]: ''

    In [27]: '' and [] and 'string1'
    Out[27]: ''

Operator or
^^^^^^^^^^^

Operator ``or``, like operator ``and``, returns the value of one of the operands.

When checking operands, the first true operand is returned:

.. code:: python

    In [28]: '' or 'string1'
    Out[28]: 'string1'

    In [29]: '' or [] or 'string1'
    Out[29]: 'string1'

    In [30]: 'string1' or 'string2'
    Out[30]: 'string1'

If all values are false, the last value is returned:

.. code:: python

    In [31]: '' or [] or {}
    Out[31]: {}

An important feature of ``or`` operator - operands, which are after the true operand, are not calculated:

.. code:: python

    In [33]: '' or sorted([44,1,67])
    Out[33]: [1, 44, 67]

    In [34]: '' or 'string1' or sorted([44,1,67])
    Out[34]: 'string1'


.. _if_example:

Example of if/elif/else construction use
---------------------------------------------

An example of a check_password.py script that checks length of the password and whether the password contains username:

.. code:: python

    # -*- coding: utf-8 -*-

    username = input('Enter username: ')
    password = input('Enter password: ')

    if len(password) < 8:
        print('Password is too short')
    elif username in password:
        print('Password contains username')
    else:
        print('Password for user {} is set'.format(username))

Script check:

::

    $ python check_password.py
    Enter username: nata
    Enter password: nata1234
    Password contains username

    $ python check_password.py
    Enter username: nata 
    Enter password: 123nata123
    Password contains username

    $ python check_password.py
    Enter username: nata
    Enter password: 1234
    Password is too short

    $ python check_password.py
    Enter username: nata
    Enter password: 123456789
    Password for user nata is set

Ternary expression
----------------------------------------

It is sometimes more convenient to use a ternary operator than an extended form:

.. code:: python

    s = [1, 2, 3, 4]
    result = True if len(s) > 5 else False

It is best not to abuse it but in simple terms such a record can be useful.

