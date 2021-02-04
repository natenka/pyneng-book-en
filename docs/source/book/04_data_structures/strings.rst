Strings
================

String in Python is:

* sequence of characters enclosed in quotes
* immutable ordered data type

Examples of strings:

.. code:: python

    In [9]: 'Hello'
    Out[9]: 'Hello'
    In [10]: "Hello"
    Out[10]: 'Hello'

    In [11]: tunnel = """
       ....: interface Tunnel0
       ....:  ip address 10.10.10.1 255.255.255.0
       ....:  ip mtu 1416
       ....:  ip ospf hello-interval 5
       ....:  tunnel source FastEthernet1/0
       ....:  tunnel protection ipsec profile DMVPN
       ....: """

    In [12]: tunnel
    Out[12]: '\ninterface Tunnel0\n ip address 10.10.10.1 255.255.255.0\n ip mtu 1416\n ip ospf hello-interval 5\n tunnel source FastEthernet1/0\n tunnel protection ipsec profile DMVPN\n'

    In [13]: print(tunnel)

    interface Tunnel0
     ip address 10.10.10.1 255.255.255.0
     ip mtu 1416
     ip ospf hello-interval 5
     tunnel source FastEthernet1/0
     tunnel protection ipsec profile DMVPN

Strings can be summed. Then they merge into one string:

.. code:: python

    In [14]: intf = 'interface'

    In [15]: tun = 'Tunnel0'

    In [16]: intf + tun
    Out[16]: 'interfaceTunnel0'

    In [17]: intf + ' ' + tun
    Out[17]: 'interface Tunnel0'

You can multiply a string by a number. In this case, string repeats specified number of times:

.. code:: python

    In [18]: intf * 5
    Out[18]: 'interfaceinterfaceinterfaceinterfaceinterface'

    In [19]: '#' * 40
    Out[19]: '########################################'

The fact that strings are an ordered data type allows to refer to characters in a string by a number starting from zero:

.. code:: python

    In [20]: string1 = 'interface FastEthernet1/0'

    In [21]: string1[0]
    Out[21]: 'i'

All characters in a string are numbered from zero. But if you need to refer to a character from the end, you can specify negative values (this time with 1).

.. code:: python

    In [22]: string1[1]
    Out[22]: 'n'

    In [23]: string1[-1]
    Out[23]: '0'

In addition to referring to a specific character you can make string slices by specifying a number range. Slicing starts with first number (included) and ends at second number (excluded):

.. code:: python

    In [24]: string1[0:9]
    Out[24]: 'interface'

    In [25]: string1[10:22]
    Out[25]: 'FastEthernet'

If no second number is specified, slice is until the end of string:

.. code:: python

    In [26]:  string1[10:]
    Out[26]: 'FastEthernet1/0'

Slice last three character of string:

.. code:: python

    In [27]: string1[-3:]
    Out[27]: '1/0'

You can also specify a step in slice. For example, you can get odd numbers:

.. code:: python

    In [28]: a = '0123456789'

    In [29]: a[1::2]
    Out[29]: '13579'

Or you can get all even numbers of string ``a``:

.. code:: python

    In [31]: a[::2]
    Out[31]: '02468'

Slices can also be used to get a string in reverse order:

.. code:: python

    In [28]: a = '0123456789'

    In [29]: a[::]
    Out[29]: '0123456789'

    In [30]: a[::-1]
    Out[30]: '9876543210'

.. note::
    Entries ``a[::]`` and ``a[:]`` give the same result but double colon makes it possible to indicate that not every element should be taken, but for example every second element.

The ``len`` function allows you to get number of characters in a string:

.. code:: python

    In [1]: line = 'interface Gi0/1'

    In [2]: len(line)
    Out[2]: 15

.. note::
    Function and method differ in that method is tied to a particular type of object and function is generally more universal and can be applied to objects of different types. For example, ``len`` function can be applied to strings, lists, dictionaries and so on, but ``startswith`` method only applies to strings.


.. toctree::
   :maxdepth: 1

   string_methods
   string_format
   string_literal_concatenation
