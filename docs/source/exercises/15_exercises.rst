.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./pytest.rst

Task 15.1
~~~~~~~~~~~~

Create a get_ip_from_cfg function that expects the name of the file containing
the device configuration as an argument.

The function should process the configuration and return the IP addresses and
masks that are configured on the interfaces as a list of tuples:

* the first element of the tuple is the IP address
* the second element of the tuple is a mask

For example (arbitrary addresses are taken):

.. code:: python

    [("10.0.1.1", "255.255.255.0"), ("10.0.2.1", "255.255.255.0")]

To get this result, use regular expressions.

Check the operation of the function using the config_r1.txt file.

Please note that in this case, you can not check the correctness
of the IP address, address ranges, and so on, since the command
output from network device is processed, not user input.


Task 15.1a
~~~~~~~~~~~~~

Copy the get_ip_from_cfg function from task 15.1 and redesign it so
that it returns a dictionary:

* key: interface name
* value: a tuple with two lines:

   * IP address
   * mask

Add to the dictionary only those interfaces on which IP addresses are configured.

Dict example (arbitrary addresses are taken):

.. code:: python

    {"FastEthernet0/1": ("10.0.1.1", "255.255.255.0"),
     "FastEthernet0/2": ("10.0.2.1", "255.255.255.0")}

To get this result, use regular expressions.

Check the operation of the function using the example of the config_r1.txt file.

Please note that in this case, you can not check the correctness
of the IP address, address ranges, and so on, since the command
output from network device is processed, not user input.


Task 15.1b
~~~~~~~~~~~~~

Check the get_ip_from_cfg function from task 15.1a on the config_r2.txt configuration.

Note that there are two IP addresses assigned on the e0/1 interface:

::

    interface Ethernet0/1
     ip address 10.255.2.2 255.255.255.0
     ip address 10.254.2.2 255.255.255.0 secondary

And in the dictionary returned by the get_ip_from_cfg function, only one of them
(first or second) corresponds to the Ethernet0/1 interface.

Copy the get_ip_from_cfg function from 15.1a and redesign it to return
a list of tuples for each interface in the dictionary value.
If only one address is assigned on the interface, there will be one tuple in the list.
If several IP addresses are configured on the interface, then the list will
contain several tuples. The interface name remains the key.

Check the function in the config_r2.txt configuration and make sure the
Ethernet0/1 interface matches a list of two tuples.

Please note that in this case, you can not check the correctness
of the IP address, address ranges, and so on, since the command
output from network device is processed, not user input.


Task 15.2
~~~~~~~~~

Create a function parse_sh_ip_int_br that expects as an argument the name
of the file containing the output of the show ip int br command.

The function should process the output of the show ip int br command
and return the following fields:

* Interface
* IP-Address
* Status
* Protocol

The information should be returned as a list of tuples:

.. code:: python

    [("FastEthernet0/0", "10.0.1.1", "up", "up"),
     ("FastEthernet0/1", "10.0.2.1", "up", "up"),
     ("FastEthernet0/2", "unassigned", "down", "down")]

To get this result, use regular expressions.

Check the operation of the function using the example of the sh_ip_int_br.txt file.


Task 15.3
~~~~~~~~~~~~

Create a convert_ios_nat_to_asa function that converts NAT rules from
cisco IOS syntax to cisco ASA.

The function expects such arguments:

* the name of the file containing the Cisco IOS NAT rules
* the name of the file in which to write the NAT rules for the ASA

The function returns None.

Check the function on the cisco_nat_config.txt file.

Example cisco IOS NAT rules

::

    ip nat inside source static tcp 10.1.2.84 22 interface GigabitEthernet0/1 20022
    ip nat inside source static tcp 10.1.9.5 22 interface GigabitEthernet0/1 20023

And the corresponding NAT rules for the ASA:

::

    object network LOCAL_10.1.2.84
     host 10.1.2.84
     nat (inside,outside) static interface service tcp 22 20022
    object network LOCAL_10.1.9.5
     host 10.1.9.5
     nat (inside,outside) static interface service tcp 22 20023

In the file with the rules for the ASA:

* there should be no blank lines between the rules
* there must be no spaces before the lines "object network"
* there must be one space before the rest of the lines

In all rules for ASA, the interfaces will be the same (inside, outside).

Task 15.4
~~~~~~~~~~~~

Create a get_ints_without_description function that expects as an argument
the name of the file containing the device configuration.

The function should process the configuration and return a list of interface names,
which do not have a description (description command).

An example of an interface with a description:

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

Check the operation of the function using the example of the config_r1.txt file.


Task 15.5
~~~~~~~~~~~~

Create a generate_description_from_cdp function that expects as an argument
the name of the file that contains the output of the show cdp neighbors command.

The function should process the show cdp neighbors command output and generate
a description for the interfaces based on the command output.

For example, if R1 has the following command output:

::

    R1>show cdp neighbors
    Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
                      S - Switch, H - Host, I - IGMP, r - Repeater

    Device ID        Local Intrfce     Holdtme    Capability  Platform  Port ID
    SW1              Eth 0/0           140          S I      WS-C3750-  Eth 0/1

For the Eth 0/0 interface, you need to generate the following description:

::

    description Connected to SW1 port Eth 0/1

The function must return a dictionary, in which the keys are the names
of the interfaces, and the values are the command specifying the description
of the interface:

::

    'Eth 0/0': 'description Connected to SW1 port Eth 0/1'

Check the operation of the function on the sh_cdp_n_sw1.txt file.
