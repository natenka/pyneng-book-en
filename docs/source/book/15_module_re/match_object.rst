Match object
------------

In ``re`` module, several functions return Match object if a match is found:

* ``search``
* ``match`` 
* ``finditer`` - returns an iterator with Match objects

This subsection covers methods of Match object.

Example of Match object:

.. code:: python

    In [1]: log = 'Jun  3 14:39:05.941: %SW_MATM-4-MACFLAP_NOTIF: Host f03a.b216.7ad7 in vlan 10 is flapping between port Gi0/5 and port Gi0/15'

    In [2]: match = re.search(r'Host (\S+) in vlan (\d+) .* port (\S+) and port (\S+)', log)

    In [3]: match
    Out[3]: <_sre.SRE_Match object; span=(47, 124), match='Host f03a.b216.7ad7 in vlan 10 is flapping betwee>'

The 3rd line output simply displays information about object. Therefore, it is
not necessary to rely on what is displayed in match part as displayed line is cut
by a fixed number of characters.

``group``
^^^^^^^

Method ``group`` returns a substring that matches an expression or an expression in a group.

If method is called without arguments, the whole substring is displayed:

.. code:: python

    In [4]: match.group()
    Out[4]: 'Host f03a.b216.7ad7 in vlan 10 is flapping between port Gi0/5 and port Gi0/15'

The same result returns group 0:

.. code:: python

    In [5]: match.group(0)
    Out[5]: 'Host f03a.b216.7ad7 in vlan 10 is flapping between port Gi0/5 and port Gi0/15'

Other numbers show only the contents of relevant group:

.. code:: python

    In [6]: match.group(1)
    Out[6]: 'f03a.b216.7ad7'

    In [7]: match.group(2)
    Out[7]: '10'

    In [8]: match.group(3)
    Out[8]: 'Gi0/5'

    In [9]: match.group(4)
    Out[9]: 'Gi0/15'

If you call a ``group`` method with a group number that is larger than number
of existing groups, there is an error:

.. code:: python

    In [10]: match.group(5)
    -----------------------------------------------------------------
    IndexError                      Traceback (most recent call last)
    <ipython-input-18-9df93fa7b44b> in <module>()
    ----> 1 match.group(5)

    IndexError: no such group

If you call a method with multiple group numbers, the result is a tuple with
strings that correspond to matches:

.. code:: python

    In [11]: match.group(1, 2, 3)
    Out[11]: ('f03a.b216.7ad7', '10', 'Gi0/5')

Group may not get anything, then it will be matched with an empty string:

.. code:: python

    In [12]: log = 'Jun  3 14:39:05.941: %SW_MATM-4-MACFLAP_NOTIF: Host f03a.b216.7ad7 in vlan 10 is flapping between port Gi0/5 and port Gi0/15'

    In [13]: match = re.search(r'Host (\S+) in vlan (\D*)', log)

    In [14]: match.group(2)
    Out[14]: ''

If group describes a part of template and there are more than one match,
method displays the last match:

.. code:: python

    In [15]: log = 'Jun  3 14:39:05.941: %SW_MATM-4-MACFLAP_NOTIF: Host f03a.b216.7ad7 in vlan 10 is flapping between port Gi0/5 and port Gi0/15'

    In [16]: match = re.search(r'Host (\w{4}\.)+', log)

    In [17]: match.group(1)
    Out[17]: 'b216.'

This is because expression in parentheses describes four letters or numbers, dot
and then there is a plus. The first and the second part of MAC
address matched to expression in parentheses. But only the last expression is
remembered and returned.

If named groups are used in expression, group name can be passed to ``group``
method and the corresponding substring can be obtained:

.. code:: python

    In [18]: log = 'Jun  3 14:39:05.941: %SW_MATM-4-MACFLAP_NOTIF: Host f03a.b216.7ad7 in vlan 10 is flapping between port Gi0/5 and port Gi0/15'

    In [19]: match = re.search(r'Host (?P<mac>\S+) '
        ...:                   r'in vlan (?P<vlan>\d+) .* '
        ...:                   r'port (?P<int1>\S+) '
        ...:                   r'and port (?P<int2>\S+)',
        ...:                   log)
        ...:

    In [20]: match.group('mac')
    Out[20]: 'f03a.b216.7ad7'

    In [21]: match.group('int2')
    Out[21]: 'Gi0/15'

Groups are also available via number:

.. code:: python

    In [22]: match.group(3)
    Out[22]: 'Gi0/5'

    In [23]: match.group(4)
    Out[23]: 'Gi0/15'

``groups``
^^^^^^^^^^

Method ``group`` returns a tuple with strings in which the elements are those
substrings that fall into respective groups:

.. code:: python

    In [24]: log = 'Jun  3 14:39:05.941: %SW_MATM-4-MACFLAP_NOTIF: Host f03a.b216.7ad7 in vlan 10 is flapping between port Gi0/5 and port Gi0/15'

    In [25]: match = re.search(r'Host (\S+) '
        ...:                   r'in vlan (\d+) .* '
        ...:                   r'port (\S+) '
        ...:                   r'and port (\S+)',
        ...:                   log)
        ...:

    In [26]: match.groups()
    Out[26]: ('f03a.b216.7ad7', '10', 'Gi0/5', 'Gi0/15')

