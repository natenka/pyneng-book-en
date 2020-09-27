Arithmetic operator support
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Special methods are also responsible for arithmetic operations support, for example, __add__ method is responsible for addition operation:

.. code:: python

    __add__(self, other)

Let’s add to IPAddress class the support of summing with numbers, but in order not to complicate method implementation we will take an advantage of *ipaddress* module possibilities.

.. code:: python

    In [1]: import ipaddress

    In [2]: ipaddress1 = ipaddress.ip_address('10.1.1.1')

    In [3]: int(ipaddress1)
    Out[3]: 167837953

    In [4]: ipaddress.ip_address(167837953)
    Out[4]: IPv4Address('10.1.1.1')

IPAddress class with __add__:

.. code:: python

    In [5]: class IPAddress:
       ...:     def __init__(self, ip):
       ...:         self.ip = ip
       ...:
       ...:     def __str__(self):
       ...:         return f"IPAddress: {self.ip}"
       ...:
       ...:     def __repr__(self):
       ...:         return f"IPAddress('{self.ip}')"
       ...:
       ...:     def __add__(self, other):
       ...:         ip_int = int(ipaddress.ip_address(self.ip))
       ...:         sum_ip_str = str(ipaddress.ip_address(ip_int + other))
       ...:         return IPAddress(sum_ip_str)
       ...:

ip_int variable refers to source address value in decimal format. And sum_ip_str is a string with IP address obtained by adding two numbers. In general, it is desirable that the summation operation returns an instance of the same class, so in the last line of method an instance of IPAddress class is created and a string with resulting address is passed to it as an argument.

Now IPAddress class instances must support addition with number. As a result we get a new instance of IPAddress class.

.. code:: python

    In [6]: ip1 = IPAddress('10.1.1.1')

    In [7]: ip1 + 5
    Out[7]: IPAddress('10.1.1.6')

Since ipaddress module is used within method and it supports creating IP address only from a decimal number, it is necessary to limit method to work only with **int** data type. If the second element was an object of another type, an exception must be generated. Exception and error message take from the analogous error of ipaddress.ip_address() function:

.. code:: python

    In [8]: a1 = ipaddress.ip_address('10.1.1.1')

    In [9]: a1 + 4
    Out[9]: IPv4Address('10.1.1.5')

    In [10]: a1 + 4.0
    ---------------------------------------------------------------------------
    TypeError                                 Traceback (most recent call last)
    <ipython-input-10-a0a045adedc5> in <module>
    ----> 1 a1 + 4.0

    TypeError: unsupported operand type(s) for +: 'IPv4Address' and 'float'

Now IPAddress class looks like:

.. code:: python

    In [11]: class IPAddress:
        ...:     def __init__(self, ip):
        ...:         self.ip = ip
        ...:
        ...:     def __str__(self):
        ...:         return f"IPAddress: {self.ip}"
        ...:
        ...:     def __repr__(self):
        ...:         return f"IPAddress('{self.ip}')"
        ...:
        ...:     def __add__(self, other):
        ...:         if not isinstance(other, int):
        ...:             raise TypeError(f"unsupported operand type(s) for +:"
        ...:                             f" 'IPAddress' and '{type(other).__name__}'")
        ...:
        ...:         ip_int = int(ipaddress.ip_address(self.ip))
        ...:         sum_ip_str = str(ipaddress.ip_address(ip_int + other))
        ...:         return IPAddress(sum_ip_str)
        ...:

If the second operand is not an instanse of **int** class, a TypeError exception is generated. In exception, information is displayed that summation is not supported between IPAddress class instances and operand class instance. Class name is derived from class itself, after calling type: ``type(other).__name__``.

Check for summation with decimal number and error generation:

.. code:: python

    In [12]: ip1 = IPAddress('10.1.1.1')

    In [13]: ip1 + 5
    Out[13]: IPAddress('10.1.1.6')

    In [14]: ip1 + 5.0
    ---------------------------------------------------------------------------
    TypeError                                 Traceback (most recent call last)
    <ipython-input-14-5e619f8dc37a> in <module>
    ----> 1 ip1 + 5.0

    <ipython-input-11-77b43bc64757> in __add__(self, other)
         11     def __add__(self, other):
         12         if not isinstance(other, int):
    ---> 13             raise TypeError(f"unsupported operand type(s) for +:"
         14                             f" 'IPAddress' and '{type(other).__name__}'")
         15

    TypeError: unsupported operand type(s) for +: 'IPAddress' and 'float'

    In [15]: ip1 + '1'
    ---------------------------------------------------------------------------
    TypeError                                 Traceback (most recent call last)
    <ipython-input-15-c5ce818f55d8> in <module>
    ----> 1 ip1 + '1'

    <ipython-input-11-77b43bc64757> in __add__(self, other)
         11     def __add__(self, other):
         12         if not isinstance(other, int):
    ---> 13             raise TypeError(f"unsupported operand type(s) for +:"
         14                             f" 'IPAddress' and '{type(other).__name__}'")
         15

    TypeError: unsupported operand type(s) for +: 'IPAddress' and 'str'

.. seealso:: Manual of special methods `Numeric magic methods <https://rszalski.github.io/magicmethods/#numeric>`__
