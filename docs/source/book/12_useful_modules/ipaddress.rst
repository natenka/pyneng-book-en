ipaddress
----------------

Module ``ipaddress`` simplifies work with IP addresses.

.. note::
    Since Python 3.3, ``ipaddress`` module is part of standard Python library.

``ipaddress.ip_address``
~~~~~~~~~~~~~~~~~~~~~~~~~~

Function ``ipaddress.ip_address`` allows to create an IPv4Address or IPv6Address respectively:

.. code:: python

    In [1]: import ipaddress

    In [2]: ipv4 = ipaddress.ip_address('10.0.1.1')

    In [3]: ipv4
    Out[3]: IPv4Address('10.0.1.1')

    In [4]: print(ipv4)
    10.0.1.1

Object has several methods and attributes:

.. code:: python

    In [5]: ipv4.
    ipv4.compressed       ipv4.is_loopback      ipv4.is_unspecified   ipv4.version
    ipv4.exploded         ipv4.is_multicast     ipv4.max_prefixlen
    ipv4.is_global        ipv4.is_private       ipv4.packed
    ipv4.is_link_local    ipv4.is_reserved      ipv4.reverse_pointer

With ``is_`` attributes you can check to what range the address belongs to:

.. code:: python

    In [6]: ipv4.is_loopback
    Out[6]: False

    In [7]: ipv4.is_multicast
    Out[7]: False

    In [8]: ipv4.is_reserved
    Out[8]: False

    In [9]: ipv4.is_private
    Out[9]: True

Different operations can be performed with received objects:

.. code:: python

    In [10]: ip1 = ipaddress.ip_address('10.0.1.1')

    In [11]: ip2 = ipaddress.ip_address('10.0.2.1')

    In [12]: ip1 > ip2
    Out[12]: False

    In [13]: ip2 > ip1
    Out[13]: True

    In [14]: ip1 == ip2
    Out[14]: False

    In [15]: ip1 != ip2
    Out[15]: True

    In [16]: str(ip1)
    Out[16]: '10.0.1.1'

    In [17]: int(ip1)
    Out[17]: 167772417

    In [18]: ip1 + 5
    Out[18]: IPv4Address('10.0.1.6')

    In [19]: ip1 - 5
    Out[19]: IPv4Address('10.0.0.252')

``ipaddress.ip_network``
~~~~~~~~~~~~~~~~~~~~~~~~~~

``ipaddress.ip_network`` function allows you to create an object that describes
the network (IPv4 or IPv6):

.. code:: python

    In [20]: subnet1 = ipaddress.ip_network('80.0.1.0/28')

As with an address, a network has various attributes and methods:

.. code:: python

    In [21]: subnet1.broadcast_address
    Out[21]: IPv4Address('80.0.1.15')

    In [22]: subnet1.with_netmask
    Out[22]: '80.0.1.0/255.255.255.240'

    In [23]: subnet1.with_hostmask
    Out[23]: '80.0.1.0/0.0.0.15'

    In [24]: subnet1.prefixlen
    Out[24]: 28

    In [25]: subnet1.num_addresses
    Out[25]: 16

Method ``hosts`` returns generator, so to view all hosts you should apply list() function:

.. code:: python

    In [26]: list(subnet1.hosts())
    Out[26]:
    [IPv4Address('80.0.1.1'),
     IPv4Address('80.0.1.2'),
     IPv4Address('80.0.1.3'),
     IPv4Address('80.0.1.4'),
     IPv4Address('80.0.1.5'),
     IPv4Address('80.0.1.6'),
     IPv4Address('80.0.1.7'),
     IPv4Address('80.0.1.8'),
     IPv4Address('80.0.1.9'),
     IPv4Address('80.0.1.10'),
     IPv4Address('80.0.1.11'),
     IPv4Address('80.0.1.12'),
     IPv4Address('80.0.1.13'),
     IPv4Address('80.0.1.14')]

Method ``subnets`` allows dividing network (subnetting). By default, it splits network into two subnets:

.. code:: python

    In [27]: list(subnet1.subnets())
    Out[27]: [IPv4Network('80.0.1.0/29'), IPv4Network(u'80.0.1.8/29')]

**Prefixlen_diff** parameter allows you to specify the number of bits for subnets:

.. code:: python

    In [28]: list(subnet1.subnets(prefixlen_diff=2))
    Out[28]:
    [IPv4Network('80.0.1.0/30'),
     IPv4Network('80.0.1.4/30'),
     IPv4Network('80.0.1.8/30'),
     IPv4Network('80.0.1.12/30')]

With ``new_prefix`` parameter you can specify which mask should be configured:

.. code:: python

    In [29]: list(subnet1.subnets(new_prefix=30))
    Out[29]:
    [IPv4Network('80.0.1.0/30'),
     IPv4Network('80.0.1.4/30'),
     IPv4Network('80.0.1.8/30'),
     IPv4Network('80.0.1.12/30')]

    In [30]: list(subnet1.subnets(new_prefix=29))
    Out[30]: [IPv4Network('80.0.1.0/29'), IPv4Network(u'80.0.1.8/29')]

IP addresses of network can be iterated in a loop:

.. code:: python

    In [31]: for ip in subnet1:
       ....:     print(ip)
       ....:
    80.0.1.0
    80.0.1.1
    80.0.1.2
    80.0.1.3
    80.0.1.4
    80.0.1.5
    80.0.1.6
    80.0.1.7
    80.0.1.8
    80.0.1.9
    80.0.1.10
    80.0.1.11
    80.0.1.12
    80.0.1.13
    80.0.1.14
    80.0.1.15

And it is possible to get a specific address:

.. code:: python

    In [32]: subnet1[0]
    Out[32]: IPv4Address('80.0.1.0')

    In [33]: subnet1[5]
    Out[33]: IPv4Address('80.0.1.5')

This way you can check if IP address is in the network:

.. code:: python

    In [34]: ip1 = ipaddress.ip_address('80.0.1.3')

    In [35]: ip1 in subnet1
    Out[35]: True

``ipaddress.ip_interface``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The ``ipaddress.ip_interface`` function allows you to create an IPv4Interface or
IPv6Interface object, respectively:

.. code:: python

    In [36]: int1 = ipaddress.ip_interface('10.0.1.1/24')

Using methods of IPv4Interface object you can get an address, mask or interface network:

.. code:: python

    In [37]: int1.ip
    Out[37]: IPv4Address('10.0.1.1')

    In [38]: int1.network
    Out[38]: IPv4Network('10.0.1.0/24')

    In [39]: int1.netmask
    Out[39]: IPv4Address('255.255.255.0')

Example of module usage
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Since module has built-in address correctness checks, you can use them,
for example, to check whether an address is a network or host address:

.. code:: python

    In [40]: IP1 = '10.0.1.1/24'

    In [41]: IP2 = '10.0.1.0/24'

    In [42]: def check_if_ip_is_network(ip_address):
       ....:     try:
       ....:         ipaddress.ip_network(ip_address)
       ....:         return True
       ....:     except ValueError:
       ....:         return False
       ....:

    In [43]: check_if_ip_is_network(IP1)
    Out[43]: False

    In [44]: check_if_ip_is_network(IP2)
    Out[44]: True

