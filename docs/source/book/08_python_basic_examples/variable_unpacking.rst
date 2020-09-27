Variable unpacking
---------------------

The unpacking of variables is a special syntax that allows to assign elements of an iterated object to variables.

.. note::
    This functionality is often referred to as tuple unpacking but the unpacking works on any iterable object, not only with tuples

Example of variable unpacking:

.. code:: python

    In [1]: interface = ['FastEthernet0/1', '10.1.1.1', 'up', 'up']

    In [2]: intf, ip, status, protocol = interface

    In [3]: intf
    Out[3]: 'FastEthernet0/1'

    In [4]: ip
    Out[4]: '10.1.1.1'

This option is much more convenient than the use of indexes:

.. code:: python

    In [5]: intf, ip, status, protocol = interface[0], interface[1], interface[2], interface[3]

When you unpack variables, each item in the list falls into the corresponding variable. It is important to keep in mind that the variables on the left should be exactly as many elements in the list.

If amount of variables are less or more, there will be an exception:

.. code:: python

    In [6]: intf, ip, status = interface
    ------------------------------------------------------------
    ValueError                 Traceback (most recent call last)
    <ipython-input-11-a304c4372b1a> in <module>()
    ----> 1 intf, ip, status = interface

    ValueError: too many values to unpack (expected 3)

    In [7]: intf, ip, status, protocol, other = interface
    ------------------------------------------------------------
    ValueError                 Traceback (most recent call last)
    <ipython-input-12-ac93e78b978c> in <module>()
    ----> 1 intf, ip, status, protocol, other = interface

    ValueError: not enough values to unpack (expected 5, got 4)

Replacement of unnecessary elements ``_``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Often only some of the elements of an iterated object are needed. The unpacking syntax requires that exactly as many variables as the elements in the object being iterated be specified.

If, for example, only VLAN, MAC and interface should be obtained from line, you still need to specify a variable for record type:

.. code:: python

    In [8]: line = '100    01bb.c580.7000    DYNAMIC     Gi0/1'

    In [9]: vlan, mac, item_type, intf = line.split()

    In [10]: vlan
    Out[10]: '100'

    In [11]: intf
    Out[11]: 'Gi0/1'

If record type is no longer needed, you can replace the item_type variable with underline character:

.. code:: python

    In [12]: vlan, mac, _, intf = line.split()

This clearly indicates that this element is not needed.

The underline character can be used more than once:

.. code:: python

    In [13]: dhcp = '00:09:BB:3D:D6:58   10.1.10.2        86250       dhcp-snooping   10    FastEthernet0/1'

    In [14]: mac, ip, _, _, vlan, intf = dhcp.split()

    In [15]: mac
    Out[15]: '00:09:BB:3D:D6:58'

    In [16]: vlan
    Out[16]: '10'

Use ``*``
~~~~~~~~~~~~~~~~~~~

The unpacking  of variables supports a special syntax that allows unpacking  of several elements into one. If you put ``*`` in front of the variable name all elements except those that are explicitly assigned will be written into it.

For example, you can get the first element in the *first* variable and the rest in the *rest*:

.. code:: python

    In [18]: vlans = [10, 11, 13, 30]

    In [19]: first, *rest = vlans

    In [20]: first
    Out[20]: 10

    In [21]: rest
    Out[21]: [11, 13, 30]

The variable with an asterisk will always contain a list:

.. code:: python

    In [22]: vlans = (10, 11, 13, 30)

    In [22]: first, *rest = vlans

    In [23]: first
    Out[23]: 10

    In [24]: rest
    Out[24]: [11, 13, 30]

If there is only one item, unpacking will still work:

.. code:: python

    In [25]: first, *rest = vlans

    In [26]: first
    Out[26]: 55

    In [27]: rest
    Out[27]: []

There could be only one variable with an asterisk in terms of unpacking.

.. code:: python

    In [28]: vlans = (10, 11, 13, 30)

    In [29]: first, *rest, *others = vlans
      File "<ipython-input-37-dedf7a08933a>", line 1
        first, *rest, *others = vlans
                                     ^
    SyntaxError: two starred expressions in assignment

This variable may not only be at the end of the expression:

.. code:: python

    In [30]: vlans = (10, 11, 13, 30)

    In [31]: *rest, last = vlans

    In [32]: rest
    Out[32]: [10, 11, 13]

    In [33]: last
    Out[33]: 30

Thus, the first, second and last element can be specified:

.. code:: python

    In [34]: cdp = 'SW1     Eth 0/0    140   S I   WS-C3750-  Eth 0/1'

    In [35]: name, l_intf, *other, r_intf = cdp.split()

    In [36]: name
    Out[36]: 'SW1'

    In [37]: l_intf
    Out[37]: 'Eth'

    In [38]: r_intf
    Out[38]: '0/1'

