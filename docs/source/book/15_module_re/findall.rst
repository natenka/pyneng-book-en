Findall function
----------------

Function ``findall()``: 

* is used to search for all disjoint matches in template
* returns:

  * list of strings that are described by the regular expression if there are no groups in regular expression
  * list of strings that match with the regular expression in the group if there is only one group in regular expression 
  * list of tuples containing strings that matches with the expression in the group if there are more than one group

Consider the work of findall() with an example of ‘sh mac address-table output’:

.. code:: python

    In [2]: mac_address_table = open('CAM_table.txt').read()

    In [3]: print(mac_address_table)
    sw1#sh mac address-table
              Mac Address Table
    -------------------------------------------

    Vlan    Mac Address       Type        Ports
    ----    -----------       --------    -----
     100    a1b2.ac10.7000    DYNAMIC     Gi0/1
     200    a0d4.cb20.7000    DYNAMIC     Gi0/2
     300    acb4.cd30.7000    DYNAMIC     Gi0/3
     100    a2bb.ec40.7000    DYNAMIC     Gi0/4
     500    aa4b.c550.7000    DYNAMIC     Gi0/5
     200    a1bb.1c60.7000    DYNAMIC     Gi0/6
     300    aa0b.cc70.7000    DYNAMIC     Gi0/7

The first example is a regular expression without groups. In this case findall() returns a list of strings that matches with regular expression.

For example, with findall() you can get a list of  matching strings with *vlan - mac – interface* and get rid of header in the output of command:

.. code:: python

    In [4]: re.findall(r'\d+ +\S+ +\w+ +\S+', mac_address_table)
    Out[4]:
    ['100    a1b2.ac10.7000    DYNAMIC     Gi0/1',
     '200    a0d4.cb20.7000    DYNAMIC     Gi0/2',
     '300    acb4.cd30.7000    DYNAMIC     Gi0/3',
     '100    a2bb.ec40.7000    DYNAMIC     Gi0/4',
     '500    aa4b.c550.7000    DYNAMIC     Gi0/5',
     '200    a1bb.1c60.7000    DYNAMIC     Gi0/6',
     '300    aa0b.cc70.7000    DYNAMIC     Gi0/7']

Note that findall() returns a list of strings, not a Match object.

As soon as a group appears in regular expression, findall() behaves differently. If one group is used in the expression, findall() returns a list of strings that matches with expression in the group:

.. code:: python

    In [5]: re.findall(r'\d+ +(\S+) +\w+ +\S+', mac_address_table)
    Out[5]:
    ['a1b2.ac10.7000',
     'a0d4.cb20.7000',
     'acb4.cd30.7000',
     'a2bb.ec40.7000',
     'aa4b.c550.7000',
     'a1bb.1c60.7000',
     'aa0b.cc70.7000']

findall() searches for a match of the entire string but returns a result similar to the group() method in Match object.

If there are several groups, findall() will return the list of tuples:

.. code:: python

    In [6]: re.findall(r'(\d+) +(\S+) +\w+ +(\S+)', mac_address_table)
    Out[6]:
    [('100', 'a1b2.ac10.7000', 'Gi0/1'),
     ('200', 'a0d4.cb20.7000', 'Gi0/2'),
     ('300', 'acb4.cd30.7000', 'Gi0/3'),
     ('100', 'a2bb.ec40.7000', 'Gi0/4'),
     ('500', 'aa4b.c550.7000', 'Gi0/5'),
     ('200', 'a1bb.1c60.7000', 'Gi0/6'),
     ('300', 'aa0b.cc70.7000', 'Gi0/7')]

If such features of findall() function prevent you from getting the desired result, it is better to use finditer() function, but sometimes this behavior is appropriate and convenient to use.

An example of using findall() in a log file parsing (parse_log_findall.py file):

.. code:: python

    import re

    regex = (r'Host \S+ '
             r'in vlan (\d+) '
             r'is flapping between port '
             r'(\S+) and port (\S+)')

    ports = set()

    with open('log.txt') as f:
        result = re.findall(regex, f.read())
        for vlan, port1, port2 in result:
            ports.add(port1)
            ports.add(port2)

    print('Loop between ports {} в VLAN {}'.format(', '.join(ports), vlan))

The result is:

::

    $ python parse_log_findall.py
    Loop between ports Gi0/19, Gi0/16, Gi0/24 в VLAN 10

