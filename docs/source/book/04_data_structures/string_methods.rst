Useful methods for working with strings
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When automating, very often it will be necessary to work with strings, since
config file, command output and commands sent - are strings.

Knowledge of various methods (actions) that can be applied to
strings helps to work with them more efficiently.

Strings are immutable data type, so all methods that convert
string returns a new string and the original string remains unchanged.

Methods upper, lower, swapcase, capitalize
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Methods ``upper()``, ``lower()``, ``swapcase()``,
``capitalize()`` perform string case conversion:

.. code:: python

    In [25]: string1 = 'FastEthernet'

    In [26]: string1.upper()
    Out[26]: 'FASTETHERNET'

    In [27]: string1.lower()
    Out[27]: 'fastethernet'

    In [28]: string1.swapcase()
    Out[28]: 'fASTeTHERNET'

    In [29]: string2 = 'tunnel 0'

    In [30]: string2.capitalize()
    Out[30]: 'Tunnel 0'

It is very important to pay attention to the fact that methods often return
the converted string. And, therefore, we must not forget to assign it to some
variable (you can use the same).

.. code:: python

    In [31]: string1 = string1.upper()

    In [32]: print(string1)
    FASTETHERNET

Method count
^^^^^^^^^^^

Method ``count()`` or a substring occurs in a string:

.. code:: python

    In [33]: string1 = 'Hello, hello, hello, hello'

    In [34]: string1.count('hello')
    Out[34]: 3

    In [35]: string1.count('ello')
    Out[35]: 4

    In [36]: string1.count('l')
    Out[36]: 8

Method find
^^^^^^^^^^

Method `` find () '' can be passed a substring or a character and it will show
at what position is the first character of the substring (for the first
matches):

.. code:: python

    In [37]: string1 = 'interface FastEthernet0/1'

    In [38]: string1.find('Fast')
    Out[38]: 10

    In [39]: string1[string1.find('Fast')::]
    Out[39]: 'FastEthernet0/1'

If no match is found, find() method returns ``-1``.

Methods startswith, endswith
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Checking if a string starts or ends with certain
symbols (methods ``startswith()``, ``endswith()``):

.. code:: python

    In [40]: string1 = 'FastEthernet0/1'

    In [41]: string1.startswith('Fast')
    Out[41]: True

    In [42]: string1.startswith('fast')
    Out[42]: False

    In [43]: string1.endswith('0/1')
    Out[43]: True

    In [44]: string1.endswith('0/2')
    Out[44]: False

Method replace
^^^^^^^^^^^^^

Replacing a sequence of characters in a string with another sequence
(method ``replace()``):

.. code:: python

    In [45]: string1 = 'FastEthernet0/1'

    In [46]: string1.replace('Fast', 'Gigabit')
    Out[46]: 'GigabitEthernet0/1'

Method strip
^^^^^^^^^^^

Often when a file is processed, the file is opened line by line. But at the end of each line, there are usually some special characters (and may be at the beginning). For example, line feed character.

To get rid of them, it is very convenient to use method ``strip()``:

.. code:: python

    In [47]: string1 = '\n\tinterface FastEthernet0/1\n'

    In [48]: print(string1)

        interface FastEthernet0/1


    In [49]: string1
    Out[49]: '\n\tinterface FastEthernet0/1\n'

    In [50]: string1.strip()
    Out[50]: 'interface FastEthernet0/1'

By default, strip() method removes blank characters. This character set includes: ``\t\n\r\f\v``

Method strip() can be passed as an argument of any characters. Then at the beginning and at the end of the line all characters that were specified in the line will be removed:

.. code:: python

    In [51]: ad_metric = '[110/1045]'

    In [52]: ad_metric.strip('[]')
    Out[52]: '110/1045'

Method strip() removes special characters at the beginning and at the end of the line. If you want to remove characters only on the left or only on the right, you can use ``lstrip()`` and ``rstrip()``.

Method split
^^^^^^^^^^^

Method ``split()`` split() splits the string using a symbol (or symbols) as separator and returns a list of strings:

.. code:: python

    In [53]: string1 = 'switchport trunk allowed vlan 10,20,30,100-200'

    In [54]: commands = string1.split()

    In [55]: print(commands)
    ['switchport', 'trunk', 'allowed', 'vlan', '10,20,30,100-200']

In example above, ``string1.split()`` splits the string by spaces and returns a list of strings. The list is saved to *commands* variable.

By default, separator is a space symbol (spaces, tabs, line feed), but you can specify any separator in brackets:

.. code:: python

    In [56]: vlans = commands[-1].split(',')

    In [57]: print(vlans)
    ['10', '20', '30', '100-200']

In *commands* list, the last element is a string with vlans, so the index -1 is used.
Then string is split into parts using split() ``commands[-1].split(',')``.
Since separator is a comma, this list is received ``['10', '20', '30', '100-200']``.

A useful feature of split() method with default separator is that the string is not only split into a list of strings by space characters, but the space characters are also removed at the beginning and at the end of the line:

.. code:: python

    In [58]: string1 = '  switchport trunk allowed vlan 10,20,30,100-200\n\n'

    In [59]: string1.split()
    Out[59]: ['switchport', 'trunk', 'allowed', 'vlan', '10,20,30,100-200']


Method ``split()`` has another good feature: by default, method splits a string not by one whitespace character, but by any number. For example, this will be very useful when processing show commands:

.. code:: python

    In [60]: sh_ip_int_br = "FastEthernet0/0       15.0.15.1    YES manual up         up"

    In [61]: sh_ip_int_br.split()
    Out[61]: ['FastEthernet0/0', '15.0.15.1', 'YES', 'manual', 'up', 'up']

And this is the same string when one space is used as the separator:

.. code:: python


    In [62]: sh_ip_int_br.split(' ')
    Out[62]:
    ['FastEthernet0/0', '', '', '', '', '', '', '', '', '', '', '', '15.0.15.1', '', '', '', '', '', '', 'YES', 'manual', 'up', '', '', '', '', '', '', '', '', '', '', '', '', '', '', '', '', '', '', '', 'up']

