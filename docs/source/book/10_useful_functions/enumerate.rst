enumerate
---------

Sometimes, when iterating objects in **for** loop, it is necessary
not only to get object itself but also its sequence number. This
can be done by creating an additional variable that will increase by
one with each iteration. However, it is much more convenient to do this with iterator ``enumerate``.

Basic example:

.. code:: python

    In [15]: list1 = ['str1', 'str2', 'str3']

    In [16]: for position, string in enumerate(list1):
        ...:     print(position, string)
        ...:
    0 str1
    1 str2
    2 str3

``enumerate`` can count not only from scratch but from any value that has been given to it after object:

.. code:: python

    In [17]: list1 = ['str1', 'str2', 'str3']

    In [18]: for position, string in enumerate(list1, 100):
        ...:     print(position, string)
        ...:
    100 str1
    101 str2
    102 str3

Sometimes it is necessary to check what iterator has generated. If you want to see full content that iterator generates you can use list() function:

.. code:: python

    In [19]: list1 = ['str1', 'str2', 'str3']

    In [20]: list(enumerate(list1, 100))
    Out[20]: [(100, 'str1'), (101, 'str2'), (102, 'str3')]

An example of using enumerate for EEM
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This example uses Cisco `EEM <http://xgu.ru/wiki/EEM>`__. In a nutshell, EEM allows you to perform some actions in response to an event.

EEM applet looks like this:

.. code:: python

    event manager applet Fa0/1_no_shut
     event syslog pattern "Line protocol on Interface FastEthernet0/0, changed state to down"
     action 1 cli command "enable"
     action 2 cli command "conf t"
     action 3 cli command "interface fa0/1"
     action 4 cli command "no sh"

In EEM, in a situation where many actions need to be performed it is inconvenient to type  ``action x cli command`` each time. Plus, most often, there is already a ready piece of configuration that must be executed by EEM.

A simple Python script can generate EEM commands based on existing command list (enumerate_eem.py file):

.. code:: python

    import sys

    config = sys.argv[1]

    with open(config, 'r') as f:
        for i, command in enumerate(f, 1):
            print('action {:04} cli command "{}"'.format(i, command.rstrip()))

In this example, commands are read from a file and then EEM prefix is added to each line.

File with commands looks like this (r1_config.txt):

.. code:: python

    en
    conf t
    no int Gi0/0/0.300
    no int Gi0/0/0.301
    no int Gi0/0/0.302
    int range gi0/0/0-2
     channel-group 1 mode active
    interface Port-channel1.300
     encapsulation dot1Q 300
     vrf forwarding Management
     ip address 10.16.19.35 255.255.255.248

The output is:

.. code:: python

    $ python enumerate_eem.py r1_config.txt
    action 0001 cli command "en"
    action 0002 cli command "conf t"
    action 0003 cli command "no int Gi0/0/0.300"
    action 0004 cli command "no int Gi0/0/0.301"
    action 0005 cli command "no int Gi0/0/0.302"
    action 0006 cli command "int range gi0/0/0-2"
    action 0007 cli command " channel-group 1 mode active"
    action 0008 cli command "interface Port-channel1.300"
    action 0009 cli command " encapsulation dot1Q 300"
    action 0010 cli command " vrf forwarding Management"
    action 0011 cli command " ip address 10.16.19.35 255.255.255.248"

