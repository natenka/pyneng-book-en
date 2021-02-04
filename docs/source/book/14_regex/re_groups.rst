Grouping
--------

Grouping indicates that sequence of symbols should be considered
as a one. However, this is not the only advantage of grouping.
In addition, by use of groups you can get only a certain portion
of string that has been described by expression.

For example, from a log file you should select strings in which
"%SW_MATM-4-MACFLAP_NOTIF" match occur and then from each such string get
MAC address, VLAN and interfaces. In this case, regular expression
has to describe a string and all parts of string to be remembered
are placed in parentheses.

For example, from the log file, you need to select the lines that contain
"%SW_MATM-4-MACFLAP_NOTIF", and then get the MAC address, VLAN and interfaces
from each such line.
In this case, the regular expression not only describes the string, but
also indicates all parts of the string to be returned in parentheses.

Python has two options for using groups:

* Numbered groups
* Named groups

Numbered groups
~~~~~~~~~~~~~~~~~~~

Group is defined by placing expression in parentheses ``()``.

Inside expression, group are numbered from left to right starting
with 1. Groups can then be selected by numbers to get text that
corresponds to group expression.

Example of groups use:

.. code:: python

    In [8]: line = "FastEthernet0/1      10.0.12.1   YES manual up          up"
    In [9]: match = re.search('(\S+)\s+([\w.]+)\s+.*', line)

In this example, two groups are specified:

-  first group - any characters other than whitespaces
-  second group - any letter or digit (symbol ``\w``) or dot

The second group could be described as the first. Other version is just for example.

You can now access a group by number. Group 0 is a string that corresponds to the entire match:

.. code:: python

    In [10]: match.group(0)
    Out[10]: 'FastEthernet0/1            10.0.12.1       YES manual up                    up'

    In [11]: match.group(1)
    Out[11]: 'FastEthernet0/1'

    In [12]: match.group(2)
    Out[12]: '10.0.12.1'

If necessary, you can list several group numbers:

.. code:: python

    In [13]: match.group(1, 2)
    Out[13]: ('FastEthernet0/1', '10.0.12.1')

    In [14]: match.group(2, 1, 2)
    Out[14]: ('10.0.12.1', 'FastEthernet0/1', '10.0.12.1')

Starting with Python 3.6, groups can be accessed as follows:

.. code:: python

    In [15]: match[0]
    Out[15]: 'FastEthernet0/1            10.0.12.1       YES manual up                    up'

    In [16]: match[1]
    Out[16]: 'FastEthernet0/1'

    In [17]: match[2]
    Out[17]: '10.0.12.1'

Method ``groups`` is used to display all substrings that correspond to groups:

.. code:: python

    In [18]: match.groups()
    Out[18]: ('FastEthernet0/1', '10.0.12.1')

Named groups
~~~~~~~~~~~~~~~~~~

When expression is complex, it is not very convenient to determine number of group.
Plus, when you modify an expression the order of groups can be changed and you
will need to change the code that refers to groups.

Named groups allow you to give a name to the group.
Syntax of named group ``(?P<name>regex)``:

.. code:: python

    In [19]: line = "FastEthernet0/1            10.0.12.1       YES manual up                    up"

    In [20]: match = re.search('(?P<intf>\S+)\s+(?P<address>\S+)\s+', line)

These groups can now be accessed by name:

.. code:: python

    In [21]: match.group('intf')
    Out[21]: 'FastEthernet0/1'

    In [22]: match.group('address')
    Out[22]: '10.0.12.1'

It is also very useful that with ``groupdict`` method you can get a dictionary
where keys are the names of groups and values are the substrings that correspond to them:

.. code:: python

    In [23]: match.groupdict()
    Out[23]: {'address': '10.0.12.1', 'intf': 'FastEthernet0/1'}

And then you can add groups to regular expression and rely on their name instead of order:

.. code:: python

    In [24]: match = re.search('(?P<intf>\S+)\s+(?P<address>\S+)\s+\w+\s+\w+\s+(?P<status>up|down)\s+(?P<protocol>up|down)', line)

    In [25]: match.groupdict()
    Out[25]:
    {'address': '10.0.12.1',
     'intf': 'FastEthernet0/1',
     'protocol': 'up',
     'status': 'up'}

