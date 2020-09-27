.. raw:: latex

   \newpage

Tasks
=======

.. include:: ./exercises_intro.rst

Task 5.1
~~~~~~~~~~~

A dictionary with information about different devices is created in the task.

You should ask user to enter device name (r1, r2 or sw1). And display information about corresponding device on standard output stream (information will be in form of a dictionary).


Example of script execution:

::

    $ python task_5_1.py
    Enter name of device: r1
    {"location": "21 New Globe Walk", "vendor": "Cisco", "model": "4451", "ios": "15.4", "ip": "10.255.0.1"}

Restriction: You cannot change london_co dictionary.

Restriction: All tasks must be performed using only covered topics.
That is, it is possible to solve this task without using *if* condition and other topics to be discussed later.


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

Modify script from Task 5.1 so that in addition to device name the device parameter that you want to display is also requested.

Display information about corresponding parameter of specified device.

Example of script execution:

::

    $ python task_5_1a.py
    Enter device name : r1
    Enter parameter name: ios
    15.4

Restriction: You cannot change london_co dictionary.

Restriction: All tasks must be performed using only covered topics.
That is, it is possible to solve this task without using *if* condition and other topics to be discussed later.

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

Modify script from task 5.1so that a list of possible parameters is displayed when you ask for parameter. List of parameters should be obtained from dictionary, not written manually.

Display information about corresponding parameter of specified device.

Example of script execution:

::

    $ python task_5_1b.py
    Enter device name: r1
    Enter parameter name (ios, model, vendor, location, ip): ip
    10.255.0.1

Restriction: You cannot change london_co dictionary.

Restriction: All tasks must be performed using only covered topics.
That is, it is possible to solve this task without using *if* condition and other topics to be discussed later.

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

Modify script from task 5.1b so that when you ask for a parameter that is not present in device dictionary, the message "No such parameter" is displayed.

.. note::
    Try typing an invalid parameter name or a nonexistent parameter to see what the result is. And then do the task.

If an existing parameter is selected display information about corresponding parameter of specified device.

Example of script execution:

::

    $ python task_5_1c.py
    Enter device name: r1
    Enter parameter name (ios, model, vendor, location, ip): ips
    No such parameter

Restriction: You cannot change london_co dictionary.

Restriction: All tasks must be performed using only covered topics.
That is, it is possible to solve this task without using *if* condition and other topics to be discussed later.

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

Modify script from task 5.1c so that when you ask for parameter, user can enter name of parameter in any register.

Example of script execution:

::

    $ python task_5_1d.py
    Enter device name: r1
    Enter parameter name (ios, model, vendor, location, ip): IOS
    15.4


Restriction: You cannot change london_co dictionary.

Restriction: All tasks must be performed using only covered topics.
That is, it is possible to solve this task without using *if* condition and other topics to be discussed later.

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

Request user to enter an IP network in format: ``10.1.1.0/24``

Then display network and mask information in this format:

::

    Network:
    10        1         1         0
    00001010  00000001  00000001  00000000

    Mask:
    /24
    255       255       255       0
    11111111  11111111  11111111  00000000

Check script on different combinations of network/mask.

Hint: Get a mask in binary format:

::

    In [1]: "1" * 28 + "0" * 4
    Out[1]: "11111111111111111111111111110000"

Restriction: All tasks must be performed using only covered topics.

Task 5.2a
~~~~~~~~~~~~

It’s like task 5.2 but if user entered host address instead of network address, you have to convert host address to network address and display network address and mask as in task 5.2.

Example of network address (all host bits are zero):

* 10.0.1.0/24
* 190.1.0.0/16

Example of host address:

* 10.0.1.1/24 - хост из сети 10.0.1.0/24
* 10.0.5.1/30 - хост из сети 10.0.5.0/30

If user entered address 10.0.1.1/24, , the output should be:

::

    Network:
    10        0         1         0
    00001010  00000000  00000001  00000000

    Mask:
    /24
    255       255       255       0
    11111111  11111111  11111111  00000000

Check script on different host/mask combinations, for example: 10.0.5.195/28, 10.0.1.1/24

Hint:

There is a binary host address and a network mask 28. Network address is the first 28 bits of host address + 4 zeros. That is, for example, host address 10.1.1.195/28 in binary format will be ``bin_ip = "00001010000000010000000111000011"``.

And network address will be the first 28 symbols from bin_ip + 0000 (4 because total address can be 32 bits and 32 - 28 = 4): ``00001010000000010000000111000000``

Restriction: All tasks must be performed using only covered topics.


Task 5.2b
~~~~~~~~~~~~

Modify script from task 5.2a so that the network/mask is not requested from user, but is passed as script argument.

Restriction: All tasks must be performed using only covered topics.

Task 5.3
~~~~~~~~~~~~

Script must request from user:

* interface mode (access/trunk)
* interface number (type and number, like Gi0/3)
* Vlan number (Vlan list will be entered for trunk mode)

Depending on selected mode, the appropriate access or trunk configuration should be displayed (command templates are in access_template and trunk_template lists).

First, interface string goes and interface number is substituted and then goes the corresponding template into which Vlan number (or Vlan list) is substituted.

Restriction: All tasks must be performed using only covered topics.
That is, it is possible to solve this task without using *if* condition and *for/while* loops.

Hint:
Leading to this task was task 5.1. To make this task easier, you can look at task 5.1 and figure out how it was possible to extract different information depending on user’s input.


The following are examples of how to execute a script to make task easier to understand.

Example of script execution when you select access mode:

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

Example of script execution if trunk mode is selected:

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


Complete script from task 5.3 in such a way that depending on selected mode the different questions are asked in request for Vlan number or Vlan list:

* for access: "Enter VLAN number:"
* for trunk: "Enter allowed VLANs:"

Restriction: All tasks must be performed using only covered topics.
That is, it is possible to solve this task without using *if* condition and *for/while* loops.

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
