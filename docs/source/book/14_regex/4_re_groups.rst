Expressions grouping
---------------------

Expressions grouping indicates that the sequence of symbols should be considered as a one. However, this is not the only advantage of grouping.

In addition, by use of groups you can get only a certain portion of the string that has been described by the expression.

For example, from a log file you should select strings in which "%SW_MATM-4-MACFLAP_NOTIF" meets and then from each such string get MAC address, VLAN and interfaces. In this case, the regular expression simply has to describe the string and all the parts of the string to be obtained are simply placed in brackets.

Python has two options for using groups:

* Numbered groups
* Named groups

Numbered groups
~~~~~~~~~~~~~~~~~~~

The group is defined by placing the expression in brackets ``()``.

Inside the expression, group are numbered from left to right starting with 1. Groups can then be approached by numbers and receive text that corresponds to the group expression.

Example of groups use:

.. code:: python

    In [8]: line = "FastEthernet0/1            10.0.12.1       YES manual up                    up"
    In [9]: match = re.search('(\S+)\s+([\w.]+)\s+.*', line)

In this example, two groups are specified:

-  the first group - any characters other than whitespaces
-  the second group - any letter or digit (symbol ``\w``) or dot

The second group could be described as the first. The other version is just for example.

You can now access the group by number. Group 0 is a string that corresponds to the entire template:

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

Method groups() is used to display all substrings that correspond to the specified groups:

.. code:: python

    In [18]: match.groups()
    Out[18]: ('FastEthernet0/1', '10.0.12.1')

Named groups
~~~~~~~~~~~~~~~~~~

When the expression is complex, it is not very convenient to determine the number of the group. Plus, when you modify an expression the order of groups can be changed and you will need to change the code that refers to the groups.

The named groups allow you to give a name to the group.

Syntax of the named group ``(?P<name>regex)``:

.. code:: python

    In [19]: line = "FastEthernet0/1            10.0.12.1       YES manual up                    up"

    In [20]: match = re.search('(?P<intf>\S+)\s+(?P<address>[\d.]+)\s+', line)

These groups can now be accessed by name:

.. code:: python

    In [21]: match.group('intf')
    Out[21]: 'FastEthernet0/1'

    In [22]: match.group('address')
    Out[22]: '10.0.12.1'

It is also very useful that with the groupdict() method you can get a dictionary where the keys are the names of groups and the values are the substrings that correspond to them:

.. code:: python

    In [23]: match.groupdict()
    Out[23]: {'address': '10.0.12.1', 'intf': 'FastEthernet0/1'}

And then you can add groups to the regular expression and rely on their name instead of order:

.. code:: python

    In [24]: match = re.search('(?P<intf>\S+)\s+(?P<address>[\d\.]+)\s+\w+\s+\w+\s+(?P<status>up|down|administratively down)\s+(?P<protocol>up|down)', line)

    In [25]: match.groupdict()
    Out[25]:
    {'address': '10.0.12.1',
     'intf': 'FastEthernet0/1',
     'protocol': 'up',
     'status': 'up'}

