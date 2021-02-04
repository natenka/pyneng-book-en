Sequence protocol
~~~~~~~~~~~~~~~~~~~~~~~~~~~

In the most basic version, sequence protocol (sequence) includes two methods:
``__len__`` and ``__getitem__``. In more complete version also methods:
``__contains__``, ``__iter__``, ``__reversed__``, ``index`` and ``count``.
If sequence is mutable, several other methods are added.

Add ``__len__`` and ``__getitem__`` methods to Network class:

.. code:: python

    In [1]: class Network:
       ...:     def __init__(self, network):
       ...:         self.network = network
       ...:         subnet = ipaddress.ip_network(self.network)
       ...:         self.addresses = [str(ip) for ip in subnet.hosts()]
       ...:
       ...:     def __iter__(self):
       ...:         return iter(self.addresses)
       ...:
       ...:     def __len__(self):
       ...:         return len(self.addresses)
       ...:
       ...:     def __getitem__(self, index):
       ...:         return self.addresses[index]
       ...:


Method ``__len__`` is called by ``len`` function:

.. code:: python

    In [2]: net1 = Network('10.1.1.192/30')

    In [3]: len(net1)
    Out[3]: 2


And ``__getitem__`` method is called when you acess item by index:

.. code:: python

    In [4]: net1[0]
    Out[4]: '10.1.1.193'

    In [5]: net1[1]
    Out[5]: '10.1.1.194'

    In [6]: net1[-1]
    Out[6]: '10.1.1.194'

``__getitem__`` method is responsible not only for access by index, but also for slices:

.. code:: python

    In [7]: net1 = Network('10.1.1.192/28')

    In [8]: net1[0]
    Out[8]: '10.1.1.193'

    In [9]: net1[3:7]
    Out[9]: ['10.1.1.196', '10.1.1.197', '10.1.1.198', '10.1.1.199']

    In [10]: net1[3:]
    Out[10]:
    ['10.1.1.196',
     '10.1.1.197',
     '10.1.1.198',
     '10.1.1.199',
     '10.1.1.200',
     '10.1.1.201',
     '10.1.1.202',
     '10.1.1.203',
     '10.1.1.204',
     '10.1.1.205',
     '10.1.1.206']

In this case, because ``__getitem__`` method uses a ``list``, errors are
processed correctly automatically:

.. code:: python

    In [11]: net1[100]
    ---------------------------------------------------------------------------
    IndexError                                Traceback (most recent call last)
    <ipython-input-11-09ca84e34cb6> in <module>
    ----> 1 net1[100]

    <ipython-input-2-bc213b4a03ca> in __getitem__(self, index)
         12
         13     def __getitem__(self, index):
    ---> 14         return self.addresses[index]
         15

    IndexError: list index out of range

    In [12]: net1['a']
    ---------------------------------------------------------------------------
    TypeError                                 Traceback (most recent call last)
    <ipython-input-12-facd90673864> in <module>
    ----> 1 net1['a']

    <ipython-input-2-bc213b4a03ca> in __getitem__(self, index)
         12
         13     def __getitem__(self, index):
    ---> 14         return self.addresses[index]
         15

    TypeError: list indices must be integers or slices, not str


You will find implementation of remaining methods of sequence protocol
in tasks to this section:

* ``__contains__`` - this method is responsible for checking the presence
  of element in sequence ``'10.1.1.198' in net1``. If object does not define
  this method, the presence of element is checked by iteration of elements
  using ``__iter__`` and if this method is also unavailable, then by index
  iteration with ``__getitem__``.
* ``__reversed__`` - is used by built-in ``reversed`` function. This method
  is usually best not to create and rely on the fact that ``reversed``
  function in absence of ``__reversed__`` method will use methods ``__len__`` and ``__getitem__``.
* ``index`` - returns index of element. Works exactly the same as ``index`` method in lists and tuples.
* ``count`` - returns number of values. Works exactly the same as ``count`` method in lists and tuples.

