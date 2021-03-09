.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./exercises_intro.rst

Task 5.1
~~~~~~~~~~~


The task contains a dictionary with information about different devices.

In the task you need: ask the user to enter the device name (r1, r2 or sw1).
Print information about the corresponding device to standard output
(information will be in the form of a dictionary).

An example of script execution:

::

    $ python task_5_1.py
    Enter name of device: r1
    {"location": "21 New Globe Walk", "vendor": "Cisco", "model": "4451", "ios": "15.4", "ip": "10.255.0.1"}

Restriction: You cannot modify the london_co dictionary.

All tasks must be completed using only the topics covered. That is, this task can be
solved without using the if condition.
Restriction: You cannot change london_co dictionary.

.. code:: python

    london_co = {
        "r1": {
            "location": "21 New Globe Walk",
            "vendor": "Cisco",
            "model": "4451",
            "ios": "15.4",
            "ip": "10.255.0.1"
        },
        "r2": {
            "location": "21 New Globe Walk",
            "vendor": "Cisco",
            "model": "4451",
            "ios": "15.4",
            "ip": "10.255.0.2"
        },
        "sw1": {
            "location": "21 New Globe Walk",
            "vendor": "Cisco",
            "model": "3850",
            "ios": "3.6.XE",
            "ip": "10.255.0.101",
            "vlans": "10,20,30",
            "routing": True
        }
    }


Task 5.1a
~~~~~~~~~~~~

Modify the script from task 5.1 so that, in addition to the device name,
the script requested and then printed the device parameter as well.

Display information about the corresponding parameter of the specified device.

An example of script execution:

::

    $ python task_5_1a.py
    Enter device name : r1
    Enter parameter name: ios
    15.4

Restriction: You cannot modify the london_co dictionary.

All tasks must be completed using only the topics covered. That is, this task can be
solved without using the if condition.

.. code:: python

    london_co = {
        "r1": {
            "location": "21 New Globe Walk",
            "vendor": "Cisco",
            "model": "4451",
            "ios": "15.4",
            "ip": "10.255.0.1"
        },
        "r2": {
            "location": "21 New Globe Walk",
            "vendor": "Cisco",
            "model": "4451",
            "ios": "15.4",
            "ip": "10.255.0.2"
        },
        "sw1": {
            "location": "21 New Globe Walk",
            "vendor": "Cisco",
            "model": "3850",
            "ios": "3.6.XE",
            "ip": "10.255.0.101",
            "vlans": "10,20,30",
            "routing": True
        }
    }

Task 5.1b
~~~~~~~~~~~~

Modify the script from task 5.1a so that, when requesting a parameter,
a list of possible parameters was displayed. The list of parameters must be obtained
from the dictionary, rather than written manually.

Display information about the corresponding parameter of the specified device.

An example of script execution:

::

    $ python task_5_1b.py
    Enter device name: r1
    Enter parameter name (ios, model, vendor, location, ip): ip
    10.255.0.1


Restriction: You cannot modify the london_co dictionary.

All tasks must be completed using only the topics covered. That is, this task can be
solved without using the if condition.

.. code:: python

    london_co = {
        "r1": {
            "location": "21 New Globe Walk",
            "vendor": "Cisco",
            "model": "4451",
            "ios": "15.4",
            "ip": "10.255.0.1"
        },
        "r2": {
            "location": "21 New Globe Walk",
            "vendor": "Cisco",
            "model": "4451",
            "ios": "15.4",
            "ip": "10.255.0.2"
        },
        "sw1": {
            "location": "21 New Globe Walk",
            "vendor": "Cisco",
            "model": "3850",
            "ios": "3.6.XE",
            "ip": "10.255.0.101",
            "vlans": "10,20,30",
            "routing": True
        }
    }

Task 5.1c
~~~~~~~~~~~~


Copy and modify the script from task 5.1b so that when you request a parameter
that is not in the device dictionary, the message 'There is no such parameter' is displayed.
The assignment applies only to the parameters of the devices, not to the devices themselves.

.. note::

    Try typing a non-existent parameter, to see what the result will be. And
    then complete the task.                                                                                           
If an existing parameter is selected, print information about the corresponding parameter.

An example of script execution:

::

    $ python task_5_1c.py
    Enter device name: r1
    Enter parameter name (ios, model, vendor, location, ip): ips
    No such parameter

Restriction: You cannot modify the london_co dictionary.

All tasks must be completed using only the topics covered. That is, this task can be
solved without using the if condition.


.. code:: python

    london_co = {
        "r1": {
            "location": "21 New Globe Walk",
            "vendor": "Cisco",
            "model": "4451",
            "ios": "15.4",
            "ip": "10.255.0.1"
        },
        "r2": {
            "location": "21 New Globe Walk",
            "vendor": "Cisco",
            "model": "4451",
            "ios": "15.4",
            "ip": "10.255.0.2"
        },
        "sw1": {
            "location": "21 New Globe Walk",
            "vendor": "Cisco",
            "model": "3850",
            "ios": "3.6.XE",
            "ip": "10.255.0.101",
            "vlans": "10,20,30",
            "routing": True
        }
    }

Task 5.1d
~~~~~~~~~~~~


Modify the script from task 5.1c so that, when requesting a parameter,
the user could enter the parameter name in any case.

An example of script execution:

::

    $ python task_5_1d.py
    Enter device name: r1
    Enter parameter name (ios, model, vendor, location, ip): IOS
    15.4


