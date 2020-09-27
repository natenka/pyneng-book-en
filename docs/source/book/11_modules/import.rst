Module import
-------------

Python has several ways to import a module:

* ``import module``
* ``import module as``
* ``from module import object``
* ``from module import *``

``import module``
~~~~~~~~~~~~~~~~~

Example of **import module**:

.. code:: python

    In [1]: dir()
    Out[1]: 
    ['In',
     'Out',
     ...
     'exit',
     'get_ipython',
     'quit']

    In [2]: import os

    In [3]: dir()
    Out[3]: 
    ['In',
     'Out',
     ...
     'exit',
     'get_ipython',
     'os',
     'quit']

After importing the **os** module appeared in the output ``dir()``.This means that it is now in the current namespace.

To invoke some function or method from the **os** module you should specify
``os.`` and then the object name:

.. code:: python

    In [4]: os.getlogin()
    Out[4]: 'natasha'

This import method is good because the module objects do not enter the namespace of the current program. That is, if you create a function named getlogin() it will not conflict with the same function of the **os** module.

.. note::
    If file name contains a dot, the standard way of importing will not work. In such cases,     `another method <http://stackoverflow.com/questions/1828127/how-to-reference-python-package-when-filename-contains-a-period/1828249#1828249>`__ is used.

``import module as``
~~~~~~~~~~~~~~~~~~~~

Construction **import module as** allows importing a module under a different name (usually shorter):

.. code:: python

    In [1]: import subprocess as sp

    In [2]: sp.check_output('ping -c 2 -n  8.8.8.8', shell=True)
    Out[2]: 'PING 8.8.8.8 (8.8.8.8): 56 data bytes\n64 bytes from 8.8.8.8: icmp_seq=0 ttl=48 time=49.880 ms\n64 bytes from 8.8.8.8: icmp_seq=1 ttl=48 time=46.875 ms\n\n--- 8.8.8.8 ping statistics ---\n2 packets transmitted, 2 packets received, 0.0% packet loss\nround-trip min/avg/max/stddev = 46.875/48.377/49.880/1.503 ms\n'

``from module import object``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Option **from module import object** is convenient to use when only one or two functions are needed from the whole module:

.. code:: python

    In [1]: from os import getlogin, getcwd

These functions are now available in the current namespace:

.. code:: python

    In [2]: dir()
    Out[2]: 
    ['In',
     'Out',
     ...
     'exit',
     'get_ipython',
     'getcwd',
     'getlogin',
     'quit']

They can be called without the module name:

.. code:: python

    In [3]: getlogin()
    Out[3]: 'natasha'

    In [4]: getcwd()
    Out[4]: '/Users/natasha/Desktop/Py_net_eng/code_test'

``from module import *``
~~~~~~~~~~~~~~~~~~~~~~~~

Option ``from module import *`` imports all module names into the current namespace:

.. code:: python

    In [1]: from os import *

    In [2]: dir()
    Out[2]: 
    ['EX_CANTCREAT',
     'EX_CONFIG',
     ...
     'wait',
     'wait3',
     'wait4',
     'waitpid',
     'walk',
     'write']

    In [3]: len(dir())
    Out[3]: 218

There are many objects in the **os** module, so the output is shortened. At the end, the length of the list of names of current namespace is specified.

This import option is best not to use. With such code import it is not clear which function is taken, for example from the **os** module. This makes it much harder to understand the code.
