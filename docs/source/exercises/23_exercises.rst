.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./pytest.rst

Task 23.1
~~~~~~~~~~~~

In this task, you must create an IPAddress class.

When creating an instance of a class, the IP address and mask are passed
as an argument, and the correctness of the address and mask must be checked:

* The address is considered to be correctly specified if it:

    * consists of 4 numbers separated by a dot
    * every number in the range from 0 to 255

* the mask is considered correct if the mask is a number and a number in
  the range from 8 to 32 inclusiveA


If the mask or address fails validation, you must raise a ValueError
with the appropriate text (output below).

Also, when creating a class, two instance variables must be created: ip and mask,
which contain the address and mask, respectively.

An example of creating an instance of a class:

.. code:: python

    In [1]: ip1 = IPAddress('10.1.1.1/24')

ip and mask attributes

.. code:: python

    In [2]: ip1 = IPAddress('10.1.1.1/24')

    In [3]: ip1.ip
    Out[3]: '10.1.1.1'

    In [4]: ip1.mask
    Out[4]: 24

Checking the correctness of the address (traceback is shortened)

.. code:: python

    In [5]: ip1 = IPAddress('10.1.1/24')
    ---------------------------------------------------------------------------
    ...
    ValueError: Incorrect IPv4 address

    Checking the correctness of the mask (traceback is shortened)
    In [6]: ip1 = IPAddress('10.1.1.1/240')
    ---------------------------------------------------------------------------
    ...
    ValueError: Incorrect mask


Task 23.1a
~~~~~~~~~~~~~

Copy and modify the IPAddress class from task 23.1.

Add two string views for instances of the IPAddress class.
How string representations should look like should be determined from
the output below.

An example of creating an instance of a class:

.. code:: python

    In [5]: ip1 = IPAddress('10.1.1.1/24')

    In [6]: str(ip1)
    Out[6]: 'IP address 10.1.1.1/24'

    In [7]: print(ip1)
    IP address 10.1.1.1/24

    In [8]: ip1
    Out[8]: IPAddress('10.1.1.1/24')

    In [9]: ip_list = []

    In [10]: ip_list.append(ip1)

    In [11]: ip_list
    Out[11]: [IPAddress('10.1.1.1/24')]

    In [12]: print(ip_list)
    [IPAddress('10.1.1.1/24')]


Task 23.2
~~~~~~~~~~~~

Copy the CiscoTelnet class from any 22.2x task and add context manager
support to the class. When exiting the context manager block, the connection
should be closed.

Example of work:

.. code:: python

    In [14]: r1_params = {
        ...:     'ip': '192.168.100.1',
        ...:     'username': 'cisco',
        ...:     'password': 'cisco',
        ...:     'secret': 'cisco'}

    In [15]: from task_23_2 import CiscoTelnet

    In [16]: with CiscoTelnet(**r1_params) as r1:
        ...:     print(r1.send_show_command('sh clock'))
        ...:
    sh clock
    *19:17:20.244 UTC Sat Apr 6 2019
    R1#


Task 23.3
~~~~~~~~~~~~

Copy and modify the Topology class from job 22.1x.

In this task, you need to add a method that will allow you to add two instances
of the Topology class. The addition should return a new instance of the Topology
class.

Creating two instances of the Topology class:

.. code:: python

    In [1]: t1 = Topology(topology_example)

    In [2]: t1.topology
    Out[2]:
    {('R1', 'Eth0/0'): ('SW1', 'Eth0/1'),
     ('R2', 'Eth0/0'): ('SW1', 'Eth0/2'),
     ('R2', 'Eth0/1'): ('SW2', 'Eth0/11'),
     ('R3', 'Eth0/0'): ('SW1', 'Eth0/3'),
     ('R3', 'Eth0/1'): ('R4', 'Eth0/0'),
     ('R3', 'Eth0/2'): ('R5', 'Eth0/0')}

    In [3]: topology_example2 = {('R1', 'Eth0/4'): ('R7', 'Eth0/0'),
                                 ('R1', 'Eth0/6'): ('R9', 'Eth0/0')}

    In [4]: t2 = Topology(topology_example2)

    In [5]: t2.topology
    Out[5]: {('R1', 'Eth0/4'): ('R7', 'Eth0/0'), ('R1', 'Eth0/6'): ('R9', 'Eth0/0')}

Summing instances of the Topology class:

.. code:: python

    In [6]: t3 = t1 + t2

    In [7]: t3.topology
    Out[7]:
    {('R1', 'Eth0/0'): ('SW1', 'Eth0/1'),
     ('R1', 'Eth0/4'): ('R7', 'Eth0/0'),
     ('R1', 'Eth0/6'): ('R9', 'Eth0/0'),
     ('R2', 'Eth0/0'): ('SW1', 'Eth0/2'),
     ('R2', 'Eth0/1'): ('SW2', 'Eth0/11'),
     ('R3', 'Eth0/0'): ('SW1', 'Eth0/3'),
     ('R3', 'Eth0/1'): ('R4', 'Eth0/0'),
     ('R3', 'Eth0/2'): ('R5', 'Eth0/0')}

Checking that the original instances haven't changed:

.. code:: python

    In [9]: t1.topology
    Out[9]:
    {('R1', 'Eth0/0'): ('SW1', 'Eth0/1'),
     ('R2', 'Eth0/0'): ('SW1', 'Eth0/2'),
     ('R2', 'Eth0/1'): ('SW2', 'Eth0/11'),
     ('R3', 'Eth0/0'): ('SW1', 'Eth0/3'),
     ('R3', 'Eth0/1'): ('R4', 'Eth0/0'),
     ('R3', 'Eth0/2'): ('R5', 'Eth0/0')}

    In [10]: t2.topology
    Out[10]: {('R1', 'Eth0/4'): ('R7', 'Eth0/0'), ('R1', 'Eth0/6'): ('R9', 'Eth0/0')}


Task 23.3a
~~~~~~~~~~~~~

In this task, you need to make sure that instances of the Topology class
are iterables. The base of the Topology class can be taken from either
task 22.1x or task 23.3.

After creating an instance of a class, the instance should act like
an iterable object. Each iteration should return a tuple that describes
one connection. The order of output of connections can be any.


An example of how the class works:

.. code:: python

    In [1]: top = Topology(topology_example)

    In [2]: for link in top:
       ...:     print(link)
       ...:
    (('R1', 'Eth0/0'), ('SW1', 'Eth0/1'))
    (('R2', 'Eth0/0'), ('SW1', 'Eth0/2'))
    (('R2', 'Eth0/1'), ('SW2', 'Eth0/11'))
    (('R3', 'Eth0/0'), ('SW1', 'Eth0/3'))
    (('R3', 'Eth0/1'), ('R4', 'Eth0/0'))
    (('R3', 'Eth0/2'), ('R5', 'Eth0/0'))

