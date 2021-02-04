Repeating the captured result
----------------------------------

When working with groups, you can use the result that matched with the group,
further in the same expression.

For example, in the output of 'sh ip bgp' the last column describes AS Path
attribute (through which autonomous systems the route passed):

.. code:: python

    In [1]: bgp = '''
       ...: R9# sh ip bgp | be Network
       ...:    Network          Next Hop       Metric LocPrf Weight Path
       ...: *  192.168.66.0/24  192.168.79.7                       0 500 500 500 i
       ...: *>                  192.168.89.8                       0 800 700 i
       ...: *  192.168.67.0/24  192.168.79.7         0             0 700 700 700 i
       ...: *>                  192.168.89.8                       0 800 700 i
       ...: *  192.168.88.0/24  192.168.79.7                       0 700 700 700 i
       ...: *>                  192.168.89.8         0             0 800 800 i
       ...: '''

Suppose you get those prefixes where the same AS number repeats several times
in the path.

This can be done by reference to a result that has been captured by the group.
For example, such an expression displays all lines in which the same number is
repeated at least twice:

.. code:: python

    In [2]: for line in bgp.split('\n'):
       ...:     match = re.search(r'(\d+) \1', line)
       ...:     if match:
       ...:         print(line)
       ...:
    *  192.168.66.0/24  192.168.79.7                       0 500 500 500 i
    *  192.168.67.0/24  192.168.79.7         0             0 700 700 700 i
    *  192.168.88.0/24  192.168.79.7                       0 700 700 700 i
    *>                  192.168.89.8         0             0 800 800 i

In this expression, ``\1`` denotes the result that falls into the group.
Number one indicates a specific group. In this case, it's Group 1, it's only one group here.

Similarly, you can describe strings where the same number occurs three times:

.. code:: python

    In [3]: for line in bgp.split('\n'):
       ...:     match = re.search(r'(\d+) \1 \1', line)
       ...:     if match:
       ...:         print(line)
       ...:
    *  192.168.66.0/24  192.168.79.7                       0 500 500 500 i
    *  192.168.67.0/24  192.168.79.7         0             0 700 700 700 i
    *  192.168.88.0/24  192.168.79.7                       0 700 700 700 i

You can reffer to the result which was captured by named group:

.. code:: python

    In [129]: for line in bgp.split('\n'):
         ...:     match = re.search('(?P<as>\d+) (?P=as)', line)
         ...:     if match:
         ...:         print(line)
         ...:
    *  192.168.66.0/24   192.168.79.7                       0 500 500 500 i
    *  192.168.67.0/24   192.168.79.7         0             0 700 700 700 i
    *  192.168.88.0/24   192.168.79.7                       0 700 700 700 i
    *>                   192.168.89.8         0             0 800 800 i

