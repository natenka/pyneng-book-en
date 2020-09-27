.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./pytest.rst

Task 19.1
~~~~~~~~~~~~

Create ping_ip_addresses() function that checks if IP addresses are pingable. Checking IP addresses should be performed in parallel in different threads.

Function parameters:

* ip_list - list of IP addresses
* limit - maximum number of parallel threads (default 3)

Function should return a tuple with two lists:

* list of available IP addresses
* list of unreachable IP addresses

You can create any additional functions to complete task.

To check availability of IP address, use ping.

.. note::

    concurrent.futures hint: if you need to ping multiple IP addresses in different threads, you need to create a function that pings one IP address and then run this function in different threads for different IP addresses with concurrent.futures (ping_ip_addresses() function should do this).

Task 19.2
~~~~~~~~~~~~

Create send_show_command_to_devices() function that sends the same show command to different devices in parallel threads and then writes output of commands to a file. Output from devices in file can be in any order.

Function parameters:

* devices - list of dictionaries with connection parameters to devices
* command - command
* filename - name of text file into which the output of all commands will be written
* limit - maximum number of parallel threads (default 3)

Function does not return anything.

Output of commands should be written to a plain text file in this format (you should write host name and command itself before output of command):

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

You can create any additional functions to complete task.

Check function with devices from device.yaml file

Task 19.3
~~~~~~~~~~~~

Create send_command_to_devices() function that sends different show commands to different devices in parallel threads and then writes the output of commands to a file. Output from devices in file can be in any order.

Function parameters:

* devices - list of dictionaries with devices connection parameters
* commands_dict - dictionary that specifies which device to send which command. Example dictionary - *commands*
* filename - name of file into which the outputs of all commands will be written
* limit - maximum number of parallel threads (default 3)

Function does not return anything.

Output of commands should be written to a plain text file in this format (you should write host name and command itself before output of command):

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
    
You can create any additional functions to complete task.

Check function with devices from device.yaml file and commands dictionary

.. code:: python

    # This dictionary is only needed to check code operation, you can change IP addresses in it
    # test takes addresses from device.yaml file

    commands = {
        "192.168.100.3": "sh run | s ^router ospf",
        "192.168.100.1": "sh ip int br",
        "192.168.100.2": "sh int desc",
    }

Task 19.3a
~~~~~~~~~~~~~

Create send_command_to_devices() function that sends a list of specified show commands to different devices in parallel threads and then writes the output of commands to a file. Output from devices in file can be in any order.

Function parameters:

* devices - list of dictionaries with devices connection parameters
* commands_dict - dictionary that specifies which device to send which command. Example dictionary - *commands*
* filename - name of file into which the outputs of all commands will be written
* limit - maximum number of parallel threads (default 3)

Function does not return anything.

Output of commands should be written to a plain text file in this format (you should write host name and command itself before output of command):

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


Commands in file can be in any order.

To complete task you can create any additional functions and use functions created in previous tasks.

Check function with devices from device.yaml file and *commands* dictionary

.. code:: python

    # This dictionary is only needed to check code operation, you can change IP addresses in it
    # test takes addresses from device.yaml file
    commands = {
        "192.168.100.3": ["sh ip int br", "sh ip route | ex -"],
        "192.168.100.1": ["sh ip int br", "sh int desc"],
        "192.168.100.2": ["sh int desc"],
    }


Task 19.4
~~~~~~~~~~~~

Create send_commands_to_devices() function that sends show or config command to different devices in parallel threads and then writes output to a file.

Function parameters:

* devices - list of dictionaries with devices connection parameters
* show - show command to send (default None)
* config - configuration mode commands to send (default None)
* filename - name of file into which outputs of all commands will be written
* limit - maximum number of parallel threads (default 3)

Function does not return anything.

Output of commands should be written to a file in this format (you should write host name and command itself before output of command):

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

Example of function call:

.. code:: python

    In [5]: send_commands_to_devices(devices, show='sh clock', filename='result.txt')

    In [6]: cat result.txt
    R1#sh clock
    *04:56:34.668 UTC Sat Mar 23 2019
    R2#sh clock
    *04:56:34.687 UTC Sat Mar 23 2019
    R3#sh clock
    *04:56:40.354 UTC Sat Mar 23 2019

    In [11]: send_commands_to_devices(devices, config='logging 10.5.5.5', filename='result.txt')

    In [12]: cat result.txt
    config term
    Enter configuration commands, one per line.  End with CNTL/Z.
    R1(config)#logging 10.5.5.5
    R1(config)#end
    R1#config term
    Enter configuration commands, one per line.  End with CNTL/Z.
    R2(config)#logging 10.5.5.5
    R2(config)#end
    R2#config term
    Enter configuration commands, one per line.  End with CNTL/Z.
    R3(config)#logging 10.5.5.5
    R3(config)#end
    R3#

    In [13]: send_commands_to_devices(devices,
                                      config=['router ospf 55', 'network 0.0.0.0 255.255.255.255 area 0'],
                                      filename='result.txt')

    In [14]: cat result.txt
    config term
    Enter configuration commands, one per line.  End with CNTL/Z.
    R1(config)#router ospf 55
    R1(config-router)#network 0.0.0.0 255.255.255.255 area 0
    R1(config-router)#end
    R1#config term
    Enter configuration commands, one per line.  End with CNTL/Z.
    R2(config)#router ospf 55
    R2(config-router)#network 0.0.0.0 255.255.255.255 area 0
    R2(config-router)#end
    R2#config term
    Enter configuration commands, one per line.  End with CNTL/Z.
    R3(config)#router ospf 55
    R3(config-router)#network 0.0.0.0 255.255.255.255 area 0
    R3(config-router)#end
    R3#


You can create any additional functions to complete task.
