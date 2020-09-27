.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./pytest.rst


Task 22.1
~~~~~~~~~~~~

Create Topology class that represents network topology.

When creating an instance of class, dictionary that describes topology is passed as an argument. Dictionary may contain duplicate connections.

Duplicate refers to situation where dictionary contains such couples:

::

    ('R1', 'Eth0/0'): ('SW1', 'Eth0/1') и ('SW1', 'Eth0/1'): ('R1', 'Eth0/0')

Each instance should have *topology* variable that contains topology dictionary, but without duplicates.

Example of class instance creation:

.. code:: python

    In [2]: top = Topology(topology_example)

After that, *topology* variable should be available:

.. code:: python

    In [3]: top.topology
    Out[3]:
    {('R1', 'Eth0/0'): ('SW1', 'Eth0/1'),
     ('R2', 'Eth0/0'): ('SW1', 'Eth0/2'),
     ('R2', 'Eth0/1'): ('SW2', 'Eth0/11'),
     ('R3', 'Eth0/0'): ('SW1', 'Eth0/3'),
     ('R3', 'Eth0/1'): ('R4', 'Eth0/0'),
     ('R3', 'Eth0/2'): ('R5', 'Eth0/0')}


.. code:: python

    topology_example = {('R1', 'Eth0/0'): ('SW1', 'Eth0/1'),
                        ('R2', 'Eth0/0'): ('SW1', 'Eth0/2'),
                        ('R2', 'Eth0/1'): ('SW2', 'Eth0/11'),
                        ('R3', 'Eth0/0'): ('SW1', 'Eth0/3'),
                        ('R3', 'Eth0/1'): ('R4', 'Eth0/0'),
                        ('R3', 'Eth0/2'): ('R5', 'Eth0/0'),
                        ('SW1', 'Eth0/1'): ('R1', 'Eth0/0'),
                        ('SW1', 'Eth0/2'): ('R2', 'Eth0/0'),
                        ('SW1', 'Eth0/3'): ('R3', 'Eth0/0')}



Task 22.1a
~~~~~~~~~~~~~

Copy Topology class from task 22.1 and change it.

If in task 22.1 duplicates removal was performed in __init__() method, it is necessary to transfer function of duplicates removal to _normalize() method.

The __init__ method has to look like this:

.. code:: python

    class Topology:
        def __init__(self, topology_dict):
            self.topology = self._normalize(topology_dict)


Task 22.1b
~~~~~~~~~~~~~

Change Topology class from task 22.1a or 22.1.

Add delete_link() method that removes specified connection. Method should also remove mirror connection if it exists (example below).

If there is no such connection, message "There is no such connection" is displayed.

Topology creation:

.. code:: python

    In [7]: t = Topology(topology_example)

    In [8]: t.topology
    Out[8]:
    {('R1', 'Eth0/0'): ('SW1', 'Eth0/1'),
     ('R2', 'Eth0/0'): ('SW1', 'Eth0/2'),
     ('R2', 'Eth0/1'): ('SW2', 'Eth0/11'),
     ('R3', 'Eth0/0'): ('SW1', 'Eth0/3'),
     ('R3', 'Eth0/1'): ('R4', 'Eth0/0'),
     ('R3', 'Eth0/2'): ('R5', 'Eth0/0')}

Link removal:

.. code:: python

    In [9]: t.delete_link(('R3', 'Eth0/1'), ('R4', 'Eth0/0'))

    In [10]: t.topology
    Out[10]:
    {('R1', 'Eth0/0'): ('SW1', 'Eth0/1'),
     ('R2', 'Eth0/0'): ('SW1', 'Eth0/2'),
     ('R2', 'Eth0/1'): ('SW2', 'Eth0/11'),
     ('R3', 'Eth0/0'): ('SW1', 'Eth0/3'),
     ('R3', 'Eth0/2'): ('R5', 'Eth0/0')}

Removal of mirror connection: dictionary has an entry ('R3', 'Eth0/2'): ('R5', 'Eth0/0'), but call of delete_link() with key and value in reverse order should remove connection:

.. code:: python

    In [11]: t.delete_link(('R5', 'Eth0/0'), ('R3', 'Eth0/2'))

    In [12]: t.topology
    Out[12]:
    {('R1', 'Eth0/0'): ('SW1', 'Eth0/1'),
     ('R2', 'Eth0/0'): ('SW1', 'Eth0/2'),
     ('R2', 'Eth0/1'): ('SW2', 'Eth0/11'),
     ('R3', 'Eth0/0'): ('SW1', 'Eth0/3')}

If there is no such connection, such message is displayed:

.. code:: python

    In [13]: t.delete_link(('R5', 'Eth0/0'), ('R3', 'Eth0/2'))
    There is no such connection

Task 22.1c
~~~~~~~~~~~~~

Change Topology class from task 22.1b.

Add delete_node() method that removes all connections with specified device. If there is no such device, message "There is no such device" is displayed.

Topology creation:

.. code:: python

    In [1]: t = Topology(topology_example)

    In [2]: t.topology
    Out[2]:
    {('R1', 'Eth0/0'): ('SW1', 'Eth0/1'),
     ('R2', 'Eth0/0'): ('SW1', 'Eth0/2'),
     ('R2', 'Eth0/1'): ('SW2', 'Eth0/11'),
     ('R3', 'Eth0/0'): ('SW1', 'Eth0/3'),
     ('R3', 'Eth0/1'): ('R4', 'Eth0/0'),
     ('R3', 'Eth0/2'): ('R5', 'Eth0/0')}

Device removal:

.. code:: python

    In [3]: t.delete_node('SW1')

    In [4]: t.topology
    Out[4]:
    {('R2', 'Eth0/1'): ('SW2', 'Eth0/11'),
     ('R3', 'Eth0/1'): ('R4', 'Eth0/0'),
     ('R3', 'Eth0/2'): ('R5', 'Eth0/0')}

If there is no such device, such message is displayed:

.. code:: python

    In [5]: t.delete_node('SW1')
    There is no such device

Task 22.1d
~~~~~~~~~~~~~

Change Topology class from task 22.1c

Add add_link() method that adds specified connection if it is not already in topology. If connection exists, display message "Such connection already exists".
If one of sides is in topology, display message "Connection with one of ports exists".


Example of creating a topology and adding connections

.. code:: python

    In [7]: t = Topology(topology_example)

    In [8]: t.topology
    Out[8]:
    {('R1', 'Eth0/0'): ('SW1', 'Eth0/1'),
     ('R2', 'Eth0/0'): ('SW1', 'Eth0/2'),
     ('R2', 'Eth0/1'): ('SW2', 'Eth0/11'),
     ('R3', 'Eth0/0'): ('SW1', 'Eth0/3'),
     ('R3', 'Eth0/1'): ('R4', 'Eth0/0'),
     ('R3', 'Eth0/2'): ('R5', 'Eth0/0')}

    In [9]: t.add_link(('R1', 'Eth0/4'), ('R7', 'Eth0/0'))

    In [10]: t.topology
    Out[10]:
    {('R1', 'Eth0/0'): ('SW1', 'Eth0/1'),
     ('R1', 'Eth0/4'): ('R7', 'Eth0/0'),
     ('R2', 'Eth0/0'): ('SW1', 'Eth0/2'),
     ('R2', 'Eth0/1'): ('SW2', 'Eth0/11'),
     ('R3', 'Eth0/0'): ('SW1', 'Eth0/3'),
     ('R3', 'Eth0/1'): ('R4', 'Eth0/0'),
     ('R3', 'Eth0/2'): ('R5', 'Eth0/0')}

    In [11]: t.add_link(('R1', 'Eth0/4'), ('R7', 'Eth0/0'))
    Such connection already exists

    In [12]: t.add_link(('R1', 'Eth0/4'), ('R7', 'Eth0/5'))
    Connection with one of ports exists


Task 22.2
~~~~~~~~~~~~

Create CiscoTelnet class that connects via Telnet to Cisco equipment.

When creating class instance, Telnet connection should be created as well as switching to enable mode. Class should use telnetlib module to connect via Telnet.

CiscoTelnet class, besides __init__(), should have at least two methods:

* _write_line() - takes as argument a string and sends to equipment a string converted to bytes and adds a line feed at the end. Method _write_line() should be used within class.
* send_show_command() - takes show command as argument and returns output received from device

