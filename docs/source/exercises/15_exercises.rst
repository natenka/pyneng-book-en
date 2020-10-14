.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./pytest.rst

Task 15.1
~~~~~~~~~~~~

Create a get_ip_from_cfg() function that expects as argument the name of file with device configuration.

Function should process configuration and return IP addresses and masks, that are configured on interfaces, as a list of tuples:

* first element of tuple - IP address
* second element of tuple - mask

For example (arbitrary addresses are taken):

.. code:: python

    [("10.0.1.1", "255.255.255.0"), ("10.0.2.1", "255.255.255.0")]

To get this result, use regular expressions.

Check function with config_r1.txt file.

Please note that in this case you can skip checking the correctness of IP address, address ranges, etc., since command output is processed, not user input.

Task 15.1a
~~~~~~~~~~~~~

Copy get_ip_from_cfg() function from task 15.1 and redo it to return dictionary:

* key: interface name
* value: tuple with two strings:

  * IP address
  * mask

Add to dictionary only those interfaces where IP addresses are configured.

For example (arbitrary addresses are taken):

.. code:: python

    {"FastEthernet0/1": ("10.0.1.1", "255.255.255.0"),
     "FastEthernet0/2": ("10.0.2.1", "255.255.255.0")}

To get this result, use regular expressions.

Check function with config_r1.txt file.

Please note that in this case you can skip checking the correctness of IP address, address ranges, etc., since command output is processed, not user input.

Task 15.1b
~~~~~~~~~~~~~

Check get_ip_from_cfg() function from 15.1a task based on config_r2.txt.

Note that there are two IP addresses assigned to interface e0/1:

::

    interface Ethernet0/1
     ip address 10.255.2.2 255.255.255.0
     ip address 10.254.2.2 255.255.255.0 secondary

And in dictionary that returns get_ip_from_cfg() function, only one IP (second) corresponds to interface Ethernet0/1.

Copy get_ip_from_cfg() function from 15.1a task and redo it so that it returns a list of tuples for each interface. If only one address is assigned to interface, one tuple will be listed. If multiple IP addresses are configured on interface, several tuples will be listed.

Check function with config_r2.txt configuration file and ensure that interface Ethernet0/1 corresponds to list of two tuples.

Please note that in this case you can skip checking the correctness of IP address, address ranges, etc., since command output is processed, not user input.

Task 15.2
~~~~~~~~~~~~

Create parse_sh_ip_int_br() function that expects as an argument the name of file in which show ip int br output is found.

Function should process the output of show ip int br command and return such fields:

* Interface
* IP-Address
* Status
* Protocol

Information should be returned in the form of a list of tuples:

.. code:: python

    [("FastEthernet0/0", "10.0.1.1", "up", "up"),
     ("FastEthernet0/1", "10.0.2.1", "up", "up"),
     ("FastEthernet0/2", "unassigned", "down", "down")]

To get this result, use regular expressions.

Check function with sh_ip_int_br.txt file.

Task 15.2a
~~~~~~~~~~~~~

Create convert_to_dict() function that expects two arguments:

* list of field names
* list of tuples with values


Function returns the result as a list of dictionaries, where keys are taken from first list and values are substituted from second list.

For example, if functions pass as arguments *headers* list and list

.. code:: python

    [("R1", "12.4(24)T1", "Cisco 3825"),
     ("R2", "15.2(2)T1", "Cisco 2911")]

Function should return such list with dictionaries:

.. code:: python

    [{"hostname": "R1", "ios": "12.4(24)T1", "platform": "Cisco 3825"},
     {"hostname": "R2", "ios": "15.2(2)T1", "platform": "Cisco 2911"}]

Function should not be tied to specific data or amount of headers/data in tuples.

Check function with:

* first argument - *headers* list
* second argument - *data* list

Restriction: All tasks must be performed using only covered topics.

.. code:: python

    headers = ["hostname", "ios", "platform"]

    data = [
        ("R1", "12.4(24)T1", "Cisco 3825"),
        ("R2", "15.2(2)T1", "Cisco 2911"),
        ("SW1", "12.2(55)SE9", "Cisco WS-C2960-8TC-L"),
    ]


Task 15.3
~~~~~~~~~~~~

Create convert_ios_nat_to_asa() function that converts NAT rules from cisco IOS syntax to cisco ASA.

Function expects such arguments:

* name of file containing NAT rules for Cisco IOS 
* name of file to which to write resulting NAT rules for ASA

Function does not return anything.

Check function with cisco_nat_config.txt file.

Example of NAT rules for cisco IOS 

::

    ip nat inside source static tcp 10.1.2.84 22 interface GigabitEthernet0/1 20022
    ip nat inside source static tcp 10.1.9.5 22 interface GigabitEthernet0/1 20023

And corresponding NAT rules for ASA:

::

    object network LOCAL_10.1.2.84
     host 10.1.2.84
     nat (inside,outside) static interface service tcp 22 20022
    object network LOCAL_10.1.9.5
     host 10.1.9.5
     nat (inside,outside) static interface service tcp 22 20023

In ASA rules file:

* no empty lines between rules
* "object network" lines should not be preceded by spaces
* there should be one space in front of the rest of lines

All ASA rules will have the same interfaces (inside,outside).

Task 15.4
~~~~~~~~~~~~

Create get_ints_without_description() function that expects as argument the name of file in which device configuration is found.

Function should process configuration and return a list of interface names that do not have a description (*description* command)..

Example of interface with description:

::

    interface Ethernet0/2
     description To P_r9 Ethernet0/2
     ip address 10.0.19.1 255.255.255.0
     mpls traffic-eng tunnels
     ip rsvp bandwidth

Interface without description:

::

    interface Loopback0
     ip address 10.1.1.1 255.255.255.255

Check function with config_r1.txt file.

Task 15.5
~~~~~~~~~~~~

Create generate_description_from_cdp() function that expects as an argument the name of file containing show cdp neighbors command output.

Function should process the output of show cdp neighbors command and generate a description for interfaces based on command output.

For example, if R1 has this command output:

::

    R1>show cdp neighbors
    Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
                      S - Switch, H - Host, I - IGMP, r - Repeater

    Device ID        Local Intrfce     Holdtme    Capability  Platform  Port ID
    SW1              Eth 0/0           140          S I      WS-C3750-  Eth 0/1

For interface Eth 0/0, you should generate this description  ``description Connected to SW1 port Eth 0/1``.

Function should return a dictionary where keys are interface names and values are command that defines interface description:

::

    "Eth 0/0": "description Connected to SW1 port Eth 0/1"


Check function with sh_cdp_n_sw1.txt file.
