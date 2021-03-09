.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./pytest.rst


Task 22.1
~~~~~~~~~~~~

Create a Topology class that represents the topology of the network.

When creating an instance of a class, a dictionary that describes the topology
is passed as an argument. The dictionary may contain "duplicate" connections.
"Duplicate" connections are a situation when there are two connections
in the dictionary:

.. code:: python

    ("R1", "Eth0/0"): ("SW1", "Eth0/1")
    ("SW1", "Eth0/1"): ("R1", "Eth0/0")

The task is to leave only one of these links in the final dictionary,
no matter which one.

In each instance, a topology instance variable must be created, which contains
the topology dictionary, but already without "duplicates". The topology instance
variable should contain a dict without "duplicates" immediately after instance
creation.

An example of creating an instance of a class:

.. code:: python

    In [2]: top = Topology(topology_example)

After that, the topology variable should be available:

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

Copy the Topology class from task 22.1 and modify it.

Transfer the functionality of removing "duplicates" to the _normalize method.
In this case, the __init__ method should look like this:

.. code:: python

    class Topology:
        def __init__(self, topology_dict):
            self.topology = self._normalize(topology_dict)


Task 22.1b
~~~~~~~~~~~~~

Copy the Topology class from either task 22.1a or 22.1 and modify it.

Add a delete_link method that deletes the specified connection.
The method should also remove the "reverse" connection, if any (an example
is given below).

If there is no such link, the message "There is no such link" should be printed.

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

Removing a link:

.. code:: python

    In [9]: t.delete_link(('R3', 'Eth0/1'), ('R4', 'Eth0/0'))

    In [10]: t.topology
    Out[10]:
    {('R1', 'Eth0/0'): ('SW1', 'Eth0/1'),
     ('R2', 'Eth0/0'): ('SW1', 'Eth0/2'),
     ('R2', 'Eth0/1'): ('SW2', 'Eth0/11'),
     ('R3', 'Eth0/0'): ('SW1', 'Eth0/3'),
     ('R3', 'Eth0/2'): ('R5', 'Eth0/0')}

Deleting the "reverse" link:
the dictionary contains an entry ``('R3', 'Eth0/2'): ('R5', 'Eth0/0')``, but calling
the delete_link method specifying the key and value in reverse order
``('R5', 'Eth0/0'): ('R3', 'Eth0/2')`` should delete the connection:

.. code:: python

    In [11]: t.delete_link(('R5', 'Eth0/0'), ('R3', 'Eth0/2'))

    In [12]: t.topology
    Out[12]:
    {('R1', 'Eth0/0'): ('SW1', 'Eth0/1'),
     ('R2', 'Eth0/0'): ('SW1', 'Eth0/2'),
     ('R2', 'Eth0/1'): ('SW2', 'Eth0/11'),
     ('R3', 'Eth0/0'): ('SW1', 'Eth0/3')}

If there is no such connection, the following message is printed:

.. code:: python

    In [13]: t.delete_link(('R5', 'Eth0/0'), ('R3', 'Eth0/2'))
    There is no such link


Task 22.1c
~~~~~~~~~~~~~

Copy the Topology class from task 22.1b and modify it.

Add a delete_node method that deletes all connections to the specified device.
If there is no such device, the message "There is no such device" is printed.

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

Removing a device:

.. code:: python

    In [3]: t.delete_node('SW1')

    In [4]: t.topology
    Out[4]:
    {('R2', 'Eth0/1'): ('SW2', 'Eth0/11'),
     ('R3', 'Eth0/1'): ('R4', 'Eth0/0'),
     ('R3', 'Eth0/2'): ('R5', 'Eth0/0')}

If there is no such device, the following message is printed:

.. code:: python

    In [5]: t.delete_node('SW1')
    There is no such device


Task 22.1d
~~~~~~~~~~~~~

Copy the Topology class from task 22.1c and modify it.

Add the add_link method, which adds the specified link if it is not already
in the topology. If the connection exists, print the message
"Such a connection already exists", If one of the sides is in the topology,
display the message "A link to one of the ports exists".

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
    Such a connection already exists

    In [12]: t.add_link(('R1', 'Eth0/4'), ('R7', 'Eth0/5'))
    A link to one of the ports exists


Task 22.2
~~~~~~~~~~~~

Create a CiscoTelnet class that connects via Telnet to Cisco equipment.

When instantiating the class, a Telnet connection should be created,
as well as the transition to enable mode. The class must use the telnetlib
module to connect via Telnet.

The CiscoTelnet class, in addition to __init__, must have at least two methods:

* _write_line - takes a string as an argument and sends the string converted
  to bytes to the hardware and adds a line end character at the end.
  The _write_line method must be used inside the class.
* send_show_command - takes the show command as an argument and returns
  the output received from the device

__init__ method parameters:

* ip - IP address
* username - username
* password - password
* secret - enable password

An example of creating an instance of a class:

.. code:: python

    In [2]: from task_22_2 import CiscoTelnet

    In [3]: r1_params = {
       ...:     'ip': '192.168.100.1',
       ...:     'username': 'cisco',
       ...:     'password': 'cisco',
       ...:     'secret': 'cisco'}
       ...:

    In [4]: r1 = CiscoTelnet(**r1_params)

    In [5]: r1.send_show_command("sh ip int br")
    Out[5]: 'sh ip int br\r\nInterface                  IP-Address      OK? Method Status                Protocol\r\nEthernet0/0                192.168.100.1   YES NVRAM  up                    up      \r\nEthernet0/1                192.168.200.1   YES NVRAM  up                    up      \r\nEthernet0/2                unassigned      YES manual up                    up      \r\nEthernet0/3                192.168.130.1   YES NVRAM  up                    up      \r\nR1#'

