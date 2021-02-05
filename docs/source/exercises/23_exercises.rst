.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./pytest.rst

Task 23.1
~~~~~~~~~~~~

In this task you need to create an IPaddress class.

When creating class instance, IP address and mask are passed as an argument and the correctness of address and mask should be checked:

Address is considered correct if it:

  * consists of 4 numbers separated by a point
  * each number in range 0 to 255

Mask is considered correct if it is between 8 and 32 inclusive

If mask or address didn't pass verification, you should generate ValueError exception with appropriate text (output below).

Also, when creating a class, two instance variables should be created: *ip* and *mask* which contain address and mask, respectively.

Example of class instance creation:

.. code:: python

    In [1]: ip = IPAddress('10.1.1.1/24')

    Атрибуты ip и mask
    In [2]: ip1 = IPAddress('10.1.1.1/24')

    In [3]: ip1.ip
    Out[3]: '10.1.1.1'

    In [4]: ip1.mask
    Out[4]: 24

Address correctness check (traceback omitted)

.. code:: python
    In [5]: ip1 = IPAddress('10.1.1/24')
    ---------------------------------------------------------------------------
    ...
    ValueError: Incorrect IPv4 address

Address correctness check (traceback omitted)

.. code:: python

    In [6]: ip1 = IPAddress('10.1.1.1/240')
    ---------------------------------------------------------------------------
    ...
    ValueError: Incorrect mask


Task 23.1a
~~~~~~~~~~~~~

Copy and change IPaddress class from task 23.1.

Add two string views for IPaddress class instances. What line views should look like is defined in the following output:

Instance creation

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

Add to CiscoTelnet class from task 22.2x support for work in context manager. When leaving context manager block, connection should be closed.

Example:

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

    In [17]: with CiscoTelnet(**r1_params) as r1:
        ...:     print(r1.send_show_command('sh clock'))
        ...:     raise ValueError('Error occurred')
        ...:
    sh clock
    *19:17:38.828 UTC Sat Apr 6 2019
    R1#
    ---------------------------------------------------------------------------
    ValueError                                Traceback (most recent call last)
    <ipython-input-17-f3141be7c129> in <module>
          1 with CiscoTelnet(**r1_params) as r1:
          2     print(r1.send_show_command('sh clock'))
    ----> 3     raise ValueError('Error occurred')
          4

    ValueError: Возникла ошибка


Task 23.3
~~~~~~~~~~~~

Copy and change Topology class from task 22.1x.

Add method that allows you to perform addition of two instances of Topology class. As a result of addition, new instance of Topology class should be returned.

Creation of two topologies:

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

Topology summation:

.. code:: python

    In [6]: t3 = t1+t2

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

Check that original topologies have not changed

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

In this task, make sure that Topology class instances are iterable objects. The base of Topology class can be taken from any task 22.1x or task 23.3.

After class instance creation, instance should work as an iterable object. After each iteration, tuple that describes a single connection should return.

Example of class run:

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


Check class run.