Unpacking examples
~~~~~~~~~~~~~~~~~~

Unpacking of iterable objects
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

These examples show that you can unpack not only lists, tuples and strings but also any other iterable objects.

Unpacking the range:

.. code:: python

    In [39]: first, *rest = range(1,6)

    In [40]: first
    Out[40]: 1

    In [41]: rest
    Out[41]: [2, 3, 4, 5]

Unpacking zip:

.. code:: python

    In [42]: a = [1,2,3,4,5]

    In [43]: b = [100,200,300,400,500]

    In [44]: zip(a, b)
    Out[44]: <zip at 0xb4df4fac>

    In [45]: list(zip(a, b))
    Out[45]: [(1, 100), (2, 200), (3, 300), (4, 400), (5, 500)]

    In [46]: first, *rest, last = zip(a, b)

    In [47]: first
    Out[47]: (1, 100)

    In [48]: rest
    Out[48]: [(2, 200), (3, 300), (4, 400)]

    In [49]: last
    Out[49]: (5, 500)

Example of unpacking in *for* loop
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

An example of a loop that runs through the keys:

.. code:: python

    In [50]: access_template = ['switchport mode access',
        ...:                    'switchport access vlan',
        ...:                    'spanning-tree portfast',
        ...:                    'spanning-tree bpduguard enable']
        ...:

    In [51]: access = {'0/12':10,
        ...:           '0/14':11,
        ...:           '0/16':17}
        ...:

    In [52]: for intf in access:
        ...:     print('interface FastEthernet' + intf)
        ...:     for command in access_template:
        ...:         if command.endswith('access vlan'):
        ...:             print(' {} {}'.format(command, access[intf]))
        ...:         else:
        ...:             print(' {}'.format(command))
        ...:
    interface FastEthernet0/12
     switchport mode access
     switchport access vlan 10
     spanning-tree portfast
     spanning-tree bpduguard enable
    interface FastEthernet0/14
     switchport mode access
     switchport access vlan 11
     spanning-tree portfast
     spanning-tree bpduguard enable
    interface FastEthernet0/16
     switchport mode access
     switchport access vlan 17
     spanning-tree portfast
     spanning-tree bpduguard enable

Instead, you can run through key-value pairs and immediately unpack them into different variables:

.. code:: python

    In [53]: for intf, vlan in access.items():
        ...:     print('interface FastEthernet' + intf)
        ...:     for command in access_template:
        ...:         if command.endswith('access vlan'):
        ...:             print(' {} {}'.format(command, vlan))
        ...:         else:
        ...:             print(' {}'.format(command))
        ...:

Example of unpacking list items in the loop:

.. code:: python

    In [54]: table
    Out[54]:
    [['100', 'a1b2.ac10.7000', 'DYNAMIC', 'Gi0/1'],
     ['200', 'a0d4.cb20.7000', 'DYNAMIC', 'Gi0/2'],
     ['300', 'acb4.cd30.7000', 'DYNAMIC', 'Gi0/3'],
     ['100', 'a2bb.ec40.7000', 'DYNAMIC', 'Gi0/4'],
     ['500', 'aa4b.c550.7000', 'DYNAMIC', 'Gi0/5'],
     ['200', 'a1bb.1c60.7000', 'DYNAMIC', 'Gi0/6'],
     ['300', 'aa0b.cc70.7000', 'DYNAMIC', 'Gi0/7']]


    In [55]: for line in table:
        ...:     vlan, mac, _, intf = line
        ...:     print(vlan, mac, intf)
        ...:
    100 a1b2.ac10.7000 Gi0/1
    200 a0d4.cb20.7000 Gi0/2
    300 acb4.cd30.7000 Gi0/3
    100 a2bb.ec40.7000 Gi0/4
    500 aa4b.c550.7000 Gi0/5
    200 a1bb.1c60.7000 Gi0/6
    300 aa0b.cc70.7000 Gi0/7

But it’s better to do this:

.. code:: python

    In [56]: for vlan, mac, _, intf in table:
        ...:     print(vlan, mac, intf)
        ...:
    100 a1b2.ac10.7000 Gi0/1
    200 a0d4.cb20.7000 Gi0/2
    300 acb4.cd30.7000 Gi0/3
    100 a2bb.ec40.7000 Gi0/4
    500 aa4b.c550.7000 Gi0/5
    200 a1bb.1c60.7000 Gi0/6
    300 aa0b.cc70.7000 Gi0/7

