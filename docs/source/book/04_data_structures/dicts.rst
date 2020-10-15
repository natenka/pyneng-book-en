Dictionary
====================

Dictionaries are mutable ordered data type:

* data in dictionary are pairs ``key: value``
* values are accessible by key, not by number as in lists
* entries in dictionary stored in order they were added
* since dictionaries are mutable, dictionary items can be changed, added, removed
* key must be an immutable object: number, string, tuple
* value can be data of any type

.. note::

    In other programming languages a similar dictionary can be called an associative array, hash, or hash table.

Example of dictionary:

.. code:: python

    london = {'name': 'London1', 'location': 'London Str', 'vendor': 'Cisco'}

You can write it down like this:

.. code:: python

    london = {
        'id': 1,
        'name':'London',
        'it_vlan':320,
        'user_vlan':1010,
        'mngmt_vlan':99,
        'to_name': None,
        'to_id': None,
        'port':'G1/0/11'
    }

In order to get a value from dictionary you have to refer to key in the same way as in lists, only key will be used instead of number:

.. code:: python

    In [1]: london = {'name': 'London1', 'location': 'London Str'}

    In [2]: london['name']
    Out[2]: 'London1'

    In [3]: london['location']
    Out[3]: 'London Str'

Similarly, a new key-value pair could be added:

.. code:: python

    In [4]: london['vendor'] = 'Cisco'

    In [5]: print(london)
    {'vendor': 'Cisco', 'name': 'London1', 'location': 'London Str'}

Or rewritten:

.. code:: python

    In [6]: london['vendor'] = 'cisco ios'

    In [7]: print(london)
    {'vendor': 'cisco ios', 'name': 'London1', 'location': 'London Str'}

In dictionary you can use a dictionary as a value:

.. code:: python

    london_co = {
        'r1': {
            'hostname': 'london_r1',
            'location': '21 New Globe Walk',
            'vendor': 'Cisco',
            'model': '4451',
            'ios': '15.4',
            'ip': '10.255.0.1'
        },
        'r2': {
            'hostname': 'london_r2',
            'location': '21 New Globe Walk',
            'vendor': 'Cisco',
            'model': '4451',
            'ios': '15.4',
            'ip': '10.255.0.2'
        },
        'sw1': {
            'hostname': 'london_sw1',
            'location': '21 New Globe Walk',
            'vendor': 'Cisco',
            'model': '3850',
            'ios': '3.6.XE',
            'ip': '10.255.0.101'
        }
    }

You can get values from nested dictionary by:

.. code:: python

    In [7]: london_co['r1']['ios']
    Out[7]: '15.4'

    In [8]: london_co['r1']['model']
    Out[8]: '4451'

    In [9]: london_co['sw1']['ip']
    Out[9]: '10.255.0.101'

Function sorted() sorts dictionary keys in ascending order and returns a new list with sorted keys:

.. code:: python

    In [1]: london = {'name': 'London1', 'location': 'London Str', 'vendor': 'Cisco'}

    In [2]: sorted(london)
    Out[2]: ['location', 'name', 'vendor']


.. toctree::
   :maxdepth: 1

   dict_methods
   create_dict
