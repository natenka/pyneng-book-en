Literal strings concatenation
---------------------------

Python has very convenient functionality — literal strings concatenation

.. code:: python

    In [1]: s = ('Test' 'String')

    In [2]: s
    Out[2]: 'TestString'

    In [3]: s = 'Test' 'String'

    In [4]: s
    Out[4]: 'TestString'

It is even possible to transfer the composite strings to different strings,
but only if they are in parentheses:

.. code:: python

    In [5]: s = ('Test'
       ...: 'String')

    In [6]: s
    Out[6]: 'TestString'

This is very convenient to use in regexs:

.. code:: python

    regex = ('(\S+) +(\S+) +'
             '\w+ +\w+ +'
             '(up|down|administratively down) +'
             '(\w+)')

This way, the regex can be split and made easier to understand. Plus you can add explanatory comments in strings.

.. code:: python

    regex = ('(\S+) +(\S+) +' # interface and IP
             '\w+ +\w+ +'
             '(up|down|administratively down) +' # Status
             '(\w+)') # Protocol

It is also convenient to use this technique when writing a long message:

.. code:: python

    In [7]: message = ('During command execution "{}" '
       ...: 'such error occured "{}".\n'
       ...: 'Exclude this command from the list? [y/n]')

    In [8]: message
    Out[8]: 'During command execution "{}" such error occured "{}".\nExclude this command from the list? [y/n]'