Parameter of __init__() method:

* ip - IP address
* username - username
* password - password
* secret - enable password 


Example of creating class instance:

.. code:: python

    In [2]: from task_22_2 import CiscoTelnet

    In [3]: r1_params = {
       ...:     'ip': '192.168.100.1',
       ...:     'username': 'cisco',
       ...:     'password': 'cisco',
       ...:     'secret': 'cisco'}
       ...:

    In [4]: r1 = CiscoTelnet(**r1_params)

    In [5]: r1.send_show_command('sh ip int br')
    Out[5]: 'sh ip int br\r\nInterface                  IP-Address      OK? Method Status                Protocol\r\nEthernet0/0                192.168.100.1   YES NVRAM  up                    up      \r\nEthernet0/1                192.168.200.1   YES NVRAM  up                    up      \r\nEthernet0/2                190.16.200.1    YES NVRAM  up                    up      \r\nEthernet0/3                192.168.130.1   YES NVRAM  up                    up      \r\nEthernet0/3.100            10.100.0.1      YES NVRAM  up                    up      \r\nEthernet0/3.200            10.200.0.1      YES NVRAM  up                    up      \r\nEthernet0/3.300            10.30.0.1       YES NVRAM  up                    up      \r\nLoopback0                  10.1.1.1        YES NVRAM  up                    up      \r\nLoopback55                 5.5.5.5         YES manual up                    up      \r\nR1#'

.. note::

    Tip:
    Мethod _write_line() is needed to shorten line ``self.telnet.write(line.encode("ascii") + b"\n")`` to such line: ``self._write_line(line)``.
    It should not do anything else.


Task 22.2a
~~~~~~~~~~~~~

Copy CiscoTelnet class from task 22.2 and change send_show_command() method by adding three parameters:

* parse - controls whether the usual command output or list of dictionaries received after processing with Textfsm will be returned. If parse=True, list of dictionaries should be returned and if parse=False, usual output should be returned. Default value is True.
* templates - path to template directory. Default value - "templates"
* index - name of file where mapping between commands and templates is stored. Default value - "index"


Example of class instance creation:

.. code:: python

    In [1]: r1_params = {
       ...:     'ip': '192.168.100.1',
       ...:     'username': 'cisco',
       ...:     'password': 'cisco',
       ...:     'secret': 'cisco'}

    In [2]: from task_22_2a import CiscoTelnet

    In [3]: r1 = CiscoTelnet(**r1_params)

Use of send_show_command() method:

.. code:: python

    In [4]: r1.send_show_command('sh ip int br', parse=False)
    Out[4]: 'sh ip int br\r\nInterface                  IP-Address      OK? Method Status                Protocol\r\nEthernet0/0                192.168.100.1   YES NVRAM  up                    up      \r\nEthernet0/1                192.168.200.1   YES NVRAM  up                    up      \r\nEthernet0/2                190.16.200.1    YES NVRAM  up                    up      \r\nEthernet0/3                192.168.130.1   YES NVRAM  up                    up      \r\nEthernet0/3.100            10.100.0.1      YES NVRAM  up                    up      \r\nEthernet0/3.200            10.200.0.1      YES NVRAM  up                    up      \r\nEthernet0/3.300            10.30.0.1       YES NVRAM  up                    up      \r\nLoopback0                  10.1.1.1        YES NVRAM  up                    up      \r\nLoopback55                 5.5.5.5         YES manual up                    up      \r\nR1#'

    In [5]: r1.send_show_command('sh ip int br', parse=True)
    Out[5]:
    [{'intf': 'Ethernet0/0',
      'address': '192.168.100.1',
      'status': 'up',
      'protocol': 'up'},
     {'intf': 'Ethernet0/1',
      'address': '192.168.200.1',
      'status': 'up',
      'protocol': 'up'},
     {'intf': 'Ethernet0/2',
      'address': '190.16.200.1',
      'status': 'up',
      'protocol': 'up'},
     {'intf': 'Ethernet0/3',
      'address': '192.168.130.1',
      'status': 'up',
      'protocol': 'up'},
     {'intf': 'Ethernet0/3.100',
      'address': '10.100.0.1',
      'status': 'up',
      'protocol': 'up'},
     {'intf': 'Ethernet0/3.200',
      'address': '10.200.0.1',
      'status': 'up',
      'protocol': 'up'},
     {'intf': 'Ethernet0/3.300',
      'address': '10.30.0.1',
      'status': 'up',
      'protocol': 'up'},
     {'intf': 'Loopback0',
      'address': '10.1.1.1',
      'status': 'up',
      'protocol': 'up'},
     {'intf': 'Loopback55',
      'address': '5.5.5.5',
      'status': 'up',
      'protocol': 'up'}]

