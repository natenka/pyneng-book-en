Methods ``__str__``, ``__repr__``
~~~~~~~~~~~~~~~~~~~~~~~~

Special methods ``__str__`` and ``__repr__`` are responsible for string
representation of the object. They are used in different places.

Consider example of IPAddress class that is responsible for representing
IPv4 address:

.. code:: python

    In [1]: class IPAddress:
       ...:     def __init__(self, ip):
       ...:         self.ip = ip
       ...:

After creating class instances, they have a default string view that looks
like this (the same output is displayed when ``print`` is used):

.. code:: python

    In [2]: ip1 = IPAddress('10.1.1.1')

    In [3]: ip2 = IPAddress('10.2.2.2')

    In [4]: str(ip1)
    Out[4]: '<__main__.IPAddress object at 0xb4e4e76c>'

    In [5]: str(ip2)
    Out[5]: '<__main__.IPAddress object at 0xb1bd376c>'

Unfortunately, this presentation is not very informative. It would be better
to show information about which address this instance represents. Special
method ``__str__`` is responsible for displaying information when using ``str``
function. As an argument this method expects only instance and must return string.

.. code:: python

    In [6]: class IPAddress:
       ...:     def __init__(self, ip):
       ...:         self.ip = ip
       ...:
       ...:     def __str__(self):
       ...:         return f"IPAddress: {self.ip}"
       ...:

    In [7]: ip1 = IPAddress('10.1.1.1')

    In [8]: ip2 = IPAddress('10.2.2.2')

    In [9]: str(ip1)
    Out[9]: 'IPAddress: 10.1.1.1'

    In [10]: str(ip2)
    Out[10]: 'IPAddress: 10.2.2.2'

A second string view which is used in Python objects is displayed when using
``repr`` function and when adding objects to containers such as lists:


.. code:: python

    In [11]: ip_addresses = [ip1, ip2]

    In [12]: ip_addresses
    Out[12]: [<__main__.IPAddress at 0xb4e40c8c>, <__main__.IPAddress at 0xb1bc46ac>]

    In [13]: repr(ip1)
    Out[13]: '<__main__.IPAddress object at 0xb4e40c8c>'

Method ``__repr__`` is responsible for this output and it should also return a
string, but it would return a string by copying which you can get an instance
of a class:

.. code:: python

    In [14]: class IPAddress:
        ...:     def __init__(self, ip):
        ...:         self.ip = ip
        ...:
        ...:     def __str__(self):
        ...:         return f"IPAddress: {self.ip}"
        ...:
        ...:     def __repr__(self):
        ...:         return f"IPAddress('{self.ip}')"
        ...:

    In [15]: ip1 = IPAddress('10.1.1.1')

    In [16]: ip2 = IPAddress('10.2.2.2')

    In [17]: ip_addresses = [ip1, ip2]

    In [18]: ip_addresses
    Out[18]: [IPAddress('10.1.1.1'), IPAddress('10.2.2.2')]

    In [19]: repr(ip1)
    Out[19]: "IPAddress('10.1.1.1')"

