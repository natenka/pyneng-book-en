Dictionary creation options
-------------------------

Literal
~~~~~~~

A dictionary can be created with help of a literal:

.. code:: python

    In [1]: r1 = {'model': '4451', 'ios': '15.4'}

dict
~~~~

Construction ``dict`` allows you to create a dictionary in several ways.

If you use strings as keys you can use this option to create a dictionary:

.. code:: python

    In [2]: r1 = dict(model='4451', ios='15.4')

    In [3]: r1
    Out[3]: {'model': '4451', 'ios': '15.4'}

The second option of creating a dictionary with dict():

.. code:: python

    In [4]: r1 = dict([('model','4451'), ('ios','15.4')])

    In [5]: r1
    Out[5]: {'model': '4451', 'ios': '15.4'}

dict.fromkeys
~~~~~~~~~~~~~

In a situation where you need to create a dictionary with known keys but so far
empty values (or identical values), ``fromkeys`` method is very convenient:

.. code:: python

    In [5]: d_keys = ['hostname', 'location', 'vendor', 'model', 'ios', 'ip']

    In [6]: r1 = dict.fromkeys(d_keys)

    In [7]: r1
    Out[7]:
    {'hostname': None,
     'location': None,
     'vendor': None,
     'model': None,
     'ios': None,
     'ip': None}


By default ``fromkeys`` sets ``None`` value. But you can also pass your own value:

.. code:: python

    In [8]: router_models = ['ISR2811', 'ISR2911', 'ISR2921', 'ASR9002']

    In [9]: models_count = dict.fromkeys(router_models, 0)

    In [10]: models_count
    Out[10]: {'ISR2811': 0, 'ISR2911': 0, 'ISR2921': 0, 'ASR9002': 0}


This option of creating a dictionary is not suitable for all cases. For example,
if you use a mutable data type in value, a reference to the same object will be created:

.. code:: python

    In [10]: router_models = ['ISR2811', 'ISR2911', 'ISR2921', 'ASR9002']

    In [11]: routers = dict.fromkeys(router_models, [])
        ...:

    In [12]: routers
    Out[12]: {'ISR2811': [], 'ISR2911': [], 'ISR2921': [], 'ASR9002': []}

    In [13]: routers['ASR9002'].append('london_r1')

    In [14]: routers
    Out[14]:
    {'ISR2811': ['london_r1'],
     'ISR2911': ['london_r1'],
     'ISR2921': ['london_r1'],
     'ASR9002': ['london_r1']}

In this case, each key refers to the same list. Therefore,
when a value is added to one of lists, others are updated.

.. note::
    A dictionary comprehension is better for this task. See section :ref:`x_comprehensions`