Restriction: You cannot modify the london_co dictionary.

All tasks must be completed using only the topics covered. That is, this task can be
solved without using the if condition.

.. code:: python

    london_co = {
        "r1": {
            "location": "21 New Globe Walk",
            "vendor": "Cisco",
            "model": "4451",
            "ios": "15.4",
            "ip": "10.255.0.1"
        },
        "r2": {
            "location": "21 New Globe Walk",
            "vendor": "Cisco",
            "model": "4451",
            "ios": "15.4",
            "ip": "10.255.0.2"
        },
        "sw1": {
            "location": "21 New Globe Walk",
            "vendor": "Cisco",
            "model": "3850",
            "ios": "3.6.XE",
            "ip": "10.255.0.101",
            "vlans": "10,20,30",
            "routing": True
        }
    }

Task 5.2
~~~~~~~~~~~

Ask the user to enter the IP network in the format: ``10.1.1.0/24``.

Then print information about the network and mask in this format:

::

    Network:
    10        1         1         0
    00001010  00000001  00000001  00000000

    Mask:
    /24
    255       255       255       0
    11111111  11111111  11111111  00000000

Check the script work on different network/mask combinations.

Hint: You can get the mask in binary format like this:

::

    In [1]: "1" * 28 + "0" * 4
    Out[1]: "11111111111111111111111111110000"

You can then take 8 bits of the binary mask using slices and convert them to decimal.

Restriction: All tasks must be done using the topics covered in this and previous chapters.

Task 5.2a
~~~~~~~~~~~~

Copy and modify the script from task 5.2 so that, if the user entered a host address
rather than a network address, convert the host address to a network address
and print the network address and mask, as in task 5.2.

An example of a network address (all host bits are equal to zero):

* 10.0.1.0/24
* 190.1.0.0/16

Host address example:

* 10.0.1.1/24 - host from network 10.0.1.0/24
* 10.0.5.195/28 - host from network 10.0.5.192/28

If the user entered the address 10.0.1.1/24, the output should look like this:

::

    Network:
    10        0         1         0
    00001010  00000000  00000001  00000000

    Mask:
    /24
    255       255       255       0
    11111111  11111111  11111111  00000000

Check the script work on different host/mask combinations, for example:
10.0.5.195/28, 10.0.1.1/24

Hint:

The network address can be calculated from the binary host address and the netmask.
If the mask is 28, then the network address is the first 28 bits host addresses + 4 zeros.
For example, the host address 10.1.1.195/28 in binary will be:

.. code:: python

    bin_ip = "00001010000000010000000111000011"

Then the network address will be the first 28 characters from bin_ip + 0000
(4 because in total there can be 32 bits in the address, and 32 - 28 = 4)

::

    00001010000000010000000111000000

Restriction: All tasks must be done using the topics covered in this and previous chapters.


Task 5.3
~~~~~~~~~~~~

The script should prompt the user for input:

* interface mode (access/trunk)
* interface number (Gi0/3)
* VLAN number (for trunk mode, a list of VLANs will be entered)

Depending on the selected mode, print corresponding access or trunk configuration
on stdout (command templates are in the lists access_template and trunk_template).

The output should first print the interface line and the interface number, and then
the corresponding template in which the VLAN number (or the list of VLANs) is inserted.

Restriction: All tasks must be done using the topics covered in this and previous chapters.
This task can be solved without using the if condition and for/while loops.

Hint:
Leading up to this task was task 5.1. To make it easier to solve this task,
you can look at task 5.1 and figure out exactly how different information
is displayed in the task, depending on user input.

Below are examples of script execution to make it easier to understand the task.

An example of script execution when the access mode is selected:

::

    $ python task_5_3.py
    Enter interface mode (access/trunk): access
    Enter type and interface number: Fa0/6
    Enter number of vlan (vlans): 3

    interface Fa0/6
    switchport mode access
    switchport access vlan 3
    switchport nonegotiate
    spanning-tree portfast
    spanning-tree bpduguard enable

An example of script execution when the trunk mode is selected:

::

    $ python task_5_3.py
    Enter interface mode (access/trunk): trunk
    Enter type and interface number: Fa0/7
    Enter number of vlan (vlans): 2,3,4,5

    interface Fa0/7
    switchport trunk encapsulation dot1q
    switchport mode trunk
    switchport trunk allowed vlan 2,3,4,5

.. code:: python

    access_template = [
        "switchport mode access", "switchport access vlan {}",
        "switchport nonegotiate", "spanning-tree portfast",
        "spanning-tree bpduguard enable"
    ]

    trunk_template = [
        "switchport trunk encapsulation dot1q", "switchport mode trunk",
        "switchport trunk allowed vlan {}"
    ]

Task 5.3a
~~~~~~~~~~~~

Copy and change the script from task 5.3 in such a way that, depending on
the selected mode, different questions were asked in the request for the VLAN number
or VLAN list:

* for access: 'Enter VLAN number:'
* for trunk: 'Enter the allowed VLANs:'

Restriction: All tasks must be done using the topics covered in this and previous chapters.
This task can be solved without using the if condition and for/while loops.

.. code:: python

    access_template = [
        "switchport mode access", "switchport access vlan {}",
        "switchport nonegotiate", "spanning-tree portfast",
        "spanning-tree bpduguard enable"
    ]

    trunk_template = [
        "switchport trunk encapsulation dot1q", "switchport mode trunk",
        "switchport trunk allowed vlan {}"
    ]
