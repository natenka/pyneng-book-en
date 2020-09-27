All
-----------

The ``all()`` function returns True if all elements are true (or the object is empty).

.. code:: python

    In [1]: all([False, True, True])
    Out[1]: False

    In [2]: all([True, True, True])
    Out[2]: True

    In [3]: all([])
    Out[3]: True

For example, it is possible to check that all octets in an IP address are numbers:

.. code:: python

    In [4]: IP = '10.0.1.1'

    In [5]: all( i.isdigit() for i in IP.split('.'))
    Out[5]: True

    In [6]: all( i.isdigit() for i in '10.1.1.a'.split('.'))
    Out[6]: False

Any
-----------

The any() function returns True if at least one element is true.

.. code:: python

    In [7]: any([False, True, True])
    Out[7]: True

    In [8]: any([False, False, False])
    Out[8]: False

    In [9]: any([])
    Out[9]: False

    In [10]: any( i.isdigit() for i in '10.1.1.a'.split('.'))
    Out[10]: True

For example, with any() you can replace ignore_command() function:

.. code:: python

    def ignore_command(command):
        '''
        Function checks if command contains a word from ignore list. 
        * command is a string. Command that need to be checked returns True 
        * if command contains a word from ignore list, False - if not.
        '''
        ignore = ['duplex', 'alias', 'Current configuration']

        for word in ignore:
            if word in command:
                return True
        return False

To this option:

.. code:: python

    def ignore_command(command):
        '''
        Function checks if command contains a word from ignore list. 
        * command is a string. Command that need to be checked returns True 
        * if command contains a word from ignore list, False - if not.
        '''
        ignore = ['duplex', 'alias', 'Current configuration']

        return any(word in command for word in ignore)

