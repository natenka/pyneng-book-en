
.. _f_string:

Formatting lines with f-strings
======================================

Python 3.6 added a new version of string formatting - f-strings or interpolation of strings. The f-strings allow not only to set values to template but also to perform calls to functions, methods, etc.

In many situations f-strings are easier to use than format() and f-strings work faster than format() and other methods of string formatting.

Syntax
~~~~~~~~~

F-string is a string literal with a letter ``f`` in front of it. Inside f-string, in curly braces there are names of variables that will be substituted: 

.. code:: python

    In [1]: ip = '10.1.1.1'

    In [2]: mask = 24

    In [3]: f"IP: {ip}, mask: {mask}"
    Out[3]: 'IP: 10.1.1.1, mask: 24'

    The same result with format() method you can achieve by:
    ``"IP: {ip}, mask: {mask}".format(ip=ip, mask=mask)``.

A very important difference between f-strings and format(): f-strings are expressions
that are processed, not just strings. That is, in case of ipython, as soon as we wrote
the expression and pressed Enter, it was performed and instead of expressions
``{ip}`` and ``{mask}`` the values of variables were substituted.

Therefore, for example, you cannot first write a template and then define variables that are used in template:

.. code:: python

    In [1]: f"IP: {ip}, mask: {mask}"
    ---------------------------------------------------------------------------
    NameError                                 Traceback (most recent call last)
    <ipython-input-1-e6f8e01ac9c4> in <module>()
    ----> 1 f"IP: {ip}, mask: {mask}"

    NameError: name 'ip' is not defined

In addition to substituting variable values you can write expressions in curly braces:

.. code:: python

    In [1]: octets = ['10', '1', '1', '1']

    In [2]: mask = 24

    In [3]: f"IP: {'.'.join(octets)}, mask: {mask}"
    Out[3]: 'IP: 10.1.1.1, mask: 24'

After colon in f-strings you can specify the same values as in format():

.. code:: python

    In [9]: oct1, oct2, oct3, oct4 = [10, 1, 1, 1]

    In [10]: print(f'''
        ...: IP address:
        ...: {oct1:<8} {oct2:<8} {oct3:<8} {oct4:<8}
        ...: {oct1:08b} {oct2:08b} {oct3:08b} {oct4:08b}''')

    IP address:
    10       1        1        1
    00001010 00000001 00000001 00000001

Special aspects of f-strings
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When using f-strings you cannot first create a template and then use it as in format() method.

F-string is immediately executed and contains the values of variables that were defined earlier:

.. code:: python

    In [7]: ip = '10.1.1.1'

    In [8]: mask = 24

    In [9]: print(f"IP: {ip}, mask: {mask}")
    IP: 10.1.1.1, mask: 24

If you want to set other values you must create new variables (with the same names) and write f-string again:

.. code:: python

    In [11]: ip = '10.2.2.2'

    In [12]: mask = 24

    In [13]: print(f"IP: {ip}, mask: {mask}")
    IP: 10.2.2.2, mask: 24


When using f-strings in loops an f-string must be written in body of the loop to «catch» new variable values within each iteration:

.. code:: python

    In [1]: ip_list = ['10.1.1.1/24', '10.2.2.2/24', '10.3.3.3/24']

    In [2]: for ip_address in ip_list:
       ...:     ip, mask = ip_address.split('/')
       ...:     print(f"IP: {ip}, mask: {mask}")
       ...:
    IP: 10.1.1.1, mask: 24
    IP: 10.2.2.2, mask: 24
    IP: 10.3.3.3, mask: 24

Examples of f-string usage
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Basic variable substitution:

.. code:: python

    In [1]: intf_type = 'Gi'

    In [2]: intf_name = '0/3'

    In [3]: f'interface {intf_type}/{intf_name}'
    Out[3]: 'interface Gi/0/3'

Column alignment:

.. code:: python

    In [6]: topology = [['sw1', 'Gi0/1', 'r1', 'Gi0/2'],
       ...:             ['sw1', 'Gi0/2', 'r2', 'Gi0/1'],
       ...:             ['sw1', 'Gi0/3', 'r3', 'Gi0/0'],
       ...:             ['sw1', 'Gi0/5', 'sw4', 'Gi0/2']]
       ...:

    In [7]: for connection in topology:
       ...:     l_device, l_port, r_device, r_port = connection
       ...:     print(f'{l_device:10} {l_port:7} {r_device:10} {r_port:7}')
       ...:
    sw1        Gi0/1   r1         Gi0/2
    sw1        Gi0/2   r2         Gi0/1
    sw1        Gi0/3   r3         Gi0/0
    sw1        Gi0/5   sw4        Gi0/2

Column width can be specified by variable:

.. code:: python

    In [6]: topology = [['sw1', 'Gi0/1', 'r1', 'Gi0/2'],
       ...:             ['sw1', 'Gi0/2', 'r2', 'Gi0/1'],
       ...:             ['sw1', 'Gi0/3', 'r3', 'Gi0/0'],
       ...:             ['sw1', 'Gi0/5', 'sw4', 'Gi0/2']]
       ...:

    In [7]: width = 10

    In [8]: for connection in topology:
       ...:     l_device, l_port, r_device, r_port = connection
       ...:     print(f'{l_device:{width}} {l_port:{width}} {r_device:{width}} {r_port:{width}}')
       ...:
    sw1        Gi0/1      r1         Gi0/2
    sw1        Gi0/2      r2         Gi0/1
    sw1        Gi0/3      r3         Gi0/0
    sw1        Gi0/5      sw4        Gi0/2

Accessing a dictionary key:

.. code:: python

    In [1]: session_stats = {'done': 10, 'todo': 5}

    In [2]: if session_stats['todo']:
       ...:     print(f"Pomodoros done: {session_stats['done']}, TODO: {session_stats['todo']}")
       ...: else:
       ...:     print(f"Good job! All {session_stats['done']} pomodoros done!")
       ...:
    Pomodoros done: 10, TODO: 5

Call len() function inside f-string:

.. code:: python

    In [2]: topology = [['sw1', 'Gi0/1', 'r1', 'Gi0/2'],
       ...:             ['sw1', 'Gi0/2', 'r2', 'Gi0/1'],
       ...:             ['sw1', 'Gi0/3', 'r3', 'Gi0/0'],
       ...:             ['sw1', 'Gi0/5', 'sw4', 'Gi0/2']]
       ...:

    In [3]: print(f'Number of connections in topology: {len(topology)}')
    Number of connections in topology: 4

Call upper() method inside f-string:

.. code:: python

    In [1]: name = 'python'

    In [2]: print(f'Zen of {name.upper()}')
    Zen of PYTHON

Converting numbers to binary format:

.. code:: python

    In [7]: ip = '10.1.1.1'

    In [8]: oct1, oct2, oct3, oct4 = ip.split('.')

    In [9]: print(f'{int(oct1):08b} {int(oct2):08b} {int(oct3):08b} {int(oct4):08b}')
    00001010 00000001 00000001 00000001

What to use format or f-strings
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In many cases f-strings are more convenient to use as template looks more understandable and compact.
However, there are cases when format() method is more convenient. For example:

.. code:: python

    In [6]: ip = [10, 1, 1, 1]

    In [7]: oct1, oct2, oct3, oct4 = ip
       ...: print(f'{oct1:08b} {oct2:08b} {oct3:08b} {oct4:08b}')
       ...:
    00001010 00000001 00000001 00000001

    In [8]: template = "{:08b} "*4

    In [9]: template.format(*ip)
    Out[9]: '00001010 00000001 00000001 00000001 '

Another situation where format() is usually more convenient to use: the need to use the same
template many times in script. F-string will execute the first time and will set current values
of variables and to use template again it has to be rewritten. This means that script will
contain copies of the same line. At the same time format() allows to create a template
in one place and then use it again substituting variables as needed.

This can be avoided by creating a function but creating a function to print a string based
on template is not always justified. Example of creating a function:

.. code:: python

    In [1]: def show_me_ip(ip, mask):
       ...:     return f"IP: {ip}, mask: {mask}"
       ...:

    In [2]: show_me_ip('10.1.1.1', 24)
    Out[2]: 'IP: 10.1.1.1, mask: 24'

    In [3]: show_me_ip('192.16.10.192', 28)
    Out[3]: 'IP: 192.16.10.192, mask: 28'

