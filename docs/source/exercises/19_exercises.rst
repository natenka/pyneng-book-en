.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./pytest.rst

Task 19.1
~~~~~~~~~~~~

Create a ping_ip_addresses function that checks if IP addresses are pingable.
Checking IP addresses should be done concurrent in different threads.

Ping_ip_addresses function parameters:

* ip_list - list of IP addresses
* limit - maximum number of parallel threads (default 3)

The function must return a tuple with two lists:

* list of available IP addresses
* list of unavailable IP addresses

You can create any additional functions to complete the task.
To check the availability of an IP address, use ping.

.. note::

    A hint about working with concurrent.futures:
    If you need to ping several IP addresses in different threads, you need to create
    a function that will ping one IP address, and then run this function in different
    threads for different IP addresses using concurrent.futures (this last part
    must be done in the ping_ip_addresses function).


Task 19.2
~~~~~~~~~~~~

Create a send_show_command_to_devices function that sends the same show command
to different devices in concurrent threads and then writes the output of
the commands to a file. The output from the devices in the file can be in any order.

Function parameters:

* devices - a list of dictionaries with parameters for connecting to devices
* command - show command
* filename - is the name of a text file to which the output of all commands will be written
* limit - maximum number of concurrent threads (default 3)

The function returns None.

The output of the commands should be written to a plain text file in this
format (before the output of the command, you must write the hostname and
the command itself):

::

    R1#sh ip int br
    Interface                  IP-Address      OK? Method Status                Protocol
    Ethernet0/0                192.168.100.1   YES NVRAM  up                    up
    Ethernet0/1                192.168.200.1   YES NVRAM  up                    up
    R2#sh ip int br
    Interface                  IP-Address      OK? Method Status                Protocol
    Ethernet0/0                192.168.100.2   YES NVRAM  up                    up
    Ethernet0/1                10.1.1.1        YES NVRAM  administratively down down
    R3#sh ip int br
    Interface                  IP-Address      OK? Method Status                Protocol
    Ethernet0/0                192.168.100.3   YES NVRAM  up                    up
    Ethernet0/1                unassigned      YES NVRAM  administratively down down

You can create any additional functions to complete the task.

Check the operation of the function on devices from the devices.yaml file.

Task 19.3
~~~~~~~~~~~~

Create a send_command_to_devices function that sends different show commands
to different devices in concurrent threads and then writes the output of the
commands to a file. The output from the devices in the file can be in any order.

Function parameters:

* devices - a list of dictionaries with parameters for connecting to devices
* commands_dict - a dictionary that specifies which device to send which command.
  Dictionary example - commands
* filename is the name of the file to which the output of all commands will be written
* limit - maximum number of concurrent threads (default 3)

The function returns None.

The output of the commands should be written to a plain text file in this
format (before the output of the command, you must write the hostname and
the command itself):

::

    R1#sh ip int br
    Interface                  IP-Address      OK? Method Status                Protocol
    Ethernet0/0                192.168.100.1   YES NVRAM  up                    up
    Ethernet0/1                192.168.200.1   YES NVRAM  up                    up
    R2#sh int desc
    Interface                      Status         Protocol Description
    Et0/0                          up             up
    Et0/1                          up             up
    Et0/2                          admin down     down
    Et0/3                          admin down     down
    Lo9                            up             up
    Lo19                           up             up
    R3#sh run | s ^router ospf
    router ospf 1
     network 0.0.0.0 255.255.255.255 area 0
    
You can create any additional functions to complete the task.

Check the operation of the function on devices from the devices.yaml file.

.. code:: python

    # This dictionary is only needed to check the operation of the code;
    # you can change the IP addresses in it.
    # The test takes addresses from the devices.yaml file
    commands = {
        "192.168.100.3": "sh run | s ^router ospf",
        "192.168.100.1": "sh ip int br",
        "192.168.100.2": "sh int desc",
    }

Task 19.3a
~~~~~~~~~~~~~

Create a send_command_to_devices function that sends a list of the specified
show commands to different devices in concurrent threads, and then writes the
output of the commands to a file. The output from the devices in the file can
be in any order.

Function parameters:

* devices - a list of dictionaries with parameters for connecting to devices
* commands_dict - a dictionary that specifies which device to send which commands.
  Dictionary example - commands
* filename is the name of the file to which the output of all commands will be written
* limit - maximum number of parallel threads (default 3)

The function returns None.

The output of the commands should be written to a plain text file in this
format (before the output of the command, you must write the hostname and
the command itself):

::

    R2#sh arp
    Protocol  Address          Age (min)  Hardware Addr   Type   Interface
    Internet  192.168.100.1          87   aabb.cc00.6500  ARPA   Ethernet0/0
    Internet  192.168.100.2           -   aabb.cc00.6600  ARPA   Ethernet0/0
    R1#sh ip int br
    Interface                  IP-Address      OK? Method Status                Protocol
    Ethernet0/0                192.168.100.1   YES NVRAM  up                    up
    Ethernet0/1                192.168.200.1   YES NVRAM  up                    up
    R1#sh arp
    Protocol  Address          Age (min)  Hardware Addr   Type   Interface
    Internet  10.30.0.1               -   aabb.cc00.6530  ARPA   Ethernet0/3.300
    Internet  10.100.0.1              -   aabb.cc00.6530  ARPA   Ethernet0/3.100
    R3#sh ip int br
    Interface                  IP-Address      OK? Method Status                Protocol
    Ethernet0/0                192.168.100.3   YES NVRAM  up                    up
    Ethernet0/1                unassigned      YES NVRAM  administratively down down
    R3#sh ip route | ex -

    Gateway of last resort is not set

          10.0.0.0/8 is variably subnetted, 4 subnets, 2 masks
    O        10.1.1.1/32 [110/11] via 192.168.100.1, 07:12:03, Ethernet0/0
    O        10.30.0.0/24 [110/20] via 192.168.100.1, 07:12:03, Ethernet0/0


