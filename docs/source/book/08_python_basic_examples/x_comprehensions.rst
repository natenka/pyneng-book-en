
.. _x_comprehensions:

List, dict, set comprehensions
==============================

Python supports special expressions that allow for compact creation
of lists, dictionaries, and sets:

-  list comprehensions
-  dict comprehensions
-  set comprehensions

These expressions not only enable more compact objects to be created but also
create them faster. Although they require a certain habit of use and understanding at first, they are very often used.

List comprehensions
-------------------

List comprehension is an expression like:

.. code:: python

    In [1]: vlans = [f'vlan {num}' for num in range(10, 16)]

    In [2]: print(vlans)
    ['vlan 10', 'vlan 11', 'vlan 12', 'vlan 13', 'vlan 14', 'vlan 15']

In general, it is an expression that converts an iterable object into a list. That is, a sequence of elements is converted and added to a new list.

Expression above is similar to this loop:

.. code:: python

    In [3]: vlans = []

    In [4]: for num in range(10, 16):
       ...:     vlans.append(f'vlan {num}')
       ...:

    In [5]: print(vlans)
    ['vlan 10', 'vlan 11', 'vlan 12', 'vlan 13', 'vlan 14', 'vlan 15']

In list comprehensions you can use **if**. Thus, you can only add some objects to the list.
For example, a loop selects only those elements that are digits, converts them and adds them to the resulting list only_digits:

.. code:: python

    In [6]: items = ['10', '20', 'a', '30', 'b', '40']

    In [7]: only_digits = []

    In [8]: for item in items:
       ...:     if item.isdigit():
       ...:         only_digits.append(int(item))
       ...:

    In [9]: print(only_digits)
    [10, 20, 30, 40]

A similar version with list comprehensions:

.. code:: python

    In [10]: items = ['10', '20', 'a', '30', 'b', '40']

    In [11]: only_digits = [int(item) for item in items if item.isdigit()]

    In [12]: print(only_digits)
    [10, 20, 30, 40]

Of course, not all loops can be rewritten as a list comprehension but when
it is possible to do so without making the expression more complex, it is better to use list comprehension.

.. note::

    In Python, list comprehensions can also replace filter and map functions
    and are considered a clearer option.

With list comprehension it is also convenient to get elements from nested dictionaries:

.. code:: python

    In [13]: london_co = {
        ...:     'r1' : {
        ...:     'hostname': 'london_r1',
        ...:     'location': '21 New Globe Walk',
        ...:     'vendor': 'Cisco',
        ...:     'model': '4451',
        ...:     'IOS': '15.4',
        ...:     'IP': '10.255.0.1'
        ...:     },
        ...:     'r2' : {
        ...:     'hostname': 'london_r2',
        ...:     'location': '21 New Globe Walk',
        ...:     'vendor': 'Cisco',
        ...:     'model': '4451',
        ...:     'IOS': '15.4',
        ...:     'IP': '10.255.0.2'
        ...:     },
        ...:     'sw1' : {
        ...:     'hostname': 'london_sw1',
        ...:     'location': '21 New Globe Walk',
        ...:     'vendor': 'Cisco',
        ...:     'model': '3850',
        ...:     'IOS': '3.6.XE',
        ...:     'IP': '10.255.0.101'
        ...:     }
        ...: }

    In [14]: [london_co[device]['IOS'] for device in london_co]
    Out[14]: ['15.4', '15.4', '3.6.XE']

    In [15]: [london_co[device]['IP'] for device in london_co]
    Out[15]: ['10.255.0.1', '10.255.0.2', '10.255.0.101']

In fact, syntax of list comprehension looks like:

.. code:: python

    [expression for item1 in iterable1 if condition1 
                for item2 in iterable2 if condition2
                ...
                for itemN in iterableN if conditionN ]

This means you can use several **for** in expression.

For example, *vlans* list contains several nested lists with VLANs:

.. code:: python

    In [16]: vlans = [[10,21,35], [101, 115, 150], [111, 40, 50]]

It's necessary to form only one list with VLAN numbers. The first option is to use **for** loop:

.. code:: python

    In [17]: result = []

    In [18]: for vlan_list in vlans:
        ...:     for vlan in vlan_list:
        ...:         result.append(vlan)
        ...:

    In [19]: print(result)
    [10, 21, 35, 101, 115, 150, 111, 40, 50]

List comprehension:

.. code:: python

    In [20]: vlans = [[10,21,35], [101, 115, 150], [111, 40, 50]]

    In [21]: result = [vlan for vlan_list in vlans for vlan in vlan_list]

    In [22]: print(result)
    [10, 21, 35, 101, 115, 150, 111, 40, 50]

Two sequences can be processed simultaneously using zip():

.. code:: python

    In [23]: vlans = [100, 110, 150, 200]

    In [24]: names = ['mngmt', 'voice', 'video', 'dmz']

    In [25]: result = [f'vlan {vl}\n name {name}' for vl, name in zip(vlans, names)]

    In [26]: print('\n'.join(result))
    vlan 100
     name mngmt
    vlan 110
     name voice
    vlan 150
     name video
    vlan 200
     name dmz

Dict comprehensions
-------------------

Dict comprehensions are similar to list comprehensions but they are
used to create dictionaries. For example, the expression:

.. code:: python

    In [27]: d = {}

    In [28]: for num in range(1, 11):
        ...:     d[num] = num**2
        ...:

    In [29]: print(d)
    {1: 1, 2: 4, 3: 9, 4: 16, 5: 25, 6: 36, 7: 49, 8: 64, 9: 81, 10: 100}