Method ``group`` has an optional parameter - default. It returned when anything
that comes into group is optional.

For example, with this line the match will be in both the first group and the second:

.. code:: python

    In [26]: line = '100     aab1.a1a1.a5d3    FastEthernet0/1'

    In [27]: match = re.search(r'(\d+) +(\w+)?', line)

    In [28]: match.groups()
    Out[28]: ('100', 'aab1')

If there is nothing in the line after space, nothing will get into the group.
But the match will be because it is stated in regex that the group is optional:

.. code:: python

    In [30]: line = '100     '

    In [31]: match = re.search(r'(\d+) +(\w+)?', line)

    In [32]: match.groups()
    Out[32]: ('100', None)

Accordingly, for the second group the value is None.

If ``group`` method is given a default value, it will be returned
instead of None:

.. code:: python

    In [33]: line = '100     '

    In [34]: match = re.search(r'(\d+) +(\w+)?', line)

    In [35]: match.groups(default=0)
    Out[35]: ('100', 0)

    In [36]: match.groups(default='No match')
    Out[36]: ('100', 'No match')

``groupdict``
^^^^^^^^^^^^^

Method ``groupdict`` returns a dictionary in which keys are group names and
values are corresponding lines:

.. code:: python

    In [37]: log = 'Jun  3 14:39:05.941: %SW_MATM-4-MACFLAP_NOTIF: Host f03a.b216.7ad7 in vlan 10 is flapping between port Gi0/5 and port Gi0/15'

    In [38]: match = re.search(r'Host (?P<mac>\S+) '
        ...:                   r'in vlan (?P<vlan>\d+) .* '
        ...:                   r'port (?P<int1>\S+) '
        ...:                   r'and port (?P<int2>\S+)',
        ...:                   log)
        ...:

    In [39]: match.groupdict()
    Out[39]: {'int1': 'Gi0/5', 'int2': 'Gi0/15', 'mac': 'f03a.b216.7ad7', 'vlan': '10'}

``start``, ``end``
^^^^^^^^^^^^^^^^^^

``start`` and ``end`` methods return indexes of the beginning and end of the match
of regex.

If methods are called without arguments, they return indexes for whole match:

.. code:: python

    In [40]: line = '  10     aab1.a1a1.a5d3    FastEthernet0/1  '

    In [41]: match = re.search(r'(\d+) +([0-9a-f.]+) +(\S+)', line)

    In [42]: match.start()
    Out[42]: 2

    In [43]: match.end()
    Out[43]: 42

    In [45]: line[match.start():match.end()]
    Out[45]: '10     aab1.a1a1.a5d3    FastEthernet0/1'

You can pass number or name of the group to methods. Then they return
indexes for this group:

.. code:: python

    In [46]: match.start(2)
    Out[46]: 9

    In [47]: match.end(2)
    Out[47]: 23

    In [48]: line[match.start(2):match.end(2)]
    Out[48]: 'aab1.a1a1.a5d3'

Similarly for named groups:

.. code:: python

    In [49]: log = 'Jun  3 14:39:05.941: %SW_MATM-4-MACFLAP_NOTIF: Host f03a.b216.7ad7 in vlan 10 is flapping between port Gi0/5 and port Gi0/15'

    In [50]: match = re.search(r'Host (?P<mac>\S+) '
        ...:                   r'in vlan (?P<vlan>\d+) .* '
        ...:                   r'port (?P<int1>\S+) '
        ...:                   r'and port (?P<int2>\S+)',
        ...:                   log)
        ...:

    In [51]: match.start('mac')
    Out[51]: 52

    In [52]: match.end('mac')
    Out[52]: 66

``span``
^^^^^^^^

Method ``span`` returns a tuple with an index of the beginning and end of
substring. It works in a similar way to ``start`` and ``end`` methods,
but returns a pair of numbers.

Without arguments ``span`` returns indexes for whole match:

.. code:: python

    In [53]: line = '  10     aab1.a1a1.a5d3    FastEthernet0/1  '

    In [54]: match = re.search(r'(\d+) +([0-9a-f.]+) +(\S+)', line)

    In [55]: match.span()
    Out[55]: (2, 42)

But you can also pass number of the group:

.. code:: python

    In [56]: line = '  10     aab1.a1a1.a5d3    FastEthernet0/1  '

    In [57]: match = re.search(r'(\d+) +([0-9a-f.]+) +(\S+)', line)

    In [58]: match.span(2)
    Out[58]: (9, 23)

Similarly for named groups:

.. code:: python

    In [59]: log = 'Jun  3 14:39:05.941: %SW_MATM-4-MACFLAP_NOTIF: Host f03a.b216.7ad7 in vlan 10 is flapping between port Gi0/5 and port Gi0/15'

    In [60]: match = re.search(r'Host (?P<mac>\S+) '
        ...:                   r'in vlan (?P<vlan>\d+) .* '
        ...:                   r'port (?P<int1>\S+) '
        ...:                   r'and port (?P<int2>\S+)',
        ...:                   log)
        ...:

    In [64]: match.span('mac')
    Out[64]: (52, 66)

    In [65]: match.span('vlan')
    Out[65]: (75, 77)