Commands can be written to a file in any order.
To complete the task, you can create any additional functions,
as well as use the functions created in previous tasks.

Check the operation of the function on devices from the devices.yaml file
and the commands dictionary

.. code:: python

    # This dictionary is only needed to check the operation of the code;
    # you can change the IP addresses in it.
    # The test takes addresses from the devices.yaml file
    commands = {
        "192.168.100.3": ["sh ip int br", "sh ip route | ex -"],
        "192.168.100.1": ["sh ip int br", "sh int desc"],
        "192.168.100.2": ["sh int desc"],
    }


Task 19.4
~~~~~~~~~~~~

Create a send_commands_to_devices function that sends a show or config command
to different devices in concurrent threads and then writes the output of the
commands to a file.

Function parameters:

* devices - a list of dictionaries with parameters for connecting to devices
* filename is the name of the file to which the output of all commands will be written
* show - the show command to be sent (by default, the value is None)
* config - configuration mode commands to be sent (default None)
* limit - maximum number of parallel threads (default 3)

The function returns None.

The show, config and limit arguments should only be passed as keyword arguments.
Passing these arguments as positional should raise a TypeError exception.

.. code:: python

    In [4]: send_commands_to_devices(devices, 'result.txt', 'sh clock')
    ---------------------------------------------------------------------------
    TypeError                                 Traceback (most recent call last)
    <ipython-input-4-75adcfb4a005> in <module>
    ----> 1 send_commands_to_devices(devices, 'result.txt', 'sh clock')

    TypeError: send_commands_to_devices() takes 2 positional argument but 3 were given

When calling the send_commands_to_devices function, only one of the show,
config arguments should always be passed. If both arguments are passed,
a ValueError exception should be raised.

The output of the commands should be written to a plain text file in this
format (before the output of the command, you must write the hostname and
the command itself):

::

    R1#sh ip int br
    Interface                  IP-Address      OK? Method Status                Protocol
    Ethernet0/0                192.168.100.1   YES NVRAM  up                    up
    Ethernet0/1                192.168.200.1   YES NVRAM  up                    up
    R2#sh arp
    Protocol  Address          Age (min)  Hardware Addr   Type   Interface
    Internet  192.168.100.1          76   aabb.cc00.6500  ARPA   Ethernet0/0
    Internet  192.168.100.2           -   aabb.cc00.6600  ARPA   Ethernet0/0
    Internet  192.168.100.3         173   aabb.cc00.6700  ARPA   Ethernet0/0
    R3#sh ip int br
    Interface                  IP-Address      OK? Method Status                Protocol
    Ethernet0/0                192.168.100.3   YES NVRAM  up                    up
    Ethernet0/1                unassigned      YES NVRAM  administratively down down

An example of a function call:

.. code:: python

    In [5]: send_commands_to_devices(devices, 'result.txt', show='sh clock')

    In [6]: cat result.txt
    R1#sh clock
    *04:56:34.668 UTC Sat Mar 23 2019
    R2#sh clock
    *04:56:34.687 UTC Sat Mar 23 2019
    R3#sh clock
    *04:56:40.354 UTC Sat Mar 23 2019

    In [11]: send_commands_to_devices(devices, 'result.txt', config='logging 10.5.5.5')

    In [12]: cat result.txt
    config term
    Enter configuration commands, one per line.  End with CNTL/Z.
    R1(config)#logging 10.5.5.5
    R1(config)#end
    R1#
    config term
    Enter configuration commands, one per line.  End with CNTL/Z.
    R2(config)#logging 10.5.5.5
    R2(config)#end
    R2#
    config term
    Enter configuration commands, one per line.  End with CNTL/Z.
    R3(config)#logging 10.5.5.5
    R3(config)#end
    R3#

    In [13]: commands = ['router ospf 55', 'network 0.0.0.0 255.255.255.255 area 0']

    In [13]: send_commands_to_devices(devices, 'result.txt', config=commands)

    In [14]: cat result.txt
    config term
    Enter configuration commands, one per line.  End with CNTL/Z.
    R1(config)#router ospf 55
    R1(config-router)#network 0.0.0.0 255.255.255.255 area 0
    R1(config-router)#end
    R1#
    config term
    Enter configuration commands, one per line.  End with CNTL/Z.
    R2(config)#router ospf 55
    R2(config-router)#network 0.0.0.0 255.255.255.255 area 0
    R2(config-router)#end
    R2#
    config term
    Enter configuration commands, one per line.  End with CNTL/Z.
    R3(config)#router ospf 55
    R3(config-router)#network 0.0.0.0 255.255.255.255 area 0
    R3(config-router)#end
    R3#

You can create any additional functions to complete task.