Can be replaced with a dict comprehension:

.. code:: python

    In [30]: d = {num: num**2 for num in range(1, 11)}

    In [31]: print(d)
    {1: 1, 2: 4, 3: 9, 4: 16, 5: 25, 6: 36, 7: 49, 8: 64, 9: 81, 10: 100}

Another example in which you need to change an existing dictionary
and convert all keys to lowercase.
First, a solution without a dict comprehension:

.. code:: python

    In [32]: r1 = {'IOS': '15.4',
        ...:       'IP': '10.255.0.1',
        ...:       'hostname': 'london_r1',
        ...:       'location': '21 New Globe Walk',
        ...:       'model': '4451',
        ...:       'vendor': 'Cisco'}
        ...:

    In [33]: lower_r1 = {}

    In [34]: for key, value in r1.items():
        ...:     lower_r1[key.lower()] = value
        ...:

    In [35]: lower_r1
    Out[35]:
    {'hostname': 'london_r1',
     'ios': '15.4',
     'ip': '10.255.0.1',
     'location': '21 New Globe Walk',
     'model': '4451',
     'vendor': 'Cisco'}

Dict comprehension version:

.. code:: python

    In [36]: r1 = {'IOS': '15.4',
        ...:   'IP': '10.255.0.1',
        ...:   'hostname': 'london_r1',
        ...:   'location': '21 New Globe Walk',
        ...:   'model': '4451',
        ...:   'vendor': 'Cisco'}
        ...:

    In [37]: lower_r1 = {key.lower(): value for key, value in r1.items()}

    In [38]: lower_r1
    Out[38]:
    {'hostname': 'london_r1',
     'ios': '15.4',
     'ip': '10.255.0.1',
     'location': '21 New Globe Walk',
     'model': '4451',
     'vendor': 'Cisco'}

Like list comprehensions, dict comprehensions can be nested. Try to convert keys in nested dictionaries in the same way:

.. code:: python

    In [39]: london_co = {
        ...:     'r1' : {
        ...:     'hostname': 'london_r1',
        ...:     'location': '21 New Globe Walk',
        ...:     'vendor': 'Cisco',
        ...:     'model': '4451',
        ...:     'IOS': '15.4',
        ...:     'IP': '10.255.0.1'
        ...:     },
        ...:     'r2' : {
        ...:     'hostname': 'london_r2',
        ...:     'location': '21 New Globe Walk',
        ...:     'vendor': 'Cisco',
        ...:     'model': '4451',
        ...:     'IOS': '15.4',
        ...:     'IP': '10.255.0.2'
        ...:     },
        ...:     'sw1' : {
        ...:     'hostname': 'london_sw1',
        ...:     'location': '21 New Globe Walk',
        ...:     'vendor': 'Cisco',
        ...:     'model': '3850',
        ...:     'IOS': '3.6.XE',
        ...:     'IP': '10.255.0.101'
        ...:     }
        ...: }

    In [40]: lower_london_co = {}

    In [41]: for device, params in london_co.items():
        ...:     lower_london_co[device] = {}
        ...:     for key, value in params.items():
        ...:         lower_london_co[device][key.lower()] = value
        ...:

    In [42]: lower_london_co
    Out[42]:
    {'r1': {'hostname': 'london_r1',
      'ios': '15.4',
      'ip': '10.255.0.1',
      'location': '21 New Globe Walk',
      'model': '4451',
      'vendor': 'Cisco'},
     'r2': {'hostname': 'london_r2',
      'ios': '15.4',
      'ip': '10.255.0.2',
      'location': '21 New Globe Walk',
      'model': '4451',
      'vendor': 'Cisco'},
     'sw1': {'hostname': 'london_sw1',
      'ios': '3.6.XE',
      'ip': '10.255.0.101',
      'location': '21 New Globe Walk',
      'model': '3850',
      'vendor': 'Cisco'}}

Similar conversion with dict comprehensions:

.. code:: python

    In [43]: result = {device: {key.lower(): value for key, value in params.items()}
             for device, params in london_co.items()}

    In [44]: result
    Out[44]:
    {'r1': {'hostname': 'london_r1',
      'ios': '15.4',
      'ip': '10.255.0.1',
      'location': '21 New Globe Walk',
      'model': '4451',
      'vendor': 'Cisco'},
     'r2': {'hostname': 'london_r2',
      'ios': '15.4',
      'ip': '10.255.0.2',
      'location': '21 New Globe Walk',
      'model': '4451',
      'vendor': 'Cisco'},
     'sw1': {'hostname': 'london_sw1',
      'ios': '3.6.XE',
      'ip': '10.255.0.101',
      'location': '21 New Globe Walk',
      'model': '3850',
      'vendor': 'Cisco'}}

Set comprehensions
------------------

Set comprehensions are generally similar to list comprehensions.
For example, get a set with unique VLAN numbers:

.. code:: python

    In [45]: vlans = [10, '30', 30, 10, '56']

    In [46]: unique_vlans = {int(vlan) for vlan in vlans}

    In [47]: unique_vlans
    Out[47]: {10, 30, 56}

Similar solution without using of set comprehensions:

.. code:: python

    In [48]: vlans = [10, '30', 30, 10, '56']

    In [49]: unique_vlans = set()

    In [50]: for vlan in vlans:
        ...:     unique_vlans.add(int(vlan))
        ...:

    In [51]: unique_vlans
    Out[51]: {10, 30, 56}