.. note::

    The _write_line method is needed in order to be able to shorten a line:
    ``self.telnet.write(line.encode("ascii") + b"\n")``

    to this:
    ``self._write_line(line)``

    He shouldn't do anything else.


Task 22.2a
~~~~~~~~~~~~~

Copy the CiscoTelnet class from job 22.2 and modify the send_show_command method
by adding three parameters:

* parse - controls what will be returned: normal command output or a list of dicts
  received after parsing command output using TextFSM.
  If parse=True, a list of dicts should be returned, and parse=False normal output.
  The default is True.
* templates - path to the directory with templates. The default is "templates"
* index is the name of the file where the correspondence between commands and
  templates is stored. The default is "index"

An example of creating an instance of a class:

.. code:: python

    In [1]: r1_params = {
       ...:     'ip': '192.168.100.1',
       ...:     'username': 'cisco',
       ...:     'password': 'cisco',
       ...:     'secret': 'cisco'}

    In [2]: from task_22_2a import CiscoTelnet

    In [3]: r1 = CiscoTelnet(**r1_params)

Using the send_show_command method:

.. code:: python

    In [4]: r1.send_show_command("sh ip int br", parse=True)
    Out[4]:
    [{'intf': 'Ethernet0/0',
      'address': '192.168.100.1',
      'status': 'up',
      'protocol': 'up'},
     {'intf': 'Ethernet0/1',
      'address': '192.168.200.1',
      'status': 'up',
      'protocol': 'up'},
     {'intf': 'Ethernet0/2',
      'address': '192.168.130.1',
      'status': 'up',
      'protocol': 'up'}]

    In [5]: r1.send_show_command("sh ip int br", parse=False)
    Out[5]: 'sh ip int br\r\nInterface                  IP-Address      OK? Method Status
    Protocol\r\nEthernet0/0                192.168.100.1   YES NVRAM  up
    up      \r\nEthernet0/1                192.168.200.1   YES NVRAM  up...'


Task 22.2b
~~~~~~~~~~~~~

Copy the CiscoTelnet class from task 22.2a and add the send_config_commands method.

The send_config_commands method must be able to send one configuration mode
command and a list of commands. The method should return output similar
to the send_config_set method of netmiko (example output below).

An example of creating an instance of a class:

.. code:: python

    In [1]: from task_22_2b import CiscoTelnet

    In [2]: r1_params = {
       ...:     'ip': '192.168.100.1',
       ...:     'username': 'cisco',
       ...:     'password': 'cisco',
       ...:     'secret': 'cisco'}

    In [3]: r1 = CiscoTelnet(**r1_params)

Using the send_config_commands method:

.. code:: python

    In [5]: r1.send_config_commands('logging 10.1.1.1')
    Out[5]: 'conf t\r\nEnter configuration commands, one per line.  End with CNTL/Z.\r\nR1(config)#logging 10.1.1.1\r\nR1(config)#end\r\nR1#'

    In [6]: r1.send_config_commands(['interface loop55', 'ip address 5.5.5.5 255.255.255.255'])
    Out[6]: 'conf t\r\nEnter configuration commands, one per line.  End with CNTL/Z.\r\nR1(config)#interface loop55\r\nR1(config-if)#ip address 5.5.5.5 255.255.255.255\r\nR1(config-if)#end\r\nR1#'


Task 22.2c
~~~~~~~~~~~~~

Copy the CiscoTelnet class from task 22.2b and modify the send_config_commands
method to check for errors.

The send_config_commands method must have an additional strict parameter:

* strict=True means that when an error is encountered, a ValueError must
  be raised (default)
* strict=False means that when an error is found, you only need to print
  the error message to the stdout

The method should return output similar to the send_config_set method of
netmiko (example output below). The text of the exception and error in
the example below.

An example of creating an instance of a class:

.. code:: python

    In [1]: from task_22_2c import CiscoTelnet

    In [2]: r1_params = {
       ...:     'ip': '192.168.100.1',
       ...:     'username': 'cisco',
       ...:     'password': 'cisco',
       ...:     'secret': 'cisco'}

    In [3]: r1 = CiscoTelnet(**r1_params)

    In [4]: commands_with_errors = ['logging 0255.255.1', 'logging', 'a']
    In [5]: correct_commands = ['logging buffered 20010', 'ip http server']
    In [6]: commands = commands_with_errors+correct_commands

Using the send_config_commands method:

.. code:: python

    In [7]: print(r1.send_config_commands(commands, strict=False))
    When executing the command "logging 0255.255.1" on device 192.168.100.1, an error occurred -> Invalid input detected at '^' marker.
    When executing the command "logging" on device 192.168.100.1, an error occurred -> Incomplete command.
    When executing the command "a" on device 192.168.100.1, an error occurred -> Ambiguous command:  "a"
    conf t
    Enter configuration commands, one per line.  End with CNTL/Z.
    R1(config)#logging 0255.255.1
                       ^
    % Invalid input detected at '^' marker.

    R1(config)#logging
    % Incomplete command.

    R1(config)#a
    % Ambiguous command:  "a"
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

    ValueError: When executing the command "logging 0255.255.1" on device 192.168.100.1, an error occurred -> Invalid input detected at '^' marker.
