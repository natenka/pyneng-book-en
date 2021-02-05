String formatting
====================

When working with strings, there are often situations where different data
needs to be substituted in string template. This can be done by combining
string parts and data, but Python has a more convenient way - strings formatting.

String formatting can help, for example, in such situations:

* need to set values to a string by a certain template
* need to format output by columns
* need to convert numbers to binary format

There are several options for string formatting:

* with operator ``%`` — older option
* method ``format`` — relatively new option
* f-строки — new option that appeared in Python 3.6

Although ``format`` is recommended, string formatting can often be found through  ``%``.

String formatting with ``format`` method
-------------------------------------

Example of ``format`` method use:

.. code:: python

    In [1]: "interface FastEthernet0/{}".format('1')
    Out[1]: 'interface FastEthernet0/1'

A special symbol ``{}`` indicates that the value that is passed to ``format``
method is placed here. Each pair of curly braces represents one place for the substitution.

Values that are placed in curly braces may be of different types. For example, it can be a string, number or list:

.. code:: python

    In [3]: print('{}'.format('10.1.1.1'))
    10.1.1.1

    In [4]: print('{}'.format(100))
    100

    In [5]: print('{}'.format([10, 1, 1,1]))
    [10, 1, 1, 1]

You can align result in columns by formatting strings. In string
formatting, you can specify how many characters are selected for the data. If
number of characters in the data is less than number of characters selected,
the missing characters are filled with blanks.

For example, you can allign data in columns of equal width of
15 characters with right side alignment:

.. code:: python

    In [3]: vlan, mac, intf = ['100', 'aabb.cc80.7000', 'Gi0/1']

    In [4]: print("{:>15} {:>15} {:>15}".format(vlan, mac, intf))
                100  aabb.cc80.7000           Gi0/1

Alignment to the left:

.. code:: python

    In [5]: print("{:15} {:15} {:15}".format(vlan, mac, intf))
    100             aabb.cc80.7000  Gi0/1

Output template can also be multi-string:

.. code:: python

    In [6]: ip_template = '''
       ...: IP address:
       ...: {}
       ...: '''

    In [7]: print(ip_template.format('10.1.1.1'))

    IP address:
    10.1.1.1

You can also use string formatting to change the display format of numbers.

For example, you can specify how many digits after the comma to show:

.. code:: python

    In [9]: print("{:.3f}".format(10.0/3))
    3.333

Using string formatting, you can convert numbers to binary format:

.. code:: python

    In [11]: '{:b} {:b} {:b} {:b}'.format(192, 100, 1, 1)
    Out[11]: '11000000 1100100 1 1'

You can still specify additional parameters such as column width:

.. code:: python

    In [12]: '{:8b} {:8b} {:8b} {:8b}'.format(192, 100, 1, 1)
    Out[12]: '11000000  1100100        1        1'

You can also specify that numbers should be supplemented with zeros instead of spaces:

.. code:: python

    In [13]: '{:08b} {:08b} {:08b} {:08b}'.format(192, 100, 1, 1)
    Out[13]: '11000000 01100100 00000001 00000001'

You can enter names in curly braces. This makes it possible to pass arguments
in any order and also makes template more understandable:

.. code:: python

    In [15]: '{ip}/{mask}'.format(mask=24, ip='10.1.1.1')
    Out[15]: '10.1.1.1/24'

Another useful feature of string formatting is argument number specification:

.. code:: python

    In [16]: '{1}/{0}'.format(24, '10.1.1.1')
    Out[16]: '10.1.1.1/24'

For example this can prevent repetitive transmission of the same values:

.. code:: python

    In [19]: ip_template = '''
        ...: IP address:
        ...: {:<8} {:<8} {:<8} {:<8}
        ...: {:08b} {:08b} {:08b} {:08b}
        ...: '''

    In [20]: print(ip_template.format(192, 100, 1, 1, 192, 100, 1, 1))

    IP address:
    192      100      1        1
    11000000 01100100 00000001 00000001

In example above the octet address has to be passed twice - one for decimal
format and other for binary.

By specifying value indexes that are passed to ``format`` method, it is possible to avoid duplication:

.. code:: python

    In [21]: ip_template = '''
        ...: IP address:
        ...: {0:<8} {1:<8} {2:<8} {3:<8}
        ...: {0:08b} {1:08b} {2:08b} {3:08b}
        ...: '''

    In [22]: print(ip_template.format(192, 100, 1, 1))

    IP address:
    192      100      1        1
    11000000 01100100 00000001 00000001


Strings formatting with f-Strings
--------------------------------------

Python 3.6 added a new version of string formatting - f-strings or
interpolation of strings. The f-strings allow not only to set values
to template, but also to perform calls to functions, methods, etc.

In many situations f-strings are easier to use than format, and f-strings
work faster than format and other methods of string formatting.

Syntax
~~~~~~~~~

F-string is a literal line with a letter f in front of it. Inside
f-string, in curly braces there are names of variables that will
be substituted:

.. code:: python

    In [1]: ip = '10.1.1.1'

    In [2]: mask = 24

    In [3]: f"IP: {ip}, mask: {mask}"
    Out[3]: 'IP: 10.1.1.1, mask: 24'


The same result with ``format`` method you can achieve by:
``"IP: {ip}, mask: {mask}".format(ip=ip, mask=mask)``.

A very important difference between f-strings and ``format``: f-strings are
expressions that are processed, not just strings. That is, in case of ipython,
as soon as we wrote the expression and pressed Enter, it was performed and
instead of expressions ``{ip}`` and ``{mask}`` the values of variables were substituted.

Therefore, for example, you cannot first write a template and then define
variables that are used in template:

.. code:: python

    In [1]: f"IP: {ip}, mask: {mask}"
    ---------------------------------------------------------------------------
    NameError                                 Traceback (most recent call last)
    <ipython-input-1-e6f8e01ac9c4> in <module>()
    ----> 1 f"IP: {ip}, mask: {mask}"

    NameError: name 'ip' is not defined

In addition to substituting variable values you can write expressions in curly braces:

.. code:: python

    In [5]: first_name = 'William'

    In [6]: second_name = 'Shakespeare'

    In [7]: f"{first_name.upper()} {second_name.upper()}"
    Out[7]: 'WILLIAM SHAKESPEARE'

After colon in f-strings you can specify the same values as in ``format``:

.. code:: python

    In [9]: oct1, oct2, oct3, oct4 = [10, 1, 1, 1]

    In [10]: print(f'''
        ...: IP address:
        ...: {oct1:<8} {oct2:<8} {oct3:<8} {oct4:<8}
        ...: {oct1:08b} {oct2:08b} {oct3:08b} {oct4:08b}''')

    IP address:
    10       1        1        1
    00001010 00000001 00000001 00000001

.. warning::

  Since for full explanation of f-strings it is necessary to show examples
  with loops and work with objects that have not yet been covered, this topic
  is also in the section :ref:`f_string` with additional examples and explanations.

