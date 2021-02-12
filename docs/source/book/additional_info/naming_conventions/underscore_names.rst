Underscore in names
----------------------

In Python, underscores at the beginning or at the end of a name indicates
special names. Most often it's just an arrangement but sometimes it
actually affects object behavior.

Underscore in name
~~~~~~~~~~~~~~~~~~~~~

In Python, one underscore is used to simply indicate that data is discarded.

For example, if you want to get MAC address, IP address, VLAN and interface from *line* string and discard the rest of fields, you can use this option:

.. code:: python

    In [1]: line = '00:09:BB:3D:D6:58  10.1.10.2 86250   dhcp-snooping   10  FastEthernet0/1'

    In [2]: mac, ip, _, _, vlan, intf = line.split()

    In [3]: print(mac, ip, vlan, intf)
    00:09:BB:3D:D6:58 10.1.10.2 10 FastEthernet0/1

This record indicates that we do not need the third and fourth elements.

You can do this:

.. code:: python

    In [4]: mac, ip, lease, entry_type, vlan, intf = line.split()

But then it may be unclear why *lease* and *entry\_type* variables are not used any further. It is better to call variable names like *ignored*.

A similar technique can be used when a loop variable is not needed:

.. code:: python

    In [5]: [0 for _ in range(10)]
    Out[5]: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]

Underscore in interpreter
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In the python and ipython interpreter undesrcore is used to get result of the last experision.

.. code:: python

    In [6]: [0 for _ in range(10)]
    Out[6]: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]

    In [7]: _
    Out[7]: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]

    In [8]: a = _

    In [9]: a
    Out[9]: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]

Single underscore
^^^^^^^^^^^^^^^^^^

One underscore before name
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

One underscore before name indicates that the name is used as an internal name.

For example, if one underscore is specified in name of function or method, this means that the object is an internal implementation and should not be used directly.

But also, when importing ``from module import *`` the objects that start with underscore will not be imported.

For instanse, example.py file contains these variables and functions:

.. code:: python

    db_name = 'dhcp_snooping.db'
    _path = '/home/nata/pyneng/'


    def func1(arg):
        print arg


    def _func2(arg):
        print arg

If you import all objects from module, those that start with underscore will not be imported:

.. code:: python

    In [7]: from example import *

    In [8]: db_name
    Out[8]: 'dhcp_snooping.db'

    In [9]: _path
    ...
    NameError: name '_path' is not defined

    In [10]: func1(1)
    1

    In [11]: _func2(1)
    ...
    NameError: name '_func2' is not defined

One underscore after name
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

One underscore after name is used when the name of object or parameter overlaps with the embedded names.

Example:

.. code:: python

    In [12]: line = '00:09:BB:3D:D6:58  10.1.10.2 86250   dhcp-snooping   10  FastEthernet0/1'

    In [13]: mac, ip, lease, type_, vlan, intf = line.split()

Two underscores
~~~~~~~~~~~~~~~~~

Two underscores before name
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Two underscores before method name are not used simply as an agreement. Such
names are transformed into format "class name + method name". This allows
the creation of unique methods and attributes of classes.

.. note::

    This transformation is only performed if less than two underscore endings or no underscores.

.. code:: python

    In [14]: class Switch(object):
        ...:     __quantity = 0
        ...:     def __configure(self):
        ...:         pass
        ...:

    In [15]: dir(Switch)
    Out[15]:
    ['_Switch__configure', '_Switch__quantity', ...]

Although methods were created without ``_Switch``, it was added.

If you create a subclass, then ``__configure`` method will not rewrite method of parent Switch class:

.. code:: python

    In [16]: class CiscoSwitch(Switch):
        ...:     __quantity = 0
        ...:     def __configure(self):
        ...:         pass
        ...:

    In [17]: dir(CiscoSwitch)
    Out[17]:
    ['_CiscoSwitch__configure', '_CiscoSwitch__quantity', '_Switch__configure', '_Switch__quantity', ...]

Two underscores before and after name
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Thus, special variables and methods are denoted.

For example, Python module has such special variables:

* ``__name__`` - this variable is equal to ``__main__`` when script runs directly, and it is equal to module name when imported
* ``__file__`` - this variable is equal to script name that was run directly, and equals to complete path to the module when it is imported

``__name__`` variable is most commonly used to indicate that a certain part of the code must be executed only when module is executed directly:

.. code:: python


    def multiply(a, b):

        return a * b

    if __name__ == '__main__':
        print(multiply(3, 5))

``__file__`` variable can be useful in determining the current path to script file:

.. code:: python

    import os

    print('__file__', __file__)
    print(os.path.abspath(__file__))

The output will be:

::

    __file__ example2.py
    /home/vagrant/repos/tests/example2.py

Python also denotes special methods in this way. These methods are called when using Python functions and operators and allow for implementation of a certain functionality.

As a rule, such methods need not be called directly. But for example, when creating your own class it may be necessary to describe such method in order to make object support some operations in Python.

For example, in order to get object length, it must support ``__len__`` method.

Another special method ``__str__`` is called when ``print`` operator is used or
``str`` function is called. If it is necessary to get a certain output,
you have to create this method in the class:

.. code:: python

    In [10]: class Switch(object):
        ...:
        ...:     def set_name(self, name):
        ...:         self.name = name
        ...:
        ...:     def __configure(self):
        ...:         pass
        ...:
        ...:     def __str__(self):
        ...:         return 'Switch {}'.format(self.name)
        ...:

    In [11]: sw1 = Switch()

    In [12]: sw1.set_name('sw1')

    In [13]: print sw1
    Switch sw1

    In [14]: str(sw1)
    Out[14]: 'Switch sw1'

There are many such special methods in Python. Some useful links where you can read about a particular method:

* `documentation <https://docs.python.org/3.6/reference/datamodel.html#specialnames>`__
* `Dive Into Python 3 <http://www.diveintopython3.net/special-method-names.html>`__
