zip
-----------

``zip`` function:

-  sequences are passed to function
-  zip returns an iterator with tuples in which n-tuple consists of n-elements of sequences that have been passed as arguments
-  for example, 10th tuple will contain 10th element of each of passed sequences
-  if sequences with different lengths have been passed to input, they will all be cut by the shortest sequence
-  the order of elements is respected

.. note::
    Since ``zip`` is an iterator, ``list`` is used to display its contents

Example of using zip:

.. code:: python

    In [1]: a = [1,2,3]

    In [2]: b = [100,200,300]

    In [3]: list(zip(a,b))
    Out[3]: [(1, 100), (2, 200), (3, 300)]

Use zip with lists of different lengths:

.. code:: python

    In [4]: a = [1,2,3,4,5]

    In [5]: b = [10,20,30,40,50]

    In [6]: c = [100,200,300]

    In [7]: list(zip(a,b,c))
    Out[7]: [(1, 10, 100), (2, 20, 200), (3, 30, 300)]

Using zip to create a dictionary
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Example of using zip  to create a dictionary:

.. code:: python

    In [4]: d_keys = ['hostname', 'location', 'vendor', 'model', 'IOS', 'IP']
    In [5]: d_values = ['london_r1', '21 New Globe Walk', 'Cisco', '4451', '15.4', '10.255.0.1']

    In [6]: list(zip(d_keys,d_values))
    Out[6]: 
    [('hostname', 'london_r1'),
     ('location', '21 New Globe Walk'),
     ('vendor', 'Cisco'),
     ('model', '4451'),
     ('IOS', '15.4'),
     ('IP', '10.255.0.1')]

    In [7]: dict(zip(d_keys,d_values))
    Out[7]: 
    {'IOS': '15.4',
     'IP': '10.255.0.1',
     'hostname': 'london_r1',
     'location': '21 New Globe Walk',
     'model': '4451',
     'vendor': 'Cisco'}
    In [8]: r1 = dict(zip(d_keys,d_values))

    In [9]: r1
    Out[9]: 
    {'IOS': '15.4',
     'IP': '10.255.0.1',
     'hostname': 'london_r1',
     'location': '21 New Globe Walk',
     'model': '4451',
     'vendor': 'Cisco'}

In example below there is a separate list which stores keys and a dictionary which stores  information about each device in form of list (to preserve order).

Collect them in dictionary with keys from list and information from dictionary *data*:

.. code:: python

    In [10]: d_keys = ['hostname', 'location', 'vendor', 'model', 'IOS', 'IP']

    In [11]: data = {
       ....: 'r1': ['london_r1', '21 New Globe Walk', 'Cisco', '4451', '15.4', '10.255.0.1'],
       ....: 'r2': ['london_r2', '21 New Globe Walk', 'Cisco', '4451', '15.4', '10.255.0.2'],
       ....: 'sw1': ['london_sw1', '21 New Globe Walk', 'Cisco', '3850', '3.6.XE', '10.255.0.101']
       ....: }

    In [12]: london_co = {}

    In [13]: for k in data.keys():
       ....:     london_co[k] = dict(zip(d_keys,data[k]))
       ....:     

    In [14]: london_co
    Out[14]: 
    {'r1': {'IOS': '15.4',
      'IP': '10.255.0.1',
      'hostname': 'london_r1',
      'location': '21 New Globe Walk',
      'model': '4451',
      'vendor': 'Cisco'},
     'r2': {'IOS': '15.4',
      'IP': '10.255.0.2',
      'hostname': 'london_r2',
      'location': '21 New Globe Walk',
      'model': '4451',
      'vendor': 'Cisco'},
     'sw1': {'IOS': '3.6.XE',
      'IP': '10.255.0.101',
      'hostname': 'london_sw1',
      'location': '21 New Globe Walk',
      'model': '3850',
      'vendor': 'Cisco'}}

