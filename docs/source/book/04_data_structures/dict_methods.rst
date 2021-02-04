Dictionary methods
~~~~~~~~~~~~~~~~~~

``clear``
^^^^^^^^^^^

Method ``clear`` allows to clear dictionary:

.. code:: python

    In [1]: london = {'name': 'London1', 'location': 'London Str'}

    In [2]: london.clear()

    In [3]: london
    Out[3]: {}

``copy``
^^^^^^^^^^

Method ``copy`` allows to create a full copy of dictionary.

If one dictionary is equal to other:

.. code:: python

    In [4]: london = {'name': 'London1', 'location': 'London Str', 'vendor': 'Cisco'}

    In [5]: london2 = london

    In [6]: id(london)
    Out[6]: 25489072

    In [7]: id(london2)
    Out[7]: 25489072

    In [8]: london['vendor'] = 'Juniper'

    In [9]: london2['vendor']
    Out[9]: 'Juniper'

In this case ``london2`` is another name that refers to dictionary ``london``. And when
you change ``london`` dictionary, ``london2`` dictionary changes as well because it's a link to the same object.

Therefore, if you want to make a copy of dictionary, use ``copy`` method:

.. code:: python

    In [10]: london = {'name': 'London1', 'location': 'London Str', 'vendor': 'Cisco'}

    In [11]: london2 = london.copy()

    In [12]: id(london)
    Out[12]: 25524512

    In [13]: id(london2)
    Out[13]: 25563296

    In [14]: london['vendor'] = 'Juniper'

    In [15]: london2['vendor']
    Out[15]: 'Cisco'

``get``
^^^^^^^^^

If you query a key that is not present in dictionary, an error occurs:

.. code:: python

    In [16]: london = {'name': 'London1', 'location': 'London Str', 'vendor': 'Cisco'}

    In [17]: london['ios']
    ---------------------------------------------------------------------------
    KeyError                                  Traceback (most recent call last)
    <ipython-input-17-b4fae8480b21> in <module>()
    ----> 1 london['ios']

    KeyError: 'ios'

Method ``get`` queries for key and if there is no key, returns ``None`` instead.

.. code:: python

    In [18]: london = {'name': 'London1', 'location': 'London Str', 'vendor': 'Cisco'}

    In [19]: print(london.get('ios'))
    None

Method get() also allows you to specify another value instead of ``None``:

.. code:: python

    In [20]: print(london.get('ios', 'Ooops'))
    Ooops

``setdefault``
^^^^^^^^^^^^^^^^

Method ``setdefault`` searches for key and if there is no key, instead of error it creates a key with ``None`` value.

.. code:: python

    In [21]: london = {'name': 'London1', 'location': 'London Str', 'vendor': 'Cisco'}

    In [22]: ios = london.setdefault('ios')

    In [23]: print(ios)
    None

    In [24]: london
    Out[24]: {'name': 'London1', 'location': 'London Str', 'vendor': 'Cisco', 'ios': None}

If key is present, ``setdefault`` returns value that corresponds to it:

.. code:: python

    In [25]: london.setdefault('name')
    Out[25]: 'London1'

The second argument allows to specify which value should correspond to key:

.. code:: python

    In [26]: model = london.setdefault('model', 'Cisco3580')

    In [27]: print(model)
    Cisco3580

    In [28]: london
    Out[28]:
    {'name': 'London1',
     'location': 'London Str',
     'vendor': 'Cisco',
     'ios': None,
     'model': 'Cisco3580'}


Method ``setdefault`` replaces this expression:

.. code:: python

    In [30]: if key in london:
        ...:     value = london[key]
        ...: else:
        ...:     london[key] = 'somevalue'
        ...:     value = london[key]
        ...:

``keys, values, items``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Methods ``keys``, ``values``, ``items``:

.. code:: python

    In [24]: london = {'name': 'London1', 'location': 'London Str', 'vendor': 'Cisco'}

    In [25]: london.keys()
    Out[25]: dict_keys(['name', 'location', 'vendor'])

    In [26]: london.values()
    Out[26]: dict_values(['London1', 'London Str', 'Cisco'])

    In [27]: london.items()
    Out[27]: dict_items([('name', 'London1'), ('location', 'London Str'), ('vendor', 'Cisco')])

All three methods return special view objects that contains keys, values, and
key-value pairs of dictionary, respectively.

A very important feature of view is that they change together with dictionary.
And in fact, they just give you a way to look at objects, but they don't make
a copy of them.

Example of uning ``keys``:

.. code:: python

    In [28]: london = {'name': 'London1', 'location': 'London Str', 'vendor': 'Cisco'}

    In [29]: keys = london.keys()

    In [30]: print(keys)
    dict_keys(['name', 'location', 'vendor'])

Now keys variable corresponds to view ``dict_keys``, in which three keys: name, location and vendor.

But if we add another key-value pair to dictionary, keys object will also change:

.. code:: python

    In [31]: london['ip'] = '10.1.1.1'

    In [32]: keys
    Out[32]: dict_keys(['name', 'location', 'vendor', 'ip'])

If you want to get a simple list of keys that will not be changed with dictionary changes, it is enough to convert view to list:

.. code:: python

    In [33]: list_keys = list(london.keys())

    In [34]: list_keys
    Out[34]: ['name', 'location', 'vendor', 'ip']

``del``
^^^^^^^

Remove key and value:

.. code:: python

    In [35]: london = {'name': 'London1', 'location': 'London Str', 'vendor': 'Cisco'}

    In [36]: del london['name']

    In [37]: london
    Out[37]: {'location': 'London Str', 'vendor': 'Cisco'}

``update``
^^^^^^^^^^

Method ``update`` allows you to add contents of one dictionary to another dictionary:

.. code:: python

    In [38]: r1 = {'name': 'London1', 'location': 'London Str'}

    In [39]: r1.update({'vendor': 'Cisco', 'ios':'15.2'})

    In [40]: r1
    Out[40]: {'name': 'London1', 'location': 'London Str', 'vendor': 'Cisco', 'ios': '15.2'}

Values can be updated in the same way:

.. code:: python

    In [41]: r1.update({'name': 'london-r1', 'ios':'15.4'})

    In [42]: r1
    Out[42]:
    {'name': 'london-r1',
     'location': 'London Str',
     'vendor': 'Cisco',
     'ios': '15.4'}