Task 22.2b
~~~~~~~~~~~~~

Copy CiscoTelnet class from task 22.2a and add send_config_commands() method.


Method send_config_commands() should be able to send one configuration mode command or list of commands. Method should return output similar to send_config_set() method of netmiko (example of output below).

Example of class instance creation:

.. code:: python

    In [1]: from task_22_2b import CiscoTelnet

    In [2]: r1_params = {
       ...:     'ip': '192.168.100.1',
       ...:     'username': 'cisco',
       ...:     'password': 'cisco',
       ...:     'secret': 'cisco'}

    In [3]: r1 = CiscoTelnet(**r1_params)

Use of send_config_commands() method:

.. code:: python

    In [5]: r1.send_config_commands('logging 10.1.1.1')
    Out[5]: 'conf t\r\nEnter configuration commands, one per line.  End with CNTL/Z.\r\nR1(config)#logging 10.1.1.1\r\nR1(config)#end\r\nR1#'

    In [6]: r1.send_config_commands(['interface loop55', 'ip address 5.5.5.5 255.255.255.255'])
    Out[6]: 'conf t\r\nEnter configuration commands, one per line.  End with CNTL/Z.\r\nR1(config)#interface loop55\r\nR1(config-if)#ip address 5.5.5.5 255.255.255.255\r\nR1(config-if)#end\r\nR1#'


Task 22.2c
~~~~~~~~~~~~~

Copy CiscoTelnet class from task 22.2b and change send_config_commands() method by adding error check.

Method send_config_commands() should have an additional parameter *strict*:

* strict=True means that if error is detected, it is necessary to generate ValueError exception
* strict=False means that if error is detected, all you have to do is to display error message

Method should return output similar to send_config_set() method of netmiko (example of output below). Exception and error text in example below.

Example of class instance creation:

.. code:: python

    In [1]: from task_22_2c import CiscoTelnet

    In [2]: r1_params = {
       ...:     'ip': '192.168.100.1',
       ...:     'username': 'cisco',
       ...:     'password': 'cisco',
       ...:     'secret': 'cisco'}

    In [3]: r1 = CiscoTelnet(**r1_params)

    In [4]: commands_with_errors = ['logging 0255.255.1', 'logging', 'i']
    In [5]: correct_commands = ['logging buffered 20010', 'ip http server']
    In [6]: commands = commands_with_errors+correct_commands

Use of send_config_commands() method:

.. code:: python

    In [7]: print(r1.send_config_commands(commands, strict=False))
    When executing command  "logging 0255.255.1" on device 192.168.100.1 error occurred -> Invalid input detected at '^' marker.
    When executing command "logging" on device 192.168.100.1 error occurred -> Incomplete command.
    When executing command "i" on device 192.168.100.1 error occurred -> Ambiguous command:  "i"
    conf t
    Enter configuration commands, one per line.  End with CNTL/Z.
    R1(config)#logging 0255.255.1
                       ^
    % Invalid input detected at '^' marker.

    R1(config)#logging
    % Incomplete command.

    R1(config)#i
    % Ambiguous command:  "i"
    R1(config)#logging buffered 20010
    R1(config)#ip http server
    R1(config)#end
    R1#

    In [8]: print(r1.send_config_commands(commands, strict=True))
    ---------------------------------------------------------------------------
    ValueError                                Traceback (most recent call last)
    <ipython-input-8-0abc1ed8602e> in <module>
    ----> 1 print(r1.send_config_commands(commands, strict=True))

    ...

    ValueError: When executing command "logging 0255.255.1" on device 192.168.100.1 error occurred -> Invalid input detected at '^' marker.

